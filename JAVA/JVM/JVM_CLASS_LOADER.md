# JVM - Class Loader

> https://tecoble.techcourse.co.kr/post/2021-07-15-jvm-classloader/ 이 페이지의 글을 정리목적으로 거의 옮기다시피 했습니다. 원문을 보시길 추천드립니다.

## 클래스 로더란?
자바는 런타임(컴파일 타임 X)에 클래스 파일을 **로드, 링크 및 초기화**한다. 이 역할을 수행하는 것이 Class Loader이다.

### 1. 로딩 - 클래스 파일을 탑재하는 과정

#### 1-1. Bootstrap Class Loader

Java의 모든 클래스 로더의 부모역할을 한다. JVM을 구동시키기 위한 필수적인 라이브러리의 클래스를 로드한다. (rt.jar, 핵심 Java API). C/C++와 같은 네이티브 언어로 구현된다.

#### 1-2. Extension Class Loader

Bootstrap Class Loader 에서 찾지 못한 클래스들을 확장 디렉토리에서 로드한다. 
> Bootstrap and if unsuccessful, loads classes from the extensions directories (e.g. security extension functions) in extension path — $JAVA_HOME/jre/lib/ext or any other directory specified by the java.ext.dirs system property.

`sun.misc.Launcher$ExtClassLoader`로 구현된다.

#### 1-3. Application Class Loader (System Class Loader)

Classpath에 있는 클래스들을 로드한다. 개발자들이 만든 Class 파일들을 JVM에 로드한다.
`sun.misc.Launcher$AppClassLoader` 로 구현된다.

> 만약 개발자가 ClassLoader 를 구현하여 사용하게 되면, 기본적으로 위의 Application ClassLoader 의 자식 형태로 사용자 정의된 ClassLoader 를 구성하게 됩니다.

로딩 과정에서 하위 ClassLoader가 로딩한 클래스 파일은 상위 ClassLoader가 로딩한 클래스 파일을 볼 수가 있다. 하지만 반대는 불가능하다.
한 번 JVM에 로딩된 클래스 파일은 종료될 때까지 JVM에서 제거되지 않습니다.

```java
public class Main {

   public static void main(String[] args) {
       UserClass userClass = new UserClass(2);
       System.out.println("Application ClassLoader: " + UserClass.class.getClassLoader());
       System.out.println("Extension ClassLoader: " + UserClass.class.getClassLoader().getParent());
       System.out.println("Bootstrap ClassLoader: " + UserClass.class.getClassLoader().getParent().getParent());
   }
}
```
- 코드 출처 : https://tecoble.techcourse.co.kr/post/2021-07-15-jvm-classloader/


### 2. 링킹 - 클래스 파일을 사용하기 위해 검증

Linking 은 로드된 클래스 파일들을 검증하고, 사용할 수 있게 준비하는 과정을 의미한다. Linking 또한 Verification , Preparation , 그리고 Resolution 이라는 세 가지 단계로 이루어져 있다.

#### 2-1. Verification

클래스 파일이 유효한지를 확인하는 과정이다. 클래스 파일이 JVM 의 구동 조건 대로 구현되지 않았을 경우에는 VerifyError 를 던지게 된다.

#### 2-2. Preparation

클래스 및 인터페이스에 필요한 static field 메모리를 할당하고, 이를 기본값으로 초기화를 한다. 기본값으로 초기화된 static field 값들은 뒤의 Initialization 과정에서 코드에 작성한 초기값으로 변경이 된다. 이 때문에 JVM 에 탑재된 클래스 파일의 코드를 작동시키지는 않는다.

#### 2-3. Resolution

Symbolic Reference 값을 JVM 의 메모리 구성 요소인 Method Area 의 런타임 환경 풀을 통하여 Direct Reference 라는 메모리 주소 값으로 바꾸어준다. 해당 단계의 영향을 받는 JVM Instruction 요소는 new 및 instanceof 가 있다.


### 3. 초기화 - 기본 값으로 초기화하는 과정

Linking 과정을 거치면 Initialization 단계에서는 클래스 파일의 코드를 읽게 된다. Java 코드에서의 class 와 interface 의 값들을 지정한 값들로 초기화 및 초기화 메서드를 실행시켜준다. 이때, JVM 은 멀티 쓰레딩으로 작동을 하며, 같은 시간에 한 번에 초기화를 하는 경우가 있기 때문에 초기화 단계에서도 동시성을 고려해주어야 한다. Class Loader 를 통한 클래스 탑재 과정이 끝나면 본격적으로 JVM 에서 클래스 파일을 구동시킬 준비가 끝나게 된다.



### Reference
- https://d2.naver.com/helloworld/1230
- https://tecoble.techcourse.co.kr/post/2021-07-15-jvm-classloader/
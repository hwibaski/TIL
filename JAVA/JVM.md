# JVM

> The Java Virtual Machine is an abstract computing machine. Like a real computing machine, it has an instruction set and manipulates various memory areas at run time.

자바 가상머신은 추상 컴퓨팅 머신이다. 실제 컴퓨팅 머신처럼 명령어 세트가 있고 실행 시 다양한 메모리 영역을 조작한다.

C, C++이 특정 플랫폼, OS 그리고 하드웨어에 맞춰 컴파일 되는 것과 달리 자바 소스 코드는 자바 컴파일러에 의해(javac) 자바 소스 코드와 native 코드의 중간 형태인 bytecode(.class file)로 컴파일된다.

바이트코드는 opcode 행이 있는 16진수 형식이고, JVM은 bytecode들을 OS가 이해할 수 있는 머신 코드로 해석, 실행 가능하다.

바이트코드와 JVM은 OS 중립적이며, Java의 기본 컨셉인 WORD (wirte once, run anywhere)을 실현할 수 있는 중요한 요소이다. 하지만 JVM은 OS, 플랫폼 의존적이며 실행되는 환경에 맞는 버전이 설치되어야 한다. 



## 1. JVM의 구성 요소 및 특징

![img_3.png](img_3.png)

- 출처 : 더 자바, "코드를 조작하는 다양한 방법" (백기선님)

JVM은 JDK내에 포함되어 있으며, 더 정확히는 JDK에 포함되어 있는 JRE에 포함되어 있다. 오라클은 자바 11부터는 JDK만 제공하며 JRE를 따로 제공하지 않는다.

JVM은 크게 Class Loader Subsystem, Runtime Date Area, Execution Engine, JNI(Java Native Interface, Native Method Libraries) 으로 구성되어 있다.

### 1-1. JVM 구조

![img_2.png](img_2.png)
- 출처 : https://medium.com/platform-engineer/understanding-jvm-architecture-22c0ddf09722


#### 1-1-1. Class Loader Subsystem

JVM은 RAM에 위치하며, 실행 중에 클래스로더 서브시스템을 이용하여 클래스 파일을 RAM으로 가져옵니다. 이를 자바의 동적 클래스 로딩 기능이라고 합니다. 이 과정은 컴파일 타임이 아니라 런타임에 일어나며, 처음으로 클래스를 참조할 때 클래스 파일(.class)을 로드하고, 링크하고, 초기화 합니다.

- .class 에서 바이트코드를 읽고 메모리에 저장
- 로딩: 클래스 읽어오는 과정
- 링크: 레퍼런스를 연결하는 과정
- 초기화: static 값들 초기화 및 변수에 할당

#### 1-1-2. Runtime Data Area

Runtime data area는 JVM 프로그램이 실행될 때 OS로부터 할당받는 메모리 영역이다. Runtime Date Area는 5가지로 구분된다.

- PC Register (각 쓰레드 별 할당)
- JVM Stack (각 쓰레드 별 할당)
- Native Method Stack (각 쓰레드 별 할당)
- Heap (쓰레드 간 공유)
- Method Area (쓰레드 간 공유)


#### 1-1-3. Execution Engine

Class Loader에 의해 JVM으로 Load된 Class 파일(바이트코드)들은 Runtime Data Areas의 Method Area에 배치되는데, JVM은 Method Area의 바이트 코드를 Execution Engine에 제공하여, Class에 정의된 내용대로 바이트 코드를 실행시킨다. 이 때, Load 된 바이트코드를 실행하는 Runtime Module이 Execution Engine(실행 엔진) 이다.

- 인터프리터: 바이트 코드를 한줄 씩 실행.
- JIT 컴파일러: 인터프리터 효율을 높이기 위해, 인터프리터가 반복되는 코드를 발견하면 JIT 컴파일러로 반복되는 코드를 모두 네이티브 코드로 바꿔둔다. 그 다음부터 인터프리터는 네이티브 코드로 컴파일된 코드를 바로 사용한다.
- GC(Garbage Collector): 더이상 참조되지 않는 객체를 모아서 정리한다.

#### 1-1-4. JNI (Java Native Interface)

- 자바 애플리케이션에서 C, C++, 어셈블리로 작성된 함수를 사용할 수 있는 방법 제공
- Native 키워드를 사용한 메소드 호출

#### 1-1-5. Native Method Libraries

- C, C++로 작성 된 라이브러리




### Reference
- https://m.blog.naver.com/ksw6169/221647376178
- https://blog.hexabrain.net/397
- https://medium.com/platform-engineer/understanding-jvm-architecture-22c0ddf09722
- https://docs.oracle.com/javase/specs/jvms/se11/html/index.html
- https://d2.naver.com/helloworld/1230
- https://github.com/jongnan/java-study-with-whiteship/blob/master/week1/week1.md
- https://javapapers.com/core-java/java-jvm-run-time-data-areas/#Program_Counter_PC_Register
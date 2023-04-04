# GRADLE

## 개요
그래들은 유연한 오픈 소스 빌드 자동화 시스템이다.
그래들은 거의 모든 타입의 소프트웨어에 사용할 수 있다.

## 특징
- 높은 성능
  - 필요한 작업만 실행하여 불필요한 작업 방지
  - 다양한 캐시를 확용하여 이전 빌드의 결과물을 재사용 한다
  - 공유 빌드 캐시를 사용하면 다른 기기에서도 결과물을 재사용 가능하다
- JVM 기반
  - GRADLE은 JVM 위에서 실행된다. 표준 JAVA API를 사용할 수 있기 때문에 JAVA에 익숙한 사용자에게는 장점이 된다
  - 다른 플랫폼에서도 쉽게 그래들을 사용하게 만들 수 있다.
- Convention(미리 정의된 기능)
  - 컨벤션을 통해 일반적인 유형의 프로젝트를 쉽게 빌드할 수 있다
  - 사용자가 커스텀하게 task, setting 을 설정할 수 있다.
- 확장성
  - 그래들을 쉽게 확장하여 사용자 지정 작업 및 플러그인과 함께 자체 빌드 로직을 제공할 수 있다
- IDE 지원
  - 여러 주요한 IDE들은 Gradle과의 상호작용을 지원한다. (IntelliJ IDEA, Eclipse, VSCode and NetBeans)
  - Visual Studio에 프로젝트를 로드하는 데 필요한 솔루션 파일을 생성할 수 있다.


## 용어 정리

- Projects
  - Gradle이 build하는 산출물, 프로젝트는 build script를 포함한다.
  - 이 스크립트는 프로젝트의 루트 디렉토리에 위치하며 일반적으로 build.gradle 또는 build.gradle.kts이다
  - 빌드 스크립트는 해당 프로젝트에 대한 Task, Dependencies, Plugins 및 기타 설정을 정의한다
  - 단일 빌드에는 하나 또는 그 이상의 프로젝트가 포함될 수 있으며 각 프로젝트에는 자체의 하위 프로젝트가 포함될 수 있다

- Tasks
  - Task는 어떤 일을 실행하는 로직을 포함하고 있다.
  - 이 로직에는 코드 컴파일, 테스트 실행 또는 소프트웨어 배포가 포함될 수 있다.
  - 대부분의 경우에는 이미 만들어진 task들을 사용할 것이다.
  - 그래들은 대부분의 빌드 시스템에서 필요한 기능들을 task의 형태로 제공한다.
  - Task는 아래의 3가지 항목으로 구성되어 있다.
    - Actions: 파일 복사나 컴파일 소스와 같은 어떤 일을 하는 작업들
    - Inputs: 작업이 사용하거나 작동하는 값, 파일 및 디렉터리
    - Outputs: 작업이 수정하거나 생성하는 파일 및 디렉터리

- Plugins
  - 플러그인은 Gradle Task의 집합이다.
  - https://kotlinworld.com/323 참고

- Build Phases
  - Gradle은 빌드 라이프사이클의 세 가지 빌드 단계에서 빌드 스크립트를 평가하고 실행합니다
  1. Initialization : 빌드 환경을 설정하고 빌드할 프로젝트를 결정
  2. Configuration : 빌드에 대한 작업 그래프를 구성. 사용자가 실행할 태스크를 기준으로 실행할 태스크와 순서를 결정
  3. Execution : Configuration 단계에서 선택된 태스크들을 실행

## 설치
- Gradle 설치 전 필요사항
  1. 8버전 이상의 JDK
  2. Groovy library 가 Gradle에 포함되어 있으므로 따로 설치할 필요 없음



### Reference
https://docs.gradle.org/current/userguide/what_is_gradle.html
https://kotlinworld.com/323
# Thread

## Java Thread

### 1. One-to-One model
- 멀티스레딩의 초기 모델
- 하나의 user-level 스레드마다 커널 스레드 매핑
- 사용자 스레드를 만들기 위해서 1:1에 해당하는 만큼의 커널 스레드가 필요하기 때문에 시스템에 부담을 준다
- 대다수의 one-to-one 모델은 스레드 생성 시 최대 갯수에 제한을 둔다.
- 쓰레드 관리 OS에 위임 -> 쓰레드 스케줄링도 커널이 수행
- 멀티코어도 잘 활용을 한다.
- 한 쓰레드가 블락이 되어도 다른 쓰레드는 잘 동작한다.
- race condition 가능성이 있음

![image](https://user-images.githubusercontent.com/85930725/226680755-9a0e371d-9814-42d3-98ed-4447613cf188.png)

### 2. Many-to-One model
- Context Switching이 더 빠름. why? OS Thread 간의 context switching 발생하지 않는다. 이는 커널이 개입하지 않기 때문에 더 빠르다.
- OS Level에서 race condition 일어날 확률이 상대적으로 적다. why? OS Thread가 하나이므로
- OS Thread가 하나기 때문에 멀티 코어를 활용하지 못한다.
- 여러개의 user thread중 하나의 쓰레드가 block I/O를 실행하는 순간 연결된 OS Thread가 block되고 이는 모든 user Thread가 블락됨을 의미한다. -> 이를 해결하기 위해 nonblock I/O를 사용
- 초기 Java 스레드 모델

![image](https://user-images.githubusercontent.com/85930725/226680787-cc8e2eba-7b91-40a6-bb6c-59959a22366e.png)

### 3. Many-to-Many model
- One-to-One Model과 Many-to-One Model의 단점을 보완하기 위해 등장
- 구현이 복잡하다.
- 각 스레드의 비용과 weight를 줄인다
- 사용자 레벨 스레드에 대한 정교한 스케줄링 제공
- 커널은 현재 활성화된 스레드만 관리

![image](https://user-images.githubusercontent.com/85930725/226680832-6f2504b0-e1c0-4a17-9be3-767c182b6354.png)


### 용어 정리

- User Thread - OS와는 독립적으로 유저 레벨에서 스케줄링되는 쓰레드
- Green Thread - Java 초창기 버전은 Many-to-One 쓰레딩 모델을 사용, 이 때 이 유저 쓰레드들을 그린 스레드라고 호칭. 하지만 Green Thread 개념이 확장되어서 User Thread를 의미하기도함

## dead lock 방지
1. 리소스를 공유 가능하게함 (현실적으로 불가능)
2. hold and wait (사용할 리소스들을 모두 획득한 뒤에 시작, 리소스를 전혀 가지지 않은 상태에서만 리소스 요청)
3. 데드락 회피 (리소스 요청을 허락해줬을 때 데드락이 발생할 가능성이 있으면 리소스를 할당해도 안전할 때까지 계속 요청을 거절하는 알고리즘)
4. 데드락 감지 & 복구 
   1. 프로세스 종료
   2. 리소스의 일시적인 선점을 허용 (데드락이 발생했다면 다른 프로세스에게 내가 가진 리소스를 양보)
   3. 데드락 무시

# Context Switching

> 쉬운코드 님의 컨텍스트 스위칭 영상을 참고해서 정리

## 컨텍스트 스위칭이란?
- CPU(CORE) 에서 실행중이던 `프로세스/쓰레드`가 다른 `프로세스/쓰레드`로 교체되는 것
- 현재의 프로세스들은 최소 하나 이상의 쓰레드를 가지므로 프로세스 컨텐스트 스위칭이란 실행되고 있던 프로세스의 쓰레드가 **또 다른** 프로세스의 쓰레드로 교체되는 것을 의미

![image](https://user-images.githubusercontent.com/85930725/221908596-315b4de2-e1a1-4acb-a52e-bd3e500bc57f.png)

## 이 주제에서의 컨텍스트란?
- 프로세스/쓰레드의 상태
- CPU (register), 메모리(PCB) - 프로세스를 실행시키기 위해 필요한 정보들이 저장되고 실제로 실행시키는 곳

## 컨텍스트 스위칭은 언제 발생하는가?
- 주어진 Quantum을 다 사용했거나, 
- IO 작업을 해야하거나,
- 다른 리소스를 기다려야 하거나, etc

## 컨텍스트 스위칭은 누구에 의해 실행되는가(trigger가 아닌 컨텍스트 스위칭을 실행)?
- OS 커널(kernel)
- 커널이란? 
  > 커널은 운영체제의 핵심 기능을 모아놓은 것. 커널은 모든 컴퓨터 자원을 관리하기 때문에
  > 사용자나 응용 프로그램은 커널을 통해서만 컴퓨터 자원에 접근할 수 있다. 어떤 사용자나 응용 프로그램도 컴퓨터 자원에 접근할 수 없다.
  >  -쉽게 배우는 운영체제(한빛아카데미)-

## Process Context Switching & Thread Context Switching

### 공통점
1. 커널 모드에서 실행 (단순하게 말하자면 커널에게 통제권이 넘어간 상황)
2. CPU 레지스터 상태를 교체

### 차이점
1. 프로세스 컨텍스트 스위칭은 가상(virtual) 메모리 주소 관련 처리를 추가로 수행
   1.MMU, TLB를 비워줘야 한다. (어떤 역할을 하는지 정확히 모르겠지만 쓰레드 컨텍스츠 스위칭보다 리소스가 더 필요하다.)

### 결론
- 앞선 차이점에서와 같이 프로세스 컨텍스트 스위칭을 위해서는 쓰레드 컨텍스트 스위칭보다 추가적인 작업이 필요하기 때문에 더 느리다.

### 컨텍스트 스위칭이 미치는 간접적인 영향
- 캐시(cache) 오염
- CPU에서 가지고 있던 캐시는 컨텍스트 스위칭이 일어난 후 (특히 프로세스 간의 컨텍스트 스위칭)에는 교체된 프로세스/쓰레드에서 사용할 수 있는 데이터가 적을 확률이 크다.
  - 이는 캐시를 활용하지 못하고 메모리에서 필요한 데이터를 가지고 와야하므로 처리 속도가 늦어진다.

> 따라서 유저 관점에서 컨텍스트 스위칭은 순수한 오버헤드이다. 컨텍스트 스위칭이 자주 일어난다는 것은 좋은 현상이 아니다.


### Reference
- https://www.youtube.com/watch?v=Xh9Nt7y07FE&list=PLcXyemr8ZeoQOtSUjwaer0VMJSMfa-9G-&index=2

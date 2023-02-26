# JVM - GC

|                     | serial                                            | parallel                | parallel compacting             | cms                                                                                              | g1                                                                                           |
|---------------------|---------------------------------------------------|-------------------------|---------------------------------|--------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| young generation 특징 | 단일 스레드                                            | 멀티 스레드                  | 멀티 스레드                          | 멀티 스레드                                                                                           |                                                                                              |
| old generation 특징   | Mark-sweep-compact                                | Mark-sweep-compact      | 새로운 알고리즘                        | 새로운 알고리즘                                                                                         |                                                                                              |
| 특징                  | - STW 시간이 상대적으로 길다. <br/>-싱글 스레드 환경 및 Heap이 매우 작을 때 사용 | - Java 8의 default GC 방식 | - parallel과 Old 영역의 처리 알고리즘이 다름 | - STW 최소화를 위해 고안<br/>- GC 작업을 어플리케이션과 동시에 실행<br/>- Parallel GC와의 가장 큰 차이점은 Compaction 작업 유무로 구분됨 | - Java 9 부터 default GC 방식 <br/>- 큰 메모레이서 사용하기 적합한 GC (대규모 Heap 사이즈에서 짧은 GC 시간을 보장하는데 목적이 있음) |

## Mark-sweep-compact 알고리즘이란?

1. 살아 있는 객체 식별 (표시 단계)
2. 영역의 객체들을 훑는 작업을 수행하며 쓰레기 객체를 식별(스윕 단계)
3. 필요 없는 객체들을 지우고 살아 있는 객체들을 한 곳으로 모은다 (컴팩션)

## parallel compacting GC의 Old 영역 GC Algorithm

1. 살아 있는 객체를 식별하여 표시해 놓는 단계 (표시 단계)
2. 이전에 GC를 수행하여 컴팩션된 영역에 살아 있는 객체의 위치를 조사하는 단계 (종합 단계)
3. 컴팩션을 수행하는 단계, 수행 이후에는 컴팩션된 영역과 비어 있는 영역으로 나뉜다 (컴팩션 단계)

## CMS GC의 Old 영역 GC Algorithm

1. 매우 짧은 대기 시간으로 살아 있는 객체를 찾는 단계 (초기 표시 단계)
2. 서버 수행과 동시에 살아 있는 객체에 표시를 해 놓는 단계 (컨커런트 표시 단계)
3. 컨커런트 표시 단계에서 표시하는 동안 변경된 객체에 대해서 다시 표시하는 단계 (재표시 단계)
4. 표시되어 있는 쓰레기를 정리하는 단계 (컨커런트 스윕 단계)

## G1 GC
- 추가적인 공부 필요

### Reference
- https://d2.naver.com/helloworld/1329
- https://d2.naver.com/helloworld/37111
- https://d2.naver.com/helloworld/329631
- https://d2.naver.com/helloworld/6043
- 자바 성능 튜닝 이야기
- https://www.youtube.com/watch?v=FMUpVA0Vvjw
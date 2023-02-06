# ISP (Interface Segregation Principle) - 인터페이스 분리 원칙
<hr>

인터페이스 분리 원칙 - 클라이언트가 자신이 이용하지 않는 메서드에 의존하지 않아야 한다는 원칙.

## ISP 적용 전

![img.png](img.png)
- 이미지 출처 : https://yoongrammer.tistory.com/99

Car 클래스는 vehicle의 fly()를 사용하지 않는다. 

## ISP 적용 후

![img_1.png](img_1.png)
- 이미지 출처 : https://yoongrammer.tistory.com/99

ISP는 인터페이스가 응집력 측면에서 작게 분할하여 클래스가 필요한 작업만 실행하도록 하는 것을 목표로 한다.



ISP는 객체가 반드시 필요한 기능만을 가지도록 제한하는 원칙이다. 불필요한 기능의 상속을 방지, 객체의 불필요한 책임을 제거한다.

### Reference
- https://blog.itcode.dev/posts/2021/08/16/interface-segregation-principle
- https://sjh836.tistory.com/159
- https://yoongrammer.tistory.com/99
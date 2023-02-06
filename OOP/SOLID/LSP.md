# LSP (Liskov Substitution Principle) - 리스코프 치환 원칙

<hr>

리스코프 치환 원칙 - 부모 객체와 이를 상속한 자식 객체가 있을 때 부모 객체를 호출하는 동작에서 자식 객체가 부모 객체를 완전히 대체할수 있다. 
서브 타입은 언제나 기반 타입으로 교체할 수 있어야한다.

- 서브타입은 언제나 자신의 기반 타입으로 교체할 수 있어야 합니다.

- 하위 클래스의 인스턴스는 상위 클래스의 인스턴스 역할을 수행하는데 문제가 없어야 합니다.
- 하위클래스 is a kind of 상위 클래스 - 하위 분류는 상위 분류의 한 종류
  구현 클래스 is a able of 인터페이스 - 구현 분류는 인터페이스 할 수 있어야 한다.

## LSP가 위배된 경우의 클래스 계층도
![image](https://user-images.githubusercontent.com/85930725/217015102-f4fb6b9b-b5e9-4c85-b9d0-5a87d70f54ad.png)

- 이미지 출처 : https://geunzrial.tistory.com/173


## LSP가 잘 지켜진 경우의 클래스 계층도
![image](https://user-images.githubusercontent.com/85930725/217015136-6ba9a4ae-82e5-4982-908a-5a13421409ef.png)

- 이미지 출처 : https://geunzrial.tistory.com/173

### Reference

- https://blog.itcode.dev/posts/2021/08/15/liskov-subsitution-principle
- https://geunzrial.tistory.com/173

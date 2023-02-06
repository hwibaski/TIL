# SRP (Single Responsibility Principle) - 단일 책임 원칙

단일 책임 원칙 - 클래스는 하나의 책임을 가져야한다.

## 책임이란?

책임이란 객체에 의해 정의되는 응집도 있는 행위의 집합으로, 객체가 유지해야 하는 정보와 수행할 수 있는 행동에 대해 개략적으로 서술한 문장이다.
즉, 객체의 책임은 객체가 '무엇을 알고 있는가'와 '무엇을 할 수 있는가'로 구성된다.

## 클래스 내의 해당 사항이 있다면 SRP가 지켜지고 있지 않다는 것을 의심
- 변수레벨
  - 하나의 속성이 여러 의미를 갖는 경우
  - 어떤 곳에서는 쓰고, 어떤 곳에선 안쓰는 속성이 있는 경우
- 메소드레벨
  - 분기처리를 위한 if 문이 많을 경우

### Reference
- https://sjh836.tistory.com/159
- https://github.com/cheese10yun/TIL/blob/master/OOP/solid-%EA%B8%B0%EC%B4%88/SRP.md
- https://blog.itcode.dev/posts/2021/08/13/single-responsibility-principle
- https://incheol-jung.gitbook.io/docs/study/object/2020-03-10-object-chap3
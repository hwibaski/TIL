# DIP (Dependency Inversion Principle) - 의존성 역전 원칙

의존성 역전 원칙 - 객체는 저수준 모듈보다 고수준 모듈에 의존해야한다.

- 고수준 모듈 : 인터페이스와 같은 객체의 형태나 추상적 개념
- 저수준 모듈 : 구현된 객체

구체적인 것이 추상화된 것에 의존해야 한다. '자주 변경되는 클래스에 의존하지 말자.' 로 요약될 수 있다. 
즉, 자신보다 변하기 쉬운 것에 의존하지 말라는 것이다.

> 객체는 객체보다 인터페이스에 의존해야한다.


## DIP를 준수하지 않은 코드
```java
class Character {
    private final String name;
    private int hp;
    private OneHandSword weapon;
    
    public Character(String name, int health, OneHandSword weapon) {
        this.name = name;
        this.hp = hp;
        this.weapon = weapon;
    }
    
    public int attack() {
        return weapon.attack();
    }
}

class OneHandSword {
    private final int damage;
    
    public OneHandSord(int damage) {
        this.damage = damage;
    }
    
    public int attack() {
        return this.damage;
    }
}
```

## DIP를 준수한 코드
```java
class Character {
    private final String name;
    private int hp;
    private Attackable weapon;
    
    public Character(String name, int health, Attackable weapon) {
        this.name = name;
        this.hp = hp;
        this.weapon = weapon;
    }
}

interface Attackable {
    int attack()
}

class OneHandSword implements Attackable {
    private final int damage;
    
    public OneHandSord(int damage) {
        this.damage = damage;
    }
    
    @Override
    public int attack() {
        return this.damage;
    }
}
```



### Reference
- https://blog.itcode.dev/posts/2021/08/17/dependency-inversion-principle
- https://sjh836.tistory.com/159
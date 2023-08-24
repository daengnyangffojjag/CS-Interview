### Casting

---

변수나 객체의 타입을 **다른 타입으로 변환**하는 과정 == **형변환**

### 참조 타입 캐스팅

---

**상속 관계**에 있는 클래스나 인터페이스 간의 타입변환

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aee7236d-2d5b-4e72-81b4-363ff7fd0027/Untitled.png)

### 1. 업 캐스팅(Upcasting)

하위(자식) 클래스의 인스턴스를 상위(부모) 클래스의 타입으로 변환

- 자동으로 수행
- 명시적인 캐스팅 연산자 필요 ❌
- 업 캐스팅 된 객체는 상위 클래스에 정의된 멤버들에만 접근 가능
    - 자식 클래스에만 있는 멤버는 접근 불가
- 업 캐스팅을 하고 메소드를 실행할 떄, 오버라이딩 된 메소드는 자식 클래스의 메소드로 실행 된다

### 2. 다운 캐스팅(DownCasting)

상위(부모) 클래스의 인스턴스를 하위(자식) 클래스의 타입으로 변환

- 연산자 괄호를 생략 불가
- 형 변환 연산자 - (자식클래스) 를 사용해야 함
- 업캐스팅한 객체를 다시 자식 클래스의 타입의 객체로 되돌리는데 목적
- 자식 클래스에만 존재하는 메소드나 속성에 접근하기 위함

```java
class Animal {
    public void sound() {
        System.out.println("Animal makes a sound");
    }
}

class Cat extends Animal {
    @Override
    public void sound() {
        System.out.println("Cat meows");
    }

    public void purr() {
        System.out.println("Cat purrs");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Dog barks");
    }

    public void wagTail() {
        System.out.println("Dog wags its tail");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal1 = new Cat();
        Animal animal2 = new Dog();

        animal1.sound(); // 다형성에 의해 Cat의 sound() 메서드 호출
        animal2.sound(); // 다형성에 의해 Dog의 sound() 메서드 호출

        // animal1.purr(); // 컴파일 에러: Animal 타입 변수로는 purr() 메서드에 접근할 수 없음
        // animal2.wagTail(); // 컴파일 에러: Animal 타입 변수로는 wagTail() 메서드에 접근할 수 없음

        Cat cat = (Cat) animal1;
        cat.purr(); // 다운 캐스팅을 통해 Cat의 purr() 메서드에 접근

        Dog dog = (Dog) animal2;
        dog.wagTail(); // 다운 캐스팅을 통해 Dog의 wagTail() 메서드에 접근
    }
}
```

### 예상질문

1. 업캐스팅과 다운캐스팅에 대해서 설명해주세요

업 캐스팅은 자식클래스의 객체를 부모 클래스 타입으로 다운 캐스팅은 부모 클래스의 객체를 다시 자식 클래스 타입으로 형 변환하는 것입니다. 업 캐스팅을 통해 부모 클래스에 정의된 메소드 중 자식 클래스에서 오버라이딩 된 메소드를 호출해 다형성을 구현할 수 있습니다. 그러나 업캐스팅된 객체는 자식 클래스에 존재하는 메소드나 속성에 접근할 수 없기 때문에 다운캐스팅을 통해 다시 자식 클래스에 접근할 수 있습니다.

[[JAVA] 자바 업캐스팅, 다운캐스팅](https://yoon-ve.tistory.com/entry/JAVA-업캐스팅-다운캐스팅)

[☕ JAVA 업캐스팅 & 다운캐스팅 - 완벽 이해하기](https://inpa.tistory.com/entry/JAVA-☕-업캐스팅-다운캐스팅-한방-이해하기)

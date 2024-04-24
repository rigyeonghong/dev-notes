# 11. 다형성2
* 타입이 다르기 때문에 함수 중복 제거 시도가 안돼..
	➡️ Dog, cat, caw 가 모두 같은 타입 사용하고, 자신의 메서드 호출한다면?
[[DAY 5. 상속, 다형성]]
```java
Dog dog = new Dog();
Cat cat = new Cat();

soundAnimal(dog);
soundAnimal(cat);

private static void soundAnimal(Animal animal) {
	animal.sound();
}
```
![[Pasted image 20240422153309.png]]
* **다형적 참조 : 부모는 자식을 담을 수 있음.**
* **메서드 오버라이딩 : 각 인스턴스의 메서드 호출 가능.**


* 배열과 for문으로 중복 제거
```java
Dog dog = new Dog();
Cat cat = new Cat();
Animal[] animalArr = {dog, cat};

for (Animal animal : animalArr) {
	System.out.println("동물 소리 테스트 시작");
	animal.sound();
	System.out.println("동물 소리 테스트 끝");
}
```

```java
Animal[] animalArr = {new Dog(), new Cat()};

for (Animal animal : animalArr) {
	soundAnimal(animal);
}

// 동물 추가되어도 변하지 않는 부분 
private static void soundAnimal(Animal animal){
	System.out.println("동물 소리 테스트 시작");
	animal.sound();
	System.out.println("동물 소리 테스트 끝");
}
```

* 변하는 부분 최소화하는 것이 좋은 코드. ➡️ 변하는 부분, 변하지 않는 부분 잘 분리하는게 좋음👍🏻
	ex. 하나의 변경사항이 발생되었을 때 어디까지 코드를 변경해야하나. 샷건 🔫

* Animal 클래스 생성할 수 있는 문제.
* Animal 클래스를 상속 받는 곳에서 sound() 메서드 오버라이딩 안할 가능성의 문제.

#### 추상 클래스1
* 추상 클래스 : 부모 클래스는 제공하나, 실제 생성되면 안되는 클래스. 실체인 인스턴스가 존재하지 않고 상속 목적으로 사용. ex. 동물
```java
abstract class AbstracAnimal {...}
```

* 추상 메서드 : 부모 클래스를 상속받는 자식 클래스가 **반드시 오버라이딩** 받아야 하는 메서드. 실체 존재하지 않고, 메서드 바디 없음.
	* 추상 메서드가 하나라도 있으면 무조건 추상 클래스.
	* 오버라이딩 하지 않으면 자식도 추상 클래스 되어야함.
```java
public abstract void sound();
```

#### 추상 클래스2
* 순수 추상 클래스 : 모든 메서드가 추상 메서드인 추상 클래스
![[Pasted image 20240422155303.png]]
* 단지 다형성 위한 부모 타입으로써 껍데기 역할 제공.
* 어떤 규격을 지켜 구현 위한 것. 마치 인터페이스. ex. usb 인터페이스.
* ~~순수 추상 클래스~~ = 인터페이스

#### 인터페이스
```java
public interface InterfaceAnimal {
	int MY_PI = 3.14;
	
	void sound();
	void move();
}
```
* 인터페이스의 모든 메서드는 public, abstract 이고 생략 가능.
* 다중 구현(다중 상속) 지원.
* 인터페이스에 멤버 변수 포함 가능. public, static, final 이 모두 포함되었다고 간주.

* 인터페이스 사용해야 하는 이유
	* 반드시 구현하라는 제약 전달
	* 다중 구현 가능

#### 인터페이스 - 다중 구현
* 다중 상속 안되는데, 다중 구현 허용한 이유는 인터페이스는 추상 메서드로 이뤄져있기 때문. (자식에서 메서드 구현하기 때문) ↔️ 다이아몬드 문제 (어떤 한 부모를 선택해야하는)
![[Pasted image 20240422160956.png]]


# 12.  다형성과 설계

#### 객체 지향 프로그래밍
* 유연하고 변경 용이 - 다형성
	ex. 운전자 역할 - 자동차 역할
			구현:	k3, 아반떼, 테슬라

#### 역할과 구현 분리 ➡️ 자바 언어의 다형성 활용해 가능
* 장점
	* 단순
	* 클라이언트는 역할(인터페이스)만 알면 되고, 내부 구조 몰라도 됨.
	* 클라이언트는 내부 구조 변경해도 영향 받지 않기에 실행 시점에 유연하게 인스턴스 변경 가능. 

#### 역할과 구현 분리의 한계 존재
* 인터페이스(역할) 자체 변경할 수도 있음
* 인터페이스 안정적으로 잘 설계하는게 중요

#### 정리
* 디자인 패턴 대부분 다형성 활용
* 제어의 역전, DI 결국 다형성 활용하는 것
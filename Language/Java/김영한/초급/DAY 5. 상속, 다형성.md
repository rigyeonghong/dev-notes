# 9.  상속

* 자식 -> 부모
	* 화살표 방향은 내가 저 클래스를 안다. ex. 부모 클래스는 자식 클래스의 존재를 모름
* 단일 상속
	* 하나의 자식이 2개의 부모를 가질 수 없음. (이름 같은 부모 클래스일 때 복잡..)
	* 인터페이스는 다중 구현 가능.

#### 상속과 메모리 구조
![[Pasted image 20240421142146.png]]
* 참조값은 하나지만 인스턴스 하나 안에 모두 공존.
	* 내부에서는 부모, 자식 모두 생성되고 공간도 구분됨.
* 함수를 호출하는 변수의 타입(클래스)을 기준으로 부모/자식 클래스의 함수 선택.
	* 자식 타입에 해당 기능(함수) 없으면 부모 타입으로 올라가서 찾음. 찾지 못하면 컴파일 오류.

#### 상속과 기능 추가
* `extends` : 상속 관계 덕에 중복은 줄고, 편리하게 확장 가능.

#### 상속과 메서드 오버라이딩
* 메서드 오버라이딩 : 부모 타입의 기능을 자식이 **재정의** 하는 것. `@Override` : 없어도 동작에 문제 없으나, 실수 줄여줌.
* @ : 프로그램이 읽을 수 있는 특별한 주석.
![[Pasted image 20240422132152.png]]

* 메서드 오버라이딩 조건
	* 메서드 이름, 매개변수, 반환 타입 같아야함
	* 접근 제어자 : 상위 클래스 보다 제한적이면 안됨.
	* 상위 클래스 메서드에 static, final, private 이면 안됨.
	* 생성자 불가. (생성자 오버라이드가 가능하다면 상속받은 부모클래스의 초기화 로직을 호출하지 않을 수 있음.)

#### 상속과 접근 제어
* `proteted` : 패키지 달라도 상속관계 호출 허용
![[Pasted image 20240422134418.png]]

#### super - 부모 참조
* 부모, 자식의 필드명 같거나 메서드 오버라이딩 시, 자식에서 부모의 필드, 메서드 호출 불가하므로 `super` 키워드 사용.
*  `this` : 자기 자신 참조. 없으면 부모 참조.
* `super` : 부모 클래스 참조. 
![[Pasted image 20240422134936.png]]

#### super - 생성자
* 상속 관계 사용시, 자식 클래스 생성자에서 부모 클래스의 생성자 반드시 호출해야함.
* 부모가 기본 생성자 없으면 super로 직접 호출해줘야함.
```java
public class ClassC extends ClassB {
	public ClassC() {
		super(10, 20); // 부모 클래스의 생성자 호출.
		...
	}
}
```
![[Pasted image 20240422135722.png]]
* 초기화는 최상위 부모부터 실행.
	* ClassC의 생성자 먼저 호출되나, 내부에서 super 통해 ClassB의 생성자 호출하기에.

#### final
* 클래스에 final : 상속 불가.
* 메서드에 final : 오버라이드 불가.


# 10.  다형성
#### 다형성 시작
* 다형성 : 한 객체가 여러 타입의 객체로 취급될 수 있는 능력. 조상 클래스 타입의 참조변수로 자손클래스의 인스턴스를 참조할 수 있도록 한 것.

* 다형적 참조
	* 부모 타입은 모든 자식 타입 참조 가능하나, 반대는 안됨.
	```java
	Parent poly = new Child();
	poly.parentMethod();

	// 자식 기능 호출 불가. 컴파일 오류. 상속 관계는 자식 방향으로 찾아내려갈 수 없기 때문.
	// poly.childMethod(); 
	```

* childMethod() 호출하고 싶으면 캐스팅 필요.

#### 다형성과 캐스팅
* 다운 캐스팅 : 부모 타입 -> 자식 타입으로 변경.
	```java
	Child child = (Child) poly;
	child.childMethod();
```
![[Pasted image 20240422144500.png]]
```java
Child child = (Child) poly
Child child = (Child) x001 // Parent poly의 타입이 변하지 않음. 참조값을 꺼내고 꺼낸 참조값이 Child 타입되는 것
Child child = x001
```

* 업캐스팅 : 부모 타입으로 변경
* 다운캐스팅 : 자식 타입으로 변경

#### 캐스팅의 종류
* 일시적 다운 캐스팅 : 해당 메서드 **호출 순간만** 다운 캐스팅
```java
((Child) poly).childMethod();
```
![[Pasted image 20240422144947.png]]

* 업캐스팅 : 현재 타입을 부모 타입으로 변경하는 것
	* 캐스팅 코드인 (타입) 생략 가능.
```java
Parent poly = new child();
```

* 업캐스팅이 안전하고 다운캐스팅이 위험한 이유는 인스턴스에 존재하지 않은 하위 타입으로 캐스팅하는 문제 발생할 수 있다. 런타임 오류.

* 컴파일 오류 : 자바 프로그램 실행 전 발생. good.
* 런타임 오류 : 고객이 해당 프로그램 실행 도중 발생 ㄷㄷ

#### instanceof
* 실제로 참조하고 있는 타입 확인하고 싶을 때 사용. 다운캐스팅 가능 여부 확인 가능.
* 오른쪽 타입에 왼쪽 인스턴스 타입에 들어갈 수 있으면 True.
```java
Parent parent2 = new Child();
call(parent2);

private static void call(Parent parent){
	if (parent instanceof Child) { // 원하는 타입인지 확인.
		Child child = (Child) parent;
		child.childMethod();
	}
}
```
* 자바16- instanceof
```java
if (parent instanceof Child child) {
		child.childMethod();
}
```

### 다형성과 메서드 오버라이딩
* **오버라이딩 된 메서드가 항상 우선권** 가진다!!
```java
Parent poly = new Child();
System.out.println(poly.value); // parent value
poly.method(); // Child.method 출력
```
* 변수는 오버라이딩 x / **메서드는 오버라이딩**
![[Pasted image 20240422151744.png]]


Q 다형적 참조가 왜 필요하지? 

#### 추가 공부
* 상속은 주의해서 써야한다.
	* A is B 이지만 리스코프 치환 원칙을 위반하는 경우가 있다. ex. 정사각형-직사각형 문제 : 수학적으로는 상속 가능하나 제거해줘야해.

* 인터페이스 : 구현으로 같은 동작하게 하는 것
* 상속 : 메서드 확장, 재사용하는 것
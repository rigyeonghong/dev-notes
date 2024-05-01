* java.lang 패키지 : 자바 기본 제공 라이브러리
	* object
	* string
	* Integer, Long, Double : 래퍼 타입, 기본형 데이터 타입을 객체로.
	* Class : 클래스 메타 정보
	* system
# 1. Object 클래스
*  부모가 없으면 묵시적으로 Object 클래스 상속.
	*  `toString()` : object 메소드

* 최상위 부모 클래스인 이유 
	* 공통 기능 제공  : 모든 객체 받을 수 있는 메서드.
	* 다형성 기본 구현 : 모든 객체 보관 가능.

#### Object 다형성
```java
Car car new Car();
Dog dog new Dog();

action(car);
action(dog);

private static void action(Object obj){
	// obj.sound(); // 컴파일 오류, Object에는 sound()가 없음.

	if (obj instanceof Car car){ // 다운 캐스팅
		car.sound();
	}
}
```
![[Pasted image 20240428134730.png]]
![[Pasted image 20240428134805.png]]


<span style="background-color:#DCFFE4; color: black;">
다형성 제대로 활용하려면 : 다형적 참조 + 메서드 오버라이드
</span>

```java
Car car new Car();
Dog dog new Dog();
Object object new Object();

Object[] objects = {dog, car, object};
```
![[Pasted image 20240428135124.png]]

#### toString()
* 패키지를 포함한 객체의 이름과 객체의 참조값(해시코드) 제공.

* toHexString 이 왜 string 인가?


#### Object 와 OCP
* Object와 Object 제공 toString() 부재시 클래스별 별도 메서드 작성해야함.
![[Pasted image 20240428144122.png]]

* OCP원칙 충족.
	* open : cat 같은 새 클래스 추가, toString() 오버라이딩으로 확장.
	* close : ObjectPrinter 변경하지 않아도됨.

* System.out.println(Object obj)
	* 세상의 모든 객체 정보 편리하게 출력.

* 정적 의존관계 vs 동적 의존관계
	* 정적 의존관계: 컴파일 시간에 결정. ex. 클래스 간 관계
	* 동적 의존관계 : 런타임에 확인. ex. ObjectPrinter.print(Objcet ob) 인자로 어떤 객체 전달될지. 런타임에 어떤 인스턴스를 사용하는지를 말함.

#### equals() 

1. 두 객체가 같다
	* 동일성 : `==` 두객체의 참조가 동일 객체인지. 물리적으로 같은 메모리에 있는 객체 인스턴스인지 참조값 확인.
	* 동등성 : `equals` 메서드 사용해 두 객체가 논리적으로 동등한지 확인. 값이 동일한지.
```java
User a = new User("id-1") // 참조 x001
User b = new User("id-1") // 참조 x002
```
* 동일성은 다름, 동등성은 같음.

* Object 기본제공 equals 는 내부 코드적으로 `==`으로 동일성 비교하므로 동등성 비교를 원할시 재정의 해야함.

💡  **String Constant Pool**
```java
String word = "hello";
```
* 리터럴로 string 객체 생성시 String Constant Pool 이라는 공간에 값 생성.
> JVM이 String Constant Pool 에 객체 만든 후, 해당 객체의 레퍼런스를 스택에 저장함.
> 클래스 로드할 때, 모든 클래스의 String 리터럴들은 Pool로 감.
![[Pasted image 20240428151326.png]]
* String 객체는 불변객체임으로 `Thread-safe`.
* String에 값 변경 시도하면, 기존 객체 유지되고 새로운 객체가 생성됨.

![[Pasted image 20240428153712.png]]

# 2. 불변 객체
* 자바 제공 기본 클래스들이 불변 객체 많음.
	* ex. String, Integer, LocalDate 등

#### 기본형(Primitive)과 참조형(Reference)의 공유
* **기본형** : 하나의 값을 여러 변수에서 절대 공유 X
* **참조형**: 하나의 객체를 참조값 통해 여러 변수에서 공유

* 기본형 예시
```java
int a = 10
int b = a
```
![[Pasted image 20240501161939.png]]

* 참조형 예시
```java
Address a = new Adress("서울");
Address b = a;
```
![[Pasted image 20240501162241.png]]

#### 공유 참조와 사이드 이펙트
(주된 작업 외 부수 효과 일으키는 것)

* 여러 변수가 하나의 객체를 공유하는 것을 막을 방법은 없다
	= 참조값의 공유를 막을 수 있는 방법이 없다.
	➡️ 어캐 해결?

#### 불변 객체 - 도입
* 문제 원인 ) 공유된 객체의 값을 변경한 것
* 해결 ) 객체의 상태(객체 내부 값, 필드, 멤버 변수)가 변하지 않는 객체 사용. 
* 장점 ) 사이드 이펙트라는 큰 문제 막을 수 있음.
```java
public class ImmutableAddress {
	private final String value; // final로 변경불가.

	public ImmutableAddress(String value){ // 생성자만 남겨둠
		this.value = value;
	}

	public String getValue(){ // get만 남겨둠
		return value;
	}
}
```

#### 불변객체 - 값 변경
```java
public class ImmutableObj {

	private final int value; // 계산 전 기존 값 존재

	public ImmutableObj(int value) {
		this.value = value;
	}

	public ImmutableObj add(int addValue) {
		return new ImmutableObj(value + addValue); // 새로운 불변 객체 반환
	}

	public int getvalue() {
		return value;
	}
}

public class Main {

	public static void main(String[] args) {
		ImmutableObj obj1 = new ImmutableObj(10);
		ImmutableObj obj2 = obj1.add(20); // 반환값 받아 사용
	}

}
```

#### 참고-관례
* 불변객체에서 값 변경시 `withYear()` 처럼 with 로 시작하는 경우 많음.

* 모든 클래스를 불변으로 만드는 것은 아니다..

* 클래스를 불변으로 설계하는 이유
	* 캐시 안정성 
		* ex. 불변 객체로 프로필 정보 설계시
	* 멀티 쓰레드 안정성
	* 엔티티의 값 타입
* 자바의 정석 유튜브
https://www.youtube.com/playlist?list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp

# 1. 클래스와 데이터

### 클래스가 필요한 이유
ex. 학생 늘어날 때마다 변수 추가 선언해야하기 때문.
➡️ 배열로도 가능하나 수정시 조심성 필요.. ➡️ 클래스

### 클래스 도입
```java
public class Student { // 카멜케이스
	String name; // 멤버 변수(특정 클래스 소속), 필드(데이트 항목 가르키는 용어)
	int age;
	int grade;
}
```

```java
Student student1; 
student1 = new Student(); //학생을 실제 메모리에 만듦 ex. x001
student1.name = "학생1";
student1.age = 15;
student1.grade = 90;

Student student2 = new Student(); // x002
student2.name = "학생2";
student2.age = 16;
student12.grade = 80;
```

![[Pasted image 20240407190831.png]]

### 객체 사용
![[Pasted image 20240407191300.png]]

### 클래스, 객체, 인스턴스 정리
* 클래스 
	* 객체 생성 위한 '틀' 또는 '설계도'
	* 객체가 가져야할 속성(변수)과 기능(메서드) 정의

* 객체
	* 클래스에서 정의한 속성과 기능 가진 실체

* 인스턴스
	* 특정 클래스로부터 생성된 객체. 
	* 객체가 어떤 클래스에 속한지 강조시 사용 (관계 맞춤 용어)

###  배열 리팩토링
*  배열 선언 최적화 : 생성과 선언 동시
	```java
	class1[] students = {student1, student2};
	```
* for문
	```java
	for (class1 s : students) {
		System.out.println("이름 : " + s.name + "나이 : " + s.age)
	}
```

### 💡자바의 정석 5-14. String 클래스
* String 클래스 = char[] + 메서드
* 내용 변경 불가 : read only
	```java
	String a = "a";
	String b = "b";
	a = a+ b; // 새로운 주소 할당 

	System.out.println(a); // ab 출력
	```
* equls() : 문자열 내용 같은지 확인
* char charAt(int index) : 문자열에서 해당 위치 있는 문자 반환

#### 9. Object 클래스와 equals()
* Object 클래스: 모든 클래스의 조상
* equlas(Object obj)
	* 자기 자신(this)와 주어진 객체(obj) 비교해서 같으면 true, 다르면 false 반환.
	* 객체의 주소(참조변수 값) 비교 ➡️ 문자열끼리 비교시에는 값 비교
* == 연산자 : 주소 비교


# 2. 기본형과 참조형

* 기본형
	* 값을 변수에 직접 넣음
	* int, long, double, boolean 
	* 연산 가능

* 참조형
	* 객체가 저장된 메모리 위치 가리킴
	* 기본형 제외 나머지. ex. Student student1, int[] students
	* 연산 불가하지만 . 통해 멤버 변수 접근시 연산 가능

* String
	* 클래스이고 참조형이나 문자 값 바로 대입 가능.

###  변수 대입
* 자바는 항상 변수 값 복사해서 대입
	```java
int a =10;
int b = 1;
a = 20;
b = a;
```

* 참조형 변수는 참조값 복사해서 대입
```java
Student student1 = new Student();
Student student2 = student1;
```

###  메서드 호출
* 자바에서 메서드의 매개변수(파라미터) 항상 값에 의해 전달됨.
	* 그 값이 실제값이냐 / 참조값이냐에 따라 동작 다름
		* 기본형 : 해당 값 복사되어 전달. 메서드 내부 값 변경시, 호출자의 변수 값은 영향 없음
		* 참조형 : 참조값이 복사되어 전달. 메서드 내부에서 매개변수로 전달된 객체의 멤버 변수 변경시, 호출자의 객체도 변경

###  변수와 초기화

* 변수 종류
	* 멤버 변수 : 클래스에 선언
		```java
	public class Example {
		int a; // 멤버 변수
	}
		```
	* 지역 변수 : 메서드에 선언, 매개변수도. 메서드 끝나면 제거.
		```java
	public static void main(String[] args) {
		int a = 10; // 지역 변수 
		Example examples = new Example(); // 지역 변수
	}

	public static void print(int b) { // 지역 변수 b
		b = 20;
	}
		```

###  변수 값 초기화
* 멤버 변수 : 자동 초기화
	* 인스턴스 멤버 변수는 생성시 자동 초기화.
		* int = 0, boolean = false, 참조형 = null
	* 개발자 직접 초기화 가능. ex. 생성자, setter, 직접 할당

* 지역 변수 : 수동 초기화
	* 항상 직접 초기화.

###  null
* 값이 존재하지 않거나 없다는 뜻
* 아무도 참조하지 않으면 가비지 컬렉션이 더이상 사용 않는 인스턴스로 판단하고 자동 제거

###  NullPointerException
* 참조값 없는 객체에 .(dot) 사용해 접근시 발생하는 예외.


### 💡스터디 추가 논의 내용
* 가변 객체 : 객체 생성된 후에도 내부 상태 변경 가능.
* 불변 객체 : 객체 한 번 생성되면 내부 상태 변경 불가.

![[Pasted image 20240408223223.png]]
![[Pasted image 20240408223244.png]]

➡️ 클래스는 메소드 영역에, 변수 주소값은 스택에, 인스턴스는 힙에.

* Array / List / ArrayList
	* Array
		* 정해진 공간 : 객체 생성시 크기 할당해줘야함
		* 조회 빠른 대신 삽입/삭제 느림
		* 명시되지 않은 데이터 유형일 때 런타임 시점에 exception 발생
	* List
		* 식별자 없고, 크기 할당 필요 없음
		* 조회 느린 대신 삽입 삭제 빠름
	* ArrayList
		* 동적이고, 인덱스 존재
		* capacity 변수 존재 : 넘어 들어오면 자동 증가.
		* 컴파일 시점에 잘못된 데이터 유형 체크

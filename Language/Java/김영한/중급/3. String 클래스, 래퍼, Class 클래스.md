# 3. String 클래스
```java
char[] arr = new char[]{'h','i'};

String str = "hi"; // 자주 사용되서 편의상 아래와 같이 변경해줌. + 연산도.
String str2 = new String("hi");
```

* String은 클래스. 참조형.

#### String 클래스 구조
```java
public final class String{

	private final char[] value; // 자바 9이전
	private final byte[] value; // 자바 9이후 : char 2byte, 영어나 숫자 1byte 효율적으로.

	public String concat(String str){...}
	...
}
```
*  생성 이후 내부 문자 변경 불가 : 불변 객체
* 이유 : 문자열 풀의 String 인스터스 값 변경시 같은 문자열 참고하는 다른 변수 값도 변경되기에.

#### 문자열 풀
* 문자열 리터럴 사용시, 자바는 메모리 효율과 성능 최적화 위해 문자열 풀 사용.
![[Pasted image 20240428151326.png]]
* 문자열 풀에서 문자 찾을 때 해시 알고리즘 사용해서 빠른 속도.
* 문자열 비교시 항상 `==` 아닌 `equals` 사용해 동등성 비교해야함.

#### StringBuilder - 가변 String
* 불변인 `String` 클래스의 단점
	* 문자 더하거나 변경시 계속해서 새로운 객체 생성하고 기존 객체 사용 안하면 GC.  ➡️  CPU, 메모리 자원 소모.

➡️ 가변 String으로 해결 = StringBuilder
```java
StringBuilder sb = new StringBuilder();
sb.append("A");
sb.append("B");
sb.append("C");

String str = sb.toString(); // 불변으로 변경
```
* 다만 사이드 이펙트 주의.

#### String 최적화
* 자바 컴파일러는 '문자열 리터럴'을 더하는 부분을 자동으로 합쳐줌.
```java
String a = "hello, " + "World"; // 컴파일 전 
// 컴파일 후 String a = "hello, World";
```
* String 변수 최적화
```java
String result = str1 + str2; // 컴파일 시점에는 어떤 값 들어갈지 모름
// String result = new StringBuilder().append(str1).append(str2).toString(); 
```
* 문자열 루프안에서 더하는건 최적화 어려움

* StringBuilder  vs  StringBuffer
	* StringBuffer : 내부 동기화. 멀티 스레드 상황에서 안전하나 동기화 오버헤드로 성능 느림
	* StringBuilder : 동기화 오버헤드 없어 속도 빠름.

#### 메서드 체이닝 - Method Chaining
* 객체의 메서드를 연속적으로 호출하는 코딩 스타일
![[Pasted image 20240502161859.png]]
![[Pasted image 20240502163432.png]]
```java
ValueAdder adder = new ValueAdder();
int result = adder.add(1).add(2).add(3).getValue();
```
* 자기 자신의 참조값 반환하기에 가능.
* 장점 ) 코드를 간결하고 읽기 쉽게 만듦.
* ex ) StringBuilder, 자바 라이브러리, 스트림 API, builder패턴(대상 만드는 보조 클래스), lombok의 빌더

	```java
	List<String> items = Arrays.asList("apple", "banana", "cherry", "date");         List<String> result = items.stream()                                                                        .filter(s -> s.startsWith("b"))                                                  .map(String::toUpperCase)   
	```
	* Builder 패턴 : 객체 생성시 사용되는 디자인 패턴. 메서드 체이닝이 구현시 자주 사용됨.
		* 맴버변수 a,b,c 하나도 null 이 아니도록 강제 가능.



#### 스터디
* 연산양 폭증시 StringBuilder
* 그렇지 않을 때는 +
	➡️ 어처피 최적화되서 상관없으나 Stringbuilder 할 때는 new 에 초점을 맞춰서 메모리 생성된다에 초점인듯.

* 리플렉션 : 런타임에 클래스와 인터페이스 등을 검사하고 조작할 수 있는 기능.
	* 컴파일 타임 : 소스 코드를 컴파일러가 컴파일하는 단계
	* 런타임 : 컴파일된 코드를 실제로 실행시키며 외부 링크와 os, 사용자와 상호 작용하는 단계

	* getClass()
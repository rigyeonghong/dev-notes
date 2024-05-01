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






# 4. 래퍼, Class 클래스


### 4. 조건문
* if문 : 자유로운 조건식
```java
int score = 85;

// 서로 연관 조건이어, 하나로 묶을 때 
if(score >= 90){
 // 조건1 참일 때 실행
} else if (score >= 80) {
 // 조건1 거짓, 조건 2 참
} else {
 // 모든 조건 거짓일 때 실행되는 코드
}

// 조건 각각 수행
if (condition4){
	//작업 수행
}
```

* Switch문 : 단순하게 값이 같은지만 비교
```java
int grade = 1;

int coupon;
switch (grade){
	case 1:
		coupon = 1000;
		break;
	case 2:
	case 3:
		coupon = 2000;
		break;
	default:
		coupon = 500;
}
```
* 자바14 switch
```java
int grade = 2;

int coupon = switch (grade){
	case 1 -> 1000;
	case 2 -> 2000;
	default -> 500;
}
```

* 삼항 연산자
```java
int age = 18;
String status = (age >= 18) ? "성인" : "미성년자";
```

### 5. 반복문
* while문
```java
int count = 0;
while (count < 3) {
	count ++;
	System.out.println("현재 숫자는:" + count);
}

int sum = 0;
int i = 1;
int endNum = 3;

while (i <= endNum) {
	sum = sum + i;
	System.out.println("i=" + i + " sum=" + sum);
	i ++;
}

```

* do-while문 : 최초 한 번 코드 블록 꼭 실행시.
```java
int i = 10;

do {
	System.out.println("현재 숫자는:" + i);
	i++;
} while (i < 3);

```

 * break : 반복문 즉시 종료
 * continue : 반복문 나머지 종료, 다음 반복문 실행

* for문 : 반복횟수 정해져 있을 때. 깔끔.
```java
for (int i = 1; i <=10; i++) {
	System.out.println(i);
}

for (; ; ) {
	// 무한 반복
}
```

* 중첩 반복문
```java
for (int i = 0; i < 2; i++) {
	System.out.println(i);
	for (int j = 0; j < 3; j++) {
		System.out.println(j);
	}
}
```
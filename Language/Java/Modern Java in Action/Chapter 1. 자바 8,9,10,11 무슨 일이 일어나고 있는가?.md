* 목표 : 람다 표현식 이해, 자주 바뀌는 요구사항에 쉽게 대응하도록 유연&간결한 코드 구현, 핵심적으로 바뀐 자바 기능(람다, 메서드 참조, 스트림, 디폴트 메섣)
### 1.1 역사의 흐름 

* 자바8 : 간결한 코드, 멀티코어 프로세서의 쉬운 활용.
	* **스트림 API** (병렬 연산 지원) : 비싼 synchronized 사용 안해도됨
	* 메서드에 코드를 전달하는 기법 (메서드 참조와 람다)
	* 인터페이스의 디폴트 메서드

* 1.1 : 자바가 멀티코어 병렬성을 더 쉽게 이용하도록 진화하는 과정
* 1.2 : 자바8 제공하는 코드를 메서드로 전달하는 기법이 어떻게 강력한새로운 프로그래밍 도구가 될 수 있는지
* 1.3 : 스트림 API (병렬형 데이터를 포현하고, 이 데이터를 병렬로 처리할 수 있음을 유연하게 보여주는)가 어째서 강력한 새 프로그래밍 도구인지
* 1.4 : 디폴트 메서드라는 새로운 자바8 기능을 인터페이스, 라이브러리의 간결성 유지 및 재컴파일 줄이는데 어캐 활용할지
* 1.5 : JVM을 구성하는 자바 및 기타 언어에서 함수형 프로그래밍이라는 존재가 어떤 영향을 미치는지

### 1.2 왜 아직도 자바는 변화하는가

* 언어별 장단점
	* c,c++ : 프로그래밍 안정성 부족하나 작은 런타임 풋프린트로 os와 임베디드 시스템에서 인기
		* 낮은 안정성으로 예기치 않게 종료되거나 바이러스 침투하는 보안 구멍 존재
	* java,c# : 실제 런타임 풋프린트에 여유가 있는 애플리케이션에서는 압도
* (런타임 풋프린트 : sw애플리케이션 실행시 사용되는 시스템 자원의 양: 효율성 평가하는데 주요한 지표 중 하나)

#### 1.2.1 프로그래밍 언어 생태계에서 자바 위치
* 많은 유용한 라이브러리 포함하는 잘 설계된 객체지향 언어.
* 스레드와 락 이용한 소소한 동시성 지원
	* 자바의 하드웨어 중립적인 메모리 모델 때문에 멀티코어 프로세서에서 병렬 수행되는 스레드는 싱글코어에서의 동작과 달리  예기치 못한 상황 일으킬 수 있음.
* 코드를 JVM 바이트 코드로 컴파일하는 특징(모든 브라우저에서 가상 머신 코드 지원) 때문에 자바는 인터넷 애플릿 프로그램의 주요 언어됨.
	* JVM과 바이트 코드 > 자바 언어 : JVM에서 실행되는 경쟁언어인 스칼라, 그루비로 대체.
* 임베디드 컴퓨팅 분야 장악.

* 대중적 인기 얻은 이유
	* 캡슐화 : c에 비해 sw 엔지니어링적인 문제가 적음
	* 객체지향의 정신적인 모델 : 윈도우 95 및 WIMP 프로그래밍 모델에 쉽게 대응

* 빅데이터 ➡️ 멀티코어 cpu/컴퓨터 클러스터링으로 처리 필요성 생김.
	* 자바8 : 더 다양한 프로그래밍 도구/문제를 더 빠르고 정확, 쉽게 유지보수 가능하다는 장점.
* 큰 시스템 설계 방식 변화 : 외부에서 큰 하위시스템 컴포넌트를 추가하고 다른 벤더가 만든 컴포넌트 이용해 개발 많이함
	* 자바8,9 : 디폴트 메소드와 모듈 제공

1.2.2~4 자바8 설계 밑바탕 이루는 3가지 프로그래밍 개념 소개
#### 1.2.2 스트림 처리
* 스트림 처리
	* 스트림 : 한 번에 한 개씩 만들어지는 연속적 데이터 항목들의 모임.
	* 스트림API : 기존에 한 번에 한 항목 처리 방식을 마치 db 질의처럼 고수준으로 추상화해서 일련의 스트림으로 만들어 처리 가능. 스트림 파이프라인 이용해 입력 부분을 여러 cpu 코어에 쉽게 할당 가능. ➡️ 스레드라는 복잡한 작업 사용하지 않으면서도 공짜로 병렬성 얻을 수 있음.

#### 1.2.3 동작 파라미터화로 메서드에 코드 전달하기
* 코드 일부를 API로 전달하는 기능 : **동작 파라미터화**
	* Comparator 객체를 만들어 Sort에 넘겨주는 방법이 아닌, 메서드를 다른 메서드의 인수로 넘겨줄 수 있음.

#### 1.2.4 병렬성과 공유 가변 데이터
* 병렬성을 공짜로 얻을 수 있다 ➡️ 스트림 메서드로 전달하는 코드의 동작 방식을 조금 바꿔야함.
	* 다른 코드와 동시에 실행되도 안전하게 실행되야함 ➡️ 공유 가변 데이터에 접근하지 않아야함 : 순수 함수, 부작용 없는 상태 없는 함수
	* 공유된 변수, 객체 존재시 병렬성에 문제 발생 ➡️ synchronized 이용해 보호하는 규칙 만들 수 있으나 ➡️ 시스템 성능에 악영향 
	  
#### 1.2.5 자바가 진화해야 하는 이유
* 이전 변화1) 제네릭, List<스트링> ➡️ 컴파일 할 때 더 많은 에러 검출 가능, 리스트의 유형 알 수 있어 가독성 좋아짐.
	* 변화2) Iterator 대신 for-each 루프 사용
	* 고전적 객체지향에서 벗어나 함수형 프로그래밍으로 다가섬 ➡️ 하려는 작업이 최우선시, 어떻게 수행하는지는 별개 문제로 취급.

➡️ 언어는 hw, 프로그래머 기대의 변화에 부응하는 방향으로 변화해야함.

### 1.3 자바 함수
* 함수를 값처럼 취급함
#### 1.3.1 메서드와 람다를 일급 시민으로
* 메서드 참조 : 메서드를 값으로 사용
	```java
	File [] hiddelnFiles = new File(".").listFiles(File::isHidden);
```
* 람다 : 익명 함수
	```java
	(int x) -> x +1 // x 호출시 x+1 반환
```
* predicate : 수학에서는 인수를 값으로 받아 true/false 반환하는 함수를 뜻함
```java
public static boolean isHeavyApple(Apple apple){
	return apple.getWeight() > 150;
}

static List<Apple> filterApples(List<Apple> inventory, Predicate<Apple>p) {
	List<Apple> result = new ArrayList<>();
	for (Apple apple: inventory) {
		if (p.test(apple)) {
			result.add(apple);
		}
	}
	return result;
}

filterApples(inventory, Apple::isGreenApple);
filterApples(inventory, Apple::isHeavyApple);
```

#### 1.3.3 메서드 전달에서 람다로
* 한 두번 사용할 메서드를 매번 정의하는 것은 귀찮은 일.
```java
filterApples(inventory, (Apple a) -> GREEN.equals(a.getColor()) );
```


### 1.4 스트림
* 컬렉션 만들고 활용함.
* 스트림 API 이용시 컬렉션 API와 다른 방식으로 데이터 처리 가능.
	* 컬렉션 API : for-each 루프로 각 요소 반복작업 : **외부 반복**
	* 스트림 API : 루프 신경 쓸 필요 없이 라이브러리 내부에서 모든 데이터 처리 : **내부 반복**

* 많은 요소 가진 목록 반복시 오랜 시간 걸림 ➡️ 멀티코어 cpu로 작업 할당시 처리시간 주일 수 있음.
	* 멀티스레딩 코드 구현해 병렬성 이용은 쉽지 않음.

#### 1.4.1 멀티스레딩은 어렵다
* 자바 제공 스레드 API 로 멀티스레딩 구현시 ➡️ 각각 스레드는 동시에 공유 데이터 접근 및 데이터 갱신 ➡️ 스레드 제어 잘 못하면 원치 않은 방식으로 데이터 변경가능

➡️ 자바8 스트림API 로 '컬렉션 처리하며 발생하는 모호함과 반복적 코드 문제' & '멀티코어 활용 어려움' 해결!!

### 1.5 디폴트 메서드와 자바 모듈
* 디폴트 메서드 : 인터페이스 쉽게 바꾸도록 지원.


### 1.6 함수형 프로그래밍에서 가져온 다른 유용한 아이디어

자바에 포함된 함수형 프로그래밍의 핵심적 두 아이디어
* 메서드와 람다를 일급값으로 사용하는 것.
* 가변 공유 상태가 없는 병렬 실행 이용해 효율/안전하게 함수, 메서드 호출.

* Q1. Synchronized 키워드에 대해 설명해 주세요.
	* Synchronized 키워드가 어디에 붙는지에 따라 의미가 약간씩 변화하는데, 각각 어떤 의미를 갖게 되는지 설명해 주세요.
	- 효율적인 코드 작성 측면에서, Synchronized는 좋은 키워드일까요?
	- Synchronized 를 대체할 수 있는 자바의 다른 동기화 기법에 대해 설명해 주세요.
- Q2. 람다 표현식이란
	- 장단점

* A1. 멀티스레딩 환경에서 여러 스레드가 동시에 같은 객체의 특정 부분에 접근하는 것을 제어하여 동기화를 보장하는 데 사용
	* 장점 비교적 간단하게 스레드 간의 동기화를 수행할 수 있습니다. 이를 통해 데이터 무결성과 일관성을 보장
	* 단점: 시스템의 성능 저하를 초래할 수 있습니다. 불필요한 범위를 넓게 잠그는 것은 스레드 경쟁 상태를 유발하고, 애플리케이션의 응답성을 저하

	* 메소드에 `synchronized` 키워드 
		* 인스턴스 메서드 : 객체
		* 정적 메소드 : 해당 클래스의 모든 인스턴스 잠금
	* 특정 객체에 키워드
		* 특정 객체 : 블록 안의 코드 동시 접근 방지
		* 특정 클래스 : 클래스 레벨 리소스 접근 동기화

* A2.  메서드로 전달할 수 있는 익명 함수를 단순화한 것.
	* 장점 : 코드 간결, 가독성, 병렬 처리에 용이
	* 단점 : 재사용 불가, 디버깅 어려움, 많이 사용시 오히려 중복 코드 양성


#### 스터디
https://www.inflearn.com/course/the-java-java8#curriculum

* 스트링 불변 객체로 설계한 이유
	* 스트링에 여러 스레드 접근시 가변이면 레이스 컨디션 발생 가능. 성능이슈 감소.
	* 메모리 최적화 : Java String Pool
	https://dundung.tistory.com/242


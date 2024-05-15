# 7. 병렬 데이터 처리와 성능
* 스트림으로 데이터 컬렉션 관련 동작 얼마나 쉽게 **병렬**로 실행할 수 있는지 설명
	* 병렬 스트림으로 데이터 병렬 처리하기
	* 병렬 스트림의 성능 분석
	* 포크/조인 프레임워크
	* Spliterator로 스트림 데이터 쪼개기

* 4-6장 
	* 스트림 인터페이스 이용 데이터 컬렉션을 선언형으로 제어하는 방법 
	* 외부 반복을 내부 반복으로 바꾸면 네이티브 자바 라이브러리가 스트림 요소 처리 제어할 수 있음을 확인. 
	* ➡️ 자바 개발자 : 컬렉션 데이터 처리속도 높이려고 고민할 필요 없음. 컴퓨터 멀티코어 활용해 파이프라인 연산 실행 가능.
	* ex. 자바7 등장 전) 데이터 컬렉션 병렬 처리 어려움. 
		* 데이터를 서브파트로 분할. 분할된 서브파트를 각 스레드로 할당. 스레드 할당 후 의도치 않은 레이스 컨디션 발생하지 않도록 적절한 동기화 추가. 부분 결과 합침.
	* 자바 7 후) 쉽게 병렬화 수행 & 에러 최소화하도록 포크/조인 프레임워크 기능 제공


## 7. 1 병렬 스트림
* 컬렉션에서 parallelStream 호출시 병렬 스트림 생성. ➡️ 사용시 모든 멀티코어 프로세서가 각각의 청크 처리하도록 할당 가능.
	* 병렬 스트림 : 각각의 스레드에서 처리하도록 스트림 요소를 여러 청크로 분할한 스트림.
	* BinaryOperator로 리듀싱 작업
		* BinaryOperator : 스트림에서 요소들을 축소(reduce)할 때 유용
		```java
		BinaryOperator<Integer> add = (a, b) -> a + b;         
		BinaryOperator<String> merge = (a, b) -> a + b;
		```
		* reduce : stream API 일부로 스트림의 모든 요소에 대해 리듀스 연산 수행. 초기화 -> 누적 연산 적용 -> 결과 반환
		* `::`  메소드 참조 나타내는데 사용. 함수형 인터페이스의 구현체를 간결하게 표현.
		```java
		List<String> words = Arrays.asList("apple", "banana", "cherry", "date"); 
		words.forEach(word -> System.out.println(word));    
		words.forEach(System.out::println);
		```

### 7. 1.1 순차 스트림을 병렬 스트림으로 변환하기
```java
public long parallelSum(long n) {
	return Stream.iterate(1L, i -> i+1)
				 .limit(n)
				 .parallel() // 병렬 스트림으로 변환 <-> sequentia
				 .reduce(0L, Long::sum);
}
```
![[IMG_F5FF3F9EF23D-1.jpeg]]
* 병렬 작업 수행 스레드 생성하는 곳 : 병렬 스트림 내부적으로 ForkJoinPool 사용.
	* ForkJoinPool : 프로세서 수(Runtime.getRuntime().availableProcessors() 반환값에 상응하는 스레드 갖음)

### 7. 1.2 스트림 성능 측정
* sw에서 추측은 위험한 방법
* 성는 최적화시, 첫째 둘째 셋째도 측정!




# 8. 컬렉션 API 개선
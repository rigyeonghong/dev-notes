# 지은이와 감사의 글
* 객사오 핵심
	* 클래스가 아니 객체
	* 객체는 독립적 존재 아닌 기능 구현위한 협력하는 공동체 존재
	* 협력에 참여하는 객체들에게 적절한 역할, 책임 부여
* 오브젝트 핵심
	* 결합도, 응집도, 캡슐화 명확히 표현
	* 역할, 책임, 협력 중심 설계 원칙

* 복잡한 코드를 수월히하려면
	* 시스템을 독립적/ 재사용 가능한 프로시저의 구성으로 바라보기
	* 객체로 바라보기
		* 요청 담은 메세지 전달 -> 객체 : 어떻게 처리할지 자율 판단 -> 내부 데이터로 필요한 작업 수행

* 오브잭트 책 특징
	* 동작하는 코드
	* 좋은 설계란 무엇인가
	* 실무 객체지향 프로그래밍 적용 시점, 직면할 다양한 문제 설명

# 0. 프로그래밍 패러다임
* 패러다임 : 모델, 패턴, 전형적인 예
* 프로그래밍 패러다임 : 성숙한 개발자 공동체에 의해 수용된 프로그래밍 방법, 문제 해결 방법 및 스타일
	* 동일한 규칙, 표준 따라 프로그램 작성함으로 불필요한 의견 충돌 방지.

# 1.  객체, 설계
* 요약 : 티켓 판매 시스템으로 책 전체 주제 함축 

* 이론 < 실무
	* 특히 sw 설계, 유지보수.
	* 그러므로, 코드를 이용해 설명하겠다!

### 01. 티켓 판매 애플리케이션 구현
* 목적 : 소극장 이벤트
* 방법 : 추첨으로 선정된 관람객에게 공유 무료 관람 초대

	```java
	// 이벤트 초대장
	public class Invitation {
		private LocalDateTime when;
	}

	// 티켓
	public class Ticket {
		private Long fee;

		public Long getFee(){
			return fee;
		}
	}

	// 관람객 소지품
	public class Bag {
		private Long amount; // 현금
		private Invitation invitation; // 초대장
		private Ticket ticket; // 티켓


		// 생성시 제약 강제
		// 이벤트x 관람객 가방 : 현금
		public Bag(Long amount){
			this(null, amount);
		}

		// 이벤트o 관람객 가방 : 현금 + 초대장
		public Bag(Invitation invitation, long amount){
			this.invitation = invitation;
			this.amount = amount;
		}

		public boolean hasInvitation(){
			return invitation != null;
		}

		public boolean hasTicket(){
			return ticket != null;
		}

		public void setTicket(Ticket ticket){
			this.ticket = ticket;
		}

		public void minusAmount(Long amount){
			this.amount -= amount;
		}

		public void setTicket(Long amount){
			this.amount += amount;
		}
	}

	// 관람객
	public class Audience {
		private Bag bag;

		public Audience(Bag bag){
			this.bag = bag;
		}

		public Bag getBag(){
			return bag;
		}
	}

	// 매표소
	// 판매할 티켓, 판매 금액 보관
	public class TicketOffice {
		private Long amount;
		private List<Ticket> tickets = new ArrayList<>();

		
		public TicketOffice(Long amount, Ticket ... tickets){
			this.amount = amount;
			this.tickes.addAll(Arrays.asList(tickets));
		}

		public Ticket getTicket(){
			return tickets.remove(0);
		}

		public void minusAmount(Long amount){
			this.amount -= amount;
		}

		public void plusAmount(Long amount){
			this.amount += amount;
		}
	}


	// 판매원
	public class TicketSeller {
		private TicketOffice ticketoffice;

		public TicketSeller(TicketOffice ticketoffice){
			this.ticketOffice = ticketOffice;
		}
	
		public TicketOffice getTicketOffice(){
			return ticketOffice;
		}
	}
	```

* 마무리 조합 
```java
// 관람객 소극장 입장
public class Theater {
	private TicketSeller ticketSeller;

	public Theater(TicketSeller ticketSeller){
		this.ticketSeller = ticketSeller;
	}


	public void enter(Audience audience){
		if(audience.getBag.hasInvitation()){
			Ticket ticket = ticketSeller.getTicketOffice().getTicket();
			audience.getBag().setTicket(ticket);
		} else { 
			Ticket ticket = ticketSeller.getTicketOffice().getTicket();
			audience.getBag().minusAmount(ticket.getFee());
			ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
			audience.getBag().setTicket(ticket);
		}
	}
}
```

### 02. 문제
* sw 모듈이 가져야하는 3기능. by 로버트 마틴의 클린 소프트웨어
	* 1. 제대로 실행
	* 2. 변경 용이
	* 3. 이해하기 쉽게

#### 예상을 빗나가는 코드
* 3번 충족 X
	* 3. 이해하기 쉬운 코드 : `동작이 예상을 크게 벗어나지 않은 코드`
		관람객 입장에서 소극장이 가방 뒤짐.
		판매원 입장에서 본인은 넋놓고 바라봄.

* 코드 이해 위해 여러 세부 내용 기억해야하는 문제.
	* Theater의 enter 메서드 이해 위해 audience는 bag 가지고, 거기에 현금, 티켓있고 등.. 기억해야함

#### 변경에 취약한 코드
* 2번 충족 X ➡️ 객체 사이 의존성 문제.
	* 관람객이 가방 들고 있지 않다면?
		➡️ audience 에서 bag 제거. 거기에 접근하던 theater의 enter 수정.
	* 신용카드 이용해 결제한다면?
	* 판매원이 매표소 밖에서 티켓 판다면?

* 의존성 : 변경에 대한 영향 암시.
	* 객체지향 설계는 서로 의존, 협력하기에 완전히 없애는 건 아님.
	* 목표 : 애플리케이션 기능 구현에 최소한의 의존성 유지 및 불필요한 의존성 제거. 
		➡️ #결합도낮춰 #변경에용이한설계 만들기
		* 의존성 ⬆️ : 결합도 높음

### 03. 설계 개선
* 해결 방안 : 세세한 부분까지 알지 못하도록 정보 차단 및 자율적 존재로 만들기.
	* 관람객 스스로 가방안의 현금, 초대장 처리
	* 판매원 스스로 매표소의 티켓과 판매 요금 처리

#### 자율성 높이기
```java
// 판매원
public class TicketSeller {
	private TicketOffice ticketoffice; // 접근 가능한 public method 미존재로 접근불가.      캡슐화

	public TicketSeller(TicketOffice ticketoffice){
		this.ticketOffice = ticketOffice;
	}

	// theater의 enter 기능 판매원에게로.
	public void sellTo(Audience audience){
		if(audience.getBag.hasInvitation()){
			Ticket ticket = ticketOffice().getTicket();
			audience.getBag().setTicket(ticket);
		} else { 
			Ticket ticket = ticketOffice().getTicket();
			audience.getBag().minusAmount(ticket.getFee());
			ticketOffice().plusAmount(ticket.getFee());
			audience.getBag().setTicket(ticket);
		}
	}
}

```java
// 관람객 소극장 입장
public class Theater {
	private TicketSeller ticketSeller;

	public Theater(TicketSeller ticketSeller){
		this.ticketSeller = ticketSeller;
	}

	// ticketOffice 가 tickeSeller 내부에 존재(구현 영역)하는지 모름.
	public void enter(Audience audience){
		ticketSeller.sellTo(audience); // ticketSeller의 인터페이스에 의존.
	}
}

```

* 캡슐화 : 개념적 물리적으로 객체 내부의 세부사항 감추는 것.
	* 목적 : 변경하기 쉬운 객체 생성. 결합도 ⬇️

* 객체를 인터페이스와 구현으로 분리. 인터페이스만 공개하는 것

#### 무엇이 개선되었나
* 변경 용이

#### 어떻게
* 자기 자신의 문제를 스스로 해결하도록 코드를 변경하여. 직관따라

#### 캡슐화와 응집도
* 핵심 : 객체 내부의 상태를 캡슐화 & 객체 간 오직 메시지 통해 상호작용
	* 응집도 : 밀접하게 연관된 작업만 수행, 연관성 없는 작업은 다른 객체에게 위임하는 객체

#### 절차지향과 객체 지향
* 절차지향 프로그래밍 : 프로세스와 데이터를 별도의 모듈에 위치
	* 직관에 위배
	* 데이터 변경으로 인한 영향을 지역적으로 고립 불가로 변경 두려움.
* 객체지향 프로그래밍 : 데이터와 프로세스가 동일한 모듈 내부에 위치
	* 의존성은 적절히 통제되고, 하나의 변경으로 인ㄴ 여파가 여러 클래스로 전파되는 것을 효율적으로 억제.

#### 책임의 이동
* 각 객체는 자신을 스스로 책임지도록 하는 것.
* 객체가 어떤 데이터를 가지냐 보다 객체에 어떤 책임을 할당할 것이냐에 초점!

##### 결론
* 목적 : 설계/변경을 어렵게 만드는 불필요한 의존성을 제거 ➡️ 객체 사이 결합도 낮추기
* 방법 : 캡슐화
* 결과 : 객체의 자율성 높이고 응집ㅗ 높은 객체들의 공동체 창조


#### 더 개선
* 변경
	* Bag 을 자율적 존재로, Audiendce가 bag의 인터페이스에만 의존하도록
	* TicketOffice 도 자율적 존재로, TicketSeller가 인터페이스에 의존하도록
* 결과
	* TicketOffice와 Audiendce 사이 의존성 추가
	* 즉, TicketOffice의 자율성 높이나, 결합도 상승
![[Pasted image 20240703125437.png]]

* 트레이드오프
	* TicketOffice의 자율성 vs Audiendce 결합도

* 무생물 역시 자율적 존재로 취급
	* 현실에서는 수동적 존재여도 객체지향 세계에서는 모든 것이 능동, 자율적 존재.
	* 의인화 : 능동적 자율적 존재로 sw 객체 설계하는 원칙. by 레베카 워프스브록

### 04. 객체지향 설계
#### 설계가 왜 필요한가
> 설계란 코드를 배치하는 것이다.
> 설계는 코드 작성의 일부이며 코드를 작성하지 않고는 검증할 수 없다.

> 좋은 설계란, 오늘 완성해야 하는 기능을 구현하는 코드를 짜는 동시에 내일 쉽게 변경할 수 있는 코드를 짜야 한다.
> 요구사항이 항상 변경되기 때문이다. 개발 시작시, 구현에 필요한 모든 요구사항 수집은 불가능에 가깝다.
> 코드 변경시, 버그 추가될 가능성 높아 불확실성으로 코드 수정 두려움이 발생한다.

#### 객체지향 설계
* 목적 : 변경에 유연하게 대응할 수 잇는 코드
* 방법 : 의존성을 효율적으로 통제

> 변경 가능한 코드란 이해하기 쉬운 코드.
> 협력하는 객체 사이의 의존성을 적절하기 관리하는 설계가 훌륭하다.


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

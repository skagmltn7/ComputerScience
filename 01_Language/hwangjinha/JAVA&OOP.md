tags : #java #OOP #CS #compiler #interpreter #String #Thread #Memory #GC #Collection #CallbyValue #CallbyReference


- java의 컴파일 과정을 설명하세요
	- 컴파일이란 고급언어로 작성된 .java 파일을 기계어인 byte code 즉, .class 파일로 변환하는 과정을 말합니다.
	- 컴파일 과정
	- 첫 번째로 프로그래머가 java언어로 소스코드를 작성합니다.
	- 두 번째 javac 컴파일러를 사용해 .java소스 파일을 컴파일하고, 바이트코드로 변환된 .class 파일을 생성합니다.
	- 세 번째 컴파일된 바이트 코드인 .class 파일을 클래스로더에 전달합니다.
	- 네 번째로 클래스 로더는 동적 로딩을 통해 필요한 클래스를 로딩 및 링크하여 런타임 데이터 영역인 jvm의 메모리에 올립니다.
	- 마지막으로 실행엔진은 jvm 메모리에 올라온 바이트코드들을 인터프리터 방식 혹은 jit 컴파일러 방식으로 실행합니다.

- compiler vs interpreter 차이를 설명하세요
	- 컴파일러는 전체 소스 코드를 한번에 기계어로 변환하고 실행파일을 생서하여 나중에 실행할 수 있습니다. 컴파일러의 예로는 c/c++/java 등이 있습니다. 컴파일러의 장점은 실행 속도가 빠르다는 것이고, 단점은 컴파일 과정에서 시간이 소요된다는 점입니다.
	- 인터프리터는 프로그램을 실행하는 동안에만 소스코드를 한 줄 씩 기계어로 변환하고 즉시 실행합니다. 인터프리터의 예로는 Python/Ruby/javaScript 등이 있습니다. 인터프리터의 장점은 개발과 디버깅이 빠르다는 것이고, 단점은 실행속도가 컴파일러에 비해 느리다는 것입니다.
	 - **JAVA에서는  기본적으로는 인터프리터, 코드반복 등이 발생하는 부분에서는 효율성을 위해 JIT 컴파일러를 사용합니다.**

- String 생성방법
	- `String s = "리터럴"` : 변수에 저장된 값이 힙 내부의 **Constant Pool**에 생성되어, 같은 값을 가지는 다른 String 변수는 이를 똑같이 참조하게 된다.
	- `String s = new String("할당")` : 새로운 객체를 생성하여 각각의 불변 문자열 객체가 된다.

- String, Stringbuffer, Stringbuilder 차이를 설명하세요.
	- String은 불변객체(immutable)한 객체입니다. 한번 생성된 객체의 값을 변결할 수 없으며 문자열을 조작할 때 마다 새로운 String객체가 생성되어 메모리를 차지하게 됩니다. 이로인해 연산이 많은 문자열에서는 작업 효율이 떨어질 수 있습니다. 그러나 불변성 덕분에 스레드 세이프하고, String객체를 자유롭게 공유할 수 있어 성능상 이점이 있을 수 있습니다.
	- StringBuffer은 가변(mutable)객체입니다. 문자열 조작시에 기존 객체의 값을 변경합니다. 멀티 스레드환경에서 동기화를 지원하여 스레드 세이프합니다. 하지만 동기화로 인한 오버헤드가 있어 단일 스레드 환경에서는 성능이 저하될 수 있습니다.
	- StringBuilder은 가변(mutable)객체입니다. 문자열 조작시에 기존 객체의 값을 변경합니다. 동기화를 지원하지 않아 스레드 세이프하지 않습니다. 하지만 동기화에 따른 오버헤드가 없기 때문에 단일 스레드 환경에서는 더 높은 성능을 제공합니다.

- Thread safe 란?
	- **thread safe**란 멀티스레드 환경에서 스레드가 동시에 해당 코드나 데이터를 접근하더라도 프로그램의 실행 결과가 올바르게 유지되는것을 의미합니다. 스레드 세이프를 달성하기 위한 방법으로는 **동기화** 여러스레드가 공유 리소스에 동시에 접근하는것을 제한하고 한번에 하나의 스레드만 해당 리소스를 사용할 수 있도록합니다. **뮤텍스/세마포어** 운영체제 수준에서 제공되는 동기화 방법으로, 공유 리소스에 대한 동시접근을 제어하는데 사용됨. 이외에도 **원자적연산**과 **lock-free**구조 등이 있습니다.
 
- Thread safe한 코드 생성 방법
	- synchronized 등의 키워드를 사용하여 lock
	- stream 사용
	- 불변객체 사용

- 자바의 메모리 영역에 대해 설명해주세요.
	- 자바의 메모리 공간은 크게 Method 영역, Stack 영역, Heap 영역으로 구분되고, 데이터 타입에 따라 할당된다.
	- 메소드(Method) 영역 : 전역변수와 static변수를 저장하며, Method영역은 프로그램의 시작부터 종료까지 메모리에 남아있다. **Thread 공유**
	- 스택(Stack) 영역 : 지역변수와 매개변수 데이터 값이 저장되는 공간이며, 메소드가 호출될 때 메모리에 할당되고 종료되면 메모리가 해제된다. LIFO(Last In First Out) 구조를 갖고 변수에 새로운 데이터가 할당되면 이전 데이터는 지워진다. **Thread 비공유**
	- 힙(Heap) 영역 : new 키워드로 생성되는 객체(인스턴스), 배열 등이 Heap 영역에 저장되며, 가비지 컬렉션에 의해 메모리가 관리되어 진다. **Thread 공유**
		- Perm 영역
			- 생성된 객체의 메타데이터가 저장된 주소 저장
			- 리플렉션 등에 사용
			- java8부터 네이티브 스택, 스태틱 영역으로 분할됨

- OOP의 5대 원칙 (SOLID)
	- S: 단일 책임 원칙(SRP, Single Responsibility Principle)
	- 단일 책임 원칙이란 하나의 클래스가 변경되는 이유는 하나뿐이어야한다. -> 하나의 클래스는 하나의 원칙만 가져야한다.
	- 하나의 책임이란것은 모호합니다. 하나의 역할만 가져야 한다기보다 문맥과 상황에 따라 잘 설계해야 합니다.
	- ex) Controller에서 비즈니스 로직을 처리한다면 클라이언트의 요청과 응답을 처리하는 역할과 비즈니스로직 처리역할 둘 다 하게됨으로 단일책임원칙에 위배됩니다. Controller는 요청과 응답을 Service는 비즈니스로직을 처리하는것이 단일책임 원칙을 지키는 것 입니다.
	- O: 개방-폐쇄 원칙(OCP, Open Closed Principle)
	- 자신의 확장에는 열려있고 주변의 변화에는 닫혀있어야 한다.
	- 기존의 코드를 변경하지 않고 새로운 기능을 추가할 수 있어야한다.
	- ex) 가장 좋은 예는 JDBC 인터페이스라고 할 수 있습니다. JDBC인터페이스로 표준화 하지 않았다면 DB를 바꿀 때 마다 각자 제공하는 API에 따라 코드를 수정했어야 했을 것 입니다. 하지만 JDBC 인터페이스를 제공함으로써 Connection을 설정하는 부분만 해당 드라이버로 수정해 주면 DB를 교체할 수 있습니다.
	- L: 리스코프 치환 원칙(LSP, Liskov Substitution Principle)
	- 하위클래스는 상위클래스의 역할을 하는데 문제가 없어야 한다.
	- 자식클래스는 부모클래스의 역할을 모두 할 수 있어야한다.
	- I: 인터페이스 분리 원칙(ISP, Interface Segregation Principle)
	- 하나의 일반적인 인터페이스보다는 여러개의 구체적인(클라이언트에 특화된) 인터페이스를 사용해야한다.
	- D: 의존 역전 원칙(DIP, Dependency Inversion Principle)
	- 의존관계를 맺을 때 변화하기 쉬운것보다 거의 변화하지 않는것에 의존해야한다.
	- 구체화에 의존하지 말고 추상화에 의존해야한다. -> 구현클래스에 의존하지 말고 인터페이스에 의존해야한다.

- OOP의 4가지 특징
	- 추상화 : 특정 관점에서 관심있거나 중요한 부분만 추려내는 작업
	- 캡슐화 : 데이터와 프로세스를 하나의 객체에 위치하도록 만드는 것
	- 상속 : 부모 객체의 특징을 그대로 물려받는 것
	- 다형성 : 같은 요청으로부터 응답이 객체의 타입에 따라 다르게 나타나는 것을 의미

- 가비지 컬렉터
	- 힙 영역에서 사용하지 않는 객체들을 제거하는 작업.
	- root로부터 참조되지 않는 객체 골라내어 삭제한다.
	- Minor GC 
		- young 영역에서 발생
		- 처음 객체가 생성될 때는 Eden 영역에서 생성
		- Survivor0, 1 영억이 있고 GC가 발생할 때 빈 영역으로 현재 참조중인 객체들을 MARK한 후 옮기고, 나머지 영역에서 객체들을 제거한다.
		- 한번의 GC동안 살아남은 객체들은 age가 증가하고, 일정 age 이상이 된 객체들은 old 영역으로 옮겨진다.
	- Major GC
		- old 영역에서 발생
		- young영역과 반대로 참조되지 않는 객체들을 MARK하고 지운다. (mark-and-sweep)
		- 메모리는 단편화 된 상태이므로 이를 한 군데에 모아주는 것을 Compaction이라 한다. (mark-sweep-compact)
		- GC를 수행하는 스레드 이외의 스레드는 모두 정지 (Stop-the-world)
	- GC 알고리즘
		1. Serial - 단일스레드
		2. parallel - 병렬, 멀티스레드 (minor)
		3. parallel old - 병렬, 멀티스레드 (minor, major)
		4. G1 - heap을 일정한 크기의 region으로 나눠서 region 단위로 탐색. JAVA 9 default

- Collection
	- List, Set
		- 콜렉션 인터페이스를 상속한다.
		- iterator comparator 등을 사용할 수 있다.
		- LinkedHashSet : 저장 순서를 기억하는 Set
	- Map
		- 콜렉션 인터페이스를 상속하지 않는다.

-  annotation를 사용하는 이유
	- compile time에 더 많은 오류를 잡을 수 있도록 해준다.
	- 빌드를 할 때 코드 자동생성 기능을 제공한다. - Lombok
	- 런타임에 특정 기능을 제공한다.

- 불변객체를 만드는 법
	- 캡슐화를 하고 setter를 사용하여 값을 바꾸도록 한다. (지양)
	- 방어적 복사를 한다. 값을 직접 바꾸지 않고 바꾼 값을 가진 객체를 생성

- Call by value VS. Call by reference
	- 메서드에서 파라미터로 전달하는 객체를 참조하는 방식의 차이
	- Call by reference : 파라미터로 넘어온 객체와 동일하지만 변수명만 다르게 전달받는다.
	- Call by value : 파라미터로 넘어온 객체의 주소가 주소를 저장하는 새로운 변수에 저장된다.

- 접근제어자의 종류
	- private : 클래스 범위
	- default : 패키지 범위
	- protected : 상속 범위
	- public : 공개(전체) 범위 

- 오버로딩 VS. 오버라이딩
	- 오버로딩
		- 같은 메서드명을 가진 다른 시그니처의 메서드
	- 오버라이딩
		- 메서드를 상속하여 무력화. 시그니처 동일
		- 접근제어자는 상위 메서드의 그것보다 범위가 크거나 같아야 한다.

- abstract class VS. interface
	- abstract class
		- 다중상속이 불가 (class이기 때문)
			- 메서드명 등이 겹칠 수 있다.
		- 부분적으로 구현 강제성을 줄 수 있다.
		- 모든 종류의 변수를 멤버변수로 가질 수 있다.
	- interface
		- 다중구현(상속) 가능
		- 상수만을 멤버변수로 가질 수 있다.
		- abstract, default, static, private, public 등의 키워드 가능
	- 시그니처가 겹친다면?
		- 항상 Class가 우선
		- interface끼리 겹칠 때는 default가 모두 있더라도 구현 강제

- null을 다루는 방법 3가지?
	- `Optional<T>`
	- assert : 테스트할 때 사용되며 이외에는 권장하지 않는다.
	- Object.isNull() : 권장하지 않는다.

- 자바의 예외, 에러
	- Error
		- 개발자가 수습할 수 없는 심각한 문제.
		- 미리 예측하여 방지할 수 없다.
	- Exception
		- Checked Exception
			- 반드시 예외처리를 해 줘야된다. (throws, try-catch-final, try with resource)
			- Exception 클래스 상속
			- 발생 시 : Commit
		- Unchecked Exception
			- 예외처리를 미리 해 주지 않아도 된다.
			- Exception을 상속하는 RuntimeException을 상속
			- 발생 시 : Roll Back

- JAVA 8에서 추가된 내용
	- 람다
		- 익명함수
		- 코드가 간단해진다.
		- 지연연산 : 효율성이 올라간다.
		- 일급객체
	- 스트림
	- 디폴트 메서드 (인터페이스)
		- 인터페이스의 메서드이지만 기본 구현체를 가지게 한다.
	- 옵셔널
		- null을 안전하게 다룰 수 있게 한다.
	- 날짜 클래스
		- 기존보다 편의성 증가
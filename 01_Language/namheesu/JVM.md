# JVM

![JVM구성요소](https://github.com/skagmltn7/skagmltn7/assets/133394749/d8ff0a18-0804-49a6-87a7-55aa4abf6122)
클래스 로더, 런타임 데이터 영역, 실행 엔진으로 구성

## 실행 순서
![img](https://github.com/skagmltn7/skagmltn7/assets/133394749/1a865b44-d91c-4a33-bdbb-646348ab0928)
1. 자바 프로그램 실행시 JVM은 OS로 부터 메모리 할당
2. 자바 컴파일러는 자바 소스 파일을 바이트 코드 파일로 변환
3. 클래스 로더는 바이트 코드 파일을 JVM 메모리 영역에 로딩 및 링킹
4. 실행 엔진은 메모리 영역에 로딩된 바이트코드를 기계어로 해석하여 프로그램 실행
***
## 클래스 로더
![img](https://github.com/skagmltn7/skagmltn7/assets/133394749/3399b768-08fe-46a6-9fc6-2e0d2fb96fbf)
클래스를 메모리에 한번에 올리지 않고 어플리케이션에서 필요한 경우 동적으로 메모리에 배치(동적 로딩)
- **로딩** : 클래스 파일을 JVM의 메모리에 로드
- **링킹**  : 클래스 파일을 사용하기 위한 검증 과정
  - **검증** : 클래스가 JVM 명세대로 구성되어있는지 검사
  - **준비** : 클래스가 필요한 메모리 할당
  - **분석** : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경
> ***심볼릭 레퍼런스*** :  참조 객체의 메모리 주소가 아닌 참조하는 대상의 이름만을 참조하는 것 <br/>
> ***다이렉트 레퍼런스*** : 참조 객체의 메모리 주소를 참조하는 것
- **초기화** : 클래스 변수들을 적절한 값으로 초기화
***
## 실행 엔진
- 클래스 로더를 통해 JVM 내의 런타임 데이터 영역에 배치된 바이트코드를 실행
- 자바 바이트 코드를 명령어 단위로 읽어서 실행
> 바이트 코드 : OpCode(1byte) + 추가 피연산자<br/>
- 동작방식 : 하나의 OpCode를 가져와서 피연산자와 작업 수행 후 다음 OpCode 실행
### 인터프리터
- 바이트 코드를 하나씩 읽어서 실행
- 하나씩 읽기 때문에 해석을 빠르지만 실행속도는 느림
### JIT
- 인터프리터의 단점을 보완하기 위해 도입
- 인터프리터 방식으로 실행하다가 적절한 시점에 전체 바이트 코드를 컴파일하여 네이티브 코드로 변경
- 해당 메서드를 더이상 인터프리팅하지않고 네이티브 코드로 직접 실행
- 네이티브 코드를 캐싱하기 때문에 한번 컴파일도니 코드는 빠르게 수행
- JVM은 내부적으로 해당 메서드의 실행 주기를 체크하여 일정 정도를 넘을 때에만 컴파일 수행
### GC
힙영역에서 사용하지 않는 객체를 메모리에서 삭제
***
## 런타임 데이터 영역
![img](https://github.com/skagmltn7/skagmltn7/assets/133394749/cb55c731-3cc5-484e-bbd3-ff429eba70c7)
- JVM이라는 프로그램이 운영체제 위에서 실행되면서 할당받는 메모리 영역
- Method Area, Heap, Stack, PC Register, Native Method Stack 구성
***
### PC Register
- 각 스레드마다 하나씩 존재
- 스레드가 시작될 때 생성
- 현재 수행중인 JVM 명령의 주소를 가짐
***
### Stack Area
![img](https://github.com/skagmltn7/skagmltn7/assets/133394749/fe4f9f9f-07b3-4010-b865-7967d261111e)
- 각 스레드마다 하나씩 존재
- 메소드 호출시 마다 각각의 스택 프레임 생성되어 메서드 안에서 사용되는 값 저장
- 메서드가 끝나면 프레임별로 사라짐
> **데이터 타입에 따른 스택, 힙 저장방식**<br/>
> - 기본값은 스택 영역에 직접 값을 가짐
> - 참조값은 힙 영역이나 메소드 영역의 객체 주소를 가짐
***
### Native Method Area
- 자바 외의 언어로 작성된 네이티브 코드를 위한 스택
- JNI(Java Native Interface)를 통해 호출
- JIT를 통해 변환된 네이티브 코드 저장되는 곳
***
### Method Area
- 모든 스레드가 공유하는 영역
- JVM이 시작될 때 생성
- JVM이 로딩한 클래스와 인터페이스에 대한 러타임 상수풀, 필드와 메소드 정보, static 변수, 메서드의 바이트코드 등을 보관하는 장소
- **Runtime Constant Pool**
  - 각 클래스나 인터페이스의 상수, 메서드와 필드에 대한 레퍼런스를 담은 테이블
  - JVM은 런타임 상수풀을 통해 해당 메서드나 필드의 실제 메모리 상 주소를 찾아 참조
***
### Heap Area
- 모든 스레드가 공유하는 영역
- 객체를 저장하는 공간으로 가비지 컬렉션 대상
- **Young Generation**
  - Minor GC
  - Eden, Survivor 1,2
  - mark - sweep - compaction
- **Old Generation**
  - Major GC
***

[JVM Internal](https://d2.naver.com/helloworld/1230)
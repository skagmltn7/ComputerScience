# 프로세스, 스레드, IPC

---

### 프로세스 & 스레드

**프로세스**

실행 중인 프로그램.

운영체제로부터 자원을 할당받은 작업의 단위(각 프로세스마다 독립된 자원을 가짐).

- 메모리와 CPU를 프로세스마다 할당받아서 사용함.
- 독립된 자원을 가짐으로써 문제발생시 다른 프로세스에 영향 X.
- OS의 최소 작업단위이기 때문에 스케줄링에 의해 따로 동기화가 필요 X.
- 자원을 공유하지 않기 때문에 프로세스간 자원 공유가 복잡함.
</br></br>

**스레드**

> 모든 프로세스는 최소 1개의 스레드를 소유.

프로세스가 할당받은 자원을 이용하는 실행 흐름의 단위.

- 프로세스가 할당받은 CPU, 메모리를 같은 프로세스의 스레드간에 공유.
- 스레드는 각각 별도의 스택만 가지고, 나머지(코드, 데이터, 힙) 영역은 공유.
- 서로 자원을 공유하기 때문에 문제발생시 다른 스레드에도 영향 O.
- OS 작업단위가 아니기 때문에 개발자가 직접 동기화 O.
- 스레드 간 자원을 공유하고 있기 때문에 자원 공유가 효율적, 컨텍스트 스위칭 과정이 빠름.

> 💡 **스레드가 스택영역만 따로 할당받는 이유**
>
> - 스레드는 독립적인 동작(함수 호출)을 할 수 있어야 함.
> - 때문에 함수의 매개변수, 지역변수 등을 저장하는 스택 메모리 영역이 따로 필요함.

---

### 프로세스의 자원 공유(IPC)

기본적으로 각 프로세스는 메모리에 별도의 주소 공간에서 실행되기 때문에, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근 불가함.

따라서 특별한 방법을 통해 프로세스가 다른 프로세스의 정보에 접근하는 것이 가능함.

그러나 프로세스 자원 공유는 단순히 CPU 레지스터 교체뿐만 아니라 RAM과 CPU 사이의 캐시 메모리까지 초기화되기 때문에 자원 부담이 크다는 단점 존재
⇒ 다중 작업에는 멀티 스레드가 효율적.
</br></br>

**IPC(Inter-Process Communication)**

<img width="699" alt="Untitled" src="https://github.com/hgene0929/hgene0929/assets/90823532/78d267f1-6168-40ba-a484-dfc69e78f1ec">

1. 공유 메모리(Shared Memory)
- 공유되는 메모리를 생성하여 해당 영역에 데이터를 쓰고 읽음으로써 정보 교환.
- 성능은 좋지만 프로세스간 동기화 문제 발생 가능.
2. 메시지 패싱(Message Passing)
- OS가 프로세스간 통신 방법을 제공하여 정보를 대신 전달해줌.
- OS가 동기화를 해주기 때문에 동기화 문제 X, 시스템 콜을 사용하기 때문에 성능이 떨어짐.
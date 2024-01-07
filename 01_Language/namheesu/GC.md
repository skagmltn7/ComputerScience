# GC(Garbage Collection)
- 런타임 메모리 영역 내 **힙 영역** 내에 사용하지 않는 메모리를 주기적으로 삭제하는 프로세스
- 장점
  - GC가 메모리 관리를 알아서 해주기 때문에 효율적인 메모리 관리가 가능
- 단점
  - GC언제 작동되는지 알 수 없음
  - GC 작동 시 GC 스레드를 제외한 다른 스레드의 동작이 멈춤(**Stop the World**) -> 오버헤드 발생
***
## GC청소방식
### mark and sweep
- GC대상이 될 객체를 표시(mark)하고 제거(sweep)한 후 듬성듬성한 메모리 영역을 채워나가는(compaction)과정
- **Mark** : Root Space로 부터 그래프 순회를 통해 연결된 객체를 찾아내서 어떤 객체를 참조하고 있는지 표시
- **Sweep** : 참조되지 않는 객체를 제거
- **Compact** : 분산된 메모리를 정리(GC모델에 따라 사용여부가 다름)
> **Root Space**<br/>
> JVM GC에서 Root Space는 Heap메모리 영역을 참조하는 method 영역, static 변수, stack 영역, native method 영역이 된다.
***
## GC 대상
### weak generation hypothesis
- 대부분의 객체는 금방 접근 불가능 상태(unreachable)가 된다
  - `Reachable` : 객체가 참조되고 있는 상태
  - `Unreachable` : 객체가 참조되지 않고 있는 상태(GC대상)
- 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재한다
>  객체는 대부분 일회성이며, 메모리에 오래 남아 있는 경우는 드물다

![img](https://github.com/skagmltn7/skagmltn7/assets/133394749/961c21ab-df71-42e1-8fd6-3f9ad8e82ab7)

### Young generation
- 새롭게 생성된 객체가 위치하는 곳
- 대부분의 객체가 금방 GC의 대상이 됨
- Eden영역이 꽉찬 경우 **Minor GC**발생
- Eden, Survivor 1, 2로 구성
#### 처리 절차
1. 새로 생성된 객체는 eden 영역에 위치
2. eden영역이 꽉차면 Minor GC 실행
3. Mark로 reachable 객체 탐색
4. 살아남은 객체는 survivor영역중 하나로 이동
5. sweep로 unreachable객체 제거
6. 하나의 Survivor영역이 가득차면 그 영역에서 살아남은 객체를 다른 Survivor영역으로 이동<br/>가득 찬 Survivor영역은 빈상태가 된다
7. 살아남은 모든 객체는 age가 1증가
8. 이 과정을 반복 후 계속 살아남은 객체는 Old영역으로 이동
> 하나의 Survivor영역은 무조건 빈 상태야 한다<br/>
> **age** : 객체가 Survivor영역에서 살아남은 횟수
> - 객체 헤더에 표시(6bit) -> age최댓값 31
### Old generation 
- young generation에서 접근불가능 상태가 되지 않아 살아남은 객체가 복사되는 곳
- young영역보다 크게 할당되어 GC가 적게 발생
- **Major GC**발생
***
## GC 알고리즘
### Serial GC
- 싱글쓰레드 -> stop the world 시간이 길다
- Minor GC : mark-sweep (싱글스레드)
- Major GC: mark-sweep-compact (싱글스레드)
- 적은 메모리와 CPU코어 개수가 적을 때 적합한 방식
### Parallel GC
- Java 8의 디폴트 GC
- Minor GC : mark-sweep (멀티스레드)
- Major GC: mark-sweep-compact (싱글스레드)
  
![image](https://github.com/skagmltn7/skagmltn7/assets/133394749/866eaa54-25ff-4c01-9b28-a6c15cfbd445)
### Parallel Old GC
- Parallel GC 개선
- Minor GC : mark-sweep (멀티스레드)
- Major GC: mark—summary-compact (멀티스레드)
> **Summary** : 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별
### G1 GC
##### ![img](https://github.com/skagmltn7/skagmltn7/assets/133394749/f3e3985c-2868-4fc4-88db-3722c4bad9f0)
- Java 9+의 디폴트 GC
- young/old 영역이 아닌 바둑판 모양의 영역(**Region**)에 객체 동적 할당
- Young의 세가지 영역에서 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식
- `메모리가 많이 차있는 영역을 우선적으로 GC`
> 이전의 GC는 **Eden -> Survivor 1,2 -> Old**순<br/>
> G1 GC는 아무 Region영역에 재할당<br/>
> ex. Survivor -> Eden 이동 가능
[Java Garbage Collection](https://d2.naver.com/helloworld/1329)

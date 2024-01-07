# Override
```java
class Parent {
    void method() { 
    	System.out.println("부모 클래스의 메서드입니다."); 
    }
}

class Child extends Parent {
    void method() { 
        System.out.println("자식 클래스의 메서드입니다.");
    }
```
- 상위 클래스에서 이미 정의된 메소드를 자식 클래스에서 같은 메소드 시그니처의 메소드로 재정의 하는 것
- **상위 클래스보다 접근 제한자의 범위가 좁으면 X**
# Overload
```java
System.out.println(96); 
System.out.println(0.721);
System.out.println("남희수"); 
System.out.println(true);
```
- 같은 이름의 메서드를 중복하여 정의
- 서로 다른 메서드 시그니처를 갖는 여러 메소드들을 같은 이름으로 정의하는 것
- **반환타입이 다른 경우 오버로딩X**
- 대표적인 예시 : `println`, 생성자
> 메서드 시그니처<br/>
> 메서드의 선언부에 명시되는 매개변수의 리스트<br/>
> 매개변수의 개수, 타입, 순서가 같다면 메서드 시그니처가 같다고 할 수 있음

# Optional 클래스  
기존에는 조건문 ```if()```를 사용하여 NullPointerException 검사하던 것을       
조건문을 사용하지 않고 보다 가독성 좋고 간결하게 처리할 수 있도록 하는 클래스이다.        
   
**구조**   
```java
public final class Optional<T> extends Object {
  private final T value;
}
``` 
Optional 은 멤버 value에 인스턴스를 저장하는 일종의 래퍼클래스이다.      
  
1. Optional<클래스> opional = Optional.of(객체);
2. Optional<클래스> opional = Optional.ofNullable(객체);
  
* ```of()``` : Optional 객체 반환, **Null일 경우 NullPointerException 발생**             
* ```ofNullable()``` : Optional 객체 반환, **Null이 더라도 Optional 객체 반환 단, value변수는 null**                
        
객체의 값이 null 이 올것 같으면 ```of()```가 아닌 **```ofNullable()```을 사용해야한다.**           
```of()``` 같은 경우 null일 경우 예외를 발생하는 것이므로 `if(obj==null)`가 아닌 예외를 발생시켜 던지는 것이다.           
반대로 ```ofNullable()```은 관련 메서드를 사용하되 ```orElse()```를 사용하여 null일 경우의 로직도 구성할 수 있다.   

**예시**
```java
Optional<String> op = Optional.of("Hello World!");   
op.map()
```

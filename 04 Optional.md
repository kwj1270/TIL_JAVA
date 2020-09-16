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

**문자열이 존재할 경우**
```java
public class StringTest {
    public static void main(String[] args) {
        String str = getString();
        Optional<String> op = Optional.ofNullable(str);
        String text = op.map(String::toUpperCase).orElse("GoodBye World!");
        System.out.println(text);
    }
    public static String getString(){
        return "Hello World!";
    }
}
// 출력 : "HELLO WORLD!"        
```
      
**Null일 경우**
```java
public class StringTest {
    public static void main(String[] args) {
        String str = getString();
        Optional<String> op = Optional.ofNullable(str);
        String text = op.map(String::toUpperCase).orElse("GoodBye World!");
        System.out.println(text);
    }
    public static String getString(){
        return null;
    }
}
// 출력 : "GoodBye World!"     
```
   
## 메서드
**중간 연산**   
|메서드|설명|
|----------|----------|
|isPresent()|Optional 객체안에 값이 있는지 boolean 타입으로 반환|
|get()|Optional 객체안에 있는 값을 반환|
|ifPresent(Consumer<? super T>)|값이 있다면 해당 값을 이용한 동작 람다로 기술|
|map(Function<? super T, ? extends U>)|T로 들어온 값을 U형태로 반환 -> 제네릭에 데이터 1개만 기술해도 2개 적용|   
|orElse(대신 반환 객체)|제네릭에 맞춘 데이터형을 반환 -> 이부분은 개발자 고려해야 한다.|
|empty()|Optional 객체안에 있는 값을 비운다. -> NULL로 만든다.|   


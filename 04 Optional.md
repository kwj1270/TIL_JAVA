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
|map(Function<? super T, ? extends U>)|T로 들어온 값을 Optional<U>형태로 반환 -> 제네릭에 데이터 1개만 기술해도 2개 적용|   
|orElse(대신 반환 객체)|제네릭에 맞춘 데이터형을 반환 -> 이부분은 개발자 고려해야 한다.|
|empty()|Optional 객체안에 있는 값을 비운다. -> NULL로 만든다.|   
|flatMap(Function<? super T, ? extends U>)|리턴형이 Optional<u>가 아닌 단순 U이다.|    
## Optional 이용 코드 

```java
import java.util.Optional;


class Friend {
    String name;
    Company cmp;

    public Friend(String name, Company cmp) {
        this.name = name;
        this.cmp = cmp;
    }

    public String getName() { return name; }
    public Company getCmp() { return cmp; }

}

class Company {
    String cName;
    ContInfo cInfo;
    
    public Company(String cName, ContInfo cInfo){
        this.cName = cName;
        this.cInfo = cInfo;
    }

    public String getcName() { return cName; }
    public ContInfo getcInfo() { return cInfo; }
}

class ContInfo{
    String phone;
    String ards;
    
    public ContInfo(String phone, String ards){
        this.phone = phone;
        this.ards = ards;
    }

    public String getPhone() { return phone; }
    public String getArds() { return ards; }
}

public class NullPointerCaseStudy2 {
    public static void showCompAddr(Optional<Friend> op){
        String addr = op.map(Friend::getCmp)
                        .map(Company::getcInfo)
                        .map(ContInfo::getArds)
                        .orElse("There's no address information");
        System.out.println(addr);
    }
    public static void main(String[] args) {
        ContInfo ci = new ContInfo("321-444-577", "Republic of Korea");
        Company cp = new Company("YaHo CO., Ltd.", ci);
        Friend frn = new Friend("LEE SU", cp);
        showCompAddr(Optional.ofNullable(frn));
    }
}
```

## flatMap() 사용    
```flatMap()```은 Optional<U> 객체로 반환하는 것이 아닌 단순 U를 반환한다.          
**그러면 도대체 어떻게 사용하려고 하는 것일까?**             
        
위 코드에서의 클래스내의 존재하는 인스턴스 참조 변수들도 Optional 처리를 해주면 좋을것이다.     
그래서 인스턴스 참조 변수들을 Optional로 감싼 변수로 만들어줬다.       
         
**개선 전**    
```java
    String phone;
    String ards;
```  
**개선 후**    
```java
    Optional<String> phone;
    Optional<String> ards;
```
그리고 기존 getter 메서드는 해당 값 그대로 리턴해야 하기 때문에 `Optional<클래스>` 하게끔 수정해준다.     

**개선 전**
```java
    public String getPhone() { return phone; }
    public String getArds() { return ards; }
```
**개선 후**
```java
    public Optional<String> getPhone() { return phone; }
    public Optional<String> getArds() { return ards; }
```

하지만 이렇게 인스턴스 참조 변수도 `Optional`로 만들고 getter도 이에 맞춰서 `Optional<클래스>`를 반환하게 한다면     
리턴에서 이미 `Optional<클래스>`이기에 `map()`을 사용하게 되면 `Optional<Optional<클래스>>`가 리턴된다.         
그러므로 이럴 때는 단순 값만 반환해주는 `flatMap()` 사용이 더 적합하다.             
     
**개선 전**
```java
    public static void showCompAddr(Optional<Friend> op){
        String addr = op.map(Friend::getCmp)
                        .map(Company::getcInfo)
                        .map(ContInfo::getArds)
                        .orElse("There's no address information");
        System.out.println(addr);
    }
```
   
**개선 후**
```java
    public static void showCompAddr(Optional<Friend> op){
        String addr = op.flatMap(Friend::getCmp)
                        .flatMap(Company::getcInfo)
                        .flatMap(ContInfo::getArds)
                        .orElse("There's no address information");

        System.out.println(addr);
    }
```
      

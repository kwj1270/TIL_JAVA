# 들어가면서 
> 목표 : 자바의 제네릭에 대해 학습하세요.    

Generic 
제네릭 사용법       
제네릭 주요 개념 (바운디드 타입, 와일드 카드)    
제네릭 메소드 만들기   
Erasure    
   
제네릭 사용법       
제네릭 주요 개념 (바운디드 타입, 와일드 카드)    
제네릭 메소드 만들기   
Erasure    

# Generics
> 레퍼런스 타입을 담을 수 있는 마법의 요술 상자     
     
      
`Generics`는 `JDK 5` 부터 도입된,     
다양한 타입의 객체들을 다루는 메서드, 클래스에 **컴파일시의 타입 체크를 해주는 기능이다.**   
객체의 타입을 런타임이 아닌, 컴파일 타임에서 체크하기 때문에       
**객체 타입의 안정성을 높이고 형변환의 번거로움을 줄여주는 효과가 있다.**     
    
여기서, 타입 안정성을 높인다는 말은 아래와 같다.   
     
* 의도하지 않은 타입의 객체가 저장되는 것을 막는다.       
* 저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄인다.     
               
상속에 대해서 공부를 했다면,      
상위 클래스 타입으로 하위 클래스 타입을 참조할 수 있다는 사실을 알고 있다.           
프로그래밍을 하다보면 이런 다형성을 이용해 유지보수에 도움을 주는 코드를 작성할 수 있지만,         
이를 잘못 사용하면, **하위 인스턴스를 사용하지 말아야 할 곳에 하위 인스턴스를 인자로 주입할 수 있고**       
또한, 이같은 상황은 **컴파일 에러가 없이 동작하기에 버그를 발견하고 찾는데에도 시간이 걸린다.**        
      
`Generics` 는 이런 문제점을 해결하기 위해 등장한 기능이다.            
`Generics`은 이런 문제를 해결하고 아래와 같은 장점이 있으니       
이를 확인하고 실제 코드를 살펴보면서 정리를 해보고자 한다.              
          
**Generics의 장점**          
1. 타입 안정성을 제공한다         
    * `ClassCastException`과 같은 `UncheckedException`을 보장받을 수 있다.    
2. 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다.      
      
## 타입 안정성   
```java 

```
  
## 타입 체크 및 형변환
```java 

```
  
# Generics 사용법       
## Generics 클래스 선언   
```java
class Name <T> {
    
    private T t;
     
    public Name(T t){
        this.t = t;
    }
    
}

class Name2 <L, R> {
    
    private L l;
    private R r;
    
    public Name2(L l, R r){
        this.l = l;
        this.r = r;
    }

}
```   
`Generics 클래스`는 클래스 이름 옆에 `<문자>`의 형태로 기술해주면 된다.        
이후, 확정되지 않은 자료형을 사용할 때는 `문자`의 형태로 기술해주기만 하면 된다.         
또한, 위 코드에서 알 수 있듯이 2가지 타입에 대한 `Generics 클래스`를 정의할 수 있다.   

`<T>`와 같은 요소를 타입 매개변수라고 부르는데, 이 타입 매개변수의 이름은 짓기 나름이다.    
그러나 일반적으로 2가지 규칙을 지켜서 이름을 짓는다.   
   
* 한 문자로 이름을 짓는다.   
* 대문자로 이름을 짓는다.  

그리고 이러한 규약에 근거해 자주 사용되는 네이밍이 있다.   

* E : Element
* K : Key
* N : Number
* T : Type
* V : Value  

## Generics 메서드   
```java
class Name <T> {
    
    private T t;
     
    public Name(T t){
        this.t = t;
    }
    
}
```
클래스 레벨에서 `Generics 타입 매개변수`를 선언하고,   
메서드에서 동일한 `Generics 타입 매개변수`를 사용한다면 따로 기술하지 않아도 된다.    

```java
class Name<T> {

    private T t;

    public Name(T t) {
        this.t = t;
    }
    
    public <V> V makeMap(V value) {
        return value;
    }
}

class Name2 {
    
    public <V> V makeMap(V value) {
        return value;
    }
}
```   
하지만, 다른 타입의 `Generics 타입 매개변수`를 사용하고자 하거나    
클래스 레벨이 아닌 메서드 레벨에서만 `Generics`를 사용하고자 한다면        
메서드에 `Generics 타입 매개변수`를 기술하여 사용할 수 있다.         
  
## 원시 타입 자료형     
`Generics`을 이용하기에 앞서 한 가지 제약사항이 있다.        
바로 `원시 타입 자료형(기본 자료형)`은 직접적으로 사용이 불가능하다는 것이다.          
정확히 말하면, `Generics`는 `레퍼런스 타입`만을 지원하고 있다.   
   
**그렇다면, 원시 타입 자료형은 사용할 수 없는 것일까? 🤔**           
                
```java
class Sample {

    public static void main(String[] args) {
        ArrayList<Integer> arr = new ArrayList<>();
        arr.add(1);
        arr.add(2);
        arr.add(3);
        
        System.out.println(arr.get(0));
    }
    
}
```
**아니다,** 원시 타입 자료형도 **각각의 자료형에 맞는 래퍼 클래스가 존재한다.**          
그리고 **오토 박싱/언박싱 작업을 통해 손쉽게 사용할 수 있다.**       
      
## Generics 바운디드 타입
`바운디드 타입`은 `Generics 타입 매개변수`의 대상이 **지정된 클래스의 하위로 제한한다는 뜻이지만,**   
필자의 관점으로는 조금 다르게 생각하여 **지정된 클래스의 하위도 사용가능하게 해준다**라고 생각한다.  
   
`Generics 타입 매개변수`에 지정된 타입들의 상속 관계는 `Generics`에서 유효하지 않다.    
이게 무슨말이냐면, Parent 클래스와 이를 상속받는 Child 자식클래스가 있다고 가정한다.    

```java
public class Test {

    private static <T> List<Integer> convertTokenSizeList(List<T> list) {
        List<Integer> result = new ArrayList<>();
        for (T t : list) {
            int tokenSize = ((String) t).split(" ").length;
            result.add(tokenSize);
        }
        return result;
    }

    public static void main(String[] args) {
        List<String> myList = new ArrayList<>();
        myList.add("자 이제 시작이야 내 꿈은~");
        myList.add("내 꿈을 위한 여행 피카츄 (피카츄)");
        myList.add("걱정 따윈 없어 (없어)");
        myList.add("내 친구와 함께니까");

        List<Integer> tokenSizes = convertTokenSizeList(myList);
        tokenSizes.stream().forEach(System.out::println);
    }
}
```



# 와일드 카드    
# Generics Method   
# Type Erasure    



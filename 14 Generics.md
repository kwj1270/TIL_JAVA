# 들어가면서 
> 목표 : 자바의 제네릭에 대해 학습하세요.    
   
[1. Generics](#generics)    
[2. Generics 사용법](#generics-사용법)         
[3. WildCard](#wildcard)       
[4. Type Erasure](#type-erasure)        
        
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
1. 타입 안정성을 제공한다.         
    * `ClassCastException`과 같은 `UncheckedException`을 보장받을 수 있다.    
2. 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다.      
      
  
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
`Generics 클래스`는 클래스 이름 옆에 `<>`의 형태로 기술해주면 된다.         
`<>`안에 들어오는 값은 `Class, Interface, Eunm`등이 올 수 있으며      
`ArrayList<HashMap<String, Integer>>`와 같이 `Generics`를 사용한 클래스도 올 수 있다.        
만약, 확정되지 않은 자료형을 사용하고 할 때는 `<문자>`의 형태로 기술해주어 타입을 가변적으로 사용할 수 있다.                 
또한, 한 번에 여러 타입에 대한 `<>`를 정의할 수 있다.   
        
위 코드에서 `<T>`와 같은 요소를 타입 매개변수라고 부르는데,     
이 타입 매개변수의 이름은 개발자가 짓기 나름이다.      
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
[AccessModifier] [Modifier] <T> [ReturnType] [MethodName](Parameter p1...) {}
```    
`Generics`는 클래스 레벨 뿐만 아니라, 메서드 레벨에서도 개별적으로 선언할 수 있다.         
메서드 레벨에서의 `Generics`선언은 `Modifier`와 `ReturnType`의 사이로     
표현하자면 `Modifier |여기| ReturnType` 형태를 가지고 있다.      

**Generics 메서드 예시**
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

**클래스 레벨에서의 Generics 메서드**
```java
class Name <T> {
    
    private T t;
     
    public Name(T t){         // 클래스 레벨에 이미 선언되었기에 <T> 가 생략되었다.
        this.t = t;
    }
    
}
```
클래스 레벨에서 `Generics 타입 매개변수`를 이미 선언했다먄,   
메서드에서는 따로 `Generics 타입 매개변수`를 선언해주지 않아도 된다.   
단, 이는 클래스 레벨과 동일한 `Generics 타입 매개변수`를 사용한다고 가정하에 말하는 것이고  
만약, 클래스 레벨과 다른 `Generics 타입 매개변수`를 사용하고 싶다면 따로 기술해주면 된다.  

      
  
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
`바운디드 타입`은 `Generics 타입 매개변수`의 대상이 **지정된 클래스의 하위로 제한한다는 뜻이다.**        
   
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
위 코드는 문제없이 돌아가는 정상적인 코드이지만 몇가지 불편한 점이 있다.       
바로 `((String) t).split(" ").length;`을 통해 `String`으로 형변환을 시키고 있다.    
그렇다면 만약 **형변환을 시킬 수 없는 데이터 타입이 들어온다면 어떻게 될까?**   

```java
package me.kwj1270.javaapi.test.domain;

import java.util.ArrayList;
import java.util.List;

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
        List<Integer> myList = new ArrayList<>();
        myList.add(1);
        myList.add(2);
        myList.add(3);
        myList.add(4);

        List<Integer> tokenSizes = convertTokenSizeList(myList);
        tokenSizes.stream().forEach(System.out::println);
    }
}
```
`Generics`는 컴파일 타임에서 코드의 적합성을 판단해 컴파일 에러를 발생시킨다.      
위 코드는 컴파일 에러가 발생하지 않는다. 
그렇다면 올바른 코드 사용 및 `Generics`를 사용한다고 말할 수 있을까?  
    
아니다. 위 코드의 결과는 아래와 같다.       
    
<img width="1552" alt="ClassCastingError" src="https://user-images.githubusercontent.com/50267433/109456636-9b10fa00-7a9c-11eb-9544-fff371ef1c65.png">   

이 처럼 `Generics`를 사용한다 하더라도 컴파일 에러를 통해 에러를 잡을 수 없으며     
오히려 어설프게 사용함으로써 생기는 `클래스 캐스팅` 관리는 개발자의 머리를 복잡하게 한다.   

`generics`는 이러한 문제를 해결하기 위해 `바운디드 타입을 지원한다.`   
바운디드 타입의 형태는 아래와 같다.  

```java
<T extends 특정클래스>   
```  
해석하자면, 이렇다.         
`T`로 아직 사용되는 레퍼런스타입을 명확히 선언하지는 않았지만         
특정 클래스와 그 하위 클래스의 인스턴스만 사용할 수 있게 제한한다.     

언뜻보면, 사용의 이유를 잘 모를 수 있지만    
바운디드 타입을 사용해서 얻을 수 있는 장점은 많다.  
    
1. 타입이 특정 클래스 및 그 하위 클래스이므로 이에 맞는 컴파일 에러를 발생시킬 수 있다.    
2. 타입이 특정 클래스 및 그 하위 클래스이므로 확정된 요소들을 사용할 수 있다.          
    * 보다 정확히 말하면, 불필요한 타입 캐스팅을 하지 않아도 된다는 것이다.           
   
**컴파일 단계에서 에러를 발견하기**
```java
package me.kwj1270.javaapi.test.domain;

import java.util.ArrayList;
import java.util.List;

public class Test {

    private static <T extends String> List<Integer> convertTokenSizeList(List<T> list) {
        List<Integer> result = new ArrayList<>();
        for (T t : list) {
            int tokenSize = ((String) t).split(" ").length;
            result.add(tokenSize);
        }
        return result;
    }

    public static void main(String[] args) {
        List<Integer> myList = new ArrayList<>();
        myList.add(1);
        myList.add(2);
        myList.add(3);
        myList.add(4);

        List<Integer> tokenSizes = convertTokenSizeList(myList);  // 컴파일 에러 발생 
        tokenSizes.stream().forEach(System.out::println);
    }
}
```
<img width="1440" alt="PresentCompileError" src="https://user-images.githubusercontent.com/50267433/109458460-29d34600-7aa0-11eb-8aa8-e9041433e9f5.png">
         
메서드의 선언부에 존재하는 `Generics`를 `<T extends String>`로 바꾸었다.         
그 결과 이전에, 볼 수 없었던 컴파일 에러가 발생한 것을 알 수 있다.          
앞서 말했듯이, `Integer`는 `String`의 하위 클래스가 아니므로        
인자로 값을 넘기는 단계에서 컴파일 에러를 발생시키는 것이다.          
        
바운디드 타입으로 인해,    
`Generics`의 장점인 컴파일 단계에서 에러 발생을 제대로 실현시킬 수 있게 되었으며  
이로 인해 `타입 안정성을 제공한다.`는 장점을 유지할 수 있게 되었다.    
       
**불필요한 타입 캐스팅을 하지않고, 해당 클래스의 요소들을 사용하기**   
```java
package me.kwj1270.javaapi.test.domain;

import java.util.ArrayList;
import java.util.List;

public class Test {

    private static <T extends String> List<Integer> convertTokenSizeList(List<T> list) {
        List<Integer> result = new ArrayList<>();
        for (T t : list) {
            int tokenSize = t.split(" ").length;
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
<img width="1552" alt="CompleteBoundedType" src="https://user-images.githubusercontent.com/50267433/109458484-38b9f880-7aa0-11eb-9424-75f3819de932.png">
     
또한, 타입이 특정 클래스의 하위 클래스로 확정이 되었기에      
`t.split(" ").length;`와 같이 불필요한 타입 캐스팅을 하지 않아도 된다.  
이전에는 클래스가 지정이 안된 `T`였기 때문에           
String 클래스의 메서드를 사용하고자 한다면 String 타입으로 캐스팅해야 했지만,        
매서드 호출에서부터 `String 및 그 이하` 클래스로 제한을 두었기에 클래스 요소들을 사용할 수 있다.      
    
바운디드 타입으로 인해 제네릭의 기능을 최대한으로 살렸고  
앞서 소개했던 **Generics의 장점**을 우리는 코드로 알 수 있었다.          
       
1. 타입 안정성을 제공한다.         
    * `ClassCastException`과 같은 `UncheckedException`을 보장받을 수 있다.    
2. 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다.   
     
**추가로**         
`<T extends 클래스>`는 특정 클래스의 하위 클래스 타입을 허용한다고 했다.          
그렇기에 아래와 같은 코드를 작성할 수 있다.     
(로직은 신경쓰지 않고, 단순히 '가능하다'만을 표현하는 코드를 짰습니다.)    

```java
class Some {
    public int length;
    
    public Some(String str){
        
    }
    
    public Some split(String str){
        return this;
    }
    
}

class SSome extends Some{
    public SSome(String str) {
        super(str);
    }
}

public class Test {

    private static <T extends Some> List<Integer> convertTokenSizeList(List<T> list) {
        List<Integer> result = new ArrayList<>();
        for (T t : list) {
            int tokenSize = t.split(" ").length;
            result.add(tokenSize);
        }
        return result;
    }

    public static void main(String[] args) {
        List<SSome> myList = new ArrayList<>();
        myList.add(new SSome("자 이제 시작이야 내 꿈은~"));
        myList.add(new SSome("내 꿈을 위한 여행 피카츄 (피카츄)"));
        myList.add(new SSome("걱정 따윈 없어 (없어)"));
        myList.add(new SSome("내 친구와 함께니까"));

        List<Integer> tokenSizes = convertTokenSizeList(myList);
        tokenSizes.stream().forEach(System.out::println);
    }

}

/// 다른 케이스 - 일반 T로도 사용가능하다
class Test2 {

    private static <T extends Some> List<Integer> convertTokenSizeList(T list) {
        List<Integer> result = new ArrayList<>();
        result.add(list.split(" ").length);
        return result;
    }

    public static void main(String[] args) {
        List<Integer> tokenSizes = convertTokenSizeList(new SSome("자 이제 시작이야 내 꿈은~"));
        tokenSizes.stream().forEach(System.out::println);
    }

}
```   
여기서 눈 여겨볼 점은 `<T extends Some>`를 통해             
`Some`의 하위 클래스인 `SSome` 타입을 가진 List를 가지고 올 수 있었다.       
        
`Generics`에서 `<>`에 들어간 클래스들간의 상속 관계는 의미가 없다.      
즉, `List<Some>`과 `List<SSome>`은 관련이 있을 것 같지만   
아무런 관계가 없기에 아래와 같은 코드는 에러를 발생시킨다.         
    
```java
        List<Some> someList = myList;           // 컴파일 에러    
```
<img width="1440" alt="바운디드 타입" src="https://user-images.githubusercontent.com/50267433/109463314-1f1caf00-7aa8-11eb-8c3c-15011a9b88ff.png">   
  
그렇다면 어떻게 위와 같은 작업이 가능했던 것일까?   
       
여기서 많이 헷갈리는 것이 **상속관계로 값이 들어온다고 생각하는데 절대 아니다**   
답은 앞서 본 **바운디드 타입으로 인해 T 자체의 값이 변동되었기 때문에 가능한 것이다.**           
`<T extends Some>`으로 인해 바운디드 타입으로 선언된 `T`가 `Some`의 하위 클래스로 선택된다.     
즉, `T` 자체가 `SSome` 으로 변환되어 작업을 처리했기 때문에 인자값으로 `myList`를 넣을 수 있던 것이다.
        
```java   
        List<SSome> someList = myList;           // T는 SSome으로 변환되어 작업
```

# WildCard  
```java
<?>
```   
`WildCard`는 제네릭을 보조해주는 또 하나의 제네릭이라 생각할 수 있다.          
`WildCard`는 기본적으로 `?`로 표시하며 `어떤 레퍼런스 타입이든 될 수 있다`라는 뜻을 가졌다.   
            
`WildCard`는 제네릭과 비슷하기에 제네릭 대신으로 사용할 수 있다.       
아니, 정확히 표현하자면 `Generics`보다 더 자주 사용되는 `Generics`의 기능이다.         

```java
public static <T> void peekBox(Box<T> box) {
    System.out.println(box);
} // 제네릭 타입 메서드의 정의 

public static <T extends SomeClass> void peekBox(Box<T> box) {
    System.out.println(box);
} // 제네릭 바운디드 타입 메서드의 정의 
```
`Generics` 메서드는 위와 같이 제어자와 반환형 사이에 `Generics 매개변수 타입`을 넣어줘야 한다.     

```java
public static void peekBox(Box<?> box) {
    System.out.println(box);
} // 와일드 타입 메서드의 정의 

public static void peekBox(Box<? extends SomeClass> box) {
    System.out.println(box);
} // 와일드 바운디드 타입 메서드의 정의 
```
`WildCard` 메서드는 `Generics`와 다르게 파라미터에만 `<?>` 을 넣어주면 된다.     
이로인해, `Generics`보다 더욱 깔끔하고 보기좋은 코드를 구현할 수 있게 되었다.   
그렇기에 이러한 장점 때문에 개발자들은 와일드 카드를 사용하는 것을 더 선호한다.  

```java
private static List<Integer> convertTokenSizeList(? list) {
    List<Integer> result = new ArrayList<>();
    result.add(list.split(" ").length);
    return result;
}

private static List<Integer> convertTokenSizeList(<?> list) {
    List<Integer> result = new ArrayList<>();
    result.add(list.split(" ").length);
    return result;
}

private static List<Integer> convertTokenSizeList(<? extends Some> list) {
    List<Integer> result = new ArrayList<>();
    result.add(list.split(" ").length);
    return result;
}
```
대신, 이와 같이 와일드 카드 단독으로 자료형이 결정되는 방법에는 사용할 수 없다.       

## WildCard의 바운디드 타입
`WildCard`의 바운디드 타입은 제네릭과 비슷하다.     
하지만, 몇가지 다른 사항이 있고 이는 와일드 카드의 가치를 높여준다.     
    
* 상한 제한이 가능하다.          
* **하한 제한이 가능하다.        
* **대상 클래스로 `Generics`를 사용할 수 있다.**    
    
## 상한 제한 
`WildCard`의 상한 제한은 `Generics`의 상한 제한과 같은 기능을 수행한다.       
단, `T`라는 구문이 빠졌기에 이 부분은 직접 타입을 선언해줘야한다.       
들어오는 모든 값을 호환해주어야 하기 때문에 `<? extends 클래스>`에 정의된  
클래스를 기반으로 코드를 기술해주는 것이 좋다.  

```java
public class Test {

    private static List<Integer> convertTokenSizeList(List<? extends String> list) {
        List<Integer> result = new ArrayList<>();
        for (String t : list) {                       // 명시적으로 기술해줘야 한다.   
            int tokenSize = t.split(" ").length;
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

## 하한 제한      
`WildCard`는 `Generics`와 다르게 하한 제한이라는 것이 존재한다.        
즉, `<? super class>`에서 특정 클래스보다 상위 클래스의 타입만을 허용한다는 뜻이다.   
그렇기에 기존에 존재하는 코드들도 `WildCard`에 맞추려면 주로, `Object`로 선언 후 사용해야한다.       

```java
public class Test {

    private static List<Integer> convertTokenSizeList(List<? super String> list) {
        List<Integer> result = new ArrayList<>();
        for (Object t : list) {
            int tokenSize = t.hashCode()%Integer.MAX_VALUE;
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
위 코드르 보면, `하한 제한`을 사용하기에 `Object`의 메서드만 사용하고 있다.          
**그렇다면 하한제한을 사용하는 목적은 무엇일까? 🧐**             
`하한 제한`의 목적은 **불필요한 요소의 사용 및 호출을 방지하는 것**이다.       

현재 코드가 `Object`관련 메서드나 상위 클래스에 존재하는 요소들만 사용한다 가정한다.  
그렇다면 우리는 코드상에서 불필요한 `하위 인스턴스`의 요소나 메서드를 사용할 일이 없다는 뜻이다.   
하지만, `하위 인스턴스`의 요소나 메서드를 사용해도 컴파일 에러는 발생하지 않는다.     

이와 같은 문제점으로 보다 코드의 안정성을 위해서 하한제한을 만들어 놓은 것이다.   

## `<? extends T> 와 <? super T>`   
앞서 `WildCard`는 **제네릭을 보조해주는 또 하나의 제네릭**이라고 소개한 적이 있다.                 
즉, 제네릭을 보조해주기 위해 `WildCard`와 `Generics`는 동시에 사용할 수 있다.     
     
가령 `Generics`에서 특정한 레퍼런스 타입을 지정하지 않았을 경우 `<T>`를 사용한다.     
하지만, 여기서 이 `Generics`또한 `상한/하한 제한`을 해야하는 경우가 있다.       
이럴 때 `WildCard`를 사용하면 이전에는 사용하지 못했던 `상한/하한 제한`을 할 수 있다.   

**상한 제한**
```java
public class Test {

    private static <T> List<Integer> convertTokenSizeList(List<? extends T> list) {
        List<Integer> result = new ArrayList<>();
        for (T t : list) {
            int tokenSize = t.split(" ").length;
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
제어자와 반환형 사이에 존재하는 `Generics 매개변수 타입`에 `extends`를 사용하지 않아도,   
와일드 카드를 통해 매개변수에서 `Generics 매개변수 타입`의 상한 제한을 할 수 있다.       
       
**하한 제한**
```java
public class Test {

    private static <T extends String> List<Integer> convertTokenSizeList(List<? super T> list) {
        List<Integer> result = new ArrayList<>();
        for (T t : list) {
            int tokenSize = t.split(" ").length;
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
`Generics 매개변수 타입`을 기준으로 불특정 클래스에 대한 `하한 제한`을 할 수 있디.   
위 코드에서 조금 장난을 했는데,      
`Generics 매개변수 타입`로는 하한제한      
`WildCard`로는 상한제한을 두어 특정 클래스만 사용가능하도록 제한을 걸었다.        
물론, 이같은 경우는 단순히 `T`만 사용해도 되기에, 사용할 수 있다는 점만 알아두자       


# Type Erasure    
자바 컴파일러는 컴파일 과정에서 제네릭에 대해 타입 소거(Type erasure)를 진행한다.         
타입 소거란 타입정보를 컴파일 타임에만 유지하고, 런타임에는 삭제시켜 버리는 것인데        
과거 제네릭이 없던 버전과의 하위 호환성을 위해서 이러한 작업을 하는 것이다.   

```java
List<Object> ol = new ArrayList<Long>(); // 컴파일 에러
ol.add("타입이 달라 넣을 수 없다");
```
위의 2줄의 코드 모두 `<>`선언된 자료형의 타입이 다르기에 컴파일 에러가 발생한다.  
그리고 이러한 컴파일 에러를 도출하고자 한 것이 `Generics`의 목적이었다.  

**타입소거 전**
```java
// 타입소거 전
class GenericClass <T> {

    public void consume(T paramenter) {
        paramenter.toString();
    }
}
```
**타입소거 후**    
```java
// 타입소거 후
class GenericClass {

    public void consume(Object paramenter) {
        paramenter.toString();
    }
}
```
Java 컴파일러는 타입소거를 아래와 같이 적용한다. 

제네릭 타입( Example<T>) 에서는 해당하는 타입 파라미터 (T) 나 Object로 변경해준다. 
Object로 변경하는 경우는 unbounded 된 경우를 뜻하며, 이는 <E extends Comparable<E>>와 같이 bound를 해주지 않은 경우를 의미한다.
따라서 이 소거 규칙에 대한 바이트코드는 제네릭을 적용할 수 있는 일반 클래스, 인터페이스, 메서드에만 해당된다.
타입 안정성 보존을 위해 필요하다면 type casting을 넣어준다.
확장된 제네릭 타입에서 다형성을 보존하기 위해 bridege method를 생성한다.
 


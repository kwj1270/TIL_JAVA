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
클래스 레벨이 아닌 메서드 레벨에서만 `Generics`를 사용하고 싶거나     
클래스 레벨 제네릭과는 다른 타입의 `Generics 타입 매개변수`를 사용하고자 한다면       
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
* **하한 제한이 가능하다.**          
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
**`타입 소거`** 란 **타입정보를** 컴파일 타임에만 유지하고, **런타임에는 삭제시켜 버리는 것**인데                
**과거 제네릭이 없던 버전과의 하위 호환성을 위해서 이러한 작업을 하는 것이다.**         
   
```  
Class 와 같은 리플랙션을 생각하면 더 좋을 수 있다.         
Class 는 제네릭 이전에 Law 타입으로 사용을 했다.        
이후 제네릭이 출현하면서 Class<SomeThing>과 같이 사용하지만,     
     
Class 형태로도 여전히 사용할 수 있다. (이를 Law 타입이라 한다)            
이는 클래스에 제네릭 타입 매개변수를 설정을 해도        
과거 이미 작성된 코드들도 여전히 동작시키도록 만들어주는 것이다.        
```
    
이러한 `타입 소거`를 진행하는 방법은 2가지가 존재하는데      
`재네릭에 선언된 클래스 타입`으로 변현되거나 `Object 타입`으로 변환을 시키는 방법이 있다.                      
         
`<T>`와 같이 일반적인 제네릭 타입 매개변수를 사용할 경우 `Object`타입으로 변형이 되고  
`<T extends SomeThing>`과 같은 바운디드 타입에서는 `SomeThing`형태로 변형이 된다.      


**`<T>`타입소거 전**
```java
// 타입소거 전
class GenericClass <T> {

    public void consume(T paramenter) {
        paramenter.toString();
    }
}
```
**`<T>` 타입소거 후**    
```java
// 타입소거 후
class GenericClass {

    public void consume(Object paramenter) {
        paramenter.toString();
    }
}
```

**`<T extends SomeThing>`타입소거 전**
```java
// 타입소거 전
class GenericClass <T extends SomeThing> {

    public void consume(T paramenter) {
        paramenter.toString();
    }
}
```
**`<T extends SomeThing>` 타입소거 후**    
```java
// 타입소거 후
class GenericClass {

    public void consume(SomeThing paramenter) {
        paramenter.toString();
    }
}
```
    
우리는 타입 소거의 결과에서 한가지 특이점을 알 수 있다.    
바로, Object 클래스 또는 바운디드 타입의 최상단 클래스라는 것을 말이다.   

잠깐 앞에서 언급했던 내용을 돌이켜보면, `primitve` 타입은 제네릭을 사용하지 못한다 했다.               
**그렇다면 왜 primitve 타입은 제네릭에 사용하지 못하는 것일까? 🤔**         
이유는 바로 **타입 소거(Type Erasure) 때문에 그렇다**          
                        
타입 소거로 인하여, 런타임시에 **레퍼런스 타입으로 변환되거나 Object로 변환이 된다.**                 
하지만, `primitive 타입` 같은 경우 **레퍼런스 타입이 아니며, `Object`를 상속받지도 않는다.**             
그렇기 때문에 어떠한 예외 없이 로직을 공통되게 처리해주기 위해서 레퍼런스 타입만을 허용한 것이고        
`primitive`를 사용하기 위해서 **Object를 상속받는 `Wrapper 클래스`** 를 사용하도록 하는 것이다.       
          
추가적으로, JVM 컴파일러는      
타입 소거로 인한 타입 안정성 보존을 위해 `type casting`을 넣어주기도 한다.     
 
```java
Entry<String, Integer> entry = ...;
String key = entry.getKey();
 
// 런타임시에 Object로 변환이 되므로     
// 컴파일러가 타입 castring 을 보장해주는 코드를 넣어준다.      

String key = (String) entry.getKey();
```
타입 소거가 진행되면, `<T>`같은 경우 `Object`로 변환이 되므로             
`HashMap<K,V>`와 같이 특정 자료형을 보장해주는 클래스는 타입 캐스팅이 필수적이다.           
그렇기에 컴파일러는 `type casting`을 진행하여 참조에 문제가 없게끔 도와준다.        
    
또한, JVM 컴파일러는 `bridge 메서드`라는 것을 추가해주기도 한다.    
```java
public interface Comparable{
	public int compareTo(Object o);
}

class Integer implements Comparable{
    private final int value;
    public Integer(int value) { this.value = value; }
    
    public int compareTo(Integer o){
        return (this.value < o.value) ? -1 : (value == o.value ? 0 : 1 );
    }
    
    // 개발자가 명시적으로 작성한 캐스팅을 위한 메서드 
    @Override
    public int compareTo(Object o) {
        return compareTo((Integer)o);
    }
}
```
제네릭이 등장하기 이전에 Comparable 인터페이스 구조는 위와 같다.   
이때, 파라미터는 모든 타입을 허용해야하므로 `Object` 타입으로 지정되어 있다.     
그리고 이 과정에서 `Object`를 알맞는 타입으로 캐스팅하여 사용하기도 했다.     

```java
public interface Comparable<T>{
	public int compareTo(T o);
}    
class Integer implements Comparable<Integer>{
    private final int value;
    public Integer(int value) { this.value = value; }
    
    @Override
    public int compareTo(Integer o){
        return (this.value < o.value) ? -1 : (value == o.value ? 0 : 1 );
    }
}
```       
하지만, 제네릭의 등장하면서 `타입 확정`으로 인하여              
이제는 `Object`가 아닌 특정 타입을 기술하여 사용할 수 있게 되었으며     
이전 코드와 달리, 특정 타입으로 캐스팅하는 메서드 또한 사용하지 않아도 된다.      
        
하지만 여기서 한가지 의문이 발생한다.        
   
> Java는 하위 호환성을 중요시 여기는 언어인데, 위와 예시와 같이      
> Generic 도입에 따라서 메소드 수가 줄어들게 되면,         
> 클래스 구조가 변경되므로 하위 호환성을 유지할 수 있을까?'      
   
일리 있는 생각이다.    
그리고 실제로 JVM Compiler에서는 브릿지 메서드를 지원한다.     
     
```java
public interface Comparable<T>{
	public int compareTo(T o);
}    
class Integer implements Comparable<Integer>{
    private final int value;
    public Integer(int value) { this.value = value; }
    
    @Override
    public int compareTo(Object o) {		// 지정된 타입마다 다르게 변환된다.
        return compareTo((Integer)o);		// 반환형, 캐스팅은 타입에 따라 알맞게 변환된다.   
    }
    
    @Override
    public int compareTo(Integer o){
        return (this.value < o.value) ? -1 : (value == o.value ? 0 : 1 );
    }
}
```   


```java
public class Main {
    public static void main(String[] args) {
        Stream.of(Integer.class.getDeclaredMethods())
                .filter(m -> "compareTo".equals(m.getName()))
                .map(Method::toGenericString)
                .forEach(System.out::println);
    }
}
/* 실핼 결과 

public int java.lang.Integer.compareTo(java.lang.Object)
public int java.lang.Integer.compareTo(java.lang.Integer)

*/
```
실제로 리플랙션을 통해 호출해보면 아래와 같은 결과가 나타난다.   

## 조금 더 생각해보자  
```java
package me.kwj1270.javaapi.test.domain;

import java.util.Arrays;

public class GenericsArrayTest<T> {

    private T[] array;

    public GenericsArrayTest(int size) {
        this.array = (T[]) new Object[size];
    }

    public void addElem(int index, T t) {
        array[index] = t;
    }

    public void printElem() {
        System.out.println(Arrays.toString(array));
    }

    public static void main(String[] args) {
        GenericsArrayTest<String> test = new GenericsArrayTest<>(5);
        test.addElem(0, "안녕하세요");
        test.addElem(1, "감사해요");
        test.addElem(2, "잘있어요");
        test.addElem(3, "다시 만나요");
        test.addElem(4, "-아따 맘마");

        test.printElem();
    }
}
```
제네릭을 이용한 배열 생성과정을 나타낸 코드이다.       
이 코드에서 우리는 한가지 의문점을 가져야 한다.          
바로, `this.array = (T[]) new Object[size];`와 같이         
객체를 생성한 후에 타입을 변환해주어야하기 때문이다.             
      
```java
    public GenericsArrayTest(int size) {
        this.array = new T[size];
    }
```
![]()

실제로 위와 같은 코드를 작성하면 컴파일 에러가 발생한다.         

```java
class MyClass<T>
{
    T field;
    public void myMethod()
    {
       field = new T(); 	// CompilerError : Type parameter 'T' cannot be instantiated directly	
    }
}
```
사실, 배열 뿐만이 아니라 `new 키워드`를 통해 `T`의 객체를 직접 생성할 수는 없다.        
이와 관련되어서 우리는 조금만 더 생각하면 된다.   

`제네릭은 컴파일 타임`에서 동작한다.         
`객체의 생성은 런타임`에서 동작한다.      
           
즉, 컴파일 단계에서 제네릭은 생성되는 객체가 무엇인지 알 수 없다.  
그렇기 때문에, 런타임 단계에서 사용될 수 있도록 `Object` 형태로 객체를 생성하고         
이후 타입 캐스팅을 통해 본래 우리가 사용하고자 하는 객체로 변환하는 것이다.       
      
```java
class Sample<T> {
    static T t;
}
```    
추가로, 제네릭은 `static 변수`에서도 **사용할 수 없다.**            
이유는 간단하다. `static`은 클래스간에 공용으로 사용가능한 요소를 만들기에        
`Sample<String>.t;`이나 `Sample<Integer>.t`와 같이 사용은 할 수 없다.(타입변경)     
    
```java
class Sample {
    
    public static <T> void run(T t){
	//로직 처리//
    }

    public static void main(String[] args) {
        Sample.run(new String("hello"));
    }
    
}
```
신기하게도 `static 메서드`에서는 제네릭이 사용 가능하다.              
단, 조건이 있는데 클래스 레벨이 아닌 메서드 레벨에서의 제네릭만 가능하다.     
    
`static 메서드`에서 메서드 레벨의 제네릭이 사용 가능한 이유는      
우선, 메서드 레벨이기 때문에 변수와 다르게 객체 생성에 영향을 받지 않는다.    
즉, `Sample<String>.run();`가 아닌 `Sample.run(new 자료형());`이기 때문이다.  
그렇기에 클래스 타입과 무관하게 동작할 수 있기에 가능한 것이고         
또한, 메서드의 지역 변수로만 사용하는 것이기에 가능한 것이다.        
     
# 참고   
[자바의 정석 - 서적](http://www.yes24.com/Product/Goods/24259565)         
[윤성우의 열혈 자바 프로그래밍 - 서적](http://www.yes24.com/Product/Goods/43755519)           
[we-hate-jvm Generic](https://github.com/Road-of-CODEr/we-hate-jvm/blob/master/Generic/README.md)    
[스터디 할래 스터디원 hsm622님의 블로그](https://blog.naver.com/hsm622/222251602836)     

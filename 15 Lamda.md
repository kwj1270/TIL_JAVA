# 들어가면서 
> 목표 : 자바의 람다식에 대해 학습하세요.

Lamda
Lamda 사용법
Functional Interface
Variable Capture
메소드, 생성자 레퍼런스

Lamda
Lamda 사용법
Functional Interface
Variable Capture
메소드, 생성자 레퍼런스
     
# Lamda         
자바는 `JDK 8` 에 도입된 `Lamda`로 인해               
**객체지향언어인 동시에 함수형 언어**가 되었다고 평가된다.              
    
**Lamda 사용 예시**
```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random()*5)+1);   // 람다   
```    
`Lamda`는 간략히 말하자면, 메서드를 하나의 식으로 표현한 것이다.          
실제로, 메서드의 이름과 반환값이 없어지는 효과를 볼 수 있다.               
특히, 함수형 인터페이스 자료형을 요구하는 `stream()`과는 찰떡 궁합이다.     
    
## Lamda 등장 이전 (JDK 8 이전)          
`Lamda`에 대해서 소개하기 이전에 `익명 클래스`를 간략히 소개하고자 한다.           
           
**익명클래스**란 이름 그대로 이름이 없는 클래스를 의미한다.         
이름이 존재하지 않기에 재사용하기 어려워 `1회용 클래스`라고도 이야기하기도 한다.     

```java
package me.kwj1270.javaapi.test;


@FunctionalInterface
interface AnonymousInterface {
    public void print();
}

abstract class AnonymousAbstractClass {
    int x = 1;
    int y = 2;

    abstract public void print();

    abstract public void print2();

}

class AnonymousClass {
    int x = 1;
    int y = 2;

    public void print() {
        System.out.println(x);
    }

    public void print2() {
        System.out.println(x);
    }

}

class AnonymousPrintTest {

    public void print(AnonymousClass anonymousClass) {
        System.out.println(anonymousClass.getClass().getName());
        anonymousClass.print();
    }

    public void print(AnonymousAbstractClass anonymousAbstractClass) {
        anonymousAbstractClass.print();
    }

    public void print(AnonymousInterface anonymousInterface) {
        anonymousInterface.print();
    }
}

public class AnonymousClassTest {
    public static void main(String[] args) {
        AnonymousPrintTest anonymousPrintTest = new AnonymousPrintTest();
        
        // 기존에 정의된 일반 클래스를 기준으로 익명 클래스 인스턴스를 생성하여 메서드를 오버라이딩 했다.  
        anonymousPrintTest.print(new AnonymousClass() {

            @Override
            public void print() {
                System.out.println("this is AnonymousClass's method1");
            }

            @Override
            public void print2() {
                System.out.println("this is AnonymousClass's method2");
            }

        });

        // 추상 클래스를 기준으로 익명 클래스 인스턴스를 생성하고 추상 클래스를 상속받아 메서드를 오버라이딩 했다.
        anonymousPrintTest.print(new AnonymousAbstractClass() {

            @Override
            public void print() {
                System.out.println("this is AnonymousClass's method1");
            }

            @Override
            public void print2() {
                System.out.println("this is AnonymousClass's method2");
            }
        });

        // 인터페이스를 기준으로 익명 클래스 인스턴스를 생성하고 인터페이스를 상속받아 메서드를 오버라이딩 했다.
        anonymousPrintTest.print(new AnonymousInterface() {
            @Override
            public void print() {
                System.out.println("this is AnonymousClass");
            }
        });


    }
}
```
위 코드를 보면 3가지 분류로 익명 클래스를 만들었다.  
   
1. 일반 클래스를 기준으로 익명 클래스 인스턴스 생성   
2. 추상 클래스를 기준으로 익명 클래스 인스턴스 생성   
3. 인터페이스를 기준으로 익명 클래스 인스턴스 생성   
       
추상 클래스와 인터페이스는 직접적으로 객체를 생성하지 못한다.           
그렇기에 추상 메서드를 구현해줄 구현 클래스를 만들고 인스턴스로 만들어서 사용해야 한다.          
하지만, 그런 과정이 너무 복잡하고 굳이 불필요한 클래스를 만드는 일이기에      
`익명 클래스`를 이용하여 메서드 호출시 바로 클래스 작성 및 객체를 생성할 수 있도록 도와준다.         
단, 앞서 말했듯이 이름이 없고 참조변수가 없기에 재사용하기는 힘들다.    
   
**여기서 생각나는 의문점**   
추상 클래스와 인터페이스는 이를 구현한 클래스가 필요해서     
익명 클래스로 선언시에 이들을 상속/구현한 클래스를 만든다.     
실제로 리플렉션을 이용해 클래스의 이름을 찍어보면 `$숫자`가 붙는 것을 알 수 있다.   
    
**그렇다면 일반적인 클래스에 대해서도 상속 동작을 하는 것일까? 🤔**   
이 같은 궁금증을 위해 위 코드를 보면 아래와 같은 코드를 추가했다.

* `System.out.println(anonymousClass.getClass().getName());`   

```java
me.kwj1270.javaapi.test.AnonymousClassTest$1
```

그리고 실제로 위와 같은 결과가 나오며,   
일반 클래스라도 이를 상속받아 진행하는 것을 알게 되었다.  
    
<img width="1486" alt="AnonymousClassByteCode" src="https://user-images.githubusercontent.com/50267433/109666175-4bbced80-7bb2-11eb-8d08-72573af24a76.png">   
   
추가로, 바이트 코드를 확인했을 때도 클래스를 상속받아 진행하는 것을 알 수 있다.      
    
## Lamda 등장 (JDK 8 이후)      
익명 클래스로 인하여 불필요한 클래스를 따로 만들지 않고           
그 자리에서 바로바로 생성하여 사용할 수 있게 되었다.                     
하지만, 앞서 보았듯이 코드가 길어지고 가독성이 안 좋아진다는 단점이 있다.       
             
때마침, `JDK 8`부터 병렬처리와 이벤트 지향 프로그래밍을 지원하고자          
`Lamda`가 등장하면서, 익명 클래스의 일정 부분은 간결한 코드로 대체가 가능해졌다.    
       
**Lamda 등장 배경**  
```
하나의 CPU 안에 다수의 코어를 삽입하는 멀터 코어 프로세서들이 등장하면서      
일반 프로그래머에게도 병렬화 프로그램이에 대한 필요성이 생기기 시작했다.        

이러한 추세에 대응하기 위해 
자바8 에서는 병렬화를 위한 컬렉션(배열, List, Set, Map)을 강화했고,    
이러한 컬렉션을 더 효율적으로 사용하기 위해 스트림이 추가되었고   
또 스트림을 효율적으로 사용하기 위해 함수형 프로그램이,    
다시 함수형 프로그래밍을 위해 람다가,   
또 람다를 위해 인터페이스의 변화가수반되었다.   
람다를 지원하기 위한 인터페이스를 함수형 인터페이스라고 한다.  
이를 정리하면 아래와 같다.

빅데이터 지원 -> 병렬화 강화 -> 컬렉션 강화 -> 스트림 강화 -> 
람다 도입 -> 인터페이스 명세 변경 -> 함수형 인터페이스 도입
```

람다의 장단점은 아래와 같다.  
     
**장점**      
1. 코드의 간결성 - 람다를 사용하면 불필요한 반복문의 삭제가 가능하며 복잡한 식을 단순하게 표현할 수 있다.     
2. 지연연산 수행 - 람다는 지연연산을 수행 함으로써 불필요한 연산을 최소화 할 수 있다.     
3. 병렬처리 가능 - 멀티쓰레디를 활용하여 병렬처리를 사용 할 수 있다.     
             
**단점**      
1. 람다식의 호출이 까다롭습니다    
2. `stream()` 에서 람다를 사용할 시에 단순 for문 혹은 while문 보다 성능이 떨어진다.      
3. 불필요하게 너무 사용하게 되면 오히려 가독성을 떨어 뜨릴 수 있다.    

### 코드의 간결성 
**Lamda로 리팩터링 전**
```java
public class AnonymousClassTest {
    public static void main(String[] args) {
        AnonymousPrintTest anonymousPrintTest = new AnonymousPrintTest();

        // 인터페이스를 기준으로 익명 클래스 인스턴스를 생성하고 인터페이스를 상속받아 메서드를 오버라이딩 했다.
        anonymousPrintTest.print(new AnonymousInterface() {
            @Override
            public void print() {
                System.out.println("this is AnonymousClass");
            }
        });


    }
}
```

**Lamda로 리팩터링 후**
```java
public class AnonymousClassTest {
    public static void main(String[] args) {
        AnonymousPrintTest anonymousPrintTest = new AnonymousPrintTest();

        // 인터페이스를 기준으로 익명 클래스 인스턴스를 생성하고 인터페이스를 상속받아 메서드를 오버라이딩 했다.
        anonymousPrintTest.print(()-> System.out.println("this is AnonymousClass"));
    }
}
```
코드가 확연히 줄어든 것을 알 수 있다.   
      
### 람다의 지연 연산과 병렬처리       
람다는 본래 병렬처리를 위해 등장했다고 한다.        
그렇다면 어떠한 이점 때문에 **병렬처리를 위해 람다를 사용하는 것일까? 🤔**         
   
우선, 이와 관련되어서 2개의 사이트에서 해답을 얻을 수 있었다.   
     
* [wedul님의 블로그](https://wedul.site/334)      
* [fitz님의 블로그](https://blog.fitz.software/46)     
    
람다에서는 지연 연산을 지원한다.     
그리고 이는 병렬처리와 매우 관련이 있다.   
    
우선, **지연 연산이란, 해당 로직이 필요한 시점에만 연산을 진행하는 것을 의미한다.**       
그렇다면 우선, 지연 연산을 하지 않는 경우를 알아보자    
     
```java    
logger.info("x : " + x + ", y : " + y);     
``` 
위와 같은 코드가 있을 때 동작 순서는 아래와 같다.  
  
1. `"x : " + x + ", y : " + y`가 연산되어 하나의 문자열로 결합된다.     
2. 결합된 문자열을 `logger.info()` 문자열로 넘긴다.    
    
즉, 메서드를 수행하기 이전에 미리 연산이 진행되고 그 값을 넘긴다.     
하지만, 여기서 우리는 생각할 점이 있다.      
만약, `"x : " + x + ", y : " + y`에서 **RaceCondition이 발생한다면?**       
                  
즉, 하나의 스레드에서는 원래의 `x`와 `y`의 값을 가지는데,             
또 다른 스레드에서는 어떤 로직으로 인하여 변형된 `x`와 `y`의 값을 문자열로 만들 수 있다.           

```java
public static void info(Logger logger, Suppiler<String> str){
    if(logger.isLoggable(Level.INFO))
        logger.info(str.get());
}
```
이 같은 문제를 해결하기 위해 위와 같은 메서드들 정의하고 사용을 하게 된다면    
이전과는 다르게 




생성하기에 버거운 객체의 초기화를 뒤로 미뤄서 초기 로딩 속도를 높이는 방법이 있다.                   
예를 들면 화면을 보여주는데 빨리 보여줄수 있는 텍스트와 무거운 이미지가 있다면,     
텍스트를 먼저 보여주면서 화면을 빠르게 띄우고,      
이미지는 실제 로딩이 필요한 순간에 가져오도록 시킬 수 있다.     
        
사실, `lazy`한 코드를 작성하기 위해서는 더 많은 노력이 들어가며 종종 버그를 만들기도 한다.    
하지만 `Lamda`의 지연 연산을 이용하면 이런 단점들을 효과적으로 커버할 수 있다.         
   
```java
class Heavy {
    public Heavy() {
        System.out.println("Heavy created");
    }

    public String toString() {
        return "quite heavy";
    }
}

//Hodler 클래스
//Holder 클래스는 heavy 클래스를 포함하고 있다.
class Holder {

    private Heavy heavy;

    public Holder() {
        System.out.println("Holder created");
    }

    public Heavy getHeavy() {
        if (heavy == null) {
            heavy = new Heavy();
        }
        return heavy;
    }

}

public class Main {
    public static void main(final String[] args) {
        final Holder holder = new Holder();
        System.out.println("deferring heavy creation...");
        System.out.println(holder.getHeavy());
        System.out.println(holder.getHeavy());
    }

}  
```
 
# Lamda 사용법
추상메서드를 1개만 가진 인터페이스를 기준으로 작성할 수 있으며                     
인터페이스를 구현하고 추상메서드를 정의해서 사용했던 것과 달리                    
**추상 메서드의 내부 로직만을 정의하여 바로 사용할 수 있다.**                     
    
**람다 예시**   
```java
@FunctionalInterface
interface Printable{
    void print(String s);
}

@FunctionalInterface
interface Printable2{
    void print();
}


public class OneParamNoReturn {
    public static void main(String[] args) {
        Printable p;
        Printable2 p2;
        p = (String s) -> { System.out.println(s); };
        p.print("Lambda exp one.");

        p = (String s) ->  System.out.println(s);
        p.print("Lambda exp two.");

        p = (s) -> System.out.println(s);
        p.print("Lambda exp three.");

        p = s -> System.out.println(s);
        p.print("Lambda exp four.");
        
        p2 = () -> System.out.println("NO ARGUMENT");
    }

}
```
   
람다는 기본적으로 아래와 같은 형태를 가진다.  
   
```java
() -> {}
```      
`()`는 메서드의 매개변수 형태를         
`{}`는 메서드의 Body 부분 및 return을 나타낸다.       
return의 경우 추상 메서드의 반환형에 맞추어 마지막 라인을 return 값으로 사용한다.      

그리고 Lamda식은 아래와 같은 문법 또한, 허용한다.   
           
1. `void일 때` 내용이 1개밖에 없을 경우 또는 리턴하는 내용만 있을 경우 `{}`를 생략할 수 있다.            
2. 자료형을 생략할 수 있다. (매개변수 갯수 제한 없음)             
3. 매개변수가 1개일때 `()`를 생략할 수 있다.          
4. 매개변수가 없는 람다일 경우 `()` 만 사용해준다.         
5. `@FunctionalInterface`는 해당 인터페이스가 추상 메서드를 1개만 가지고 있는지 검증해준다. -> 람다식에 부합한지 검사             
   
실제로 위와같이 사용하지 않고 **매개변수로 함수형 인터페이스(람다형 인터페이스)를 가진 메서드에 주로 사용된다.**   
     
**실제 사용 용도**
```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.stream.Collectors;

public class OneParamNoReturn {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);

        List<Integer> list2 = list.stream().filter( intValue -> intValue <= 3 ).collect(Collectors.toList()); // 람다 사용
        List<Integer> list3 = list.stream().map( intValue -> intValue = 3 ).collect(Collectors.toList()); // 람다 사용 

        Iterator<Integer> it =  list2.iterator();
        while (it.hasNext()){
            System.out.print(" list2 : " + it.next()+",");
        }

        System.out.println();

        it =  list3.iterator();
        while (it.hasNext()){
            System.out.print(" list3 : " + it.next());
        }
    }

}
```

# Functional Interface

## 표준 인터페이스 레퍼런스와 구현체
자바에서는 표준으로 정의하고 있는, 즉 '미리 정의된' 함수형 인터페이스들이 있다.      
함수형 인터페이스란 인터페이스의 추상 메서드가 1개인 메서드를 의미한다.      

```java
@FunctionalInterface
interface SampleInterface {
    void sampleAbstractMethod();
}
```   
   
클래스는 `IS-A` 관계로 상속에 있어 물려줄 구성 요소가 적당히 많으면 좋다.       
반대로 인터페이스는 `Has-A` 관계를 가지기에 추상메서드의 갯수가 적을수록 이상적이다.                     
추상메서드를 1개만 이같이 이상적인 형태의 인터페이스를 함수형 인터페이스라 말한다.          
추상메서드가 1개인데 여러개의 default, static 메서드를 가져도 함수형 인터페이스이다.     
참고로 `@FunctionalInterface`를 통해 함수형 인터페이스 정의를 강제할 수 있다.         

   
**본론으로**       
자바에서 표준으로 제공하는 인터페이스는 아래와 같다.      

|인터페이스|추상 메서드|
|-------|--------|
|`Predicate<T>`|`boolaen test(T t)`|
|`Supplier<T>`|`T get()`|
|`Consumer<T>`|`void accept(T t)`|
|`Function<T, R>`|`R apply(T t)`|
    
이런 함수형 인터페이스와 추상형메서드에 대한 사용은 조금 복잡할 수 있다.       
조금 복잡하더라도 양해 부탁드리고 아래 로직을 반복해서 읽기를 바란다.  
물론, 내일중으로 그림을 그려 설명할 예정이다.       
      
```
AbstractClass 의 AbstractMethod 존재      
AbstractMethod 매개변수로 함수형 인터페이스를 원함       
AbstractMethod 내부에서 함수형 인터페이스의 추상 메서드를 사용하기 때문   

이를 이제 구현된 관점에서 보면 아래와 같다.    
AbstractClass 구현한 ConcreteClass
ConcreteClass 클래스이기에 AbstractMethod 구현한 ConcreteMethod 존재   
ConcreteMethod는 AbstractMethod를 따르기에 매개변수로 함수형 인터페이스를 원함     
정확히 말하면 다형성을 위해 함수형 인터페이스를 구현한 클래스를 원함     
함수형 인터페이스를 구현한 클래스는 함수형 인터페이스의 추상 메서드를 구현했고    
ConcreteMethod는 구현된 추상메서드를 사용하기 때문이다.      
```
    
## `Predicate<T>`    
`boolaen test(T t)` 추상 메서드를 가진 함수형 인터페이스다.    
전달된 인자를 판단하여 true/false 값을 리턴하는데 사용된다.   

```java
@FunctionalInterface
public interface Predicate<T> {

    boolean test(T t); // 혼자만 추상형 메서드

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    default Predicate<T> negate() {
        return (t) -> !test(t);
    }

    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }

    static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }
}

```
```java
    Stream<T> filter(Predicate<? super T> predicate);
```
```java
public class Main {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1,2,3,4,5);
        list = list.stream().filter(i -> i <= 3).collect(Collectors.toList());
        list.stream().forEach(System.out::println);
    }
}
```
* `Stream<E>`인터페이스의 `filter()`는 매개변수로 `Predicate<T>`구현 객체를 원한다.  
* `filter()` 내부적으로 `boolaen test(T t)`메서드를 사용하기 때문이다.   
* `filter()` 의 정확한 로직은 모르지만 `true`를 반환하는 요소만 남길 것이다.
   
    
## `Supplier<T>`
`T get()` 추상 메서드를 가진 함수형 인터페이스다.    
매개변수가 없으며 제네릭 타입으로 값을 리턴하는데 사용된다.   


```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```
```java
    // stream에서 예제를 찾기 힘들어 사용자가 정의한 메서드를 예시로 들었다.   
    // 소스코드 출처 : 윤성우의 열혈 java 프로그래밍

    public static List<Integer> makeIntList(Supplier<Integer> s, int n) {
        List<Integer> list = new ArrayList<>();
        for(int i = 0; i < n; i++)
            list.add(s.get());   // 난수를 생성해 담는다.
        return list;
    }
```
```java
public class InterfaceStudy {
    public static List<Integer> makeIntList(Supplier<Integer> s, int n) {
        List<Integer> list = new ArrayList<>();
        for(int i = 0; i < n; i++)
            list.add(s.get());   // 난수를 생성해 담는다.
        return list;
    }

    public static void main(String[] args) {
        Supplier<Integer> spr = () -> {
            Random rand = new Random();
            return rand.nextInt(50);
        };

        List<Integer> list = makeIntList(spr, 5); // Supplier 타입의 매개변수 사용
        System.out.println(list);

        list = makeIntList(spr, 10);
        System.out.println(list);
    }

}
```
* `makeIntList()`는 `Supplier<T>`을 구현한 객체를 원하고 있다.   
* `makeIntList()` 내부적으로 구현된 `T get()`를 사용하기 때문이다.      
* `makeIntList()` 의 정확한 로직은 모르지만 반환된 요소들을 활용할 것이다.   
    
       
## `Consumer<T>`
`void accept(T t)` 추상 메서드를 가진 함수형 인터페이스다.        
반환형이 없으며 제네릭 타입의 매개변수로 들어온 객체를 사용만 한다.      

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);

    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}
```
```java
    void forEach(Consumer<? super T> action);
```
```java
public class InterfaceStudy {

    public static void main(String[] args) {
        List<Integer> arr = Arrays.asList(1, 2, 3, 4, 5);
        arr.stream().forEach(i -> System.out.println(i));
        // arr.stream().forEach(System.out::println);
    }
    
}
```
* `forEach()`는 `Consumer<T>`을 구현한 객체를 원하고 있다.           
* `forEach()` 내부적으로 구현된 `void accept(T t)`를 사용하기 때문이다.           
* `forEach()` 의 정확한 로직은 모르지만 인자로 들어온 요소를 활용하고 반환은 없다.       

참고로 `Consumer<>`를 구현한 객체를 원하는 경우, 메서드 레퍼런스를 사용하는 경우가 있다.        
* 메서드 레퍼런스란? :
  * 람다식이 메서드 하나만 호출하는 경우에 사용하는 방법으로 람다식을 더 간략히 해준다.      
  * `Consumer<>`의 `accept(T t)`메서드에 람다식을 정의할 때            
  * 매개변수를 다른 함수의 매개변수로 사용하고 해당 로직만 기술한다고 가정한다면          
  * 람다식이 하나의 메서드만 호출하는 경우이므로 메서드 레퍼런스로 기술할 수 있다. 
   
     
## `Function<T, R>`      
`R apply(T t)` 추상 메서드를 가진 함수형 인터페이스다.                 
제네릭의 첫 번째 타입이 매개변수의 자료형이며 두 번째 타입이 반환형인 특징이 있다.          
즉, 첫 번째 제네릭 타입을 통해 두 번째 제네릭 타입을 이끌어내는 경우에 사용한다.      
   
```java
@FunctionalInterface
public interface Function<T, R> {

    R apply(T t);

    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    static <T> Function<T, T> identity() {
        return t -> t;
    }
}

```
```java
    <R> Stream<R> map(Function<? super T, ? extends R> mapper);
```
```java
public class InterfaceStudy {

    public static void main(String[] args) {
        List<Integer> arr = Arrays.asList(1, 2, 3, 4, 5);
        List<String> strArr = arr.stream().map(i -> String.valueOf(i)).collect(Collectors.toList());
        strArr.stream().forEach(System.out::println);
    }

}
```
* `map()`는 `Function<T, R>`을 구현한 객체를 원하고 있다.           
* `map()` 내부적으로 구현된 `R apply(T t)`를 사용하기 때문이다.           
* `map()` 의 정확한 로직은 모르지만 인자로 들어온 요소를 활용하고 알맞은 반환을 한다.    
    
`Function<T, R>`의 `R apply(T t)`를 사용하는 가장 대표적인 예는 `map()`이다.   
`map()`은 인자로 들어온 값을 특정 자료형으로 반환하는 특징이 있다.        
즉, 매개변수를 활용해 알맞은 자료형의 데이터를 반환한다는 것이다.        

# Variable Capture
# 메소드, 생성자 레퍼런스
# 참고 
[KTKO 개발 블로그와 여행 일기 - 자바 람다와 함수형 인터페이스](https://ktko.tistory.com/entry/자바-18-버전-특성람다-인터페이스-부분-정리해보기)    


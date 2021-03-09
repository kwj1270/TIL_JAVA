# 들어가면서 
> 목표 : 자바의 람다식에 대해 학습하세요.
  
[1. Lambda](#lambda)           
[2. Lambda 사용법](#lambda-사용법)             
[3. Functional Interface](#functional-interface)        
[4. Variable Capture](#variable-capture)         
[5. 메소드, 생성자 레퍼런스](#메소드생성자-레퍼런스)      
              
# Lambda         
자바는 `JDK 8` 에 도입된 `Lambda`로 인해               
**객체지향언어인 동시에 함수형 언어**가 되었다고 평가된다.              
    
**Lambda 사용 예시**
```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random()*5)+1);   // 람다   
```    
`Lambda`는 간략히 말하자면, 메서드를 하나의 식으로 표현한 것이다.          
실제로, 메서드의 이름과 반환값이 없어지는 효과를 볼 수 있다.               
특히, 함수형 인터페이스 자료형을 요구하는 `stream()`과는 찰떡 궁합이다.     
    
## Lambda 등장 이전 (JDK 8 이전)          
`Lambda`에 대해서 소개하기 이전에 `익명 클래스`를 간략히 소개하고자 한다.           
           
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
    
## Lambda 등장 (JDK 8 이후)      
익명 클래스로 인하여 불필요한 클래스를 따로 만들지 않고           
그 자리에서 바로바로 생성하여 사용할 수 있게 되었다.                     
하지만, 앞서 보았듯이 코드가 길어지고 가독성이 안 좋아진다는 단점이 있다.       
             
때마침, `JDK 8`부터 병렬처리와 이벤트 지향 프로그래밍을 지원하고자          
`Lambda`가 등장하면서, 익명 클래스의 일정 부분은 간결한 코드로 대체가 가능해졌다.    
       
**Lambda 등장 배경**  
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
**Lambda로 리팩터링 전**
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

**Lambda로 리팩터링 후**
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
   
우선, 이와 관련되어서 아래의 사이트에서 해답을 얻을 수 있었다.   
      
* [tourspace님의 블로그](https://tourspace.tistory.com/76)

람다에서는 지연 연산을 지원한다.     
그리고 이는 병렬처리와 매우 관련이 있다.   
    
우선, **지연 연산이란, 해당 로직이 필요한 시점에만 연산을 진행하는 것을 의미한다.**       
그리고 아래와 같은 상황일 때 지연 연산을 사용한다.    
    
- 별도의 스레드에서 코드 실행
- 코드를 여러번 실행
- 적절한 시점에 실행
- 어떤 일이 발생(이벤트)했을 때 실행
   
그리고, **생성하기에 버거운 객체의 초기화를 미뤄서 초기 로딩 속도를 높이는 방법이 있다.**                     
예를 들면 화면을 보여주는데 빨리 보여줄수 있는 텍스트와 무거운 이미지가 있다면,     
텍스트를 먼저 보여주면서 화면을 빠르게 띄우고,   
이미지는 실제 로딩이 필요한 순간에 가져오도록 시킬 수 있다.       
               
사실, `lazy`한 코드를 작성하기 위해서는 기존 코드보다 더 많은 노력이 들어간다.          
하지만 **`Lambda`의 지연 연산을 이용하면 이런 단점들을 효과적으로 커버할 수 있다.**           
        
```java
import java.util.function.Supplier;

class Heavy {
    public Heavy() {
        System.out.println("Heavy created");
    }

    public String toString() {
        return "quite heavy";
    }
}

class Holder {
    private Supplier<Heavy> heavy = () -> createAndCacheHeavy();

    public Holder() {
        System.out.println("Holder created");
    }

    public Heavy getHeavy() {
        return heavy.get();
    }

    private synchronized Heavy createAndCacheHeavy() {
        class HeavyFactory implements Supplier<Heavy> {
            private final Heavy heavyInstance = new Heavy();

            public Heavy get() {
                return heavyInstance;
            }
        }
        if (!HeavyFactory.class.isInstance(heavy)) {
            heavy = new HeavyFactory();
        }
        return heavy.get();
    }

    public static void main(final String[] args) {
        final Holder holder = new Holder();
        System.out.println("deferring heavy creation...");
        System.out.println(holder.getHeavy());
        System.out.println(holder.getHeavy());
    }
}
```
`heavy`란 멤버변수는 람다식이므로 `get()`을 호출하기 전까지는 수행이 지연된다.   
`getHeavy()`에서 `heavy.get()`을 호출하면      
그때 `createAndCacheHeavy()`란 함수가 수행된다.     
`createAndCacheHeavy()` 함수는 `synchronized` 라서 `thread-safe`하다.    
   
1. `heavy.get()`이 호출된다.   
2. `createAndCacheHeavy()`함수가 호출된다.   
3. `if문`으로 `HeavyFactory`의 `instance`인지를 확인하는 과정에서       
    `HeavyFactory`가 언급되면서 `heavyIsntance`가 생성된다.    
4. 따라서 `heavy = new HeavyFactory()`가 수행된다.      
   (`heavy`에는 `HeavyFactory` 객체가 담긴다.)   
6. `HeavyFactory`의 `get()`함수가 호출되면서 `Heavy`의 `instance`가 `return`된다.    
     
**두번째 getHeavy()를 호출하면 어떻게 될까?**      
1. `heavy.get()`이 불립니다.    
2. 단. 위 4번에서 **이미 heavy 변수에는 HeavyFactory 객체를 담았다.**   
3. 따라서 `HeaveFactory의 get()`이 불리면서 `Heavy`객체가 반환된다.   
       
이렇게 구조를 만드면 **처음 생성시에만 `synchronized된 함수를 사용하고`**          
그 이후에는 **일반함수를 호출하는 형태가 된다.**         
그리고 이와 같은 형식을 `Virtual Proxy Pattern`이라고 부른다.    

# Lambda 사용법
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

그리고 Lambda식은 아래와 같은 문법 또한, 허용한다.   
           
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
`Variable Capture`란 `Lambda`의 익명 클래스 생성을 활용한 방법으로         
`Lambda` 외부에서 참조변수를 통해 `Lambda`를 통해 생성된 익명클래스를 참조한다.   
       
하지만, `Variable Capture` 에는 한 가지 조건이 있다.            
바로, `Lambda` 식에서 사용하는 캡처된 변수들은 `effectively final`이어야 한다.            
              
```
캡처된 변수란?        
중첩 된 클래스에서 사용할 수 있도록 복사 된 변수를 뜻한다.              
즉, 매개변수로 들어온 복사된 값이 아닌 실제 값을 사용한다고 생각하면 된다.         
```

보다 정확히 설명하면, 익명 클래스는 그들을 둘러싼 영역(메서드)의 지역 변수를 캡처할 수 있다.           
하지만, 캡처된 지역 변수는 해당 메서드 스택 영역에만 존재하는 지역 변수이며            
`JVM 메모리 구조 특성상 스레드간의 공유가 되지 않는다`는 문제가 있다.     
    
* 서로 다른 스레드끼리 지역변수를 공유할 수 없다. 
                         
이러한 이유에 대해서도 설명하자면                    
스레드에서 `해당 영역(메서드)`이 종료되면 `지역 변수` 또한, 메모리에 `쓰레기 값으로 남겨진다.`               
하지만, 다른 스레드에 `공유된 객체`가 동작을 할 때 `해당 지역 변수`를 사용한다면?                                   
**`공유된 객체`는 해당 지역 변수의 메모리를 가지고 `접근할 수 있다는 문제가 발생`한다.**                  
              
또한, 스택이 해제되면서 해당 `지역 변수가 존재했던 메모리 영역` 또한,            
언제든 **새로운 메모리로 할당되어 값이 변형될 수 있기에**                      
**`공유된 객체`도 항상 일정한 로직을 실행한다는 보장을 할 수 없다.**                 
                  
그렇기에 스레드간의 지역 변수를 공유하지 못하도록 한 것이며       
이는 다른 프로그래밍 언어에서도 마찬가지이다.      
     
반대로, 객체나 정적 요소들은 왜 사용이 가능한 것인가?        
이는 메모리 구조에 대한 연장선으로 이들은 `클래스 영역`이나 `힙 영역`에 존재한다.     
이들은 스레드들 간에 공유를 해도 문제가 없기에 사용을 가능하도록 설정한 것이다.    
   
**익명 클래스**
```java
   JButton button = new JButton("Press me!");
   String message = "Never been pressed";
   button.addActionListener(new ActionListener() {

      @Override
      public void actionPerformed(ActionEvent e) {
         message = "Pressed";
      }
          
   });
```
**결과**    
```java
Cannot refer to a non-final variable message inside an inner class defined in a different method
```  

다시 본론으로 돌아와서        
Java 컴파일러는 `외부` 변수와 `내부` 변수 값 사이의 불일치를 피하기 위해      
캡처된 모든 변수가 `effectively final`임을 확인을 하고 아닐 경우, 에러를 발생시킨다.                 
                          
또한, Java는 로컬 변수의 **복사본을 만들고** 내부 클래스에 대한 **참조로 제공**한다.          
그리고 두 복사본 간의 일관성을 유지하기 위해 지역 변수는 `final`로 제한을 두었다.             

하지만, 앞서 `effectively final`임을 확인한다고 했다.      
**그렇다면 `effectively final`는 무엇일까? 🤔**   
              
**`effectively final`이란** Java 8 에 추가된 syntactic sugar 일종으로,     
**초기화 된 이후 값이 한번도 변경되지 않은 것을 의미한다.**                       
`effectively final`변수는 `final` 키워드가 붙어있지 않았지만 값이 변형되지 않았기에    
**`final` 키워드를 붙힌 것과 동일하게 컴파일러에서 처리한다.**           
그리고 이러한 특성 덕분에 람다/익명 클래스에서 무리없이 사용이 가능하다.   


## Local Variable Capture  
`Lambda 표현식`은 본문 외부에 존재하는 `지역 변수`를 캡처할 수 있다.         
    
```java
public interface MyFactory {
    public String create(char[] chars);
}
```
```java
String myString = "Test";

MyFactory myFactory = (chars) -> {
    return myString + ":" + new String(chars);
};
```


## Instance Variable Capture   
`Lambda 표현식`은 `인스턴스 참조 변수`를 캡처할 수 있다.       

```java
public class EventConsumerImpl {

    private String name = "MyConsumer";

    public void attach(MyEventProducer eventProducer){
        eventProducer.listen(e -> {
            System.out.println(this.name);
        });
    }
}
```
캡처 후 인스턴스 변수의 값을 변경할 수도 있으며 값은 람다 내부에 반영된다.


## Static Variable Capture    
`Lambda 표현식`은 `static 변수`를 캡처할 수 있다.        
      
```java
public class EventConsumerImpl {
    private static String someStaticVar = "Some text";

    public void attach(MyEventProducer eventProducer){
        eventProducer.listen(e -> {
            System.out.println(someStaticVar);
        });
    }
}
```
그리고, `static 변수`의 값은 람다가 캡처 한 후에도 변경할 수 있다.   


# 메소드/생성자 레퍼런스
## 메서드 레퍼런스 
람다식이 하나의 메서드만 호출하는 경우에 사용하는 방법으로 람다식을 더 간략히 해준다.     
아래는 필자가 정리해본 메서드 래퍼런스 사용가능 영역에 대해서 나눈 것이다.  


1. static 메서드 레퍼런스  
2. 동일 영역 인스턴스의 메서드
3. 외부 영역 인스턴스의 메서드 
      
|종류|람다|메서드 참조|       
|---|---|--------|   
|static 메서드 레퍼런스|(x) -> ClassName.method(x)|`클래스이름::메서드`|        
|동일 영역 인스턴스의 메서드|(o1, o2) -> o1.method(o2)|`클래스이름:메서드`|        
|외부 영역 인스턴스의 메서드|(x)-> extObj.method(x)|`인스턴스변수::메서드`|         

위 테이블을 필자 나름대로 해석하면 이렇다.   
        
* 인스턴스 메서드일 경우, 인스턴스 상태에서 메서드 레퍼런스를 진행한다.    
* 단, 동일 영역에서 다른 인자값을 매개변수로 가지는 경우 클래스로 진행한다.     
* static 메서드는 인스턴스를 생성하지 안고도 사용되어야 하므로 클래스로 진행한다.    
  
     
### static 메서드 레퍼런스         
**람다식 코드**
```java
import java.util.function.Function;

public class StaticTest {
    public static void main(String[] args) {
        Function<String, Integer> f = s -> Integer.parseInt(s);
        System.out.println(f.apply("2").getClass().getName());
    }
}
```

**메서드 참조 코드**   
```java
import java.util.function.Function;

public class ArrangeList {
    public static void main(String[] args) {
        Function<String, Integer> f = Integer::parseInt;
        System.out.println(f.apply("2").getClass().getName());
    }
}
```  
`Integer`의 `parseInt()`는 `static 메서드`이다.         
이 같은 `static 메서드`는 `클래스이름 ::메서드`형태로 메서들 레퍼런스를 작성하면 된다.        
추가로, 우리가 가장 쉽게 접할 수 있는 메서드 레퍼런스 유형이지 않을까 싶다.         
   
### 동일 영역 인스턴스의 메서드 레퍼런스 
**람다식 코드**
```java
import java.util.function.ToIntBiFunction;

class MyBox{
    int number;
    public MyBox(int number){
        this.number = number;
    }
    public int lager(MyBox other){
        if(this.number >= other.number) return this.number;
        return other.number;
    }
}

public class ToIntBiFunctionTest {
    public static void main(String[] args) {
        MyBox myBox1 = new MyBox(1);
        MyBox myBox2 = new MyBox(2);

        ToIntBiFunction<MyBox, MyBox> bf = (box1, box2) -> box1.lager(box2);
        System.out.println(bf.applyAsInt(myBox1, myBox2));
    }

}
```  

**메서드 레퍼런스 코드**   
```java
import java.util.function.ToIntBiFunction;

class MyBox{
    int number;
    public MyBox(int number){
        this.number = number;
    }
    public int lager(MyBox other){
        if(this.number >= other.number) return this.number;
        return other.number;
    }
}

class ToIntBiFunctionTest {
    public static void main(String[] args) {
        MyBox myBox1 = new MyBox(1);
        MyBox myBox2 = new MyBox(2);

        ToIntBiFunction<MyBox, MyBox> bf = MyBox::lager;
        System.out.println(bf.applyAsInt(myBox1, myBox2));
    }

}
```
동일 영역에 있는 인자값을 사용하는 메서드 레퍼런스 같은 경우     
이미 인스턴스로 생성을 했다 하더라도 `클래스이름::메서드` 형태로 작성해야한다.     
    
### 외부 영역 인스턴스의 메서드 레퍼런스   
**람다식 코드**
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.function.Consumer;

class JustSort{
    public void sort(List<?> list){
        Collections.reverse(list);
    }
}

public class ArrangeList3 {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1,3,5,7,9);
        list = new ArrayList<>(list);
        JustSort justSort = new JustSort();

        Consumer<List<Integer>> c = e -> justSort.sort(e);
        c.accept(list);
        System.out.println(list);
    }
}
```

**메서드 참조 코드**   
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.List;
import java.util.function.Consumer;

class JustSort{
    public void sort(List<?> list){
        Collections.reverse(list);
    }
}

public class ArrangeList3 {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1,3,5,7,9);
        list = new ArrayList<>(list);
        JustSort justSort = new JustSort();

        Consumer<List<Integer>> c = justSort::sort;
        c.accept(list);
        System.out.println(list);
    }
}
```   
외부 영역에 존재하는 인스턴스를 사용할 시에 `인스턴스::메서드` 형태로 작성한다.           
물론, 메서드가 `static`이면 `클래스::메서드`형태로 작성을 해도 된다.           
하지만, 이는 위해서 설명한 유형이고 인스턴스 메서드일 경우 꼭 인스턴스를 만들어 사용해야한다.        
    
## 생성자 레퍼런스    
    
생성자도 특정 조건이 갖추어졌을 경우 생성자 레퍼런스를 활용할 수 있다.        
또한, 매개변수가 없는 생성자도 똑같은 방법으로 만들 수 있다.             
여기서도 알아둘 점은 람다식에 사용하는 코드가 생성자 1개 뿐일 때 가능하다.   

**람다식 코드**
```java
import java.util.function.Function;

public class StringMaker {
    public static void main(String[] args) {
        Function<char[], String> f = ar -> new String(ar);

        char[] src = {'R', 'o', 'b', 'o', 't'};
        String str = f.apply(src);

        System.out.println(str);
    }
}
```

**생성자 레퍼런스**     
```java
import java.util.function.Function;

public class StringMaker {
    public static void main(String[] args) {
        Function<char[], String> f = String::new;

        char[] src = {'R', 'o', 'b', 'o', 't'};
        String str = f.apply(src);

        System.out.println(str);
    }
}
```
코드에서 보이는 것처럼 `클래스이름::new`라는 형태로 작성해주면 된다.  
메서드 레퍼런스와 마찬가지로, 매개변수의 개수 및 형태가 일치해야한다.  

```java
Function<Integer, int[]> f = x -> new int[x];
Function<Integer, int[]> f = int[]::new;
```
배열을 생성할 수도 있는데, 이때는 `자료형[]::new` 형태로 작성해주면 된다.     
    
## System.out.println    

```java
import java.util.Arrays;
import java.util.List;

public class ArrangeList {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("BOX", "ROBOT");
        list.forEach(s -> System.out.println(s));
        list.forEach(System.out::println);
    }
}
```
참고로 `System 클래스` 내에 존재하는 `out`은 `인스턴스 참조 변수` 이다.      
`printStream` 인스턴스를 참조하고 있으며 이를 통해, `print-`메서드를 사용할 수 있던 것이다.       
`out`을 통해 `print-`메서드를 사용하는 경우 `인스턴스를 통한 인스턴스 메서드 참조`가 성립된다.   
그렇기에 `System.out.println()` 에 대해서도 메서드 참조가 가능하다.       
    

# 참고 
[KTKO 개발 블로그와 여행 일기 - 자바 람다와 함수형 인터페이스](https://ktko.tistory.com/entry/자바-18-버전-특성람다-인터페이스-부분-정리해보기)       
[stackoverflow-What are captured variables in Java Local Classes](https://stackoverflow.com/questions/22025161/what-are-captured-variables-in-java-local-classes)   
[vagabond95님의 블로그 - lambda 와 effectively final](https://vagabond95.me/posts/lambda-with-final/)   

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

람다의 장점이라고 말하자면,

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










# Lamda 사용법
# Functional Interface
# Variable Capture
# 메소드, 생성자 레퍼런스
# 참고 
[KTKO 개발 블로그와 여행 일기 - 자바 람다와 함수형 인터페이스](https://ktko.tistory.com/entry/자바-18-버전-특성람다-인터페이스-부분-정리해보기)    


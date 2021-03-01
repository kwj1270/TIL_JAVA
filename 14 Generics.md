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
     
이를 코드로 표현해서 간략히 얘기하면 아래와 같다.     
   
```java 
class Parent {
    
    public void print() {
        System.out.println("100만원 출금")    
    }
    
}

class Child extends Parent {                             // Parent를 final로 선언할 수 있지만, 다른 하위 클래스를 사용해야하는 경우에는?   
                                                         // 그리고 이러한 상속에는 제한이 없어 개발자도 모르게 안 좋은 클래스르 만들 수 있다.  
    @Override
    public void print() {
        System.out.println("10만원 출금")    
    }
    
}

class SomeThing {
    
    public static void main(String[] args) {
        Parent parent = new Parent();                    
        sampleMethod(parent);
        Child child = new Child();                       
        sampleMethod(child);                             // child 타입이 들어갔지만, 컴파일 에러가 발생하지 않는다.    
    }
    
    private static void sampleMethod(Parent parent) {    // Parent 타입이기에 자식 클래스인 Child도 올 수 있다.   
        parent.print();                                  // super 100만원을 출금해야하는데 10만원이 출금되면?      
    }
}
```


# Generics 사용법       
# Generics 주요 개념 (바운디드 타입, 와일드 카드)    
# Generics Method   
# Type Erasure    

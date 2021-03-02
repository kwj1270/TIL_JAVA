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
new AnonymousClass {
    
}
```




# Lamda 사용법
# Functional Interface
# Variable Capture
# 메소드, 생성자 레퍼런스

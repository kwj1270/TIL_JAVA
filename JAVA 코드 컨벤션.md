# JAVA 코드 컨벤션 

# 왜 코드 컨벤션이 필요할까?   
> 코딩 규칙은 여러가지 이유에서 프로그래머에게 중요하다    
   
* 소프트웨어를 개발하는 일련의 모든 과정에 들어가는 비용 중 80%가 유지보수에 쓰여진다.      
* 소프트웨어의 직접 개발한 개발자가 그 소프트웨어의 유지보수를 담당하는 경우는 거의 보기 힘들다.      
* 코딩 규칙을 지키면 가독성이 높아져 다른 개발자가 소스 코드를 처음 보더라도 더 빠르고 완벽하게 이해할 수 있게된다.  
* 자신의 소스 코드를 제품으로 팔려고 한다면, 자신의 다른 소스 코드들과 잘 어울리도록 패키지(package)를 적절하게 구성할 필요가 있다.   

# 파일 이름
## 확장자
|파일형태|확장자|
|------|----|
|자바 소스|.java|
|자바 바이트코드|.class|

## 공통으로 시작되는 파일 이름
|파일 이름|사용|
|-------|---|
|GNUmakefile make| 파일 이름으로 사용, 소프트웨어를 빌드할 때는 gnumake 명령어를 사용|
|README|특정 디렉토리의 내용을 요약하는 파일 이름으로 사용|

# 파일 구조
* 파일은 빈 줄이나 다른 구역임을 나타내주는 주석으로 나누어지는 여러 구역(section)들로 구성되어 있다.
* 2,000 라인을 넘는 파일은 이해하기가 쉽지 않기 때문에 될 수 있으면 피해야 한다.

# 자바 소스 파일
* 시작 주석
* Package 문과 Import 문
* Class와 Interface 선언

## 시작 주석
> 모든 소스 파일은 클래스 이름, 버전 정보, 날짜, 저작권 주의를 보여주는 C 스타일의 주석과 함께 시작한다.

```java
/*
 * 클래스 이름 
 * 
 * 버전 정보 
 * 
 * 날짜 
 * 
 * 저작권 주의 
 */
```

## Package 문과 Import 문
> 자바 소스 파일의 첫 번째 줄은 Package 문이다. 그 후에 Import 문이 뒤따라 나온다.

```java
package java.code.convention;

import java.util.*;
```

## Class와 Interface 선언

|순서|클래스/인터페이스 <br> 선언의 구성요소|설명|
|---|-------------------------|----|
|1|문서화 주석 <br>`(/** .. */)`|이 주석이 포함하는 정보들은 아래 "5장의 2절 문서화(Documentation) 주석"을 참고|
|2|클래스/인터페이스 문||	 
|3|구현 주석 <br>`(/* .. */)`|이 주석은 클래스/인터페이스 문서화 주석에 적합하지 않은 하나의 클래스/인터페이스에만 해당하는 정보들을 포함해야 한다.|	 
|4|클래스(static) 변수|`public` -> `protected` -> `package` -> `private`|
|5|일반 변수|`public` -> `protected` -> `package` -> `private`|
|6|생성자||	 
|7|메서드|메서드들은 범위나 접근성을 기준으로 나누기 보다는 기능성에 의해서 구성되어야 한다.<br> 예를 들어, private 클래스 메서드가 두 개의 public 메서드들 사이에 존재할 수도 있다.<br> 이렇게 하는 목적은 코드가 더 쉽게 읽히고, 더 쉽게 이해되도록 돕기 위해서이다.<br> 쉽게 얘기하면 호출하는 메서드와 호출되는 메서드는 가까이 붙는 것이 좋다.|

# 들여쓰기 
* 4개의 빈 칸(space)를 들여쓰기 단위로 사용한다. 
* 들여쓰기의 정확한 구현(빈 칸을 사용하거나 탭을 사용하거나)은 정해져 있지 않다. 
* 탭은 4개의 빈 칸이 아니라, 8개의 빈 칸으로 설정하는 것이 좋다. 
 
## 한 줄의 길이
* 한 줄에 80자 이상 쓰는 것은 대부분의 터미널(terminal)과 툴에서 다룰 수 없기 때문에 피해야 한다.
* 문서화 주석을 가지고 문서를 만들 때, 일반적으로 한 줄에 70자 이상을 가지지 않는다.

## 2 줄 나누기
> 하나의 식이 한 줄에 들어가지 않을 때에는, 다음과 같은 일반적인 원칙들을 따라서 두 줄로 나눈다.

* 콤마 후에 두 줄로 나눈다. 
* 연산자(operator) 앞에서 두 줄로 나눈다.
* 레벨이 낮은 원칙보다는 레벨이 높은 원칙에 따라 두 줄로 나눈다.
* 앞줄과 같은 레벨의 식(expression)이 시작되는 새로운 줄은 앞줄과 들여쓰기를 일치시킨다.
* 만약 위의 원칙들이 코드를 더 복잡하게 하거나 오른쪽 끝을 넘어간다면, 대신에 8개의 빈 칸을 사용해 들여쓴다.

```java
longName1 = longName2 * (longName3 + longName4 - longName5)
        + 4 * longname6; // 될 수 있으면 이 방법을 사용한다. 
        
longName1 = longName2 * (longName3 + longName4 
                       - longName5) + 4 * longname6; // 될 수 있으면 피한다.
```
* 첫번째 예제가 괄호로 싸여진 표현식 밖에서 줄 바꿈이 일어나고 더 높은 레벨이기 때문에 첫번째 예제를 더 많이 사용한다.

```java
// 일반적인 들여쓰기 
someMethod(int anArg, Object anotherArg, String yetAnotherArg, 
        Object andStillAnother) { 
    ... 
} // 너무 멀리 들여쓰는 것을 피하기 위해 8개의 빈 칸으로 들여쓰기 

private static synchronized horkingLongMethodName(int anArg, 
        Object anotherArg, String yetAnotherArg, 
        Object andStillAnother) { 
    ... 
}
```
* 블록을 진행하기 전에 일반적인 들여쓰기를 사용한다면 4개의 빈칸 대신에, 8개의 빈 칸을 이용하여 들여쓴다.

```java
// 아래와 같은 들여쓰기는 사용하지 말자. 
if ((condition1 && condition2) 
    || (condition3 && condition4) 
    ||!(condition5 && condition6)) { // 좋지 않은 들여쓰기 
    doSomethingAboutIt(); // 메서드 본문 시작이 명확하지가 않다. 
} 

// 대신에 아래와 같은 들여쓰기를 사용한다. 
if ((condition1 && condition2) 
        || (condition3 && condition4) 
        ||!(condition5 && condition6)) { 
    doSomethingAboutIt(); 
} 

// 또는 아래와 같은 들여쓰기를 사용한다. 
if ((condition1 && condition2) || (condition3 && condition4) 
        ||!(condition5 && condition6)) { 
    doSomethingAboutIt(); 
}
```
* 블록을 진행하기 전에 일반적인 들여쓰기를 사용한다면 4개의 빈칸 대신에, 8개의 빈 칸을 이용하여 들여쓴다.

```java
alpha = (aLongBooleanExpression) ? beta : gamma; 

alpha = (aLongBooleanExpression) ? beta 
                                 : gamma; 

alpha = (aLongBooleanExpression) 
    ? beta 
    : gamma;
```
* 삼항식(ternary expression)에서는 세 가지 방법 전부 사용 가능하다.    
    
# 주석 
  
**구현 주석 :**      
* `//` or `/*...*/`   
* 구현 주석은 각각의 구현에 대한 추가적인 설명이 필요할 때, 또는 코드를 주석 처리할 때 사용할 수 있다.    
   
**문서화 주석 :**       
* `/** ... */`      
* javadoc 툴을 사용하면 문서화 주석을 포함하는 HTML 파일을 자동으로 만들 수 있다.        
* 또한 문서화 주석은 소스 코드가 없는 개발자들도 읽고 이해할 수 있도록,           
실제 구현된 코드와는 상관이 없는 코드의 명세 사항(specification)을 포함한다.          

주석은 코드에 대한 개요와 코드 자체만 가지고는 알수 없는 추가적인 정보들을 제공하기 위해 사용되어야 한다.        
주석은 프로그램을 읽는 것과 이해하는 것에 관계된 정보만을 포함하고 있어야 한다.         
예를 들어, '패키지를 어떻게 만들 것인가?' 또는 '파일들을 어느 디렉토리에 위치시킬 것인가?'에 대한 정보는 주석에 포함되어서는 안된다.       

중요하거나 코드만으로는 명확하지 않은 프로그램 설계에 대한 추가 설명을 포함하는 것은 적절하지만,      
그러나 코드 상에 이미 표현되어 있는 중복 정보는 피해야 한다.        

중복된 주석을 작성하는데 많은 시간을 허비해서 계획된 시간안에 프로그램을 완성하지 못하는 경우도 발생한다.         
일반적으로 코드가 계속 발전하면 더이상은 유효하지 않을 것 같은 정보들은 주석에서 제외하는 것이 더 나은 방법이다.    
     
주석을 별표(`*`) 또는 다른 문자를 사용하여 그린 큰 사각형에 넣는 방법은 피하도록 하자.     
주석은 폼 피드(form-feed: 프린터에서 용지 바꿈)나 백스페이스(backspace)와 같은 특수 문자를 포함해서는 안 된다.       

```
불필요한 주석은 코드의 품질이 낮다는 것을 증명하고 가독성을 낮추는 것은 물론, 코드와 같이 관리해야한다는 번거로움이 생긴다.    
주석을 추가해야 한다고 느낄 때, 코드를 좀 더 명확하게 다시 작성하는 것을 고려해 보는 것이 좋다.    
```   

## 구현 주석 
> 프로그램은 다음과 같은 4가지 형식의 구현 주석을 포함할 수 있다.
     
* 블록(Block) 주석    
* 한 줄(Single-Line) 주석    
* 꼬리(Trailing) 주석    
* 줄 끝(End-Of-Line) 주석   

### 블록(Block) 주석
> 메서드, 자료구조, 알고리즘에 대한 설명을 제공할 때 사용된다.

```java
/*
 * 여기에 블록 주석을 작성한다.   
 */
```
* 각각의 파일이 시작될 때와 메서드 전에 사용된다.
* 메서드 안에서와 같이 다른 장소에서 사용될 수도 있다.
  * 메서드 안에 존재하는 블록 주석은 주석이 설명하는 코드와 같은 단위로 들여쓰기를 해야한다.
* 블록 주석은 다른 코드들과 구분하기 위해서 처음 한 줄은 비우고 사용한다. (위 코드에서 보면 2번째에 시작)   

```java
/*- 
 * 여기에 들여쓰기를 무시하는 특별한 
 * 블록 주석을 작성한다. 
 * 
 *     one 
 *         two 
 *             three 
 */

```
* 블록 주석의 들여쓰기를 다시 고치지 못하도록 하기 위한 특별한 블록 주석은 `/*-` 으로 시작할 수 있다.

### 한 줄(Single-Line) 주석       
> 짧은 주석은 뒤따라 오는 코드와 같은 동일한 들여쓰기를 하는 한 줄로 작성할 수 있다.    

```java
if (condition) { 

    /* 이 조건을 만족하면 실행된다. */ 
    ... 
}
```
* 만약 주석이 한 줄에 다 들어가지 않는다면, 블록 주석 형식을 따라야 한다. 
* 한 줄 주석은 빈 줄로 시작되어야 한다.

### 꼬리(Trailing) 주석    
> 아주 짧은 주석이 필요한 경우 주석이 설명하는 코드와 같은 줄에 작성한다.

```java
if (a == 2) {
    return TRUE;            /* 특별한 경우 */
} else {
    return isPrime(a);      /* a 가 홀수인 경우 */
}
```
* 실제 코드와는 구분될 수 있도록 충분히 멀리 떨어뜨려야 한다.

### 줄 끝(End-Of-Line) 주석   
> 주석 기호 // 는 한 줄 모두를 주석 처리하거나 한 줄의 일부분을 주석 처리해야 할 때 사용할 수 있다.    

```java
if (foo > 1) {
  
    // double-flip을 실행한다.
    ...
}
else {
    return false;          // 이유를 여기에 설명한다.
}
//if (bar > 1) {
//
//    // double-flip을 실행한다.
//    ...
//}
//else {
//    return false;
//}
```
* 긴 내용을 주석에 포함하기 위해서 연속되는 여러 줄에 이 주석을 사용하는 것은 안되지만,     
어떤 코드의 일부분을 주석 처리하기 위해서 여러 줄에 연속해서 사용하는 것은 허락된다.     

## 문서화 주석 
> 문서화 주석은 다음 5가지를 설명한다.
   
* 자바 클래스
* 인터페이스
* 생성자
* 메서드
* 필드
    
각각의 문서화 주석은 주석 경계 기호인 `/** ... */` 안으로 들어간다.    
그리고 각각의 문서화 주석은 클래스, 인터페이스 그리고 멤버 당 하나씩 가진다.    
문서화 주석은 선언 바로 전에 나와야 한다.   

```java
/**
 * Example 클래스는 ...
 */
public class Example { ...
```   
* 클래스에 대한 문서화 주석(`/**`)의 첫 번째 줄은 들여쓰지 않는다.         
* 그 다음에 나오는 문서화 주석은 별표를 수직으로 맞추기 위해 각각 1개의 빈 칸을 들여쓰기를 한다.    
* 만약 클래스, 인터페이스, 변수 또는 메서드에 대한 어떤 정보를 제공하고 싶지만 문서화 주석에 어울리지 않는다고 생각된다면,    
* 클래스 선언 바로 후에 블록(Block) 주석 또는 한 줄(Single-Line) 주석을 사용한다.      
* 예를 들어, 클래스의 구현에 대한 세부 사항들은 클래스에 대한 문서화 주석이 아니라, 클래스 선언문 다음에 블록 주석을 사용해야 한다.      
* 자바는 문서화 주석을 주석 이후에 처음 나오는 선언문과 연결시키기 때문에 문서화 주석은 메서드 또는 생성자 구현 안에 위치해서는 안 된다.

# 선언
## 한 줄당 선언문의 수
> 한 줄에 하나의 선언문을 쓰는 것이 주석문 쓰는 것을 쉽게 해주기 때문에 한 줄에 하나의 선언문을 쓰는 것이 좋다.

**한 줄에 여러 선언문**
```java
int level, size;
```

**한 줄에 하나의 선언문**
```java
int level; // 들여쓰기 단위
int size;  // 테이클 크기
```

**한 줄에 여러 타입 선언문**
```java
int foo,  fooarray[]; //절대 이렇게 사용하지 말자
```
* 절대 금지   

**스페이스를 준 선언문**
```java
int      level;            // 들여쓰기 단위 
int      size;             // 테이블 크기 
Object   currentEntry;     // 테이블에서 현재 선택된 데이터
```
* 코드 컨벤션에서는 사용가능한 하나의 방법이라 말한다.      
* 하지만, 클린 코드에서는 이같은 방법을 지양하고 있다.   
* 개인적인 생각으로는 코드 이름이 길면 들여쓰기도 달라지고 이를 관리해야하는 어려움이 있지 않을까 싶다.     

## 초기화    
* 지역 변수의 경우, 지역 변수를 선언할 때 초기화 하는 것이 좋다.     
* 변수의 초기화 값이 다른 계산에 의해서 결정되는 경우라면, 변수를 선언할 때 초기화 하지 않아도 괜찮다.    
* 개인적인 경험으로 싱글톤 패턴에서 static 초기화를 진행하지 않으면 RaceCondition이 발생하는 위험도 있었다.     
  * https://velog.io/@kwj1270/Singletone-pattern

## 배치  

```java
void myMethod() { 
    int int1 = 0;       // 메서드 블록의 시작 
    if (condition) { 
        int int2 = 0;   // "if" 블록의 시작 
        ... 
    } 
}
```
* 선언은 블록의 시작에 위치해야 한다(보통 블록은 중괄호 "{ }" 로 둘러싸인 코드를 의미한다). 
* 변수를 처음 사용할 때까지 변수의 선언을 미루지 말자. 
* 이러한 경우 부주의한 프로그래머들은 혼돈에 빠질 수 있고, 영역내에서 코드를 이동해야 하는 경우에 문제를 야기시킬 수 있다.

```java
for (int i = 0; i < maxLoops; i++) { ... }
```
* 이러한 원칙에도 예외는 존재한다. 그 중 하나가 for 문에서 선언하는 반복문을 위한 인데스이다.

```java
int count;              // 먼저 이름이 사용된 변수  
... 
myMethod() { 
    if (condition) { 
        int count = 0; // 이렇게 사용하지 말 것! 
        ... 
        } 
    ... 
}
```
* 상위 영역에서 선언된 것을 숨기기 위해서 블록의 처음 부분에서 다시 선언하는 것은 피해야 한다. 
* 예를들어, 블록 안의 블록에서 동일한 변수 이름을 사용해서 선언하지 말아야 한다.
* 궁금증 : 그렇다면 생성자와 같은 부분에서 변수의 이름을 달리해줘야 하나?    

## 클래스와 인터페이스의 선언  
> 자바 클래스와 인터페이스를 선언할 때, 다음 3가지 원칙을 따르도록 하자     
   
* 메서드 이름과 그 메서드의 파라미터 리스트의 시작인 괄호 `(` 사이에는 빈 공간이 없어야 한다.  
  * `public void method(int a)`
* 여는 중괄호 `{` 는 클래스/인터페이스/메서드 선언과 동일한 줄의 끝에 띄어쓰기 포함하여 사용하자.
  * `public void method(int a) {` 
* 닫는 중괄호 `}` 는 `}` 가 여는 중괄호 `{` 후에 바로 나와야 하는 null 문인 경우를 제외하고는,      
여는 문장과 동일한 들여쓰기를 하는 새로운 줄에서 사용하자.        

```java
class Sample extends Object {
    int ivar1;
    int ivar2;
 
    Sample(int i, int j) {
        ivar1 = i;
        ivar2 = j;
    }
 
    int emptyMethod() {}
 
    ...
}
```
* 또한, 메서드들을 구분하기 위해서 각 메서드들 사이에는 한 줄을 비우도록 하자

# 문
## 간단한 문
> 각각의 줄에는 최대한 하나의 문(statement)만 사용하도록 한다.

```java
argv++; // 올바른 사용법 
argc--; // 올바른 사용법 
argv++; argc--; // 이렇게 작성하는 것은 피해라
```

## 복합문
> 복합문은 중괄호 `{ statements }` 로 둘러싸여진 문들의 리스트를 포함하는 문이다.

```java
for (int i = 0; i < maxLoops; i++) { 
    for (int j = 0; j < maxLoops; j++) { 
        System.out.println(i); 
        System.out.println(j); 
    }
}
```

* 둘러싸여진 문들은 복합문보다 한 단계 더 들여쓰기를 한다. (중괄호 안에 있으면 한 단계 더 들여쓰기)       
* 여는 중괄호 `{` 는 복합문을 시작하는 줄의 마지막에 위치해야 한다.    
* 닫는 중괄호 `}` 는 새로운 줄에 써야하고, 복합문의 시작과 같은 들여쓰기를 한다.      
* 중괄호들이 if-else 문이나 for 문 같은 제어 구조의 일부분으로 사용될 때에는       
중괄호들이 모든 문들(단 하나의 문일 경우에도)을 둘러싸는데 사용되어져야 한다.         
(이렇게 사용하는 것이 중괄호를 닫는 것을 잊어버리는 것 때문에 발생하는 버그를 만들지 않고, 문을 추가하는 것에 큰 도움을 준다).       
  * 즉, 한 줄 짜리 코드만 있더라도 중괄호를 써주는 습관을 들이자는 말이다.       

## return 문
> 값을 반환하는 return 문은 특별한 방법으로 더 확실한 return 값을 표현하는 경우를 제외하고는 괄호를 사용하지 않는 것이 좋다.

```java
return;
  
return myDisk.size();
  
return (size ? size : defaultSize);
```
* 쉽게 얘기하면 삼항 연산자나 연산자 우선 순위로 인해 헷갈릴때와 같이 구분하기 힘들때 사용하는 것이 좋다.   

## if, if-else, if else-if else 문
> if-else 문을 사용할 때는 다음과 같이 작성한다.    
   
```java
if (condition) {
    statements;
}
                     
if (condition) {
    statements;
} else {
    statements;
}
  
if (condition) {
    statements;
} else if (condition) {
    statements;
} else {
    statements;
}
```
* if/elseif/else 와 `()` 사이에 한 칸 띄어쓰기를 진행한다.   
* `()`과 `{}`사이에 한 칸 띄어쓰기를 진행한다.
* `if-elseif-else` 는 서로가 끝날 때 시작하게 코드를 작성한다.
* 닫는 중괄호는 첫번째 if 의 들여쓰기와 맞춰준다.   

```java
if (condition) // 이렇게 중괄호 {}를 생략해서 사용하지 말자 
    statement;
```
* if 문은 항상 중괄호를 사용한다. 다음과 같은 에러가 발생할 수 있는 상황은 피해야 한다.

## for 문
> for 문은 다음과 같이 사용하자         
   
```java
for (initialization; condition; update) {
    statements;
}
```
* for 와 `()`사이에 한 칸 띄어쓰기를 진행해준다.  
* `()`과 `{}`사이에 한 칸 띄어쓰기를 진행한다.

```java
for (initialization; condition; update);
```
```java
for (initialization; condition; update)
;
```
* 빈 for 문(모든 작업이 initialization, condition, update 에서 완료되는)은 위와 같은 형태를 가져야 한다.
* 하지만 클린 코드에서는 아래와 같은 방법을 지향하니까 본인이 속한 팀의 규칙대로 사용하면 괜찮을 것이다.      
 
이외에도        
for 문의 initialization 또는 update 부문에서 콤마(,) 연산자를 사용할 때에는 세개 이상의 변수들을 사용하는 복잡성은 피해야 한다.       
만약 필요하다면, for 문 이전에 문을 분리시켜 사용(initialization절의 경우)하거나 for 문의 마지막에 문을 분리시켜 사용(update절의 경우)한다.      
    
## while 문
> while 문은 다음과 같이 작성한다.    
    
```java
while (condition) {
    statements;
}
```
* while 과 `()`사이에 한 칸 띄어쓰기를 진행해준다.  
* `()`과 `{}`사이에 한 칸 띄어쓰기를 진행한다.

```java
while (condition);
```
```java
while (condition)
;
```
* 빈 while 문은 위와 같은 방법으로 작성한다.
* 하지만 클린 코드에서는 아래와 같은 방법을 지향하니까 본인이 속한 팀의 규칙대로 사용하면 괜찮을 것이다.      
   
## do-while 문   
> do-while 문은 다음과 같이 작성한다.        
    
```java
do {
    statements;
} while (condition);
```
* do 와 `{}`사이에 한 칸 띄어쓰기를 진행해준다.  
       
## switch 문
> switch 문은 다음과 같이 작성한다.    
    
```java
switch (condition) {
case ABC:
    statements;
    /* 다음줄로 계속 진행한다. */
  
case DEF:
    statements;
    break;
  
case XYZ:
    statements;
    break;
  
default:
    statements;
    break;
}
```
* switch 와 `()` 사이에 한 칸 띄어쓰기를 진행해준다. 
* `()`과 `{}`사이에 한 칸 띄어쓰기를 진행한다.
* case 와 value 사이에 한 칸 띄어쓰기를 진행한다.  
* value 와 `:`는 붙여쓰기를 진행한다.    
* 모든 case를 수행해야 하는 경우에는 break 문을 사용안하면 된다. (첫번째 case와 같이)   
* 모든 switch 문은 default case를 포함해야 한다.      
* default case에서 break는 중복적이지만, 이후에 또 다른 case가 추가되어질 경우 에러를 방지하기 위해 작성해주자   

## try-catch 문    
> try-catch 문은 다음과 같이 작성한다.    
     
```java
try {
    statements;
} catch (ExceptionClass e) {
    statements;
}
```   
* try-catch 문은 try 블록이 성공적으로 완료되든지, 아니면 중간에 에러가 발생하든지에 상관없이    
실행되어야 하는 부분을 추가하기 위해서 finally 부분을 사용할 수 있다.          
    
```java
try {
    statements;
} catch (ExceptionClass e) {
    statements;
} finally {
    statements;
}
```
* 궁금증 : 코드가 중복이 일어날 수 있지 않나? 오히려 다른 설계법을 찾을 것 같다.       

# 빈 공간(White Space)    
## 한 줄 띄우기(Blank Lines)     
> 한 줄을 띄우고 코드를 작성하면, 논리적으로 관계가 있는 코드들을 쉽게 구분할 수 있기 때문에 코드의 가독성을 향상시킨다.  
  
**한 줄을 띄우는 경우**     
* 메서드들 사이에서 
* 메서드 안에서의 지역 변수와 그 메서드의 첫 번째 문장 사이에서  
* 블록(Block)주석 또는 한 줄(Single-Line)주석 이전에 
* 가독성을 향상시키기 위한 메서드 내부의 논리적인 섹션들 사이에  
       
**두 줄을 띄우는 경우**        
* 소스 파일의 섹션들 사이에서 
* 클래스와 인터페이스의 정의 사이에서  

## 공백     
> 공백은 다음과 같은 경우에 사용한다.      
    

```java
while (true) {
    ...
}
```
* 괄호와 함께 나타나는 키워드는 공백으로 나누어야한다.   

```java
a += c + d;
a = (a + b) / (c * d);
 
while (d++ = s++) {
    n++;
}

printSize("size is " + foo + "\n");
```
* 메서드의 이름과 메서드의 여는 괄호 사이에 공백이 사용되어서는 안된다는 것을 명심하자.       
  * 이렇게 하는 것은 메서드 호출과 키워드를 구별하는데 도움을 준다.   
* 공백은 인자(argument) 리스트에서 콤마 이후에 나타나야 한다.      
* `.`을 제외한 모든 이항(binary) 연산자는 연산수들과는 공백으로 분리되어져야 한다.   
* 단항 연산자 `++` , `--` 같은 경우에는 공백을 사용하면 안 된다.      

```java
for (int i = 0; i < 10; i++) {
    ...
}
```
* for문에서 사용하는 세 개의 식(expression)들은 공백으로 구분해서 나누어야 한다.  

```java
myMethod((byte) aNum, (Object) x);
myMethod((int) (cp + 5), ((int) (i + 3)) + 1);
```
* 변수의 타입을 변환하는 캐스트(cast)의 경우에는 공백으로 구분해야 한다.
  * `int int_value = (int) value;   

# 명명(Naming)규칙
> 이름을 정하는 규칙은 프로그램을 더 읽기 쉽게 만들어 줌으로써 더 이해하기 쉽게 만들어 준다.      
> 또한, 식별자(identifier)를 통해서 기능에 대한 정보도 얻을 수 있다. (클래스인지 상수인지 패키지인지 알 수 있도록)   

**식별자 타입 6가지**
* Packages 
* Classes 
* Interfaces
* Methods
* Variables
* Constants

## Pakages   
> 명명 규칙은 다음과 같다.      

```java
package com.sum.eng;

package com.apple.quicktime.v2;

package edu.cmu.cs.bovik.cheese;
```
   
* 패키지 이름의 최상위 레벨은 항상 ASCII 문자에 포함되어 있는 **소문자**로 쓰고,    
가장 높은 레벨의 도메인 이름 중 하나이어야 한다.        
* 현재는 `com`, `edu`, `gov`, `mil`, `net`, `org` 또는   
1981년 ISO Standard 316 에서 명시된 영어 두 문자로 표현되는 나라별 코드가 사용된다.     
* 패키지 이름의 나머지 부분은 조직 내부의 명명 규칙을 따르면 된다.   
  * 이러한 규칙을 따라 만들어진 이름은 디렉토리 구조에서 디렉토리 이름으로도 사용된다.   
  * 예를 들면 부서명, 팀명, 프로젝트명, 컴퓨터 이름, 또른 로그인 이름등 

## Classes
> 명명 규칙은 다음과 같다.   

```java
class Raster {}

class ImageSprite {}
```
   
* 클래스 이름은 **명사**이어야 하며, 복합 단어일 경우 **각 단어의 첫 글자는 대문자**이어야 한다.     
* 클래스 이름은 간단하고 명시적으로 작성해야 한다.   
* 완전한 단어를 사용하고 두 문자어와 약어는 피하도록 한다. 
  * 만약 ,약어가 URL이나 HTML과 같이 더 많이, 더 넓게 사용되고 있다면 약어를 사용하는 것도 괜찮다.    

## Interfaces
> 명명 규칙은 다음과 같다.    
   
```java
interface RasterDelegate {}

interface Storing {}
```

* 인터페이스 이름도 클래스 이름과 같은 대문자 규칙을 사용해야 한다.      
* 궁금증 : 주로 `-able` 구문이 많이 보이는데 사용해도 나쁘지 않을 것일까?    

## Methods    
> 명명 규칙은 다음과 같다.   
  
```java
run();

runFast();

getBackground();
```

* 메서드의 이름은 **동사**이어야 하며, 복합 단어일 경우 첫 단어는 소문자로 시작하고 그 이후에 나오는 단어의 첫 문자는 대문자로 사용해야 한다.   
* 추가 : boolean 타입일 경우 대개 `is-`를 붙인다.   

## Variables
> 명명 규칙은 다음과 같다.  

```java
int i;

char c;

float myWidth;
```

* 변수 이름의 **첫 번째 문자는 소문자**로 시작하고, 각각의 **내부 단어의 첫 번째 문자는 대문자**로 시작해야하다.       
* 변수 이름이 언더바 `_` 또는 달러 `$` 표시 문자로 시작하는 것이 허용되기는 하지만, 이 문자들로 시작하지 않도록 주의하자       
* 이름은 짧지만 의미 있어야 한다.            
* 이름의 선택은 그 변수의 **사용 의도를 알아낼 수 있도록 의미적**이어야 한다.      
* 한 문자로만 이루어진 변수 이름은 암시적으로만 사용하고 버릴 변수일 경우를 제외하고는 피해야 한다.   
* 보통의 임시 변수들의 이름  
  * integer 일 경우 : `i`, `j`, `k`, `m`, `n` 을 사용 (알파벳 소문자 l과 o 의 사용은 엄격히 금한다.)    
  * character 일 경우 :`c`, `d`, `e` 을 사용   
  
## Constants   
> 명명 규칙은 다음과 같다.  

```java
static final int MIN_WIDTH = 4;

static final int MAX_WIDTH = 999;

static final int GET_THE_CPU = 1;
```

* 클래스 상수로 선언된 변수들과 ANSI 상수들의 이름은 모두 **대문자**로 쓰고 각각의 단어는 언더바 `_`로 분리 해야 한다.   
* 디버깅을 쉽게 하기 위해서 ANSI 상수들의 사용은 자제하도록 한다.   

**ANSI 상수란? : ANSI 에서 32개의 키워드 지정.**
```
auto        beak     case     char     const       continue
default     do       double   else     enum        extern
float       for      goto     if       int         long
register    return   short    signed   sized       static
struct      switch   typed    union    unsigned    void
volatile    while
```

# 좋은 프로그래밍 습관   
## 인스턴스 변수와 클래스 변수를 외부에 노출하지 말고 대신 접근을 제공  
> 정보 은닉     
   
* 인스턴스 변수 또는 클래스 변수를 합당한 이유없이 `public`으로 선언하지 말자.   
* 인스턴스 변수들은 명시적으로 선언될 필요가 없는 경우가 많다. (`public`으로 쓰는 경우가 거의 없다는 말)   
* 인스턴스 변수가 public으로 선언되는 것이 적절한 경우는 클래스 자체가 어떤 동작(method)을 가지지 않는 데이터 구조(struct)일 경우이다.
* 다시 말해서 만약 클래스 대신 구조체를 사용해야 하는 경우라면(만약 Java가 struct를 지원한다면) public으로 선언하는 것이 적합하다.

## 클래스(static) 변수와 클래스(static) 메서드는 클래스 이름을 사용하여 호출    

```java
classMethod();             // 좋은 사용방법
AClass.classMethod();      // 좋은 사용방법
anObject.classMethod();    // 나쁜 사용방법
```
* 클래스(static) 변수 또는 클래스 메서드를 호출하기 위해서 객체를 사용하는 것을 피해야한다.   
* 대신에 **클래스 이름을 사용해라** (클래스에서 바로 호출)   

## 숫자는 바로 사용하지 않고 선언해서 변수 이름으로 접근 

* 숫자 상수는 `for루프`에 카운트 값으로 나타나는 `-1`, `0`, `1`을 제외하고는 숫자 자체를 코드에 사용하지 말자.   
* 번외 : `Money`와 같이 숫자들을 담는 객체를 만들어 사용하기도 한다. (더 많은 기능 수행할 수 있기 때문에)    

## 변수에 값을 할당할 때 주의할 것들  

```java
fooBar.fChar = barFoo.lchar = 'c'; // 이렇게 사용하지 말자
```
* 하나의 문(state)에서 같은 값을 여러 개의 변수들에 할당하지 말자. (가독성이 떨어진다.)   

```java
// 이렇게 사용하지 말자 (자바가 허용하지 않음)
if (c++ = d++) {
    ...
}

// 다음과 같이 써야 한다
if ((c++ = d++) != 0) {
    ...
}
```
* 비교 연산자(equality operator: `==`)와 혼동되기 쉬운 곳에 할당 연산자(assignment operator: `=`)를 사용하지 말자

```java
// 이렇게 사용하지 말자
d = (a = b + c) + r;

// 다음과 같이 써야 한다
a = b + c;        
d = a + r;
```
* 실행시 성능 향상을 위해서 할당문(assignment statement)안에 또 다른 할당문을 사용하지 말자

## 그 외 신경써야 할 것들
### 괄호   

```java
if (a == b && c == d)     // 이렇게 사용하지 말자
if ((a == b) && (c == d)) // 이렇게 사용하자
```
* 연산자 우선순위 문제를 피하기 위해서 복합 연산자를 포함하는 경우에는 자유롭게 괄호를 사용하는 것이 좋은 생각이다
* 작성자는 연산자 우선 순위를 확실하게 알고 있다고 할지라도, 다른 프로그래머에게는 생소할 수 있다는 것을 기억하자.

### 반환 값   
> 프로그램의 구조와 목적이 일치해야 한다.    
```java
// 이렇게 사용하지 말자
if (booleanExpression) {
    return true;
} else {
    return false;
}

// 다음과 같이 써야 한다
return booleanExpression;
```

```java
// 이렇게 사용하지 말자
if (condition) {
    return x;
}
return y;

// 다음과 같이 써야 한다
return (condition ? x : y);
```

### 조건 연산자 '?' 이전에 나오는 식(expression)

```java
(x >= 0) ? x : -x;
```
* 삼항 연산자(ternary operator -`?:`) 에서     
* `?` 이전에 이항 연산자(binary operator)를 포함하는 식(expression)이 있는 경우에는, 꼭 괄호를 사용해야 한다.

# 코드 예제   
## 자바 소스 파일 예제   
```java
/*
 * @(#)CodeConvention.java        0.82 2000/1/17
 *
 * [저작권 및 라이센스 관련 정보를 여기에 작성한다.]
 * Copyright (c) 2015 Kwangshin Oh.
 * ComputerScience, ProgrammingLanguage, Java, Seoul, KOREA
 * All rights reserved.
 *
 * This software is the confidential and proprietary information of Kwangshin
 * Oh ("Confidential Information").  You shall not
 * disclose such Confidential Information and shall use it only in
 * accordance with the terms of the license agreement you entered into
 * with Kwangshin Oh.
 */
  
  
package kwangshin.codeconvention;
  
import kwangshin.*;
  
/**
 * 클래스에 대한 설명을 여기에 작성한다.
 *
 * @version          1.00 2015년 2월 9일
 * @author           오광신
 */
public class CodeConvention extends Convention {
    /* 클래스의 구현 주석을 여기에 작성한다. */
  
    /** 클래스 변수 classVar1에 대한 설명을 여기에 작성한다. (문서 주석) */
    public static int classVar1;
  
    /** 
     * 클래스 변수 classVar2에 대한 설명이 (문서 주석이)
     * 한 줄 이상일 경우 이렇게 작성한다.(접근 제어자가 private일 경우 나오지는 않음.)
     */
    private static Object classVar2;
  
    /** 인스턴스 변수 instanceVar1에 대한 설명을 여기에 작성한다.(문서 주석) */
    public Object instanceVar1;
  
    /** 인스턴스 변수 instanceVar2에 대한 설명을 여기에 작성한다.(문서 주석) */
    protected int instanceVar2;
  
    /** 인스턴스 변수 instanceVar3에 대한 설명을 여기에 작성한다.(문서 주석) 접근 제어자가 private일 경우 나오지는 않음. */
    private Object[] instanceVar3;
  
    /** 
     * ... 생성자 CodeConvention()에 대한 설명을 여기에 작성한다.(문서 주석) ...
     */
    public CodeConvention() {
        // ... 여기에 실제 코드를 작성한다. ...
    }
  
    /**
     * ... 메서드 doSomething()에 대한 설명을 여기에 작성한다.(문서 주석) ...
     */
    public void doSomething() {
        // ... 여기에 실제 코드를 작성한다. ... 
    }
  
    /**
     * ... 메서드 doSomethingElse()에 대한 설명을 여기에 작성한다.(문서 주석) ...
     * @param someParam 파라미터에 대한 설명
     * @return String 리턴값에 대한 설명
     * @exception exception 예외사항에 대한 설명
     */
    public String doSomethingElse(Object someParam) {
        // ... 여기에 실제 코드를 작성한다. ... 
    }
}
```

# 참고 
블로그 :    
https://myeonguni.tistory.com/1596       
http://kwangshin.pe.kr/blog/java-code-conventions-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%94%A9-%EA%B7%9C%EC%B9%99/       
https://velog.io/@aidenshin/Java-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%94%A9-%EA%B7%9C%EC%B9%99-Java-Code-Conventions       

# JAVA 코드 컨벤션 

# 왜 코드 컨벤션이 필요할까?   
> 코딩 규칙은 여러가지 이유에서 프로그래머에게 중요하다    
   
* 소프트웨어를 개발하는 일련의 모든 과정에 들어가는 비용 중 80%가 유지보수에 쓰여진다.      
* 소프트웨어의 직접 개발한 개발자가 그 소프트웨어의 유지보수를 담당하는 경우는 거의 보기 힘들다.      
* 코딩 규칙을 지키면 가독성이 높아져 다른 개발자가 소스 코드를 처음 보더라도 더 빠르고 완벽하게 이해할 수 있게된다.  
* 자신의 소스 코드를 제품으로 팔려고 한다면, 자신의 다른 소스 코드들과 잘 어울리도록 패키지(package)를 적절하게 구성할 필요가 있다.   

# 파일
## 파일 이름
### 확장자
|파일형태|확장자|
|------|----|
|자바 소스|.java|
|자바 바이트코드|.class|

### 공통으로 시작되는 파일 이름
|파일 이름|사용|
|-------|---|
|GNUmakefile make| 파일 이름으로 사용, 소프트웨어를 빌드할 때는 gnumake 명령어를 사용|
|README|특정 디렉토리의 내용을 요약하는 파일 이름으로 사용|

## 파일 구조
* 파일은 빈 줄이나 다른 구역임을 나타내주는 주석으로 나누어지는 여러 구역(section)들로 구성되어 있다.
* 2,000 라인을 넘는 파일은 이해하기가 쉽지 않기 때문에 될 수 있으면 피해야 한다.

## 자바 소스 파일
* 시작 주석
* Package 문과 Import 문
* Class와 Interface 선언

### 시작 주석
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

### Package 문과 Import 문
> 자바 소스 파일의 첫 번째 줄은 Package 문이다. 그 후에 Import 문이 뒤따라 나온다.

```java
package java.code.convention;

import java.util.*;
```

### Class와 Interface 선언

|순서|클래스/인터페이스 <br> 선언의 구성요소|설명|
|---|-------------------------|----|
|1|문서화 주석 <br>`(/** .. */)`|이 주석이 포함하는 정보들은 아래 "5장의 2절 문서화(Documentation) 주석"을 참고|
|2|클래스/인터페이스 문||	 
|3|구현 주석 <br>`(/* .. */)`|이 주석은 클래스/인터페이스 문서화 주석에 적합하지 않은 하나의 클래스/인터페이스에만 해당하는 정보들을 포함해야 한다.|	 
|4|클래스(static) 변수|`public` -> `protected` -> `package` -> `private`|
|5|일반 변수|`public` -> `protected` -> `package` -> `private`|
|6|생성자||	 
|7|메서드|메서드들은 범위나 접근성을 기준으로 나누기 보다는 기능성에 의해서 구성되어야 한다.<br> 예를 들어, private 클래스 메서드가 두 개의 public 메서드들 사이에 존재할 수도 있다.<br> 이렇게 하는 목적은 코드가 더 쉽게 읽히고, 더 쉽게 이해되도록 돕기 위해서이다.<br> 쉽게 얘기하면 호출하는 메서드와 호출되는 메서드는 가까이 붙는 것이 좋다.|

## 들여쓰기 
* 4개의 빈 칸(space)를 들여쓰기 단위로 사용한다. 
* 들여쓰기의 정확한 구현(빈 칸을 사용하거나 탭을 사용하거나)은 정해져 있지 않다. 
* 탭은 4개의 빈 칸이 아니라, 8개의 빈 칸으로 설정하는 것이 좋다. 
 
### 한 줄의 길이
* 한 줄에 80자 이상 쓰는 것은 대부분의 터미널(terminal)과 툴에서 다룰 수 없기 때문에 피해야 한다.
* 문서화 주석을 가지고 문서를 만들 때, 일반적으로 한 줄에 70자 이상을 가지지 않는다.

### 2 줄 나누기
> 하나의 식이 한 줄에 들어가지 않을 때에는, 다음과 같은 일반적인 원칙들을 따라서 두 줄로 나눈다.

* 콤마 후에 두 줄로 나눈다. 
* 연산자(operator) 앞에서 두 줄로 나눈다.
* 레벨이 낮은 원칙보다는 레벨이 높은 원칙에 따라 두 줄로 나눈다.
* 앞줄과 같은 레벨의 식(expression)이 시작되는 새로운 줄은 앞줄과 들여쓰기를 일치시킨다.
* 만약 위의 원칙들이 코드를 더 복잡하게 하거나 오른쪽 끝을 넘어간다면, 대신에 8개의 빈 칸을 사용해 들여쓴다.

```java
longName1 = longName2// 일반적인 들여쓰기 someMethod(int anArg, Object anotherArg, String yetAnotherArg, Object andStillAnother) { ... } // 너무 멀리 들여쓰는 것을 피하기 위해 8개의 빈 칸으로 들여쓰기 private static synchronized horkingLongMethodName(int anArg, Object anotherArg, String yetAnotherArg, Object andStillAnother) { ... }

출처: https://myeonguni.tistory.com/1596 [명우니닷컴] * (longName3 + longName4 - longName5) 
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

 
# 참고 
블로그 :    
https://myeonguni.tistory.com/1596    
http://kwangshin.pe.kr/blog/java-code-conventions-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%94%A9-%EA%B7%9C%EC%B9%99/    
https://velog.io/@aidenshin/Java-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%94%A9-%EA%B7%9C%EC%B9%99-Java-Code-Conventions    

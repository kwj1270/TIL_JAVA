# 들어가면서
> **목표 :** 자바 소스 파일(.java)을 JVM으로 실행하는 과정 이해하기.       
   
[1. JVM이란 무엇인가](#jvm이란-무엇인가)   
[2. 컴파일하는방법](#컴파일-하는-방법)        
[3. 실행하는 방법](#실행하는-방법)       
[4. JIT 컴파일과 동작원리](#jit-컴파일과-동작원리)    
[5. JVM 구성 요소](#jvm-구성-요소)  
[6. JDK와 JRE의 차이](#jdk와-jre의-차이)   
[7. 라이브 강의](#라이브-강의)      
   
   
# JVM이란 무엇인가  
> Java Virtual Machine | 자바 가상 머신   
       
코드를 실행하고 해당 코드에 대해 런타임 환경을 제공하는          
소프트웨어 프로그램에 대한 사양(Specification)이다.     
     
![](https://images.velog.io/images/kwj1270/post/bfbe5365-3571-475c-b82b-a82cdb31b3c1/jvm.png)        
        
* `자바 애플리케이션`을 `클래스 로더`를 통해 읽어 들여 `자바 API`와 함께 **실행**한다.         
* JAVA와 OS 사이의 **중개자 역할**을 수행하여 JAVA가 OS에 종속되지 않고 재사용할 수 있도록 해준다.        
* **메모리 관리**, **Garbage collection**을 수행한다.     
* 스택기반의 가상 머신으로 LIFO 원칙으로 동작한다.        

**Write once, run anywhere**    
<img width="802" alt="스크린샷 2021-01-04 오후 12 21 12" src="https://user-images.githubusercontent.com/50267433/103498244-6101ee00-4e87-11eb-91ef-f3af5c0606e6.png">
    
* 각 OS마다 똑같은 기계어를 사용하더라도 해석되는 방법은 다르다.      
* 즉, 같은 코드라도 OS마다 명령이 다를 수 있다는 의미이다.      
* JVM은 자바 코드를 기계어 코드로 바꾸기 전에 중간 단계인 바이트 코드를 작성한다.    
* JVM은 현재 컴퓨터의 OS를 파악해 바이트코드를 알맞는 기계어로 변환하는 작업을 한다.   
* 추가적으로 어떤 언어를 사용했더라도 이를 바이트코드로만 변환시키면 JVM을 사용할 수 있다.       
   
     
# 컴파일 하는 방법   
> **Compile :** 사람이 이해할 수 있는 언어를 컴퓨터가 이해할 수 있는 언어로 바꿔주는 과정         
> **Java에서는 :** `.java 파일`을 `.class 파일`로 변환하는 과정을 의미한다.           
> 즉, 컴퓨터가 이해할 수 있는 언어가 아닌 JVM이 이해할 수 있는 코드로 변환하는 과정    
     
**`.java 파일` :** Java 언어로 작성한 파일     
**`.class 파일` :** Java 파일을 JVM이 이해할 수 있는 언어로 변환한 파일     
   
```cmd     
> javac [ options ] [ sourcefiles ] [ classes ] [ @argfiles ]
```   
![](https://images.velog.io/images/kwj1270/post/4293bd8f-7a3c-44d7-b59b-4d619777bc9b/complie.png)     
      
자바 컴파일러`(javac)`가             
자바 소스코드`(.java)`를 읽어들여         
자바 바이트코드`(.class)`로 변환시킨다.     
      
**잠깐!**      
컴퓨터가 이해할 수 있는 언어가 아닌 JVM이 이해할 수 있는 언어로 표현했다.    
이유는 각 OS마다 아키텍처가 달라 기계어를 해석한 결과가 다르기 때문이다.     
때문에 자바 파일을 JVM이 이해할 수 있는 바이트코드로 변환 시키고       
런타임시에 JVM에서 명령어를 직접 실행하거나 OS에 맞는 기계어로 변환환다.   
`참고 :` https://quasarzone.com/bbs/qf_cmr/views/31834       
    
**컴파일 옵션**     
     
|커맨드|예시|설명|   
|-----|---|---|  
|-cp, -classpath|-classpath(cp) path<br>-cp `.; C:\my\dir` |컴파일시 필히 참조할 클래스의 경로 입력<br>컴파일 대상이 아닌 연관된 클래스의 경로<br>2개 입력 원할시 `;`으로 추가하면 된다.|
|-d|-d directory(루트 경로)<br> -d `C:\my\dir`|클래스 파일이 생성될 루트 디렉토리 지정<br>자바 파일을 클래스 파일로 변환할 때 저장될 공간 지정<br>루트 디렉터리를 지정하므로 파일들은 패키지에 맞춰 하위에 추가된다.<br>즉, `루트 디렉터리\패키지 경로..\파일이름.class`로 생성|
|-encoding|-encoding type<br>-encoding UTF-8|소스 파일에 사용된 문자열 인코딩을 설정.<br>옵션이 설정되어 있지 않으면, 기본적인 컨버터가 사용된다.|
|-g|`-g:{lines, vars, source}:`<br>-g:none: 생성X|모든 디버깅 정보를 생성<br>옵션이 설정되어 있지 않으면<br>기본적으로, 라인 넘버만 생성한다.<br>lines은 라인정보<br>vars는 지역변수<br>sounce는 소스 파일 정보|
|-deprecation|-deprecation|더 이상 사용되지 않는 클래스의 사용법 또는 재정의에 대한 설명을 표시<br>소스 코드내에서 사용된 deprecated API의 위치를 출력한다.|
|-nowarn|-nowarn|파일에서 발생한 warning message를 생성시키지 않는다.|
|-verbose|-verbose|컴파일러와 링커가 현재 어느 자바 소스파일이 컴파일되고 있고<br>어느 파일이 링크되고 있는지에 대한 정보를 출력한다.|
|-sourcepath|-sourcepath path|자바 소스파일의 위치를 지정한다.|
|**-source**|-source JavaVersion|지정한 JavaVersion의 클래스 파일을 생성한다.|
|**-target**|-target VMVersion|지정한 VM version에 호환되게끔 클래스 파일을 생성한다.<br>-source와 같이 사용하면 좋다.|
|-bootclasspath|-bootclasspath bootclasspath|특정한 bootstrap또는 확장 클래스를 지정할수 있다.<br>내장된 bootstrap말고<br>다른 플랫폼의 bootstrap과 확장 클래스를 통해서 컴파일한다.|
|-extdirs|-extdirs path|특정 확장 디렉토리를 지정한다.<br>즉, 컴파일시에 기술한 디렉터리의 클래스 파일을 참조한다.<br>`;`을 통해 여러 경로를 지정할 수도 있다.|
|-endorseddirs|-endorseddirs path|표준 경로의 위치를 재정의한다.|
|-version|-version|버전 정보를 출력한다.|
|-help|-help|표준 옵션의 개요를 인쇄합니다.|
|-Werror|-Werror|경고가 발생하면 컴파일을 종료한다.|
|-X|-X|비표준 옵션에 대한 정보를 표시하고 종료한다.|

   
# 실행하는 방법   
> **Run :** 컴퓨터 프로그램을 실행         
> **Runtime :** 컴퓨터 프로그램이 실행되고 있는 동안의 동작     
     
```cmd  
> java [ options ] class [ arguments ]
```      
![](https://images.velog.io/images/kwj1270/post/cb5ec97d-cb09-46fe-898d-ef2d3821ccbf/runtime.png)   
   
JVM을 통해 OS에 맞는 기계어(Machine code)로 변환한다.  
컴퓨터는 기계어(Machine code)를 해석하여 프로그램을 실행한다.      
   
**잠깐**     
컴퓨터는 바이트코드를 해석할 수 없는 것인가?     
바이트코드는 말 그대로 `바이트 단위로 이루어진 코드`이다.     
반면, **컴퓨터**는 `0`과`1`로만 이루어진 **`비트 단위의 코드만 해석 가능하다.`**     

**런타임 옵션**     
    
|커맨드|예시|설명|   
|-----|---|---|  

    
# 전체적인 과정   
     
1. JVM은 OS로부터 프로그램이 필요로 하는 메모리를 할당받는다.         
2. JVM은 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.      
3. 자바 컴파일러가 자바 소스코드를 읽어들여 자바 바이트코드로 변환시킨다.     
4. Class Loader를 통해 class 파일들을 JVM으로 로딩한다.     
5. 로딩된 class 파일들은 Execution engine을 통해 해석된다.        
6. 해석된 바이트코드는 Runtime Data Areas에 배치되어 수행이 이루어진다.        
7. 실행 과정 속에서 JVM은 필요에 따라     
Thread Synchronization과 GC같은 관리작업을 수행한다.         
    
# 바이트코드란 무엇인가
> **바이트코드(Bytecode, portable code, p-code) :**    
> 특정 하드웨어가 아닌 VM에서 돌아가는 실행 프로그램을 위한 이진 표현법     
> **Java 바이트코드 :**          
> 자바 가상 머신이 이해할 수 있는 언어로 변환된 자바 소스 코드     
      
**바이트코드는**      
* 코드의 명령어 크기가 1바이트라서 바이트코드라고 불린다.    
* 하드웨어가 아닌 소프트웨어에 의해 처리되기 때문에, 기계어보다 추상적이다.  
* 한 번에 하나의 명령어를 읽은 후 실행하여 높은 이식성을 갖는다.     
* 특정 하드웨어의 기계 코드를 만드는 컴파일러의 입력으로 사용되거나,    
가상 컴퓨터에서 명령어를 바로 실행한다.  
* 결과적으로 특정 하드웨어에 대한 의존성을 줄인다.     
     
**자바 바이트코드는**    
* JVM이 이해할 수 있는 언어로 변환된 자바 소스 코드를 의미합니다.    
* 확장자는 `.class`이다.    
* 각각의 바이트코드는 1바이트로 구성되지만    
몇 개의 파라미터가 사용되는 경우가 있어   
총 몇 바이트로 구성되는 경우가 있다.    
* 자바 파일로 '역 컴파일'하는 것에 대해 취약점이 있다.        
* 256개의 명령코드 모두가 사용되지는 않는다.    
    
# JIT 컴파일과 동작원리     
>  런타임시에 필요에 따라 바이트코드를 기계어로 변환해 주는 컴파일러     
> 동적 번역(dynamic translation)이라고도 불리기도 한다.   
  
프로그램의 실행 속도를 향상시키기 위해 개발되었다.   
프로그램 실행 전에 바이트코드가 JIT 컴파일러에 의해 기계 코드로 번역된다.   
   
번역으로 인해 프로그램 실행 전에 지연시간이 발생하게 되지만     
보통 인터프리터보다는 훨씬 좋은 성능을 보여 준다.  
        
가상 컴퓨터의 이식성을 없애지만,    
바이트코드 자체에 대한 이식성을 잃지는 않는다는 특징이 있다.     
   
![](https://images.velog.io/images/kwj1270/post/b68a5626-61d7-482b-ac4e-a13c6667f120/jit.gif)    
     
**JIT 컴파일러 동작 기준과 원리**     
JIT 컴파일러가 컴파일하는 과정은    
바이트코드를 인터프리팅하는 것보다 훨씬 오래걸리므로    
한 번만 실행되는 코드라면 인터프리팅이 유리하다.  
따라서 JIT 컴파일러를 사용하는 JVM들은    
내부적으로 해당 메서드가 얼마나 자주 수행되는지 체크하고,    
일정 정도를 넘을 때에만 컴파일을 수행한다.   
      
**백기선 개발자님의 설명**       
자바는 라인 바이 라인으로 인터프리터로 번역하면서 동작한다.        
해석하는 바이트 코드 중에서 반복, 중복으로 발생하는 코드를        
JIT 컴파일러가 기계어로 변환하고 Caching 해놓는다.         
Caching을 사용하여 다시 번역할 필요 없이 해당 기계어를 가져와 사용한다.    
Caching의 위치는 JVM안에 들어간다.      
그렇기 때문에 C 보다 느리다는 말은 옛말이 되었다.       




# JVM 구성 요소
![](https://images.velog.io/images/kwj1270/post/bf625190-c35f-48dd-bdfc-bcc0f038c31e/JVM%20%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9.jpeg)      
   
* Java source : 사용자가 정의한 자바 파일    
* Compiler : 자바를 클래스 파일로 변환   
* ByteCode : 클래스 파일들   
* ClassLoader : 클래스 파일들을 JVM으로 로드 및 링크하여 RuntimeDataArea에 배치    
* ExecutionEngine : 로딩된 클래스 파일들의 바이트코드를 해석하여 JVM에서 실행가능하도록 한다.   
* RuntimeDataArea : JVM이라는 프로세스가 프로그램을 수행하기 위하여 OS로부터 받은 메모리 공간   
* Interpreter : 자바 바이트코드를 명령어 단위로 읽고 해석하는 역할      
* JIT Compiler : 자바 바이트코드를 기계어로 변환시키는 역할     
* Garbage Collect : 더 이상 사용되지 않는 메모리를 해제해주는 역할   
       
**Class Loader(클래스 로더)**         
* JVM내로 클래스`(.class)`를 로드하고, 링크를 통해 배치 작업을 수행하는 모듈.            
* Runtime 시에 동적으로 클래스를 로드한다.       
* jar파일 내 저장된 클래스들을 JVM내의 런타임 데이터 영역에 배치한다.    
* 사용 안하는 클래스는 메모리에서 해제한다.              
* 쉽게 말하면, 클래스를 처음으로 참조할 때 해당 클래스를 로드하고 링크한다.   
       
**Execution Engine (실행 엔진)**    
* 로드된 클래스 파일의 바이트코드를 실행하는 런타임 모듈         
* 바이트코드를 실제로 JVM내에서 실행할 수 있는 형태로 변경한다.          
* **Interpreter(인터 프리터) :**        
  바이트코드를 명령어 단위로 읽어서 실행한다.        
  단, 한 줄씩 수행하기 때문에 느리다.           
* **JIT(Just - in - time) :**        
  바이트코드 전체를 컴파일하여 기계어로 변경하고,           
  이후에는 기계어 코드로 직접 실행하는 방식이다.         
  JIT 컴파일러가 컴파일하는 과정은 오래걸리므로       
  한 번만 실행되는 코드라면 인터프리팅이 유리하다.      
* **Garbage collector :**      
  메모리가 부족할 때 사용되지 않는 메모리를 해제해준다.      
  메모리 부족 요청시 실행 또는 JVM이 한가할 때 동작한다.     
  GC의 동작시간은 일정하게 정해져 있지 않기 때문에     
  언제 객체를 정리할지는 알 수 없다.      
  즉, 바로 참조가 없어지자마자 작동하는 것이 아니라는 것이다.     
  또한 GC를 수행하는 동안 GC Thread를 제외한    
  다른 모든 Thread는 일시정지상태가 된다.      
  특히, Full GC가 일어나는 수 초간 모든 Thread가 정지한다면      
  심각한 장애로 이어질 수 있다.        
            
**Runtime Data Area**          
* 프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간     
* **PC Register :**        
  Thread가 시작될때 생성되는 공간으로 Thread마다 하나씩 존재한다.         
  Thread가 어떤 부분을 어떤 명령어로 실행해야할 지에 대한 기록을 한다.     
  현재 수행중인 JVM 명령의 주소를 갖는다.         
* **JVM 스택영역 :**         
  프로그램 실행과정에서 임시로 할당되었다가     
  메서드를 빠져나가면 바로 소멸되는 데이터를 저장하기 위한 영역이다.          
  메서드 호출 시 각각의 스택 프레임(해당 메스드만을 위한 공간)이 생성되며     
  이 안에 각종 형태의 변수나 임시 데이터, 스레드나 메서드 정보를 저장한다.       
  메서드 수행이 끝나면 프레임 별로 삭제를 진행한다.             
* **Native method stack :**           
  바이트코드가 아닌 기계어로 작성된 프로그램을 실행시키는 영역이다.       
  JAVA가 아닌 다른 언어로 작성된 코드를 위한 공간이다.           
  커널이 스택을 잡아 독자적으로 프로그램을 실행시킨다.           
* **Method Area(class area/static area) :**           
  클래스를 처음 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간이다.     
  Runtime Constant Pool이라는 상수 자료형을 저장 및 참조하여     
  중복을 막는 별도의 관리 영역도 함께 존재한다.   
  Heap 과 마찬가지로 GC의 관리 대상에 포함된다.       
  **올라가는 클래스 정보의 종류 :**      
    * Field Information :         
    맴버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보        
  * Method Information :   
  메서드의 이름, 리턴타입, 매개변수, 접근제어자에 대한 정보          
  * Type Information :      
  class인지 interface인지의 여부 저장    
  Type의 속성, 전체 이름, super class의 전체 이름      
    
* **Heap 영역 :**        
클래스가 아닌 객체를 저장하는 가상 메모리 공간이다.           
new 연산자로 생생된 객체와 배열을 저장한다.           
Method area 영역에 올라온 클래스들만 객체로 생성할 수 있지만        
인스턴스는 소멸 방법과 소멸 시점이 다르기에 힙이라는 별도의 영역에 할당된다.    
힙에 저장된 인스턴스의 존재 이유가 없을때 GC가 소멸시킨다.     
**힙의 3부분 :**    
  * Permanent Generation :      
  생성된 객체들의 정보의 주소값이 저장된 공간    
  클래스 로더에 의해 로드되는 class, method등에 대한    
  meta 정보가 저장되는 영역이고 JVM에 의해 사용된다.      
  Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다.   
  스프링 프레임워크의 경우 내부적으로 Reflection 기능을 사용하기에    
  이 영역에 대한 고려가 필요하다.      
  * New/Young 영역 :     
  Eden : 객체들이 최초로 생성되는 공간      
  survivor 0 / 1 : Eden에서 참조되는 객체들이 저장되는 공간       
  * Old 영역 :     
  New area에서 일정 시간 참고되고 있는, 살아남은 객체들이 저장되는 공간      
  Eden 영역에 객체가 가득차게 되면 첫번째 GC가 발생       
  Eden 영역에 있는 값들을 Survivor 1영역에 복사하고     
  이 영역을 제외한 나머지 영역의 객체를 삭제한다.      
      
    
# JDK와 JRE의 차이    
      
![](https://images.velog.io/images/kwj1270/post/aeeea8a4-1004-47b7-8e36-7565831d61c3/jdk:jre.webp)    
**JRE**       
> Java Runtime Environment       
   
Java 프로그래밍 언어로 작성된 애플리케이션 및 응용 프로그램 실행을 위한   
`JVM`, `클래스 라이브러리`, `java 명령` 및 `기타 인프라` 구성 요소를 모은 패키지이다.    
       
단, 컴퓨터가 Java 프로그램을 실행시킬 수 있는 환경을 만드는 것이지    
Java 언어를 통해 애플리케이션이나 응용 프로그램을 직접 만들지는 못한다.     
개발 관련된 도구는 JDK에 존재하고 JRE에는 존재하지 않기 때문이다.        
          
* Java Virtual Machine의 실제 구현체       
* Java 프로그램을 실행하는 데 필요한 플러그인      
* JVM, 코어 라이브러리 및 Java로 작성된 응용 프로그램 실행을 위한 구성요소 포함     
       
**JDK**  
> Java Development Kit    
   
자바의 모든 기능을 갖춘 [SDK](https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4_%EA%B0%9C%EB%B0%9C_%ED%82%A4%ED%8A%B8)로     
컴퓨터에서 Java를 실행시킬 수 있는 환경을 구성해주는 JRE는 물론,      
애플리케이션 및 응용 프로그램 개발에 필요한 컴파일러 및 디버거와 같은 도구를 포함한다.   
    
* Java 기반 응용 프로그램을 개발하려면 JDK가 필요하다.      
* Java 기반 응용 프로그램을 개발하는 데 사용할 수 있는 소프트웨어 번들     
* JRE, API 클래스 세트, Java 컴파일러, 애플리케이션 제작에 필요한 파일 포함      

   
# 라이브 강의   
> **의문점 :**   
> 스프링 프레임워크를 자바 14버전으로 만들었다 가정한다.      
> 하지만 내가 사용하고 있는 버전은 8버전인데 호환이 되었다.    
> 어떻게 가능한 것이지?        
   

```
/Library/Java/JavaVirtualMachines/jdk-14.0.1.jdk/Contents/Home/bin/javac Hello.java

/Library/Java/JavaVirtualMachines/jdk-14.0.1.jdk/Contents/Home/bin/java Hello
```
* 14.0.1 버전으로 `Hello.java` 파일을 컴파일 했다.     
* 14.0.1 버전으로 실행하면 잘 동작한다.     

```cmd
/Library/Java/JavaVirtualMachines/jdk-14.0.1.jdk/Contents/Home/bin/javac Hello.java

/Library/Java/JavaVirtualMachines/corretto-1.8.0_625.jdk/Contents/Home/bin/java Hello

UnsupportedClassVersionError: 클래스이름 has been compiled by a more recent version of the Java Runtime(class file version 58.0) this version of the Java Runtime only recognizes class file versions up to 52.0 

/* 참고
JDK 14.0.0 : version 58.0
JDK 1.8.0 : version 52.0
자바8에서 구동이 가능하다는 얘기이다.
* /
```
* 14.0.1 버전으로 `Hello.java` 파일을 컴파일 했다.     
* 1.8.0_625 버전으로 실행을 시키니 에러가 발생했다.     
* 하위 버전의 바이트코드는 상위 버전의 자바 프로그램이 실행할 수 있다.    
* 하지만, 상위 버전의 바이트코드는 하위 버전의 자바 프로그램이 실행할 수 없다.    
* 정확히 말하면 하위 버전 호환은 **특정 옵션을 주어야만 가능하다.**        
* 아무런 옵션을 주지 않았으므로 안 되는 것이었다.       
  
```linux
C\:>javac -source 1.6 -target 1.6 -bootclasspath C:\jdk1.6.0\lib\rt.jar -extdirs "" OldCode.java
```   
자세한 내용은 [참고 자료](https://stackoverflow.com/questions/15492948/javac-source-and-target-options)를 통해 확인하자      
complie 관련 옵션도 위 [링크](#컴파일-하는-방법)에서 확인하자    
      
* 그렇다면 상위 버전의 최신 기능은 사용하지 못하는걸까?      
* 아니다. 최신 기능도 사용할 수 있다.     
* 옵션 컴파일로 8 버전에서 인식하는 바이트코드를 만드는 것 뿐이다.   
* 즉, 최신 기능을 포함한 바이트코드로 변환되었다 생각하면 된다.       
* 하지만, 모든 하위 버전이 가능한 것은 아니고 최소 버전이 지정되어 있다.    
   
**바이트 코드 확인법**     
```
javap -c Hello.class // 바이트코드를 op코드로 볼 수 있다.   
```    
* `javap -c` 명령어를 통해 바이트코드를 확인할 수 있다.     
* op코드 : [참조](https://ehpub.co.kr/tag/%EC%97%B0%EC%82%B0-%EC%A2%85%EB%A5%98op-code/)    
* 1바이트이므로 2^8승. 즉, 256개의 명령어를 가진다.    
       
**잠깐**       
* 사실 JVM은 파일이 바이트코드로 이루어져 있으면 실행시켜줄 수 있다.    
* 프로그래밍 언어가 달라도 바이트코드로 변환만 해주면 JVM에서 동작할 수 있다.   
* 그래서 코틀린, Groovy 등과 같이 다른 언어를 사용해도 실행시킬 수 있다.      
* 우리는 오랜 기간 JVM을 사용하면서, 안정적이며,     
여러가지 모니터링 툴, 커스터마이징 및 운영 노하우가 쌓였다.      
* 때문에, JVM을 사용하는 것은 노하우와 툴을 적용하기에 좋다고 볼 수 있다.        

  
**추가적으로**       
```xml
<properties>
		<java-version>1.8</java-version>
</properties>
```   
* `pom.xml`의 태그 중에서 자바 버전과 관련된 태그가 있었다.        
* Apache 서버 관련 설정을 할 때도 자바 버전을 바꾸는 설정이 있었다.     
* 상위 버전 코드를 하위 버전에 맞추는게 가능하게 해준다는 것을 알 수 있다.    
   
   
# 참고
* [JVM 전체적인 내용](https://asfirstalways.tistory.com/158)    
* [JVM 간략 정리](https://github.com/gyoogle/tech-interview-for-   developer/blob/master/Language/%5Bjava%5D%20%EC%9E%90%EB%B0%94%20%EA%B0%80%EC%83%81%20%EB%A8%B8%EC%8B%A0(Java%20Virtual%20Machine).md)   
* [javac option-oracle doc](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/javac.html)    
* [javac option-blog](http://sjava.net/2008/02/javac-%EB%AA%85%EB%A0%B9%EC%96%B4%EC%9D%98-%EC%98%B5%EC%85%98-%EC%A0%95%EB%A6%AC/)   
* [바이트코드-위키](https://ko.wikipedia.org/wiki/%EB%B0%94%EC%9D%B4%ED%8A%B8%EC%BD%94%EB%93%9C)  
* [바이트코드-tcpschool](http://www.tcpschool.com/java/java_intro_programming)   
* [GC 내용 추가 참고](https://github.com/league3236/startJava/blob/master/live_study/week1.md)   
* [JDK/JRE비교](https://hashcode.co.kr/questions/260/jdk%EC%99%80-jre%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%B4-%EB%AD%94%EA%B0%80%EC%9A%94)      
* [필자의 깃허브](https://github.com/kwj1270/TIL_JAVA/blob/master/JVM.md)  




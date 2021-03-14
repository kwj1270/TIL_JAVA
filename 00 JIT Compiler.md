# JIT Compiler    
      
**JVM의 핵심 기능**이라고 할 수 있는 `JIT Compiler`에 대해서 알아보고자 합니다.       
                                        
`JIT`의 의미는 `Just-In-Time`이라는 이름 그대로 `그 순간`을 의미합니다.    
한국말로, `적시생산시스템`이라 말할 수 있으며         
**`필요할 때, 필요한 만큼만 생산한다.`** 라는 철학을 가지고 있습니다.        
     
```  
단어의 어원 미국의 록히드 항공사가 슈퍼마켓 방식을 생산에 도입함으로써 상당한 효과를 봤다는        
홍보물을 본 도요다 자동차를 비롯한 일본 자동차 회사에서 자동차 생산에 적용한 데서 시작되었습니다.        
필요한 것을, 필요한 때에, 필요한 만큼 만들어라라는 모토를 가지고 있습니다.   
```        
         
**🤔 그렇다면 `JIT Compiler`는 어떤 것을 생산하는 것일까요?**            
바로, 런타임시에 CPU에서 직접 사용할 `바이너리 코드를`를 만드는 작업을 합니다.      
     
# 컴파일과 인터프리트           
CPU는 **어셈블리나 바이너리코드라고 불리는 특정 명령만을 실행시킬 수 있습니다.**            
즉, CPU가 실행하는 **프로그램들은 이러한 언어로 변환되어 실행되어야 한다**는 의미기도 합니다.      
   
* **컴파일형 언어 :** `C++`, `Forttarn`과 같은 언어는 한 번에 모든 코드를 읽고 해석         
* **인터프리터 언어 :** `PHP`, `Perl`, `python`과 같은 언어는 한 번에 한줄의 코드만 읽고 해석  
                                           
`Java`는 흔히들 `컴파일 언어`라고 잘못 알고 있는 경우가 많은데 실은 **`인터프리터 언어`입니다.**                      
그렇기에 **한 번에 한 줄씩 수행하여 느리다는 인터프리터의 단점을 그대로 가지고 있습니다.**              
                                                                                   
`Java`는 여기서 절충안을 찾으려고 시도합니다.                 
바이트 코드를 `JIT Complier` 한 줄씩 읽으면서 `바이너리 코드`로 `변환`을 하는 방식을 채택했습니다.                    
여기까지는` Java Interpreter` 와 크게 다를게 없지만 **`JVM`의 `CodeCache`에 `Caching`해놓습니다.**                   
그렇기에, 같은 코드가 나오더라도 매번 바이너리 코드로 해석하는 작업을 하는 `Java Interpreter`와 달리,                
같은 코드가 나온다면 **다시 번역할 필요 없이 Code Cache에서 해당 바이너리 코드를 가져와 사용합니다.**                      
    
* **참고 - 자바가 인터프리터를 채택한 이유?:**               
프로그램이 커질 수록 컴파일 언어는 컴파일 시간이 길어진다는 문제가 있다.     
반면에, 인터프리터를 사용하면 해당 부분만 수정을 하면 된다는 이점이 있다.      
사실 자바 뿐만 아니라 70%의 프로그래밍 언어가 이러한 이유때문에 인터프리터를 채택하고 있다.  
         
## 핫스팟 컴파일     
[파레토 법칙](https://ko.wikipedia.org/wiki/%ED%8C%8C%EB%A0%88%ED%86%A0_%EB%B2%95%EC%B9%99)처럼, 일반적인 프로그램은 전체 코드 중 일부만 자주 실행되며        
애플리케이션의 성능은 이 일부가 얼마나 빠르게 실행되는가에 의해 좌우됩니다.              
그리고 이러한 영역을 애플리케이션의 `핫스팟`이라고 합니다. (코드가 많이 실행될수록 핫해진다는 의미입니다.)       
          
`JIT Compiler`는 `Hot Spot Detection` 이라는 기술을 이용해   
일정시간 동안, 인터프리터가 코드를 해석하며 컴파일하는 과정에서           
충분할 정도로 자주 호출되는 메소드를 식별하고 핫스팟이라고 판단하여 컴파일을 진행합니다.(최적화도)       
그리고 이러한 핫스팟을 식별하고 컴파일 하는 방식을 `핫스팟 컴파일`이라고 부릅니다.  

      
만약 중복된 코드가 아닌 모든 코드를 컴파일하면 안되는 걸까? 🤔   
모든 코드를 컴파일하는 방식의 수행 시간 자체는 빠르지만   
프로그램 크기가 커지고 기기별 이식성이 떨어진다는 문제점이 있다.  
그렇기에 JVM에서는 기본으로 Interpreter 방식을 사용하지만,  
여러 번의 중복이 발생하는 경우에만 Hot Spot Detection 방식을 사용한다.   

 
`JVM`은 코드를 실행하자 마자 바로, `코드 컴파일`을 하지 않습니다.       
그리고 여기에는 기본적인 이유가 두 가지가 있습니다.      
           
* **첫째 :**        
  코드가 한 번만 실행된다면 컴파일은 헛수고다.       
  컴파일해서 컴파일된 코드를 한번만 실행하는 것보다(컴파일 하는 시간 때문에)    
  자바 바이트코드를 인터프리트하는 편이 더 빠를 것이다. 
    
* **둘째 :**     
  최적화 때문이다.     
  `JVM`이 특정 메소드나 루프를 실행하는 시간이 길어질 수록 코드에 대해 얻어지는 정보가 많다.    
  이를 통해 JVM이 코드를 컴파일할 때 최적화를 많이 적용할 수 있다.
   
```java  
b = obj1.equals(obj2); 
```
여기는 나중에 채우자  

    
# JIT Compiler의 종류         
`JIT Compiler`는 `서버`와 `클라이언트` 두 가지 형태로 나눠지며,      
사용할 형태는 **프로그램이 수행되는 기간**과 **초기 스타트업 시간의 중요도**를 바탕으로 선택합니다.          
(현재 사용하는 컴파일러 종류는 java -version을 보면 확인 가능합니다.)      
                        
**Client Compiler**           
- `Start-up` 시간이 빠르며, 최적화를 위한 대기시간이 짧습니다        
- 하지만 최적화가 덜하기 때문에 `코드실행은 서버 컴파일러가 더 빠릅니다`      
              
**Server Compiler**            
- 클라이언트 컴파일러에 비해 `Start-up`이 느립니다.    
- 하지만, 컴파일 전에 많은 정보를 수집하여 **최적화에 중점을 두고 있습니다.**              
- 그리고, 서버 컴파일러는 절대로 모든 코드를 컴파일하지 않습니다.  (이유에 대해서는 찾아봐야겠습니다.)          

    
**Tiered Compile (-server -xx:+TieredCompilation )**     
- JDK 7 부터 도입되었으며, JDK 8 부터 기본 옵션으로 적용되었습니다.  
- **클라이언트 컴파일러로 스타트업 시간을 빠르게 하고, 많이 쓰이는 부분을 서버 컴파일러로 다시 컴파일하여 대체합니다.**    
- 이러한 `트레이드 오프`하는 과정에서 역최적화 후 서버 컴파일러로 재컴파일 됩니다.    
- 재컴파일 되는 시간은 성능에 영향을 줄 정도로 크지 않습니다.
- "-server -xx:+TieredCompilation" 옵션으로 명시합니다.       
    
**시간 비교**
- 스타트업 시간 : `-client` < `-xx:+TieredCompilation` < `-server`
- 코드실행 시간 : `- xx:+TieredCompilation` = `-server` < `-client`
- 상황에 따라 알맞은 컴파일러를 지정해서 사용하면 됩니다!          
       
       
# CoadCache 튜닝     
앞서 말했듯이, `Coad Cache`는 재사용될 바이너리 코드가 저장되는 공간입니다.        
하지만, **가득 차게 되면 `JVM`은 더 이상 코드를 추가적으로 바이너리 코드로 컴파일 할 수 없습니다.**     
이 말은 **애플리케이션의 많은 양의 부분이 인터프리터로 실행될 수 있다는 말입니다.**           
[실제 사례](https://helpx.adobe.com/kr/experience-manager/kb/aem-slow-java7.html)    

그렇기에 `Tiered Compile`나 `Client Compiler`를 사용하는           
**대형 프로그램**이라면 `Coad Cache`사이즈를 늘릴 필요가 있습니다.              
(Server Compiler는 몇개의 소수 클래스만 컴파일 하므로 Coad Cache를 채울 일은 그다지 없습니다.)       
           
**다양한 플랫폼에서의 Coad Cache 디폴트 값 목록**
|JVM 타입|디폴트 코드 캐시 크기|
|--------|-------------------|
|32bit 클라이언트 자바 8|32MB|
|티어드 컴파일을 하는 32bit 자바 8|240MB|
|티어드 컴파일을 하는 64bit 자바 8|240MB|
|32bit 클라이언트 자바 7|32MB|
|32bit 서버 자바 7|32MB|
|64bit 서버 자바 7|48MB|
|티어드 컴파일을 하는 64bit 서버 자바 7|96MB|

                
단, 최적의 `Coad Cache`를 알아내기 위한 매커니즘은 따로 없습니다.         
그렇기에 `Coad Cache`의 최대 크기를 `-XX:ReservedCodeCacheSize=N` 플래그를 통해      
기존 설정된 값의 2배 또는 4배로 늘리고 모니터링을 하면서 찾는 방법밖에 없습니다.                  
   
모니터링을 하는 방법은 JDK가 설치되어있을 경우    
`cmd`나 `terminal`에서 `jconsole` 입력하면 실행가능합니다.      
이후, `Memory`탭으로 이동한 뒤에 `Memory Pool Code Cache` 상자를 클릭하면 됩니다.     
   
**궁금증 : 그러면 최대 코드 캐시로 실제 큰 값을 지정하여 공간이 부족하지 않게 하면 되지 않을까?🤔**   
`Coad Cache`의 크기는 `JVM`에서 사용 가능한 `물리적인 메모리`의 크기에 의존합니다.  
즉, 물리적인 메모리의 크기만 충분하다면 코드 캐시의 최대 크기를 마음껏 늘려도 됩니다.     
      
**명령어**  
   
```
-XX:ReservedCodeCacheSize=N
```
코드 캐시의 최대 크기를 지정한다

```
-XX:InitialCodeCacheSize=N
```
초기 코드 캐시의 크기를 지정한다
   
# 컴파일 임계치    
> method entry counter + back-edge loop counter     
     
JIT 컴파일러가 컴파일하는 조건은 얼마나 자주 코드가 실행됐는가 입니다.     
일정한 횟수만큼 실행되고 나면 컴파일 임계치에 도달하고       
컴파일러는 컴파일하기에 충분한 정보가 쌓였다고 생각합니다.       
  
    
**컴파일의 기준이 되는 2개의 카운터**      
   
* JVM 내에 있는 메소드가 호출된 횟수
* 메소드가 루프를 빠져 나오기까지 돈 횟수 
            
`JVM`은 두 카운터의 합계를 확인하고 메소드가 컴파일 될 자격이 있는지 결정합니다.    
메서드가 컴파일 될 자격이 있다면 컴파일하기 위해 컴파일 큐에서 대기시키며       
큐에 있는 메소드들은 컴파일 스레드에 의해 컴파일 됩니다.     
이러한 컴파일 방식을 `일반 컴파일`이라고 부릅니다.     
     
**OSR : On - Stack Replacement**        
메서드의 길이가 너무 길거나, 루프가 정말 길 경우 **중간에 컴파일될 필요가 있습니다.**         
그래야 남은 반복을 중복하지 않고 빠르게 실행할 수 있기 때문입니다.             
따라서, `JVM`은 **루프의 실행을 그때그때 카운트**하고 임계치를 넘게되면            
전체 메소드가 아닌 **루프만을 컴파일하여 컴파일된 버전을 바로 실행시킵니다.**             
이렇듯 스택상에서 **컴파일된 버전을 바로 실행시키는 것을 `OSR`이라고 합니다.**            
    
**컴파일 임계치 변경하기**    
컴파일 임계치를 변경하는 것은 권고하고 있습니다.       
* 어플리케이션이 `warm-up` 하는데 필요한 시간을 절약할 수 있습니다.      
  * 어차피 10,000번 실행되서 컴파일될 코드는 8,000번으로 줄여도 큰 차이 없기 때문입니다
* 컴파일 임계치를 낮추지 않으면 절대로 컴파일 되지 않을 메소드들이 있다.   
  * 컴파일 임계치에 가까이 있어,    
    아슬아슬하게 걸쳐서 컴파일되지 않는 메서드들을 컴파일해서 실행속도를 높힐 수 있습니다.
             
카운터 값은 일정 시간(safepoint)동안 측정되는 상대적인 값 입니다.   
그렇기 때문에, lukewarm method(긴 시간에 걸쳐서 많이 호출되는 메소드)들은 카운터값이 낮을 수 밖에 없습니다.       
따라서 컴파일 임계치를 낮춰서 컴파일을 해주는 것이 훨씬 더 좋습니다.         

**명령어**
```
-XX:CompileThreshold=N
```      
   
클라이언트 컴파일러에서 기본 값은 `1,500`     
서버 컴파일러의 기본 값은 `10,000`입니다.    
       
ps)        
최근에 `스프링 네이티브`와 관련해서 이것 저것 간략히만 알아봤는데           
JVM은 이런 `warm-up`이 느리다는 문제가있어서 FaaS 환경에서 돌아가기 힘들다고 하네요       
그래서 `스프링 네이티브`는 `GraalVM`을 사용해서 이러한 문제를 조금이나마 해결했다고 하네요!         

# 컴파일 스레드
컴파일 임계치에 다다라서 메소드가 컴파일 대상이 되면 컴파일 큐에 들어가 대기하게 됩니다.       
              
컴파일 큐는 한 개 이상의 **백그라운드 스레드**에 의해 처리되며      
`비동기 프로세스`로 대상 코드가 컴파일 중인 동안에도 계속해서 프로그램이 실행됩니다.  
     
```
컴파일 큐는 FIFO 방식이 아니며 우선순위가 높은 메서드를 먼저 컴파일합니다.            
우선순위 선정 기준은 호출 카운터가 더 높은 메소드를 대상으로 더 높은 순위를 부여합니다.       
```     
   
그리고 컴파일러마다 사용하는 **백그라운드 스레드**의 갯수는 다릅니다.  
  
* 클라이언트 컴파일러 : 1개
* 서버 컴파일러 : 2개
* 티어드 컴파일러 : JVM이 대상 플랫폼의 CPU 개수와 이중 로그에 관한 복잡한 방정식에 근거해 디폴트 스레드를 정한다.
    
**티어드 컴파일에서 C1과 C2 컴파일러 스레드 디폴트 값**
|CPU 개수|Cilent Compiler 스레드 개수|Server Compiler 스레드 개수|
|-------|------------------------|-----------------------|
|1|1|1|
|2|1|1|
|4|1|2|
|8|1|2|
|16|2|6|
|32|3|7|
|64|4|8|
|128|4|10|   
        
이러한 티어드 컴파일에서의 `컴파일러 스레드 갯수`는  `-X:CICompilerCount=N`플레그를 통해 조정이 가능합니다.         
**그렇다면 언제 이 값을 조정하는게 나을까요? 🤔**

프로그램이 단일 CPU에서 실행된다면 컴파일러 스레드는 한 개만 쓰는 편이 낫습니다.
제한된 CPU만을 사용할 수 있는 환경에서는 더 적은 스레드가 그 자원을 두고 다투는 환경이 성능에 도움이 될 것 입니다.
물론 준비 기간에서는 컴파일러 스레드가 더 많으면 스타트업 시간에는 우위가 있을 것입니다. 하지만 현실에서는 이 이득이 그렇게 크지 않습니다.
준비 기간 이후에는 많이 쓰이는 메소드들이 점점 컴파일 되므로 CPU를 두고 경쟁하는 일이 없어질 것이므로 CPU 보다 적게 스레드를 두는게 낫습니다.
컴파일 스레드에 또 다른 설정은 XX:+BackgroundCompilation 플래그 입니다. 기본값은 true로 큐가 비동기 방식으로 처리되는 것을 말합니다.
플래그를 false로 설정한다면 메소드가 컴파일 되면 그걸 실행하길 원하는 코드는 실제로 컴파일될 때까지 기다리게 되빈다.


    







      
      

# JIT Compiler     
> **JIT Compiler :** Just-In-Time Compiler         
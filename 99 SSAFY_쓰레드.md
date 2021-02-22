# 스레드 

Concurrent 는 어떤 Job이 여러개 동시에 처리된다는 개념입니다.    
브라우저 여러 개 띄워서 동시에 여러 site에 접속할 수 있거나,        
엑셀 문서를 여러 개 동시에 편집하는 등을 예로 들 수 있습니다.       
 
이에비해 Parallel 은 어떤 하나의 **Job을 쪼개서 여러 Sub-Job으로 나누고,**         
이를 물리적으로 **분리된 구조에서 동시에 처리해서 완성하는 개념입니다.**              
자동차 조립을 여러 사람이 동시에 하는 것, 또는 CPU의 Core 여러 개를 표현하는 것 등이 이에 해당합니다.   

혼용해서 사용하는 경우가 많아 명확하게 맞다 틀리다라고 말하기 어렵습니다.   

프로세스 : 개별적으로 동작하는 프로그램
쓰레드 : 프로세스를 구성하는 독립적인 세부 실행 단위 
멀티 프러세스 : 여러 개의 프로세스를 동시에 수행
멀티 스레드 : 한 프로세스에서 여러 개의 쓰레드가 동시에 수행 

## 쓰레드 사용하기  
```java
package lab.ssafy.corona.virus;

public class CoronaRunnable implements Runnable {
	int num;
	
	public CoronaRunnable(int num) {
		this.num = num;
	}
	
	@Override
	public void run() {
		
		System.out.println("#" + num + " is Started");
		for( int i=0; i<10000; i++ ) {
			int j = i*100;
		}
		System.out.println(num);
	}
}
```
쓰레드의 작업 동작인 Runnable 구현체를 만듬   
  
```java
package lab.ssafy.corona.app;

import lab.ssafy.corona.virus.CoronaRunnable;

public class CoronaThreadTest {

	public static void main(String[] args) {
		for( int t=0; t<1000; t++ ) {
			
			CoronaRunnable cr = new CoronaRunnable(t);
			Thread thread = new Thread(cr);
			thread.start();
			
		}
	}
}
```
쓰레드 생성자 인자로 Runnable을 넣어준다.   

쓰레드는 순서가 없으므로 멀티쓰레드 동작시 뒤섞인다.   

## 실습2
   
메인쓰레드는 하나의 쓰레드이다.      
그렇기에 다른 쓰레드를 기다리지 않는다.     
   
쓰레드 객체에서 start() 2번사용하면? 오류가 난다.       
      
쓰레드안에서 쓰레드를 또 실행할 수 있다.(단, 공유자원(변수/자료구조) 등에 대한 각별한 주의가 필요하다       






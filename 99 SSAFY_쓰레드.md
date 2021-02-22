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


## sleep()  
Thread.sleep() , static method  
  
호출하면 해당 영역의 쓰레드의 실행을 중지시키는 역할을 한다.       
1/1000초 동안 중지이므로 1000을 넣으면 1초 쉰다.     
그리고 이 상황에 다른 스레드에게 실행 흐름이 넘어갈 수 있다.   

## join()   
엎어 탄다. (스레드가 또다른 스레드에 엎어타서 밑에 있는 내용들을 해먹는 것)    
  
Thread 객체는 특정 Thread 객체가 종료될 때까지     
수행하던 Job을 멈추고 특정 Thread 객체가 종료되면 다시 Job을 수행할 수 있다.     
    
만약, A Thread 객체 a의 종료시까지 대기하려면 (a가 종료되고 작업하려면)     
자신 Thrad의 실행 코드에 a.join() 형태로 메서드를 호출한다.   

main()을 수행하는 MatnThread가 다른 스레드 종료 후,  
계속 Job을 수행하는 코드를 테스트하자  

```java
package lab.ssafy.corona.app;

import lab.ssafy.corona.virus.CoronaThread;

public class CoronaThreadTest {

	public static void main(String[] args) {
		
		System.out.println("Main Thread Start!!");
		
		CoronaThread thread = new CoronaThread(2020);
		thread.start();
		
		try {
			thread.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.println("Main Thread End!!");
	}
}

```

```java
package lab.ssafy.corona.virus;

public class CoronaThread extends Thread {
	int num;
	
	public CoronaThread(int num) {
		this.num = num;
	}
	
	@Override
	public void run() {
		
		try {
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.println(num);
	}
}
```
메인 -> 코로나 스레드  
여기서 코로나 스레드는 sleep(2000)을 하기에 다시 메인스레드로 흐름이 넘어가야 하지만.    
메인에서 코로나스레드.join() 했기에 메인 스레드는 코로나 스레드가 끝날 때까지 기다린다.   
즉, `메인 -> 코로나 스레드 -> 메인이 된다.`   
   
## interuupt()   
Thread 객체는 statrt()가 호출되면 바로 실행되지 않습니다.      
모든 Thread는 기본적으로 서로 경쟁입니다.     
전체 Thread는 실행 -> 종료까지 다양한 상태로 존재하게 되고,     
JVM은 이 상태를 이용해서 전체 Thread의 실행을 제어한다.        
다음 Page에 전체 상태에 대한 그림을 함께 봅시다.       
    
대기 Pool에 있는 특정 Thread 객체를 방해하여 다시 Runnable 상태로 이동시킨다.     
interrupt()를 통해 가능한데, 특정 Thread 객체의 interrupt()를 호출하면 된다.    
개인적인 의견으로 스레드의 실행을 멈추는 느낌이다.  

CoronaThreadClass를 수정하고, CoronaThreadInterruptTestClass를 새로 작성해서 테스트해보자     

```java
public class CoronaThread extends Thread {
	int num;
	
	public CoronaThread(int num) {
		this.num = num;
	}
	
	@Override
	public void run() {
		try{
			System.out.println("thread try start"); 
			Thread.sleep(5000);  
			System.out.println("thread try end");  
		}catch(InterruptedException e){
			System.out.println("thread interrupted raised"); 
		}
		System.out.println(num);
	}
}
```
```java

import lab.ssafy.corona.virus.CoronaThread;

public class CoronaThreadInterruptTest {

	public static void main(String[] args) {
		CoronaThread t = new CoronaThread(2020);
		t.start();
		t.interrupt();
		
		try {
			Thread.sleep(3000);
		}catch(InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.println("Main Thread End!!");
	}
}
```



### 스레드의 상태  
1. new : new Thread() 
2. runnable : 아직 실행 대기중인 상태 
    1. 등록된것 + 스레드 스케줄러로 인해 실행에 있다가 다시 대기되는 객체가 대기되는 공간   
3. Ruinning : 현재 실행중,  
4. waiting timed waiting  풀: 러닝중 sleep()/join(),wait(),I/O블록킹 발생하면 실행대기가 아닌, 대기풀로 간다.  
    1. waiting timed waiting 풀에서 시간이 지나도 호출 안하면 runnable로 다시 대기상태로 만든다.     
5. `yield()`를 사용하면 러너블 형태로 되돌리는 메서드(순서 양보)    
     
## this,yield()   
Thread.yield(), static method  

Thread의 Job 수행 중 지금 당장 JOB 수행할 필요가 없을 경우 호출하여,  
Runnable 상태에 있는 다른 Thread 들에게 수행을 양보한다.  
  
단, 이 경우 wait상태로 가지 않고,      
Runnable 상태로 이동하여 언제든 다시 경쟁을 이기고 Running 상태가 되면,     
yield() 호출 이후 코드를 이어서 수행한다.    

# 동기화 - synchronuzed   
여러 Thread 들이 공유하는 자원에 대한 관리는 필수입니다.    

어떤 Thread가 자원에 대한 중요한 job을 실행할 때,    
그 job이 완료되기 전에 다른 Thread가 접근해서 다른 Job을 실행하게 되면 심각한 오류가 발생할 수 있다.   
    
계좌 출금 및 잔고 예시를 살펴보자   

## 싱크로 
Thread의 중요 Job을, 한 개의 방해받지 않는 job으로 처리하는 것을 동기화라고 한다.  
Critical session(임계영역)을 다른 스레드가 접근하지 못하게 처리하는 것 

보다 자세한 내용은 이미 정리햇으니 넘어감

## DeadLock 
서로다른 스레드가 동시에 서로가 점유한 자원을 동기화로 점유하고 있을 경우 발생 
싱크로나이즈드 블락을 너무 길게 잡거나 잘못 코드를 짜면 데드락이 발생한다.(비슷한 문제도)  
그리고 이런 부분을 해결할 수 있는 메카니즘을 `wait()` `notify()`를 제공한다.   
  
어떤 Thread가 공유 자원에 대한 Lock을 소유하더라도,    
그 시점에 바로 중요 Job을 수행할 수 없으면 Lock을 잠시 반납하면 된다.     
단, 필요한 Job을 완성하지 않았기 때문에, 기다렸다가 다시 수행해야한다.   

특정 자원에 대한 Lock을 반납하는 겨웅 `wait()` 메서드를 호춤함으로써 가능하다.   

wait() 자원을 반납한 T1은 Thread는 wait()를 호출함으로써 A객체의 Lock을 내려놓고 웨이팅풀에서 대기하는 상태가 된다,     
그럼 다른 A를 원하는 스레드들끼리 경쟁을 한다.  

이후 Thread A가 다시 Runnable상태가 되어 A의 Lock을 얻고 실행된다,  

___
어떤 Thread t2가 공유 자원 A에 필요한 작업을 완료하면,   
해당 자원을 기다리면서 Waiting Poll에서 대기중인 다른 Thread에게 이제 A자원이 **다시 준비되었음을 알릴 수 있다.**    
notify() 또는 notifyAll()을 통해서 가능하다.  
notify()는 웨이팅 풀에서 한 개의 쓰레드에게만, notifyAll()은 모든 스레드에게 알림을 한다.

알림을 받은 쓰레드들은 경쟁하면서 자원을 사용할 기회를 얻게 된다.  

wait()와 notify()는 상호 보완적인 구조   
준비가 된 쓰레드에게 우선적으로 사용되도록 처리해주며  
기다리는 쓰레드는 통지를 받아 다시 수행을 한다.  









  






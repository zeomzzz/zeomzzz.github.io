---
title:  "프로세스와 스레드"
excerpt: ""

categories:
  - Computer Science
tags:
  - [Operating System, Process, Thread]

toc: true
toc_sticky: true
 
date: 2023-12-10
last_modified_at: 2023-12-10
---

<br>

>💡 **요약**
>
>- 프로세스란
>    - 운영체제로부터 자원을 할당 받는 단위로, 실행 중인 프로그램
>- 스레드란
>    - 프로세스가 할당받은 자원을 사용하는 실행 단위로, 프로세스 안에서 실행되는 여러 흐름의 단위
>- 차이점
>    - 자원을 공유하지 않는 프로세스, 자원을 공유하는 스레드
>    - 이에 따라 하나의 스레드에서 문제가 발생하면 전체 프로세스에 영향을 미침

<br>
<br>

# **1. 프로세스 (Process)**

<br>

<p align="center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computerscience/process-vs-thread/01.png" width="550px">
</p>

- 정의
    - 실행 중인 프로그램(program)
        
        >💡 **프로그램**
        >
        >- 파일이 저장장치에 저장되어 있지만 메모리에는 올라가 있지 않은 정적인 상태
        >- 프로그램이 실행 중이다?
        >    - 디스크에 저장되어 있던 프로그램을 메모리에 저장한 뒤 운영체제의 CPU 제어권을 받을 수 있는 상태가 됨
        
    - 운영체제로부터 자원을 할당받는 작업의 단위
- 특징
    - 각각 독립된 메모리 영역을 할당 받음 (Code, Data, Stack, Heap)
        - 코드 : 실행할 프로그램의 코드나 명령어들을 기계어 형태로 저장. 컴파일 → 메모리에 올린 것 (0100101..) 전체 일감
        - 데이터 : 생성자로 생성하지 않고 선언한 전역변수 ex. `static`, `global`
        - 스택 : 호출된 함수, 지역변수 등 임시 데이터를 저장. 호출된 함수가 종료 되었을 때 되돌아오기 위한 경로를 저장
        - 힙 : 동적으로 생성된 데이터. 생성자로 만들어짐
    - 프로세스당 최소 1개의 스레드(메인 스레드)를 가짐
    - 한 프로세스가 다른 프로세스의 자원에 접근하려면 IPC(Inter Process Communication)를 사용해야 함 ex. 파이프, 파일, 소켓 등을 이용

<br>
<br>

# **2. 스레드 (Thread)**

<br>

<p align="center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computerscience/process-vs-thread/02.png" width="550px">
</p>

- 정의
    - 프로세스 안에서 실행되는 여러 흐름의 단위
    - 프로세스가 할당 받은 자원을 이용하는 실행 단위
- 특징
    - 프로세스 내에서 Stack만 따로 할당받고, Code, Data, Heap 영역은 공유
        - 한 스레드가 프로세스 자원을 변경하면, 다른 이웃 스레드(sibling thread)도 그 변경 결과를 볼 수 있음
        
        >💡 **왜 스택은 스레드마다 독립적으로 할당할까?**
        >
        >- 스택 : 함수 호출시 전달되는 인자, 되돌아갈 주소값 및 함수 내에서 선언하는 변수 등을 저장하기 위해 사용되는 메모리 공간
        >- 따라서 스택 메모리 공간이 독립적이라는 것은 독립적인 함수 호출이 가능하다는 것    
        >    == 독립적인 실행 흐름이 추가
        >- 스레드 정의에 따라 독립적인 실행 흐름을 추가하기 위한 최소 조건으로 독립된 스택을 할당
        
    - 프로세스마다 최소 1개의 스레드를 소유함 (메인 스레드 포함)
    - 함수를 실행한 결과를 저장

ex. 스레드를 사용하는 프로그래밍 ([출처](https://www.youtube.com/watch?v=iks_Xb9DtTM))

```java
import java.util.Scanner;

public class RamenProgram 
{

	public static void main(String[] args) 
	{
		int num;
		Scanner input = new Scanner(System.in);
		
		System.out.println("라면 몇 개 끓일까요?");
		
		num = input.nextInt();
		
		System.out.println(num + "개 주문 완료! 조리시작!");
		try
		{
			RamenCook ramenCook = new RamenCook(num);
			new Thread(ramenCook,"A").start();
			new Thread(ramenCook,"B").start();
			new Thread(ramenCook,"C").start();
			new Thread(ramenCook,"D").start();
		}
		catch(Exception e)
		{
			e.printStackTrace();
		}

	}

}

interface Runnable
{
	public void run();
}

class currentThread extends Thread
{
	public RamenCook ramenCook;
	static String nam;
	
	currentThread()
	{
		this(new RamenCook(5) , "");
	}
	
	currentThread(RamenCook ramenCook , String nam)
	{
		this.ramenCook = ramenCook;
		this.nam = nam;
	}
}

class RamenCook extends Thread implements Runnable
{
	private int ramenCount;
	private String[] burners = {"_","_","_","_"};
	
	public RamenCook(int count)
	{
		ramenCount = count;
	}
	
	@Override
	public void run()
	{
		while(ramenCount > 0)
		{
			synchronized(this)
			{
				ramenCount--;
				System.out.println(Thread.currentThread().getName() + " : " + ramenCount + "개 남았습니다");
			}
			
			for(int i = 0; i < burners.length; i++)
			{
				if(!burners[i].equals("_"))
				{
					continue;
				}
				
				synchronized(this)
				{
					//if(burners[i].equals("_"))
					//{
						burners[i] = Thread.currentThread().getName();
						System.out.println("                 " + Thread.currentThread().getName() + " : [" + (i + 1) + "]번 버너 ON");
						showBurners();
					//}
				}
				
				try
				{
					Thread.sleep(2000);	
				}
				catch(Exception e)
				{
					e.printStackTrace();
				}
				
				synchronized(this)
				{
					burners[i] = "_";
					System.out.println("                                  " + Thread.currentThread().getName() + " : [" + (i + 1) + "]번 버너 OFF" );
					showBurners();
				}
				break;
			}
			
			try
			{
				Thread.sleep(Math.round(1000 * Math.random()));
			}
			catch(Exception e)
			{
				e.printStackTrace();
			}
		}
	}
	
	private void showBurners()
	{
		String stringToPrint = "                                                             ";
		for(int i = 0; i < burners.length; i++)
		{
			stringToPrint += (" " + burners[i]);
		}
		System.out.println(stringToPrint);
	}

}
```

<br>
<br>

# **3. 프로세스와 스레드의 차이점**

<br>

| 프로세스 | 스레드 |
| --- | --- |
| 자원을 공유하지 않음 (독립된 메모리 영역을 할당) | 자원을 공유 (Code, Data, Heap 영역) |
| -> 다른 프로세스와 정보 공유를 위해 IPC를 사용 | -> 다른 스레드와 정보 공유가 용이 |
| 하나의 프로세스에 문제가 생겨도 다른 프로세스에 영향을 미치지 않음 | 한 스레드에 문제가 생기면 전체 프로세스에도 영향을 미침 |

<br>

<div class="descipt-font">
그렇다면 멀티프로세싱과 멀티스레딩은 무엇일까?
-> [멀티 프로세스와 멀티 스레드](https://zeomzzz.github.io/computer%20science/multi-process-vs-multi-thread/)
</div>

<br>

---

**참고 자료**

<br>

- [프로세스는 뭐고 스레드는 뭔가요?](https://www.youtube.com/watch?v=iks_Xb9DtTM)
- [프로세스와 스레드 (Process vs Tread)](https://net-gate.tistory.com/87)

<br>

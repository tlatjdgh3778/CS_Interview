
# 운영체제란?
운영체제(Operation System)란 컴퓨터 시스템의 자원들을 효율적으로 관리하며, 사용자가 컴퓨터를 편리하고 효과적으로 사용할 수 있도록 환경을 제공하는 여러 프로그램의 모임이다.

# 1. 프로세스와 스레드
## 1-1. 프로세스

프로세스(process)란 일반적으로 CPU에 의해 처리되는 사용자 프로그램, 시스템 프로그램 즉 **실행중인 프로그램**을 의미하며, 작업(Job), 태스크(Task)라고도 한다.

### 프로세스과 프로그램

**프로그램**은 일반적으로 하드 디스크 등에 저장되어 있는 실행 코드를 뜻하고, **프로세스**는 프로그램을 구동하여 프로그램 자체와 프로그램의 상태가 메모리 상에서 실행되는 작업 단위를 지칭한다.

프로세스와 프로그램의 차이는 프로그램 자체는 생명이 없다. 프로그램은 보조 기억장치에 존재하며 실행되기를 기다리는 명령어와 정적인 데이터의 묶음이다. 이 프로그램의 명령어와 정적 데이터가 자원을 할당받고 메모리에 적재되면 프로세스가 된다.

## 1-2. 프로세스의 특징

<p align="center"><img src="https://media.vlpt.us/images/wiostz98kr/post/d3bc38b6-8f79-456a-8850-f49336f5d57e/image.png"></p>

프로세스는 각각 독립된 영역(Code, Data, Stack, Heap)을 할당 받는다.

* Code(Text) 영역
  * Code 영역은 실행 명령을 포함하는 코드들이 들어가는 부분이다.
  * 프로그램을 시작할 때 컴파일한 프로그램(기계어)이 저장되어 있고, 읽기 전용 영역이기때문에 프로세스가 함부로 변경할 수 없고 변경 시 오류를 발생시킨다.
* Data 영역
  * 프로그램이 실행될 때 생성되고 프로그램이 종료되면 시스템에 반환된다.
  * 전역변수, 정적변수, 배열, 구조체 등이 저장된다.
  * Data 영역은 다시 Data(GVAR) 영역과 BSS 영역으로 나눌 수 있다.
  * 초기화된 데이터는 Data(GVAR) 영역에 저장되고 초기화되지 않은 데이터는 BSS 영역에 저장된다.
* Heap 영역
  * 메모리를 동적으로 할당할 때 사용하는 메모리 영역이다.
* Stack 영역
  * 프로그램이 자동으로 사용하는 메모리 영역이다.
  * 함수 호출과 관계되는 지역변수와 매개변수가 저장된다. 함수 호출 시 생성되고 함수가 끝나면 반환된다.
  
## 1-3. 스레드

스레드(Thread)란 어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위이다.

<p align="center"><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Multithreaded_process.svg/220px-Multithreaded_process.svg.png"></p>

## 1-4. 스레드의 특징

<p align="center"><img src="https://gmlwjd9405.github.io/images/os-process-and-thread/thread.png"></p>

 * 스레드는 프로세스 내에서 각각 Stack 만 따로 할당 받고 Code, Data, Heap 영역은 공유한다.
 * 스레드는 한 프로세스 내에서 동작되는 여러 실행의 흐름으로, 프로세스 내의 주소 공간이나 자원들(heap 공간 등)을 같은 프로세스 내에 스레드끼리 공유하면서 실행된다.
 * 같은 프로세스 안에 있는 여러 스레드들은 같은 heap 공간을 공유한다. 반면에 프로세스는 다른 프로세스의 메모리에 직접 접근할 수 없다.
 
 
# 2. 멀티 프로세스와 멀티 스레드
## 2-1. 멀티 프로세스

멀티 프로세스(Multi Process)란 하나의 응용 프로그램을 여러 개의 프로세스로 구성하여 각 프로세스가 하나의 작업을 처리하도록 하는 것이다.

### 장점

* 멀티 프로세싱은 여러 개의 자식 프로세스 중 하나에 문제가 발생해도 다른 프로세스들이 있기 때문에 영향이 확산되지 않는다.

### 단점

* Context Switching 과정에서 캐시 메모리 초기화 등 무거운 작업이 진행되고 많은 시간이 소모되는 등 오버헤드가 발생한다.
* 프로세스는 각각 독립된 메모리 영역을 가지고 있기 때문에 프로세스 사이에서 공유하는 메모리는 없다. 따라서 Context Switching 이 발생하면 캐시에 있는 모든 데이터를 모두 리셋하고 다시 캐시 정보를 불러와야 한다.
* 독립된 영역 때문에 프로세스간의 통신이 필요하다.

[Context Switching이란](#3-Context-Switching)

## 2-2. 멀티 스레드
멀테 스레드는 하나의 프로그램을 여러 개의 스레드로 구성하는 방식이다.

<p align="center"><img src="https://media.vlpt.us/images/hoo00nn/post/ac27535e-13fc-4bcc-8fdd-6e584998f059/image.png"></p>

### 장점
 * 멀티 스레드는 Stack을 제외한 자원들을 공유하고 있기 때문에 Context Switching 시에 캐시 메모리를 비울 필요가 없고 이를 통해서 리소스를 아낄 수 있다.
 * Stack 이외의 메모리를 공유하고 있기 때문에 통신의 부담도 적다.
 
### 단점
* 내부의 메모리를 공유하고 있어서 한 프로세스의 스레드가 문제가 생기면 해당 프로세스 안의 다른 스레드에도 문제가 생긴다.
* 같은 데이터를 공유하기에 데이터 동기화에 신경을 써야한다.

## 2-3. 멀티 프로세스 vs 멀티 스레드

멀티 스레드는 멀티 프로세스보다 적은 메모리 공간을 차지하고 Context Switching 이 빠르다는 장점이 있지만, 오류로 인해서 하나의 스레드에 문제가 생기면 다른 스레드에도 문제가 생길 수 있다는 점과 동기화 문제를 가지고 있다. 반면에 멀티 프로세스는 하나의 프로세스가 문제가 생기더라도 다른 프로세스에는 영향을 끼치지 않는다는 장점이 있지만, 멀티 스레드보다 많은 메모리 공간과 CPU 시간을 차지한다는 단점이 있다.

이 두가지는 동시에 여러 작업을 수행한다는 점을 동일하지만 적용해야 하는 시스템에 따라 멀티 프로세스와 멀티 스레드를 잘 선택해야 한다.

# 3. Context Switching
Context Switching 이란 하나의 프로세스가 CPU를 사용중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태를 보관하고 새로운 프로세스의 상태를 적재하는 작업을 말한다. 한 프로세스의 문맥(상태)은 프로세스 제어 블록(PCB)에 기록되어 있다.

<p align="center"><img src="https://s3.ap-south-1.amazonaws.com/afteracademy-server-uploads/what-is-context-switching-in-operating-system-context-switching-flow.png"></p>

P1 -> P2, P2 -> P1 의 Context Switching 과정은 다음과 같다.

1. P1의 실행 중에 Interrupt 또는 System call 이 발생하면 P1은 idle 상태가 된다.
2. P1이 executing 상태에서 idle 상태로 변할 때 프로세스와 실행에 대한 데이터는 레지스터에 존재하는데 이 데이터를 메모리에 저장한다.(save state into PCB1)
3. 그 다음 P2가 실행되는데 P2가 실행되기 위해서 메모리에 존재하는 P2에 대한 데이터를 레지스터에 올려야 한다.(reload state from PCB2)
4. P2가 실행되었다가 다시 idle 상태로 변경되면서 자신의 데이터를 메모리에 저장한다.(save state into PCB2)
5. 다음에 P1이 실행되는데 P1의 데이터를 메모리에 저장했었다. 이 데이터를 읽어서 레지스터에 올리고 P1을 이전에 멈추었던 시점에서 이어서 실행한다.

# 참고
* https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4
* https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%A0%88%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85)
* https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html
* https://doorbw.tistory.com/26
* https://afteracademy.com/blog/what-is-context-switching-in-operating-system

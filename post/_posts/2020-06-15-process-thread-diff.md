---
layout: post
title: 프로세스(Process)와 쓰레드(Thread)의 차이
noindex: true
comments: true
---
평소에 프로세스(Process)와 쓰레드(Thread)에 대한 이야기를 많이 한다.<br>
대략적으로 이 둘의 차이는 알지만 정확히 어떤 차이가 있는지<br>
자세히 정리하려고 한다.
---

### 프로세스(Process) 정의
---
프로세스(Process) 정의는 아래와 같다.
- 컴퓨터에서 연속적으로 **실행되고 있는 프로그램**
- 메모리에 올라와 실행되고 있는 프로그램의 인스턴스(독립적인 개체)
- 운영체제로부터 시스템 자원을 할당받는 작업(Job)의 단위
- 동적인 개념으로는 **실행된 프로그램**을 의미

즉, 프로세스(Process)란 `CPU에 의해 처리되고 있는 프로그램`을 의미하며, `작업(Job)` 혹은 `태스크(Task)`라고도 한다.
예를 들어, 하드디스크에 있는 프로그램을 실행하면 실행을 위해서 **메모리 할당**이 이루어지고 할당된 메모리 공간에 **Binary Code**가 올라가게 된다.
이 순간부터 프로세스(Process)라고 불리게 된다.

아래는 프로세스(Process)의 구조이다.

![process](/assets/img/posts/process.png)

Process의 구조에 대해 간략하게 설명하면,
- Code : `프로그램을 실행시키는 실행 파일 내의 명령어들이 올라가는 영역` 즉, 프로그램의 Source Code가 올라간다고 생각하면 된다.
- Data : `전역변수, static 변수`가 올라가는 영역
- Heap : `동적할당을 위한 메모리 영역` 예를 들면, java에서 `new 연산자`를 사용하여 생성한 인스턴스가 올라가는 영역이라고 보면 된다.
- Stack : `지역변수, 함수 호출 시 전달되는 인자`를 위한 메모리 영역

프로세스(Process)의 특징은 아래와 같다.
- 프로세스(Process)당 **최소 1개의 메인 쓰레드(Main Thread)**를 가지고 있다.
- 각 프로세스(Process)는 별도의 주소 공간에서 실행되며, **다른 프로세스(Process)의 변수나 자료구조에 접근할 수 없다.**
- 다른 프로세스(Process)에 접근하기 위해서는 **프로세스 간 통신(IPC : Inter-process-communication)**을 사용하여야한다.

#### IPC 란 ? <br>
> 프로세스(Process)들 사이에서 서로 데이터를 주고 받는 행위를 의미한다.<br>
> 즉, 컴퓨터 내부에서 프로세스(Process)들 끼리 데이터를 주고 받기 위한 일종의 통신이라고 생각하면 된다.<br>
> 참고로, 프로세스(Process)간 통신을 위해 PIPE같은 개념이 등장하였다.



### 쓰레드(Thread) 정의
---
쓰레드(Thread) 정의는 아래와 같다.
- 프로세스(Process) 내에서 실행되는 여러 흐름의 단위
- 프로세스(Process)의 특정한 수행 경로
- **프로세스(Process)가 할당받은 자원을 이용하는 실행의 단위**

즉, 쓰레드(Thread)란 `프로세스(Process) 내부의 작업의 흐름, 단위`라고 생각하면 된다.
쓰레드(Thread)는 한 프로세스(Process) 내부에 적어도 하나 이상이 존재하며, 만약 쓰레드(Thread)가 2개 이상의 여러개가 존재한다면 이를 `멀티 쓰레드(multi Thread)`라고 한다.

아래는 쓰레드(Thread)의 구조이다.

![thread](/assets/img/posts/thread.png)

쓰레드(Thread)의 특징은 아래와 같다.
- 쓰레드(Thread)는 프로세스(Process) 내에서 각각 **스택(Stack)만 따로 할당**받고, **이외의 Code, Data, Heap은 공유**한다.
- 쓰레드(Thread)는 하나의 프로세스(Process) 내에서 동작되는 흐름, 단위 이기 때문에 **프로세스(Process) 내의 자원들을 서로 공유**하며 수행된다.
- 하나의 쓰레드(Thread)가 프로세스(Process)의 자원을 변경하면, 다른 이웃 쓰레드(sibling thread)도 그 변경 결과를 즉시 볼 수 있다.

### 멀티 프로세스(multi Process)와 멀티 쓰레드(multi Thread) 차이
---
- 멀티 프로세스(multi Process)
  - 하나의 프로그램을 여러 개의 프로세스(Process)로 구성하여 각 프로세스(Process)가 하나의 작업(Job) 혹은 태스크(Task)를 처리하도록 하는 것이다.
  - 장점
    - 여러 개의 프로세스(Process) 중 하나에 문제가 발생하더라도 해당 프로세스(Process)만 죽는 것 외에는 다른 영향이 확산되지 않는다.
  - 단점
    - **컨택스트 스위칭(Context Swtiching)의 오버헤드(OverHead) 발생**
      - 컨택스트 스위칭(Context Switching) 과정에서 캐시 메모리(Cache Memory) 초기화 등 무거운 작업이 진행되야 하며, 많은 시간이 소모 되는 등의 오버헤드(OverHead)가 발생한다.
      - 프로세스(Process)는 각각 **독립적**이기 때문에 **컨택스트 스위칭(Context Switching)이 발생하면 캐시 메모리(Cache Memory)에 있는 모든 데이터를 리셋**하고 해당 프로세스(Process)의 데이터(Data)를 다시 불러와야 한다
      - 프로세스 간 통신(IPC)을 사용해야 하지만, 어렵고 복잡한 기법이다.

#### 컨택스트 스위칭(Context Switching) 이란 ? <br>
> CPU에서 여러 프로세스(Process)를 돌아가면서 작업을 처리하는데 이 과정을 말한다.<br>
> 즉, CPU사용 권한을 프로세스(Process)들 끼리 넘겨주는 과정을 말한다.<br>
> 동작 중인 프로세스(Process)가 대기 상태로 되면 해당 프로세스(Process)의 상태(Context)를 보관하고
> 대기하고 있던 다음 프로세스(Process)가 동작하면서 해당 프로세스(Process)의 상태(Context)를 복구하는 작업을 말한다.

- 멀티 쓰레드(multi Thread)
  - 하나의 프로그램을 **여러 개의 쓰레드(Thread)로 구성**하고 각 Thread로 하여금 하나의 작업(Job) 혹은 태스크(task)를 처리하도록 하는 것이다.
  - 웹 서버는 대표적인 멀티 쓰레드(multi Thread) 응용 프로그램이다.
  - 장점
    - 프로세스(Process)를 생성하여 자원을 할당하는 시스템 콜이 줄어들어 자원을 효율적으로 관리할 수 있다.
    - 프로세스(Process) 간 통신보다 하나의 프로세스(Process)내에서 여러 쓰레드(Thread) 간 통신이 비용이 훨씬 적기때문에 작업(Job)들 간 통신 부담을 줄일 수 있다.
    - 쓰레드(Thread) 간 사이의 컨택스트 스위칭(Context Switching) 이 훨씬 빠르다.
    - 쓰레드(Thread)는 프로세스(Process) 내의 **스택(Stack) 영역을 제외한 모든 메모리를 공유**하기 때문에 통신의 부담이 적다.
  - 단점
    - 주의 깊은 설계가 필요하다.
      - 멀티 쓰레드(multi Thread)의 경우 **자원 공유의 문제**가 발생한다.(동기화 문제)
    - 디버깅이 까다롭다.
    - 다른 프로세스(Process)에서 쓰레드(Thread)를 제어할 수 없다. 즉, 프로세스(Process) 밖에서 쓰레드(Thread) 각각을 제어할 수 없다.
    - **하나의 쓰레드(Thread)에 문제가 발생하면 전체 프로세스(Process)가 영향을 받는다.**

### 멀티 프로세스(multi Process) 대신 멀티 쓰레드(multi Thread)를 사용한다.
---
![multi-thread](/assets/img/posts/multi-thread.png)
여러 개의 프로그램을 키는 것 보다 하나의 프로그램안에서 여러 작업(Job)을 해결하는 것이다.
자세히 설명하자면 아래와 같다.
- 자원의 효율성 증대
  - 멀티 프로세스(multi Process)로 실행되는 작업(Job)을 멀티 쓰레드(multi Thread)로 실행할 경우, 프로세스(Process)를 생성하여 자원을 할당하는 System Call이 줄어 들어 자원을 효율적으로 관리할 수 있다. 
  - 프로세스(Process) 간 컨택스트 스위칭(Context Switching)은 단순 CPU 사용 권한 뿐 아니라 램(Ram)과 CPU 사이의 캐시 메모리(Cache Memory)에 대한 Data까지 초기화 해야되기 때문에 오버헤드(OverHead)가 크다.
  - 쓰레드(Thread)는 프로세스(Process)내의 메모리를 공유하기 때문에 독립적인 프로세스(Process)와 달리 쓰레드(Thread) 간 데이터를 주고 받는 것이 간단해지고 시스템 자원 소모가 줄어든다.
- 처리 비용 감소 및 응답 시간 단축
  - 프로세스 간 통신(IPC)보다 쓰레드(Thread) 간 통신의 비용이 적기 때문에 작업(Job) 간 통신 부담이 줄어든다.
  - 즉, 쓰레드(Thread)는 스택(Stack) 영역을 제외한 모든 메모리를 공유하기 때문에 부담이 적다.
  - 프로세스(Process) 간 전환 속도보다 쓰레드(Thread) 간 전환 속도가 빠르다.
  - 즉, 쓰레드(Thread)는 프로세스(Process) 내에서 **스택(Stack) 영역을 제외한 나머지 영역은 공유**하기때문에, 
  **컨택스트 스위칭(Context Switching) 시 스택(Stack)만 초기화**하면 되므로 빠르다.
- 주의 사항
  - 쓰레드(Thread) 간 동기화 문제를 잘 생각하여야 한다.
    - 쓰레드(Thread) 간 메모리 영역이 공유되기 때문에 함께 사용하는 경우 충돌이 발생할 수 있다.

## 참고
---
- [Wikipedia: 프로세스](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4)
- [Wikipedia: 쓰레드](https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%A0%88%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85))
- [프로세스와 스레드의 차이](https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html)
- [프로세스란?](https://blockdmask.tistory.com/22)




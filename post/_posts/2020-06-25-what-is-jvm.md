---
layout: post
title: JVM(Java Virtual Machine)이란 ?
noindex: true
comments: true
---

여짓껏(?) 가장 많이 사용한 언어가 자바 이다. 자바를 배우다 보면 많이 등장하는 단어가 있는데 바로 JVM(Java Virtual Machine)이다.<br> 오늘은 JVM이 어떻게 구성되어있고 또 어떤 방식으로 동작하는지 한번 알아보려고 한다.

---

### JVM(Java Virtual Machine) 이란 ?
---
JVM(Java Virtual Machine) 정의는 아래와 같다.<br>
>자바 바이트 코드를 실행할 수 있는 주체이다. 일반적으로 **인터프리터나 JIT 컴파일방식**으로 다른 컴퓨터 위에서 바이트코드를 실행할 수 있도록 구현되나 jop 자바 프로세서처럼 하드웨어와 소프트웨어를 혼합해 구현하는 경우도 있다. (이론적으로는 100% 하드웨어 구현도 가능하나 비효율적이다.)<br> 자바 바이트코드는 **플랫폼에 독립적**이며 모든 자바 가상 머신은 자바 가상 머신 규격에 정의된 대로 자바 바이트코드를 실행한다. 따라서 표준 자바 API까지 동일한 동작을 하도록 구현한 상태에서는 **이론적으로 모든 자바 프로그램은 CPU나 운영 체제의 종류와 무관하게 동일하게 동작할 것을 보장한다.**

즉, 쉽게 말하면 JVM은 자바를 코딩하고 실행하기 위한 일종의 `가상 컴퓨터`라고 생각하면 된다. 결국 **운영체제에 관계없이 JVM이 설치 되어있으면 어떤 곳에서든 실행이 가능**하단 이야기이다.
그러면, 자바 코드를 이용하여 JVM은 무엇을할까?<br>JVM(Java Virtual Machine)의 역할은 다음과 같다.
- Java Application을 `클래스 로더(Class Loader)`를 통해 읽어 들여 자바 API와 함께 실행한다.
- JVM은 자바와 운영체제 사이에서 `중개자 역할`을 수행하여 운영체제에 **독립적인 플랫폼**을 갖게 해준다.
 즉, 운영체제의 메모리 영역에 직접 접근하지 않고 JVM이라는 가상 머신을 통해 **간접적으로 접근**하게 한다.
 - C 언어의 경우 개발자가 직접 메모리 할당과 회수를 해줘야되지만, 자바의 경우 JVM이 메모리 관리를 알아서 해준다. (GC, Garbage Collector)

JVM의 역할은 위의 3가지 정도로 정리할 수 있다. <br>그럼 여기서 우리가 만든 자바코드가 JVM을 통해 실행되는 과정은 어떻게 될까?


### Java Application 실행 과정
---
Java Application을 실행하게되면 아래의 과정을 거쳐 실행하게 된다.

![java-application-process](/assets/img/posts/java-application-process.png)

1. Application이 실행되면 JVM은 운영체제로부터 필요로 하는 `메모리`를 할당받는다.<br> 그러면, JVM은 메모리를 용도에 맞게 여러 영역으로 나누어 관리한다.
2. 자바 컴파일러(javac)는 Java Application의 자바 소스코드(.java)를 읽어들여 자바 바이트코드(.class)로 변환시킨다.
3. 클래스 로더(Class Loader)를 통해 class 파일들을 JVM으로 로딩한다.
4. 로딩된 클래스 파일(.class)들은 실행 엔진(Execution Engine)을 통해 `해석`된다.
5. 해석된 바이트코드(.class)는 **런타임 데이터 영역(Runtime Data Areas)에 배치**되고 수행이 이루어지게 된다.
6. 이러한 과정 속에 JVM은 필요에 따라 GC(Garbage Collector) 같은 관리 작업을 계속 수행한다.

위의 과정을 통해 Java Application은 동작하게된다. <br> 아래는 각 과정별에 등장한 요소들에 대한 간략한 설명이다.
- 자바 컴파일러(Java Compiler)<br>
자바 소스코드(.java)를  바이트 코드(Byte Code(.class))로 `변환`하는 역할을 한다.
- 클래스 로더(Class Loader)<br>
자바의 바이트 코드(Byte Code)를 읽어서 JVM의 실행 엔진(Execution Engine)이 사용할 수 있도록 **런타임 데이터 영역(Runtime Data Area)에 적재하는 역할**을 한다.
- 실행 엔진(Execution Engine)<br>
클래스(.class)를 실행시키는 역할을 한다. 실행 엔진(Execution Engine)은 **메모리에 적재된 바이트 코드(Byte Code)를 실제로 JVM 내부에서 기계(Machine)가 실행할 수 있는 형태로 변경**한다. <br>이 때에는 두가지 방식을 사용하게 된다.
	- 인터프리터(Interpreter)<br>
	명령어를 그때 그때 해석해서 실행하는 방식이다.
	- JIT(Just-In-Time)<br>
	인터프리터(Interpreter)의 단점(매번 해석하기 때문에 느리다.)을 보완하기 위해 도입된 `컴파일러(Compiler)`이다.<br> 인터프리터(Interpreter) 방식으로 실행하다가 **적절한 시점에서 바이트 코드(Byte Code) 전체를 컴파일(Compile)하여 네이티브 코드(Native Code)로 변경하고, 이후에는 더 이상 인터프리팅(Interpreting_해석)하지 않고 네이티브 코드(Native Code)로 직접 실행하는 방식이다.** 네이티브 코드(Native Code)는 캐시(Cache)에 보관하기 때문에 한 번 컴파일(Compile)된 코드는 빠르게 수행한다.
- 런타임 데이터 영역(Runtime Data Areas)<br>
런타임 데이터 영역(Runtime Data Areas)은 Application을 수행하기 위해 **운영체제에서 할당받는 메모리 영역이다.** 즉, 다시 말해 JVM이 프로그램을 수행하기 위해 운영체제에서 할당받는 JVM 메모리영역이라 할 수 있다. 이 영역은 5개의 영역으로 나눌 수 있다. 이 중 PC 레지스터(Program Counter Register), 스택(Stack Area), 네이티브 메서드 스택(Native Method Stack)은 쓰레드(Thread)마다 하나씩 생성되며 **힙(Heap Area), 메소드 영역(Method Area) 은 모든 쓰레드(Thread)가 공유**해서 사용한다.

![jvm-runtimearea](/assets/img/posts/jvm-runtimearea.png)

- PC 레지스터(Program Counter Register)<br>
쓰레드(Thread)가 시작될 때 생성된다. PC 레지스터(Program Counter Register)는 **쓰레드(Thread)가 어떤 명령어로 실행되어야 할지에 대한 기록을 하는 부분**이다. <br> 즉, **현재 쓰레드(Thread)가 실행되는 부분의 주소와 명령을 저장하고 있는 영역**이다.
- 스택 영역(Stack Area)<br>
Application 실행 과정에서 임시로 할당되었다가 메소드(method)를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역이다. <br>**지역 변수, 매개 변수, 메소드 정보, 연산 중 발생하는 임시 데이터 등이 저장** 된다. 메소드(Method) 호출 시마다 각각의 스택 프레임(Stack Frame_그 메소드(Method)만을 위한 공간)이 생성된다. 메소드(Method) 수행이 끝나면 프레임(Frame) 별로 삭제한다.<br> 예를 들어, `int a = 10;` 소스를 작성했다면 정수값이 할당될 수 있는 **메모리공간을 a라고 잡아두고 그 메모리 영역에 값이 10이 들어간다.**
또, `Person p= new Person();` 이라는 소스를 작성했다면 **Person p 는 스택 영역(Stack Area)에 생성**되고 **new로 생성된 Person클래스의 인스턴스(instance)가 힙 영역(Heap Area)에 생성**된다.<br>그리고 **스택 영역(Stack Area)에 생성된 p의 값으로 힙 영역(Heap Area)의 주소값을 가지고 있다.** <br>즉, 스택 영역(Stack Area)에 생성된 p가 힙 영역(Heap Area)에 생성된 객체를 가리키고(참조하고)있는 것이다.
- 네이티브 메서드 스택(Native Method Stack)<br>
일반적으로 JVM은 네이티브(Native) 방식을 지원한다. 쓰레드(Thread)에서 네이티브(Native) 방식의 메소드(method)가 실행되는 경우 네이티브 메서드 스택(Native Method Stack)에 쌓인다. <br> 메소드(method)를 실행하는 경우 스택 영역(Stack)에 쌓이다가 해당 메소드(method) 내부에 네이티브(Native) 방식을 사용하는 메소드(method)가 있다면 해당 메소드(Method)는 네이티브 메서드 스택(Native Method Stack)에 쌓인다.<br>**즉, 자바 외 언어로 작성된 네이티브 코드(Native Code)를 위한 메모리 영역이라고 보면된다. 보통 C/C++등의 코드를 수행하기 위한 영역이다.(JNI)**
- 힙 영역(Heap Area)<br>
동적으로 할당되는 데이터가 저장되는 영역이다. <br>예를들어 `객체`, `배열`등이 생성되었을 때 **저장되는 공간**이다. 힙(Heap)에 할당된 데이터는 GC(Garbage Collector)의 대상이다.<br> JVM 성능 등의 이슈에서 가장 많이 언급되는 공간이다. 
- 메소드 영역(Method Area)<br>
메소드 영역(Method Area)은 **모든 쓰레드(Thread)가 공유하는 영역**으로 JVM이 시작될 때 생성된다.<br> JVM이 읽어 들인 각각의 클래스(Class)와 인터페이스(Interface)에 대한 런타임 상수 풀(Runtime Constant Pool) 필드와 메소드 코드(method Code), Static 변수, 메소드(Method)의 바이트코드(Byte Code) 등을 보관한다.
- 런타임 상수 풀(Runtime Constant Pool)<br>
메소드 영역(Method Area) 내부에 존재하는 영역으로, 각 클래스(Class)와 인터페이스(Interface)의 상수(Constant)뿐만 아니라, 메소드(Method)와 필드(Field)에 대한 모든 레퍼런스(Reference)까지 담고 있는 테이블이다. <br>**즉, 상수 자료형을 저장하여 참조하고 중복을 막는 역할을 수행한다.**

여기까지가 전반적인 Application이 실행되면서 JVM이 하는 역할과 구조이다.<br> 여기서 JVM의 성능 등의 가장 많이 언급되는 부분이 힙 영역(Heap Area)인데 이 부분에 대해서는 추후 GC(Garbage Collector)의 동작 방식에 대해 공부할 때 같이 정리를 하려고 한다.


  
### 참고
---
- [Wikipedia: 자바 가상 머신](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EA%B0%80%EC%83%81_%EB%A8%B8%EC%8B%A0)
- [자바가상머신(JVM)이란 무엇인가?](https://hanul-dev.netlify.app/java/%EC%9E%90%EB%B0%94%EA%B0%80%EB%A8%B8%EC%8B%A0(jvm)%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80/)
- [JVM 이란 ?](https://skibis.tistory.com/330)

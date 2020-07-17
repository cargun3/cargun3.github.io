---
layout: post
title: 쓰레드 세이프(Thread) 란
noindex: true
comments: true
---

프로그래밍을 하다보면 항상 필수요소라고 생각될 만큼 따라오고 또 들리는 단어들 중 하나가 바로 쓰레드(Thread)이다. 쓰레드를 사용함으로써 좀 더 다양한 잡(Job)이 가능한 프로그램을 만들 수가 있지만, 그것과 더불어 항상 쓰레드(Thread)에서 나오는 이슈는 찾기도 또 잡기도 어렵다. 그래서 이번에는 그러한 이슈를 방지하기 위해 필요한 내용인 쓰레드 세이프(Thread Safe) 에 대해 알아보려고 한다.

---

### 쓰레드 세이프(Thread Safe) 정의
---
쓰레드 세이프(Thread Safe) 정의는 아래와 같다.<br>
>멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 여러 스레드로부터 **동시에 접근**이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다. <br>보다 엄밀하게는 하나의 함수가 **한 스레드로부터 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하여 동시에 함께 실행**되더라도 각 스레드에서의 함수의 수행 결과가 올바로 나오는 것으로 정의한다.

[저번 글](https://cargun3.github.io/post/2020-06-15-process-thread-diff/)에 올렸던 내용 중에 쓰레드(Thread)는 프로세스(Process)와 달리 공유된 자원을 사용하면서 Job(잡)을 실행한다고 하였는데 문제는 공유된 자원을 사용하기 때문에 쓰레드(Thread)를 활용해 코딩했을 때 기대했던 기대값과 실제 나오는 값이 다른 경우가 생기게 된다. 그래서 의도한 결과값이 나오게 하기 위해 나온 개념이 쓰레드 세이프(Thread Safe)이다.

그럼 어떻게 우리가 의도한 값과 실제 결과값이 달라질 수 있는지 아래의 예를 한번 살펴보자.

먼저 아래와 같은 class가 있다고 가정해보자.

```java
public class Calculator {
	private int index;

	public Calculator(index) {
		this.index = index;
	}

	public int getNextIndex() {
		return ++index;
	}
}
```
이 class는 Integer Type인 index라는 멤버변수를 가지고 getNextIndex()라는 함수를 호출하면 해당 index가 `1씩 증가`한다.

우선, 아래와 같이 main함수를 통해 2개의 쓰레드(Thread)를 생성하여 실행해보자.

```java
public static void main(String[] args) throws Exception {
	Calculator calculator = new Calculator(0);
	Set<Integer> set1 = new Set<Integer>();
	Set<Integer> set2 = new Set<Integer>();	
	
	Thread thread1 = new Thread(() -> {
		for(int i = 0; i < 100000; i++) {
			set1.add(calculator.getNextIndex());
		}
	});
	
	Thread thread2 = new Thread(() -> {
		for(int i = 0; i < 100000; i++) {
			set2.add(calculator.getNextIndex());
		}
	});
	thread1.start();
	thread2.start();
	
	Thread.sleep(10000);
	
	System.out.println(set1.size());
	System.out.println(set2.size());
}
```
해당 코드에 대한 결과값은 se1과 set2 모두 `100000`이 나올거라고 예상된다.
하지만 실제 결과는 아래와 같이 나오게 된다.(참고로 쓰레드(Thread)를 돌리기 때문에 매번 결과값이 달라질 수 있다. 즉, se1, set2 모두 100000이 나올 수도 있다.)

![multi-thread-result](/assets/img/posts/multi-thread-result.png)

그러면, 왜 우리가 예상했던 값이 아닌 심지어는 매번 `다른 결과`가 나오게 되는 건지 생각해보자.

위의 예제에서 결과가 다르게 나오는 이유는 두개의 쓰레드(Thread)에서  calculator.getNextindex() 함수 실행 시 index에 대한 값이 동일하게 중복되어 들어가는 경우가 있기 때문에 우리가 예상하는 Set의 사이즈가 100000이 아니게 되는 상황이 발생한다.(참고로 Set 컬렉션은 중복을 허용하지 않는다.)

위의 말이 좀 이해가 잘 되지 않는다. 그래서 조금 더 쉽게 설명을 위해서는 우선 **원자적 연산 (atomic operation)**에 대해 먼저 알아아야 한다.

### 원자적 연산 (atomic operation)
---
원자적 연산(atomic operation)에 정의는 아래와 같다.
>**기능적으로 분할할 수 없거나 분할되지 않도록 보증된 조작.**<br> 원자와 같이 분할할 수 없다는 것을 비유하여 원자조작은 끼어들기가 불가능하며, 만일 중지되면 동작 개시 직전의 상태로 시스템을 복귀시킬 것을 보증하는 복구(백업과 복원)기능이 제공된다.

즉, 쉽게 말하면 원자적 연산이란 **중단이 불가능한 연산**을 의미하고 **연산의 최소 단위**를 의미하며, **쓰레드(Thread)가 절대 간섭할 수 없는 연산을 의미한다.**

자바(Java)에서는 **메모리에 값을 할당하는 연산은 기본적으로 원자적 연산(atomic operation)에 해당**한다. 즉, 위의 예제에서 index=0은 **중단이 불가능한 연산**이다.
조금 더 자세히 보면, 쓰레드(Thread)가 생성될 때는 해당 쓰레드(Thread)를 위한 스택(Stack)이 만들어진다. ([저번 글 참고](https://namu.wiki/w/%EC%84%B8%EB%A7%88%ED%8F%AC%EC%96%B4)) 이 스택(Stack)을 자세히 살펴보면 안에는 프레임(Frame)이라는 것이 들어간다. 프레임(Frame)은 모든 메서드 호출에서 만들어지는 것으로 매개변수, 지역변수, 반환주소를 포함한다.<br>그렇다면 index 값을 초기화하는 연산을 바이트 코드를 그림으로 표현해보자.<br>

![atomic-thread-stack](/assets/img/posts/atomic-thread-stack.png)

1. 맨 처음 메서드가 호출 될 때 다음과 같이 this (현재객체) 가 스택에 쌓인다. 
2. 상수값이 그 위에 쌓이고 이 상수 값을 index 변수에 넣어서 초기화를 한 뒤 프레임이 비워지게 된다.

이 연산의 결과는 상수 값을 초기화를 하는 일이므로 여러 쓰레드(Thread)가 수행하더라도 결과는 같게 된다.
**하지만, 문제는 ++ 연산이다.**  이 연산은 아래와 같은 단계로 이루어지게 된다.
3. 변수의 값을 가져온다.
4. 변수의 값을 증가한다.
5. 변경된 변수를 저장한다.

그래서 ++연산은 원자적 연산(atomic operation)에 속하지 않는 연산자이다. 이 말은 즉, **연산의 최소 단위가 아니며, 쓰레드(Thread)가 간섭할 수 있다**는 말이 된다.

![mutli-thread-stack](/assets/img/posts/mutli-thread-stack.png)

이 내용을 위의 예제에 적용 해보면,
>쓰레드1번에서 최초 프레임(Frame)을 만들고 this 를 로딩한다.<br>
변수의 ++를 호출하기 위해 값을 가져온다.(get field)<br>
예를 들어, 10을 가져왔다고 한다.<br>
그런데 갑자기 쓰레드2 가 끼어든다.<br> 
**쓰레드 1은 인덱스를 1증가 시키려고 프레임(Frame)에 index 값(10)을 로드한 상태로 멈춰버린다.**<br>
쓰레드2가 index를 가져오고 값을 1증가시키고 리턴까지 해버린다.<br>
결국 리턴 값은 11이 된다. calculator index의 값은 11로 증가된 상태로 변한다.<br>
이제 쓰레드 1이 재개된다.<br>
이미 프레임(Frame)에 올라와 있는 값은 10인 상태 이므로 1증가 시켜서 11로 만들고 리턴을 한다.<br>
결국 calculator 의 index 를 다시 11로 덮어 쓴다.<br>

위의 상태가 쓰레드1,쓰레드2가 돌면서 반복되다 보니 최초 결과 값이 예상치 못한 결과값이 나오는 형태가 된다.(쓰레드의 개입시점은 매번 다르기 때문에 결과가 매번 다르게 나올 수 있다.)
결국 ++연산은 원자적 연산(Atomic operation)이 아니기 때문에 발생되는 문제이다.

그러면, 어떻게 하면 우리가 원하는 결과가 나오도록 쓰레드(Thread)의 개입을 방지할 수 있을까?

### 쓰레드 세이프(Thread Safe)
---
쓰레가(Thread)간 개입을 방지하기 위해선 여러가지 방법이 존재한다.
**자바 기준**으로 우선 대표적으로 메소드(method) 앞에  `synchronized` 키워드를 붙혀 감싸면 된다.
이 것을 `동기화`라고 한다. 이 방식으로 `동기화`를 시키면 저 메소드(method)를 수행할 때 **다른 쓰레드가 간섭하지 못하게 된다.**

이 외에도 AtomicInteger 라는 것을 사용하는 방법있다. 이름에서도 보듯이 원자 연산을 지원하는 클래스라고 한다. AtomicInteger 외에도 AtomicBoolean, AtomicReference 등 여러 원자 연산을 지원하는 클래스가 있다.
그러면 `synchronized` 만 사용하면 쓰레드(Thread)간 간섭을 제어하여 쓰레드 세이프(Thread Safe)가 되는 것일까? 물론 가능은 하겠지만 단점도 존재한다. 
**synchronized 의 단점은 스레드가 침범하지 않더라도 항상 락(lock) 을 거는 것이다**. 
자바 버전이 올라가면서 내장 락의 성능이 올라가긴 하지만 여전히 비용이 비싼 것이 큰 단점이다.
이런 잠금 형태를 보고 **비관적 잠금(pessimistic locking)** 이라고 한다. 즉, 자원 경합이 일어날 것으로 보고 독점적으로 자원을 잠그는 형태이다. 

이러한 방식 외에도 **낙관적 잠금(Compare And Swap(CAS))** 의 형태도 있다. 
이름 그대로 비교하고 잠그는 형태인데, 이 것은 쓰레드(Thread)가 같은 자원으로 경쟁하는 일이 잦지 않다는 가정에서 출발한다. 락(lock)을 거는 쪽보다 문제를 감지하는 쪽이 비용이 적게 들기 때문에 더 효율적이라고 볼 수 있다.
즉, 낙관적 잠금(CAS)은 원자적 연산이며 공유변수를 갱신하려 든다면 현재 변수값이 최종으로 알려진 값인지 확인을 하고 맞다면 락을 걸지 않고 데이터를 변경한다.
그렇지 않다면 다른 쓰레드(Thread)가 끼어든 것이므로 갱신하지 않는다.

이 처럼 쓰레드(Thread)간 간섭을 제어하기 위한 여러 기법들이 존재하는데 이 부분엔 몇 가지 원칙이 존재한다.

### 동시성 방어 원칙
---
- #### 단일 책임 원칙
	- **단일 책임원칙**은 주어진 메소드(method)/클래스(class)/컴포넌트(component)를 **변경할 이유가 하나여야 한다는 것이다.** 
	- 동시성에 관한 내용은 `동시성`이라는 복잡성 하나만으로 분리할 이유가 충분하기 때문에 동시성 코드는 다른 코드와 분리해야 한다는 것이다.  
- #### 임계영역  보호
	- 공유 객체를 사용하는 코드 내 **임계영역(critical section)을 synchronized 키워드로 보호하는 것이 권장**된다. 
	- 중요한 부분은 **필요한 임계영역을 구분**하는 것이다. 임계영역 개수를 줄인다고 **거대한 영역 하나에 몰아 넣는 것은 프로그램 성능이 떨어지는 원인**이기 때문에 필요한 부분에 **최대한 작게 설정**해야 한다.
- #### 사본 이용
	- 공유 객체를 피하는 방법이 있다면 코드 상에서 문제를 일으킬 가능성이 낮아진다. 
	- 사본을 이용하여 동기화를 피할 수 있다면, **synchronized 를 수행하는 비용을 없앨 수 있어 부하를 낮추는 것이 가능**하다. 단, 객체를 복사하는 비용을 잘 고려해야 한다.
- #### 라이브러리(Library) 혹은 프레임워크(Framework) 활용
	- 자바로 쓰레드(Thread)를 사용할 때는 쓰레드(Thread) 환경에 안전한 컬렉션을 사용하거나 Executor 라는 프레임워크를 이용한다.

결국 쓰레드(Thread)간 간섭을 줄이기 위해서는 이러한 원칙들을 잘 지켜서 쓰레드(Thread) 를 설계하는게 중요하다. 특히 자바에서는 Executor라는 프레임워크(Framework)를 제공하는데 이부분에 대해서도 알아보자.

### Executor 프레임워크(Framework)
---
자바에서는  **Executor 라는 프레임워크(Framework)를 지원**한다. 
이 것은 쓰레드(Thread) 풀링으로 정교한 실행을 지원한다고 한다. 그리고 쓰레드 풀(Thread Pool)을 관리하고, 풀 크기를 조정하는 등 다양한 기능을 지원한다. 게다가  `Future`  라는 것을 사용해 결과값이 나올 때 까지 기다릴수 있다고 한다. `Future`는 독립된 연산이 모두 끝나기를 기다릴 때 유용하다.

처음 예제에는 10 만번의 연산을 기다리기 위해 강제로 sleep을 주었는데 `Future` 를 사용한다면 연산이 끝날 때 까지 기다릴 수 있게 설정할 수 있다.

위의 계산기 예제를 Executor 를 사용한 코드로 변경해보자.

![executor-framework](/assets/img/posts/executor-framework.png)

Runnable 과 달리 Callable 은 반환형이 있다는 것을 확인할 수 있다. 
ExecutorService 는 쓰레드 풀(Thread Pool)의 개수를 지정할 수 있습니다. 각 쓰레드(Thread)에서 독립적으로 연산을 실행한 후 isDone 메서드를 호출하여 검사한다.

  
### 참고
---
- [Wikipedia: 쓰레드 세이프](https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%A0%88%EB%93%9C_%EC%95%88%EC%A0%84)
- [Immutable class와 thread-safe](https://pjh3749.tistory.com/268)
- [Thread(스레드)에 관한 고찰 - 스레드 관련 코드를 어떻게 짜야할까](https://pjh3749.tistory.com/268)

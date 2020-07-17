---
layout: post
title: JVM heap 영역과 CG
noindex: true
comments: true
---

이전에는 전반적으로 JVM의 구조와 우리가 작성한 Java 파일이 어떻게 적재되는지에 대한 과정에
대해 알아 봤다.
이번에는 JVM 구조 중에 특히 heap 영역과 이 heap영역에 있는 data가 어떻게 정리되는지 JVM에서 작동하는
Gabage Collection 에 대해 알아보려고 한다.

---

### JVM Heap 영역
---

저번 글에서 봤던 JVM의 Run Data Area중에 우리가 가장 많이 듣는 영역이 바로 heap 영역일 것이다.
아마 대부분 그 이유가 heap 영역의 memory issue로 인해 application이 죽는 경우도 있기 때문일 것이다.
그러면 도대체 이 heap 영역에는 어떤 것들이 올라가게 되는 걸까?
놀랍게도 heap 영역에는 단지 `Instance`와 `Array 관련 객체` 두 가지 종류만 올라가게 된다.
또 heap은 thread들 간에 공유되는 영역이기도 하다. 
그래서 **heap 영역에 올라간 data 들의 동기화 이슈가 발생하는 곳이기도 하다.**

아래의 코드를 보자.
```java
Car car = new Car();
```
이 코드를 실행하게 되면 heap에는 Car의 Instance가 올라가게 될 것이다. <br>
즉, heap에 할당된 메모리 일부를 car라는 Instance가 차지하게 된다는 것이다.
이제 car를 다 사용했기 때문에 **메모리를 해제하려고 하면 우리에게는 제공되는 함수가 없다.**
그러면 어떻게 해제를 할까?<br>
heap 영역에서의 메모리해제는 오직 `Garbage Collection`을 통해서만 가능하다.
GC는 그럼 어떻게 메모리를 해제할까? GC에 대해 알아보자.

---

### GC 동작 과정
---

먼저 GC가 일어나게 되면, `stop-the-world`가 일어 나게 된다.
**stop-the-world란, GC을 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것이다.**<br>
stop-the-world가 발생하면 GC를 실행하는 쓰레드를 제외한 **나머지 쓰레드는 모두 작업을 멈춘다.**
GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다.

그럼 GC의 동작과정을 살펴보자.

<Java 7 heap구조>
```bash
<----- Java Heap ----->             <--- Native Memory --->
+------+----+----+-----+-----------+--------+--------------+
| Eden | S0 | S1 | Old | Permanent | C Heap | Thread Stack |
+------+----+----+-----+-----------+--------+--------------+
                        <--------->
                       Permanent Heap
S0: Survivor 0
S1: Survivor 1
```

<Java 8 heap구조>
```bash
<----- Java Heap -----> <--------- Native Memory --------->
+------+----+----+-----+-----------+--------+--------------+
| Eden | S0 | S1 | Old | Metaspace | C Heap | Thread Stack |
+------+----+----+-----+-----------+--------+--------------+
S0: Survivor 0
S1: Survivor 1
```

앞에 살펴본 heap 영역은 크게 `Young Generation`과 `Old Generation`으로 나누어 볼 수 있다.
Young Generation은 `Eden` 이란 영역과 `Survivor`이란 영역으로 구성되는데, Eden 영역은 Instance가 heap에 최초로 하랑되는 장소이며,
Eden이 꽉 차게 되면 Instance들의 참조 여부를 판단하여 필요하다고 판단되는 Insatnce를 Survivor영역으로 넘기고, <br>
이 과정이 모두 완료되면 그때부터 GC가 Eden 영역을 청소(memory 해제)한다.
그리고, Survivor영역은 2개의 영역으로 분리되는데 위의 과정과 마찬가지로 Survivor1이 꽉차면 Survivor2로 반복작업을 진행한다.
이러한 과정을 `Minor GC`라고 한다.

위의 과정을 반복하다보면 언젠가는 Survivor2 도 꽉차게 된다.<br>
그러면 이때에도 여전히 참조되고 있는 Instance가 있으면 Old Generation으로 이동하게 된다.<br>
Old Generation은 **새로운 Instance가 들어오는 영역이 아니라 기존 Insatance가 오래 살아남은 즉, 앞으로도 사용될 확률이 높은 Insatance가 저장되는 영역이다.**<br>
그러면 이 Old Generation마저 언젠가는 다 차게 될텐데 그땐 어떻게 되는걸까?<br>
그 때에도 역시 GC가 일어나게 되는게 이 때의 GC를 바로 `Major GC(Full GC)`라고 한다.

>왜 GC가 일어나면 쓰레드가 멈추는 걸까?<br>
>왜냐하면 GC가 일어난 다는 건 heap 영역의 참조 data를 재배치 한다는 말이고,<br>
>heap 영역의 경우 모든 thread가 공유하는 영역이기 때문에 GC가 일어나는 상태에서 thread가 작업을 진행하면<br>
>data 참조에 문제가 발생할 수 있기 때문이다.


---
### Perm 영역과 Metaspace 영역
---
**Perm 영역은 보통 Class의 Class의 Meta 정보나 Method의 Meta 정보, Static 변수와 상수 정보들이 저장되는 공간으로 흔히 메타데이터 저장 영역이다.**<br>
**하지만 해당 영역은 Java 8부터는 Native 영역으로 이동하여 Metaspace영역으로 변경되었다.**<br>
즉, Java 7 까지는 Perm 영역이 존재하지만 Java 8부터는 Perm 영역을 Metaspace영역이 대신한다.<br>
그러면 왜 Java 8 부터는 Perm 영역이 사라진걸까?<br>
이유는 아래와 같다.<br>

> 최근 Java 8에서 JVM 메모리 구조적인 개선 사항으로 Perm 영역이 Metaspace 영역으로 전환되고 기존 Perm 영역은 사라지게 되었다. <br>
> Metaspace 영역은 Heap이 아닌 Native 메모리 영역으로 취급하게 된다. <br>
> (Heap 영역은 JVM에 의해 관리된 영역이며, Native 메모리는 OS 레벨에서 관리하는 영역으로 구분된다) <br>
> Metaspace가 Native 메모리를 이용함으로서 개발자는 영역 확보의 상한을 크게 의식할 필요가 없어지게 되었다.<br>

다시 말해 기존에 Perm영역의 경우 heap 영역에 속하였기 때문에 Perm영역은 반드시 heap의 일정부분을 차지하였다.<br>
그래서 동적으로 로딩되는 Class가 많을 수록 Meta 정보가 많아지기때문에 Permanent Space issue 들이 발생하기도 하였다.<br>
하지만 Java 8부터느 Metaspace가 생겨 각종 Meta 정보를 해당 영역으로 옮겨 Perm 영역의 사이즈 제한을 없앤 것이라 할 수 있다.<br>



### 참고
---
- [JVM 이란 ?](https://skibis.tistory.com/330)
- [JVM 메모리 구조](https://12bme.tistory.com/382)
- [JDK 8에서 Perm 영역은 왜 삭제됐을까](https://johngrib.github.io/wiki/java8-why-permgen-removed)
- [Java Garbage Collection](https://d2.naver.com/helloworld/1329)

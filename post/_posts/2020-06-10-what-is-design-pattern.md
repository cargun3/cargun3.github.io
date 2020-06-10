---
layout: post
title: 디자인패턴이란
noindex: true
comments: true
---
프로그래밍에 대해 공부하다보면 한번쯤은 디자인패턴이란 단어를 들어봤을 것이다.<br>
어떤 것인지는 대략적으로 알겠지만, 막상 실제로 디자인 패턴이 무엇인가요? 라고 물어보면,<br>
디자인패턴은 이런 겁니다 라고 대답하기가 어렵다.<br>
그래서 이번에 디자인패턴에 대한 개념을 알아보려고 한다.<br>

---

## 디자인패턴이란?
---
일상 생활에서 엘레베이터를 기다리는 경험을 생각해보자.
엘레베이터는 2대만 있는 상황에서 엘레베이터를 100명의 사람들이 이용하려면, 
100명이 줄을 서서 이용해야 한다.<br>
그러던 중 한명이 이런 아이디어를 생각해낸다.
>홀수층과 짝수층을 나누어서 이용하게 하면 엘레베이터 이용이 더 편리할 것이다.

이 아이디어를 엘레베이터 이용자 혹은 안내원들에게 전파하게되면,<br>
여기서부터 엘레베이터 이용에 대한 `규칙`이 생겼다고 할 수 있고 나아가 적은 수의 엘레베이터를 운영하는 건물들이 이 `규칙`을 사용하면서 적은 수의 엘레베이터 이용의 `공식`가 될 수 있다.

위의 예를 프로그래밍에 대입해보면, 우리 나라의 모든 개발자들을 한곳에 모아놓고 겪었던 문제들을 이야기 해보면 
분명 겪었던 문제들과 해결하기 위해 사용했던 방법들이 겹치거나 비슷한 내용들이 있을 것이다. 그렇다는 건, 우리보다
훨씬 오랜 선배들도 동일한 형태들이 있었을 것 이다.
그래서 그 중 몇몇 선배들은 이러한 생각을 했다.
> 비슷한 문제를 해결하기 위한 방법을 기억해두고 패턴화 해두면 해당 문제를 만났을 때 잘 해결할 수 있을 것이다.

따라서 일상 생활에 문제를 해결하기 위해 만들었던 어떤 `규칙`을 프로그래밍에 대입하여 생각해보면 어떤 `패턴`이라는 것을 알 수있다.

그래서 디자인패턴에 대한 정의를 위키피디아에서는 아래와 같이 정의하고 있다.
>소프트웨어 디자인 패턴(software design pattern)은 소프트웨어 공학에서 소프트웨어 디자인에서 특정 문맥에서 공통적으로 발생하는 **문제**에 대해 
>**재사용 가능한 해결책**이다. 
>소스나 기계 코드로 바로 전환될수 있는 완성된 디자인은 아니며, 다른 상황에 맞게 사용될 수 있는 문제들을 해결하는데에 쓰이는 서술이나 템플릿이다. 
>디자인 패턴은 프로그래머가 어플리케이션이나 시스템을 디자인할 때 **공통된 문제**들을 **해결**하는데에 쓰이는 형식화 된 가장 좋은 관행이다.

## 디자인패턴의 요소
---
디자인패턴은 다음과 같이도 말할 수 있다.
>어떤 컨텍스트 내에서 일련의 제약조건에 의해 영향을 받을 수 있는 문제에 봉착 했다면, 
>그 제약 조건 내에서 목적을 달성하기 위한 해결책을 찾아낼 수 있는 디자인을 적용하면 된다.

즉, 위의 내용을 아래와 같이 세분화된 요소로 정의 할 수 있다.
- 컨텍스트(context)
  - 문제가 발생하는 여어 상황을 기술한다. 즉, 패턴이 적용될 수 있는 상황을 나타낸다.
  - 경우에 따라서는 패턴이 유용하지 못한 상황을 나타내기도 한다.
  - `패턴이 적용되는 상황`을 의미한다.
- 문제(problem)
  - 패턴이 적용되어 해결될 필요가 있는 여러 디자인 이슈들을 기술한다.
  - 이때 여러 제약 사항과 영향력도 문제 해결을 위해 고려해야 한다.
- 해결(solution)
  - 문제를 해결하도록 설계를 구성하는 요소들과 그 요소들 사이의 관계, 책임, 협력 관계를 기술한다.
  - 해결은 반드시 구체적인 구현 방법이나 언어에 의존적이지 않으며 다양한 상황에 적용할 수 있는 일종의 템플릿이다.
  
## GoF(Gang of Four) 디자인패턴
---
GoF(Gang of Four)는 소프트웨어 개발 영역에서 디자인 패턴을 구체화하고, 체계화한 4명의 사람들을 지칭한다.
- 에리히 감마(Erich Gamma)
- 리차드 헬름(Richard Helm)
- 랄프 존슨(Ralph Johnson)
- 존 블리시디스(John Vlissides)

이 사람들은 디자인 패턴을 23가지로 정리하고 `생성`, `구조`, `행위`의 3가지로 분류했다.<br>
이번 포스팅엔 간략한 개념에 대한 내용만을 정리한다.
### 생성(Creational)
- 객체를 생성하는데 관련된 패턴들
- 객체가 생성되는 과정의 유연성을 높이고 코드의 유지를 쉽게 함

### 생성패턴 종류
- Abstract Factory(추상 팩토리)
  - 구체적인 클래스를 지정하지 않고 관련성을 갖는 객체들의 집합을 생성하거나 서로 독립적인 객체들의 집합을 생성할 수 있는 인터페이스를 제공
- Builder(빌더)
  - 복합 객체의 생성 과정과 표현 방법을 분리함으로써 동일한 생성 공정이 서로 다른 표현을 만들 수 있게 한다.
- Factory Method(팩토리 메서드)
  - 객체 생성 처리를 서브 클래스로 분리해 처리하도록 캡슐화하는 패턴이다.
- Prototype(프로토 타입)
  - 프로토타입의 인스턴스를 이용해서 생성할 객체의 종류를 명세하고 이 프로토타입을 복사해서 새로운 객체를 생성한다.
- Singleton(싱글턴)
  - 전역 변수를 사용하지 않고 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴이다.

### 구조(Structural)
- 프로그램 구조에 관련된 패턴들
- 프로그램 내의 자료구조나 인터페이스 구조 등 프로그램의 구조를 설계하는데 활용할 수 있는 패턴들

### 구조패턴 종류
- Adapter(어댑터)
  - 클래스의 인터페이스를 클라이언트가 기대하는 다른 인터페이스로 변환한다. 
  - 호환성이 없는 인터페이스 때문에 함께 사용할 수 없는 클래스를 개보하여 함께 작동하도록 해 준다.
- Bridge(브릿지)
  - 추상화와 구현을 분리하여 각각을 독립적으로 변형할 수 있게 한다. 구현과 추상화 개념을 분리하려는 것이다.
  - 이로써 구현 자체도 하나의 추상화 개념으로 다양한 변형이 가능해지고, 구현과 독립적으로 인터페이스도 다양함을 가질 수 있게 된다.
- Composite(컴퍼지트)
  - 여러 개의 객체들로 구성된 복합 객체와 단일 객체를 클라이언트에서 구별 없이 다루게 해주는 패턴이다.
- Decorator(데커레이터)
  - 객체의 결합을 통해 기능을 동적으로 유연하게 확장할 수 있게 해주는 패턴이다.
- Facade(퍼사드)
  - 서브시스템에 있는 인터페이스 집합에 대해서 하나의 통합된 인터페이스를 제공한다. 
  - 서브시스템을 좀 더 사용하기 편하게 하기 위해서 높은 수준의 인터페이스를 정의한다.
- Flyweight(플라이웨이트)
  - 작은 크기의 객체들이 여러 개 있는 경우, 객체를 효과적으로 사용하는 방법으로 객체를 공유하게 한다.
- Proxy(프록시)
  - 다른 객체로의 접근을 통제하기 위해서 다른 객체의 대리자 또는 다른 객체로의 정보 보유자를 제공한다.

### 행위(Behavioral)
- 반복적으로 사용되는 객체들의 상호작용을 패턴화 해놓은 패턴들

### 행위패턴 종류
- Chain of Responsibility(책임 연쇄)
  - 요청을 처리할 수 있는 기회를 하나 이상의 객체에게 부여함으로써 요청하는 객체와 처리하는 객체 사이의 결합도를 없애려는 것이다. 
  - 요청을 해결할 객체를 만날 때까지 객체 고리를 따라서 요청을 전달한다.
- Interpreter(인터프리터)
  - 언어에 따라서 문법에 대한 표현을 정의한다. 또 언어의 문장을 해석하기 위해 정의한 표현에 기반하여 분석기를 정의한다.
- Command(커맨드)
  - 실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴
- Iterator(반복자)
  - 내부 표현 방법을 노출하지 않고 복합 객체의 원소를 순차적으로 접근할 수 있는 방법을 제공한다.
- Mediator(중재자)
  - 객체들 간의 상호작용을 객체로 캡슐화한다. 
  - 객체들 간의 참조 관계를 객체에서 분리함으로써 상호작용만을 독립적으로 다양하게 확대할 수 있다.
- Memento(메멘토)
  - 캡슐화를 위배하지 않고 객체 내부 상태를 객체화하여, 나중에 객체가 이 상태로 복구 가능하게 한다.
- Observer(옵저버)
  - 한 객체의 상태 변화에 따라 다른 객체의 상태도 연동되도록 일대다 객체 의존 관계를 구성하는 패턴이다.
- State(스테이트)
  - 객체의 상태에 따라 객체의 행위 내용을 변경해주는 패턴이다.
- Strategy(스트래티지)
  - 행위를 클래스로 캡슐화해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴이다.
- Template Method(템플릿 메서드)
  - 어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴이다.
- Visitor(방문자)
  - 객체 구조의 요소들에 수행할 오퍼레이션을 표현한 패턴이다. 
  - 오퍼레이션이 처리할 요소의 클래스를 변경하지 않고도 새로운 오퍼레이션을 정의할 수 있게 한다.
  
## 알고리즘과 디자인패턴
---
디자인패턴은 결국 문제를 해결하기 위한 어떤 방법을 의미하는데, 이러한 부분은 알고리즘과 비슷하다고 생각이 된다.
그렇다면, 알고리즘과 디자인패턴의 차이를 무엇일까?<br>
둘의 공통점은 아래와 같다.
> 어떤 문제를 해결 하기 위한 해결법을 만드는 형태

즉, 어떤 문제를 해결 하기 위한 효율적인 해결법을 만드는데 둘의 목적은 같다.

그러나 알고리즘과 디자인패턴은 문제를 해결 하는 범위나 관점에 차이가 있다.<br>
알고리즘의 경우 `문제`나 `처리`의 단위에 있어서 `시간`과 `비용`에 대한 효율성을 추구하지만,<br>
디자인패턴의 경우 `주어진 상황`이나 향후 `유지 보수`에 대한 효율성을 추구한다.<br>
 
  
## 참고
---
- [Wikipedia: design pattern pages](http://handlebarsjs.com/)
- [디자인 패턴이란 무엇인가?](https://shoark7.github.io/programming/knowledge/what-is-design-pattern)
- [디자인 패턴이란?](https://doublesprogramming.tistory.com/222)
---
layout: post
title: Framework Library 차이
noindex: true
comments: true
---

---

### Framework 정의
---
Framework의 정의는 아래와 같다.
- 소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것

정의만 들으면 어떤 의미인지 잘 와 닿지가 않는다.<br/>
쉽게 이야기 하면 개발자들이 개발을 하기위한 기본적인 요소들을 미리 만들어 환경을 제공하는 것을 의미한다.<br/>
즉, 이미 **환경(요소)**들은 만들어져있는 상태에서, **개발자가 원하는 형태로 프로그램이 동작**하도록 그 환경 위에 Application을 작성하는 형태를 의미한다.

### Libaray 정의
---
Library의 정의는 아래와 같다.
- 개발 시 활용 가능한 도구들의 집합

Library는 개발자가 개발하는데 필요한 도구들의 집합 이라고 보면 된다.<br/>
즉, 개발자가 자주 사용하거나 혹은 공통적으로 사용할 때 활용 가능하도록 도구를 제공하는 것을 Library라고 한다.<br/>

### Framework와 Library의 차이
---
언뜻 보기엔 Framework와 Library 간에 차이가 별로 나지 않는 것 같고, 구분하기도 어려운 것 같지만,
자세히 보면 이 둘 간의 차이는 `주체`에 있다.<br/>
Framework는 개발자가 작성한 프로그램을 호출하는 `주체`가 `Framework` 이다.<br/>
Library는 개발자가 개발하는 중에 필요에 따라 호출을 하므로 ```주체```가 ```개발자```이다.<br/>
<br> 

![framework-library-diff](/assets/img/posts/framework-library-graph.png)

<br>
위의 내용을 정리하면,
Framework 에는 `제어 역전(Inversion Of Control - IoC)` 개념이 적용되어 있다.


#### IoC 란 ? <br>
>기존에는 객체의 인스턴스 생성 방법에 대한 제어권을 개발자들이 가지고 있었다.<br>
>IoC 란 이러한 인스턴스 생성의 제어를 개발자가 아닌 다른 누군가에게 권한을 준다는 개념이다.<br>
>즉, 인스턴스의 생성부터 소멸까지의 인스턴스의 생명주기 관리를 내가 아닌 <br>
>Framework가 대신 해준다는 뜻으로 제어 역전(Inversion of Control)이라고 한다.<br>

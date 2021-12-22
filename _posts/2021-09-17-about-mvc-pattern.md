---
layout: post
title: "[Design Pattern] MVC 패턴이란?"
excerpt: "MVC 패턴에 관해 알아보자"
categories: [Design Pattern]
comments: true
---

MVC 패턴을 이야기하기 앞서 디자인 패턴이 무엇이고, 왜 필요할까? <br>
예를 들어 생각해보자. 페이지 작성을 위해서는 기본적으로 **HTML, CSS, JavsScript**가 필요하다. 먄약 이 세 가지 언어가 하나의 파일에 작성되다면 어떻게 될까? <br>
규모가 커질수록 유지보수는 극악이 될 것이다. 과거에는 이러한 문제가 많았다고 한다. 이러한 에로사항을 해결하고자 발전할 것이 **디자인 패턴**이다.

>디자인 패턴이란 반복해서 발생되는 문제를 효과적으로 해결하기 위한 **코드 구성 방식**이다.

### 그래서 MVC는 뭔데?
> <span style="color:#8d7edc">**M**</span>odel + <span style="color:#8d7edc">**V**</span>iew + <span style="color:#8d7edc">**C**</span>ontrollers

MVC는 Model, View, Controller와 같은 3가지 요소를 이용해 코드를 구성하는 디자인 패턴이다.
![MVC IMAGE]({{site.url}}/img/DesignPattern/the-mvc-pattern.png)
**Controller:** 클라이언트의 요청을 받고 모델과 뷰를 적절히 컨트롤한다. <br>
**Model:** 컨트롤러의 요청에 대한 데이터(=결과)를 컨트롤러에게 전달한다. <br>
**View:** 컨트롤러로 부터 받은 데이터를 이용해 클라이언트에게 ul를 제공한다.<br><br>
위와 같은 구성으로 코드를 작성하면 MVC 패턴을 사용한 것이다. <br><br>
하지만 이때 정말 중요한 점이 있다. 본래 디자인 패턴을 사용하는 이유는 코드를 작성하는 과정에서 발생하는 여러 에로사항을 효과적으로 해결하기 위함이다. 단순히 위와 같은 방식으로 코드를 작성한다고 해서 모든 에로사항이 해결되지 않는다. 그 이유는 **의존성** 때문이다. 의존성을 고려하지 않을채 코드를 작성하게 되면 특정 코드에서 발생한 문제가 수없이 많은 다른 코드와 연관돼 한 파일에 코드를 작성하는 것 만큼 복잡한 상황을 겪을 수 있다. 때문에 MVC 패턴을 이용해 코드를 구성할 때는 다음과 같은 규칙을 지켜야 한다.

### MVC 패턴의 규칙
![rulls of mvc]({{site.url}}/img/DesignPattern/rulls-of-mvc.png){:height="500"}
<div style="text-align:center">이미지 출처 : https://www.youtube.com/watch?v=ogaXW6KPc8I</div>
위와 같이 적절하게 의존성에 제한을 걸었을 때, 본래 의도에 맞게 MVC 패턴을 사용할 수 있을 것이다!
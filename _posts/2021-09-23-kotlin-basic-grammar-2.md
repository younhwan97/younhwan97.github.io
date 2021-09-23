---
layout: post
title: "[Kotlin] 코틀린의 기본 문법[2] (객체지향 프로그래밍)"
excerpt: "코틀린의 기본 문법에 대해서 알아본다."
categories: [Kotlin]
comments: true
---

## <span style="color:#0f7b6c">1. 객체지향 프로그래밍이란?</span>
---

객체지향 프로그래밍은 프로그래밍을 하는데 있어서 ‘객체’를 사용하는 것 이다. 객체란 포현하고자 하는 대상, 물건, 개념 등을 컴퓨터라는 장치를 이용해 표현하기 위한 수단이며, 표현하고자 하는 대상을 보다 잘 표현하기 위해서 객체는 상태와 행동을 갖는다.
> 객체(Object) = 상태(State) + 행동(Method)

조윤환이라는 객체를 표현하기 위해서는 나이, 직업, 거주지 등의 상태 값도 필요하지만 코딩, 운전 등의 행동도 필요하기 때문에 객체의 구성은 위와 같다.

#### 1-1. 클래스
- 코틀린에서 객체는 자바와 동일하게 클래스를 설계하고 이를 통해 생성한다.
- 클래스에서 정의한 변수와 메서드의 구조대로 객체가 생성되며, 같은 형태의 객체가 필요하다면 같은 클래스로 객체들을 생성하면 된다.
<pre><code>
    class 클래스명 {
        // 멤버 변수
        var a1 = 0
        val a2 = 0

        // 메서드
        fun testMethod(){

        }
    }
</code></pre>
> 클래스를 이용해 객체를 생성하는 것은 객체를 실제로 사용하기 위해 메모리에 할당하는 것 이다.

## <span style="color:#0f7b6c">2. 생성자</span>
---

- 클래스를 통해 객체를 생성할 때 자동으로 수행될 코드를 작성하는 곳
- 메서드와 비슷해 보이지만 반환 타입이 없어 메서드라고 부르지 않는다.
- 생성자의 역할은 클래스가 가지고 있는 변수의 값을 초기화 하는데 주로 이용된다.

#### 2-1. init 코드 블록
- 코틀린은 클래스에 init 코드 블록을 만들어 주면 객체 생성시 자동으로 처리되는 코드를 만들 수 있다.
- init을 사용함으로써, 생성자는 멤버 변수 초기화를 위해서만 사용한다.

#### 2-2. 보조 생성자
- constructor를 이용하여 생성자(보조)를 정의할 수 있다.
- 생성자는 매개변수의 개수나 자료형을 달리하여 여러 개를 만들어 사용할 수 있다.

<pre><code>
    class testClass{
        constructor(){
            println("매개변수가 없는 생성자")
        }

        constructor(a1:Int){
            println("매개변수가 1개인 생성자")
        }

        constructor(a1:Double){
            println("매개변수가 1개인 생성자")
        }

        constructor(a1:Double, a2:Int){
            println("매개변수가 2개인 생성자")
        }
    }
</code></pre>

#### 2-3. 기본 생성자
- 클래스를 정의할 때 클래스 이름 우측에 정의하는 행성자
- 기본 생성자의 매개 변수는 멤버 변수로 자동 등록된다.

<pre><code>
    class testClass (var a1:Int, var a2:Int) {

    }
</code></pre>

**기본 생성자가 정의돼 있을 때 보조 생성자의 정의**
<pre><code>
    class testClass (var a1:Int, var a2:Int) {

        constructor(a1: Int) : this (a1, 1000){

        }
        // 주 생성자가 정의돼 있는 경우 this를 이용해서 보조 생성자에서 주 생성자를 호출해야 한다.
    }
</code></pre>

#### 2-4. 생성자 호출 순서
<pre><code>
    fun main() {
        var obj:testClass = testClass(100)
    }

    class testClass (var a1:Int, var a2:Int) {
        init {
            println("init 블록 호출")
            println("a1: ${a1}")
            println("a2: ${a2}")
        }

        constructor(a1: Int) : this (a1, 1000){
            println("보조 생성자 호출")
        }
    }
</code></pre>

출력 결과: <br>
    init 블록 호출 <br>
    a1: 100 <br>
    a2: 1000 <br>
    보조 생성자 호출 <br>

<pre><code>
    fun main() {
        var obj:testClass = testClass(100, 200)
    }

    class testClass (var a1:Int, var a2:Int) {
        init {
            println("init 블록 호출")
            println("a1: ${a1}")
            println("a2: ${a2}")
        }

        constructor(a1: Int) : this (a1, 1000){
            println("보조 생성자 호출")
        }
    }
</code></pre>

출력 결과: <br>
    init 블록 호출 <br>
    a1: 100 <br>
    a2: 200 <br>

>매개 변수 조건(개수와 타입)이 주 생성자와 일치하면 주 생성자를 호출하고 init 블록을 호출한다. (보조 생성자를 호출할 필요가 없다.) <br><br>
매개 변수의 조건이 주 생성자와 일치하지 않으면 해당 조건과 일치하는 보조 생성자를 찾고, 보조 생성자를 이용해 주 생성자를 호출한다. 이후 init 블록을 호출하고 보조 생성자 내부 코드를 호출하게 된다.

## <span style="color:#0f7b6c">3. 상속</span>
---

- 클래스를 설계할 때 다른 클래스가 가지고 있는 부분을 물려 받는 것을 의미한다.
- 이를 통해 클래스마다 중복된 부분을 클래스 한 곳에 만들 수 있다.
- 예를 들면 학생, 주부, 직장인과 같은 클래스는 모두 사람이라는 중복된 부분을 가지므로 사람 클래스를 상속받을 수 있는 것이다.

<pre><code>
    open class superClasss{
        // 상속이 가능하게 만들기 위해서는 클래스를 정의할 때 open이라는 키워드를 사용해야 한다.
        // open 키워드를 사용하지 않으면 java의 final로 클래스가 생성된다.
    }

    class subClass : superClass() {

    }
</code></pre>

## <span style="color:#0f7b6c">4. 패키지와 모듈</span>
---

#### 4-1. 패키지

- 소프트웨어를 개발하다 보면 클래스도 많이 만들게 되고 kt파일도 많이 만들게 된다.
- 파일이 많아지면 관리가 불편하고 배포가 힘들기 때문에 특정 기준을 세워 파일을 폴더별로 나누어 관리하면 파일 관리가 용이 해진다.
- 코틀린에서 kt파일들을 폴더 별로 나눠 관리하는 개념을 패키지라고 부른다.

![create new package]({{site.url}}/img/Kotlin/kotlin-create-new-package.png){:height="300" width="800"}

다른 패키지에 있는 함수나 클래스를 사용하기 위해서 import를 이용해 사용하고자 하는 패키지를 명시할 수 있다.

#### 4-2. 모듈

- kt파일들을 모아 관리하는 개념이 패키지라면 모듈은 패키지를 모아 관리하는 개념이다.
- 패키지가 많아지면 관리하기가 어려워질 수 있는데 이때 모듈을 사용한다.

## <span style="color:#0f7b6c">5. 접근제한자</span>
---

- 객체가 가지고 있는 변수는 . 연산자를 통해 자유롭게 접근이 가능하다.
- 만약 변수에 담기는 값이 어떠한 조건이 있을 경우 외부에서 자유롭게 접근할 수 있다면 엉뚱한 값이 저장될 가능성이 있다.
- 이에 변수나 메서드의 접근 권한을 설정할 수 있는 접근 제한자를 제공하고 있으며 클래스를 설계하는 개발자가 각 변수나 메서드의 접근 권한을 설정해 오동작 하는 것을 사전에 막을 수 있게 한다.


**동작원리**
- 자바와 달리 코틀린에서는 클래스의 정의한 모든 변수가 자바 코드로 변경될 때 private로 설정된다. (캡슐화를 위해서)
- 접근 제한자의 종류에 따라 자바 코드로 변경될 때 setter 메서드 추가 여부가 결정된다.   

![kotlin to java]({{site.url}}/img/Kotlin/kotlin-to-java-access.png){:width="800" height="300"} <br>
![kotlin to java]({{site.url}}/img/Kotlin/kotlin-to-java-access-2.png){:width="800" height="300"} <br>

앞서 설명했다 싶이 클래스 내부에 선언된 변수의 경우 기본값이 private인 것을 볼 수 있다. <br>
그리고 var의 변우 추후 변경이 가능한 변수이기 때문에 get, set 메서드가 만들어 졌으나, val의 경우 선언된 이후 read only이기 때문에 set 메서드가 만들어지지 않은 것을 볼 수 있다.

#### 5-1. 클래스의 접근제한자

|제한자|내용|
|---|---|
|private|외부에서 객체를 생성할 수 없다.| 
|public|외부에서 객체를 생성할 수 있다. (기본값)| 
|protected|클래스에는 사용되지 않는 접근제한자| 
|internal|모듈이 같을 경우에만 객체를 생성할 수 있다.| 

#### 5-2. 변수와 메서드의 접근제한자

|제한자|내용|
|---|---|
|private|외부에서 접근할 수 없다.| 
|public|외부에서 접근이 자유롭다.| 
|protected|상속관계일 경우에만 접근이 가능하다.| 
|internal|모듈이 같을 경우에만 접근이 가능하다.| 

## <span style="color:#0f7b6c">6. Property</span>
---

- 캡슐화가 적용된 변수는 외부에서 접근이 차단된다.
- 캡슐화가 적용된 변수에 대해 메서드를 통해 값을 설정하거나 가져다 사용할 수 있도록 구성한 것을 **Property**라고 부른다.
- 변수에 값을 넣는 메서드를 setter 메서드라고 부른다.
- 변수에서 값이 가져오는 메서드를 getter 메서드라고 부른다.

#### 6-1. 캡슐화
- 객체가 가지고 있는 변수는 메서드에 구현한 코드가 정상적으로 동작하기 위한 데이터를 담고 있다.
- 만약 변수에 엉뚱한 데이터가 담긴다면 구현한 코드가 정상적으로 동작한다고 하더라도 정상적인 결과를 기대할 수 없다.
- 이에 변수의 직접적인 접근을 차단하여 외부에서 값을 변경하지 못하도록 막는 것을 캡슐화라고 부른다.

> 자바와 달리 코틀린에서는 객체가 가진 모든 변수가 Property 형태로 제공된다. <br><br>
즉, 우리가 . 연산자를 이용해 값에 직접적인 접근을 하는 것처럼 보이지만 사실은 자바 코드로 변하는 과정에서 set, get 메서드가 추가되고 해당 메서드를 이용해 변수에 접근이 이뤄진다.

![kotlin to java]({{site.url}}/img/Kotlin/kotlin-to-java-access-2.png){:width="800" height="300"} <br>
굉징히 편리하다 😆

**직접 get, set 메서드를 정의하는 방법**
![kotlin to java]({{site.url}}/img/Kotlin/kotlin-to-java-access-3.png){:width="800" height="300"} <br>
위와 같은 방식으로 자동으로 생성되는 get, set을 이용하지 않고 개발자가 직접 get, set 메서드를 정의할 수 있다.

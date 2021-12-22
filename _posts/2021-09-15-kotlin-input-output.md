---
layout: post
title: "[Kotlin] 코틀린의 입력과 출력"
excerpt: "코틀린의 입력과 출력에 대해서 알아본다"
categories: [Kotlin]
comments: true
---

## <span style="color:#0f7b6c">1. Output</span>

![Image Alt 텍스트]({{site.url}}/img/Kotlin/kotlin-input.png){:width="800" height="200"}

## <span style="color:#0f7b6c">2. Input<span>

![Image Alt 텍스트]({{site.url}}/img/Kotlin/kotlin-output.png){:width="800" height="200"}

readLine 함수의 경우 input stream으로 부터 한줄을 입력받는다. 이때 input의 경우 Null 값을 입력받을 수 있기 때문에, 일반적으로 '!!' 연산자를 이용해 not null type으로 자료형 변환을 거친다.

```
var inputDate  = readLine()!!.split(" ")
```

위와 같은 코드를 이용해 "hello world"라는 입력을 받았을 때, inputData는 배열의 자료형을 갖고 첫번째 원소로 hello를 두번째 원소로 world를 가질 것 이다.

위 방법과 같이 null check를 할 수 있고, 또 다른방식으로는 기본값을 지정해 줄 수 있다.

```kotlin
val inputData: Int = readLine()?.toIntOrNull() ?: 0
```


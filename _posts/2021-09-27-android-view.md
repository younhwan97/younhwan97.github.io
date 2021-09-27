---
layout: post
title: "[Android] 안드로이드 View"
excerpt: "안드로이드 View에 관해 알아본다."
categories: [Android]
comments: true
---

- 안드로이드에서 눈에 보이는 모든 요소를 View라고 부른다.
- 개발자가 배치하는 모든 View 들은 Class로 제공되는데 모두 View라는 클래스를 상속받고 있다.
- View 클래스는 모든 UI 요소들의 부모 클래스로써 **Layout**과 **Widget**으로 나뉜다.

> 안드로이드 4대 구성요소 중 눈에 보이는 화면을 관리하는 실행단위는 **Activity**다. <br><br>
즉, **Activity**가 눈에보이는 화면을 나타내기 위해서 View를 사용하는 것 이다.

## <span style="color:#0f7b6c">1. Layout과 Widget</span>

**Layout**

- Container, View Group이라고 부르기도 한다.
- 다른 View들을 포함하고(Container), 내부의 View를 통합 관리하고(View Group), 내부의 View들이 배치되는 모양을 결정(Layout)한다.

> Layout 같은 경우 일반적인 버튼, 이미지, 텍스트 입력창과 같이 눈에 보이는 View는 아니다. <br><br>
Layout의 경우 다른 View를 포함하고 관리하며, 눈에 보이는 View들을 사용자에게 어떠한 형태로 보여줄지에 관한 **UI 구조를 정의하는 View**이다.

![android layout concept]({{site.url}}/img/Android/layout-concept.png){:height="400" width="800"} 

**Widget**

- 문자열 입력, 문자열 출력 등 어떤 기능을 가지고 있고 사용자와 상호 작용하는 View들을 통칭해서 Widget이라고 부른다.

#### 1-1. 화면 구성

- 안드로이드는 화면에 Layout을 배치하고 그 안에 다른 Layout이나 Widget을 배치하여 화면의 모양을 만든다.
- 이렇게 만들어진 화면은 모두 객체로 생성되므로 개발자는 이 객체들을 이용해 코드에서 필요한 작업을 할 수 있다.

어플리케이션 실행 과정: <br>
[https://younhwan97.github.io/articles/2021-09/about-android](https://younhwan97.github.io/articles/2021-09/about-android)

![android layout concept]({{site.url}}/img/Android/app-start.png){:height="400" width="800"} 
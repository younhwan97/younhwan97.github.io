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

## <span style="color:#0f7b6c">2. Layout</span>

- 안드로이드는 화면을 구성할 때 배치되는 View들이 어디에 배치된다는 좌표를 설정하지 않는다.
- 다른 단말기에서도 자연스럽게 보이기 위해 자표가 아닌 배치되는 모양을 결정한다.
- 개발자가 배치되는 모양을 결정하고 View들을 배치하면 안드로이드 OS가 단말기에 적합한 좌표를 계산하고 배치한다.

#### 2-1. LinearLayout

- 선형 방항성을 가지고 View를 배치하는 Layout
- 수직, 수평 방향으로 배치할 수 있으며 한 칸에 하나의 View만 배치할 수 있다.
- 안드로이드에서 가장 많이 사용하는 Layout으로 여러 LinearLayout을 조합하여 다양한 모양을 만들 수 있다.

![android linear layout]({{site.url}}/img/Android/maxresdefault-1.jpeg){:height="500" width="800"} 

|주요 속성|내용|
|---|---|
|orientation|View들이 배치될 방향을 설정할 수 있다.|
|weight|LinearLayout 안에 배치되는 View들의 비율을 설정한다. <br>일단 배치를 하고, 남은 공간을 기준으로 정한다!|


#### 2-2. FrameLayout

- 내부에 배치된 View들이 같은 자리에 계속 중첩 배치되는 Layout
- 화면을 구성하기 보단 탭 등과 같은 기능을 만들 때 사용하는 경우가 많음
- FrameLayout에 배치되는 View는 모두 좌측 상단에 배치된다.
- margin 속성이나 layout_gravity 속성을 이용해 배치되는 위치를 결정하여 사용한다.

#### 2-3. TableLayout

- 표를 작성하는 방법으로 View를 배치하는 Layout
- HTML의 table 태그와 유사하다.

**TableLayout의 구조**

- TableLayout 안에 TableRow를 배치한다.
- TableRow는 줄 하나를 의미한다.
- TableRow에 View를 배치하면 배치한 View의 개수 만큼 칸이 생겨 난다.

![android table layout]({{site.url}}/img/Android/tablelayout.jpeg){:height="300" width="600"} 

**TableLayout 주요 속성**

|속성|내용|
|---|---|
|stretchColumns|TableRow 안의 View들이 가로로 늘어날 비율을 조정한다.|
|shrinkColumns|TableRow 안의 View들이 화면에 보일 수 있도록 줄어들게 한다.|

**TableRow 주요 속성**

|속성|내용|
|---|---|
|layout_column|View가 배치될 위치를 설정한다.|
|layout_span|View가 배치될 칸의 개수를 설정한다.|

#### 2-4. GridLayout

- Grid를 설정하여 View를 배치하는 Layout
- TableLayout를 보완하기 위해 제공되는 Layout
- TableLayout은 가로방향으로 '몇 칸을 차지하겠다'만 지정할 수 있다면 GridLayout의 경우 세로방향도 가능하다.

![android table layout]({{site.url}}/img/Android/gridlayout.png){:height="300" width="600"} 

**GridLayout 주요 속성**

|속성|내용|
|---|---|
|rowCount|그리드 레이아웃의 줄의 개수|
|columnCount|그리드 레이아웃의 칸의 개수|

**GridLayout에 배치되는 View의 주요 속성**

|속성|내용|
|---|---|
|layout_column|View가 배치될 칸의 위치(0 부터 시작)|
|layout_row|View가 배치될 줄의 위치(0부터 시작)|
|layout_columnSpan|View가 차지할 칸의 수|
|layout_rowSpan|View가 차지할 줄의 수|
|layout_columnWeight|남은 공간을 차지할 가로 비율|
|layout_columnHeight|남은 공간을 차지할 세로 비율|

#### 2-5. RelativeLayout

- Parent나 다른 view와의 **관계**를 설정하여 배치하는 layout이다.
- Relative Layout에는 특별한 속성이 없지만 배치되는 view 들의 속성을 이용해 배치를 결정하게 된다.
- 주로 Parent와의 관계를 이용해 배치를 하고자 할 때 사용한다.

**배치되는 view 들의 주요 속성**

|속성|내용|
|---|---|
|layout_alignParentTop|자신의 상단을 parent의 상단 부분과 일치 시킨다.|
|layout_alignParentBottom|자신의 하단을 parent의 하단 부분과 일치 시킨다.|
|layout_alignParentLeft|자신의 좌측을 parent의 좌측 부분과 일치 시킨다.|
|layout_alignParentRight|자신의 우측을 parent의 우측 부분과 일치 시킨다.|
|layout_alignWithParentMissing|다른 view를 정렬 기준으로 설정하였을 경우 기준으로 설정한 <br> view가 없을 때는 parent를 기준으로 정렬한다.|
|layout_alignTop|자신의 상단을 지정된 View의 상단 부분과 일치 시킨다.|
|layout_alignBottom|자신의 하단을 지정된 View의 하단 부분과 일치 시킨다.|
|layout_alignLeft|자신의 좌측을 지정된 View의 좌측 부분과 일치 시킨다.|
|layout_alignRight|자신의 우측을 지정된 View의 우측 부분과 일치 시킨다.|

![android relative layout]({{site.url}}/img/Android/relativelayout.png){:height="300" width="600"} 

#### 2-6. ContraintLayout

- RelativeLayout을 개선한 layout으로 RelativeLayout 보다 유연하게 화면을 구성할 수 있다.
- RelativeLayout 처럼 부모와의 관계나 다른 View와의 관계를 설정한다.
- 제약 조건은 다음과 같이 두 가지를 사용할 수 있다.

![android relative layout]({{site.url}}/img/Android/constraint.png){:height="300" width="600"} 

- 실선 제약 조건: 지정된 기준으로부터 얼마큼 떨어진 위치에 있는지 좌표를 설정한다.
- 스프링 제약 조건: 지정된 기준으로부터 얼마큼 떨어진 위치에 있는지 비율을 설정한다.

**Relative layout과의 차이** <br>
Relative layout과 같은 경우는 View를 가운데 설정할 때만 스프링 제약 조건을 사용할 수 있다. 하지만 Constraint layout의 경우 모든 경우에 있어 스프링 제약 조건을 사용할 수 있다. (Parent와의 관계를 설정할 때만 가능하고 다른 view와의 관계를 설정할 때는 contraint도 실선 제약 조건만 가능하다.)

#### 2-7. Space

- Space는 layout은 아니지만 layout을 이용해 화면을 구성할 때 보조 수단으로 사용하는 view이다.
- 화면을 구성할 때 여백이 필요한 경우 사용한다.
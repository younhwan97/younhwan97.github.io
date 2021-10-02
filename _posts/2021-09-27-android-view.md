---
layout: post
title: "[Android] 안드로이드 View"
excerpt: "안드로이드 View에 관해 알아본다."
categories: [Android]
comments: true
---

![android]({{site.url}}/img/Android/giphy.gif)

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
Layout의 경우 다른 View를 포함하고 관리하며, 눈에 보이는 View들을 사용자에게 어떠한 형태로 보여줄지에 관한 <span style="color:#8d7edc">**UI 구조를 정의하는 View**</span>이다.

![android layout concept]({{site.url}}/img/Android/layout-concept.png){:height="400" width="800"} 

**Widget**

- 문자열 입력, 문자열 출력 등 어떤 기능을 가지고 있고 사용자와 상호 작용하는 View들을 통칭해서 Widget이라고 부른다.


### 1-1. 화면 구성

- 안드로이드는 화면에 Layout을 배치하고 그 안에 다른 Layout이나 Widget을 배치하여 화면의 모양을 만든다.
- 이렇게 만들어진 화면은 모두 객체로 생성되므로 개발자는 이 객체들을 이용해 코드에서 필요한 작업을 할 수 있다.

어플리케이션 실행 과정: <br>
[https://younhwan97.github.io/articles/2021-09/about-android](https://younhwan97.github.io/articles/2021-09/about-android)

![android layout concept]({{site.url}}/img/Android/app-start.png){:height="400" width="800"} 

## <span style="color:#0f7b6c">2. Layout</span>

- 안드로이드는 화면을 구성할 때 배치되는 View들이 어디에 배치된다는 좌표를 설정하지 않는다.
- 다른 단말기에서도 자연스럽게 보이기 위해 자표가 아닌 배치되는 모양을 결정한다.
- 개발자가 배치되는 모양을 결정하고 View들을 배치하면 안드로이드 OS가 단말기에 적합한 좌표를 계산하고 배치한다.


### 2-1. LinearLayout

- 선형 방항성을 가지고 View를 배치하는 Layout
- 수직, 수평 방향으로 배치할 수 있으며 한 칸에 하나의 View만 배치할 수 있다.
- 안드로이드에서 가장 많이 사용하는 Layout으로 여러 LinearLayout을 조합하여 다양한 모양을 만들 수 있다.

![android linear layout]({{site.url}}/img/Android/maxresdefault-1.jpeg){:height="500" width="800"} 

**LinearLayout의 주요 속성**

|주요 속성|내용|
|---|---|
|orientation|View들이 배치될 방향을 설정할 수 있다.|
|weight|LinearLayout 안에 배치되는 View들의 비율을 설정한다. <br>일단 배치를 하고, 남은 공간을 기준으로 정한다!|


### 2-2. FrameLayout

- 내부에 배치된 View들이 같은 자리에 계속 중첩 배치되는 Layout
- 화면을 구성하기 보단 탭 등과 같은 기능을 만들 때 사용하는 경우가 많음
- FrameLayout에 배치되는 View는 모두 좌측 상단에 배치된다.
- margin 속성이나 layout_gravity 속성을 이용해 배치되는 위치를 결정하여 사용한다.


### 2-3. TableLayout

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

<br>

**TableRow 주요 속성**

|속성|내용|
|---|---|
|layout_column|View가 배치될 위치를 설정한다.|
|layout_span|View가 배치될 칸의 개수를 설정한다.|


### 2-4. GridLayout

- Grid를 설정하여 View를 배치하는 Layout
- TableLayout를 보완하기 위해 제공되는 Layout
- TableLayout은 가로방향으로 '몇 칸을 차지하겠다'만 지정할 수 있다면 GridLayout의 경우 세로방향도 가능하다.

![android table layout]({{site.url}}/img/Android/gridlayout.png){:height="300" width="600"} 

**GridLayout 주요 속성**

|속성|내용|
|---|---|
|rowCount|그리드 레이아웃의 줄의 개수|
|columnCount|그리드 레이아웃의 칸의 개수|

<br>

**GridLayout에 배치되는 View의 주요 속성**

|속성|내용|
|---|---|
|layout_column|View가 배치될 칸의 위치(0 부터 시작)|
|layout_row|View가 배치될 줄의 위치(0부터 시작)|
|layout_columnSpan|View가 차지할 칸의 수|
|layout_rowSpan|View가 차지할 줄의 수|
|layout_columnWeight|남은 공간을 차지할 가로 비율|
|layout_columnHeight|남은 공간을 차지할 세로 비율|

### 2-5. RelativeLayout

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

### 2-6. ContraintLayout

- RelativeLayout을 개선한 layout으로 RelativeLayout 보다 유연하게 화면을 구성할 수 있다.
- RelativeLayout 처럼 부모와의 관계나 다른 View와의 관계를 설정한다.
- 제약 조건은 다음과 같이 두 가지를 사용할 수 있다.

![android relative layout]({{site.url}}/img/Android/constraint.png){:height="300" width="600"} 

- 실선 제약 조건: 지정된 기준으로부터 얼마큼 떨어진 위치에 있는지 좌표를 설정한다.
- 스프링 제약 조건: 지정된 기준으로부터 얼마큼 떨어진 위치에 있는지 비율을 설정한다.

**Relative layout과의 차이** <br>
Relative layout과 같은 경우는 View를 가운데 설정할 때만 스프링 제약 조건을 사용할 수 있다. 하지만 Constraint layout의 경우 모든 경우에 있어 스프링 제약 조건을 사용할 수 있다. (Parent와의 관계를 설정할 때만 가능하고 다른 view와의 관계를 설정할 때는 contraint도 실선 제약 조건만 가능하다.)

### 2-7. Space

- Space는 layout은 아니지만 layout을 이용해 화면을 구성할 때 보조 수단으로 사용하는 view이다.
- 화면을 구성할 때 여백이 필요한 경우 사용한다.

## <span style="color:#0f7b6c">3. Widget</span>

- **안드로이드의 view 중 기능을 갖고 사용자와 상호작용을 하는 것들을 widget이라고 부른다.**
- Widget은 layout 위에 배최되어 화면에 나타나고 코드를 통해 widget을 통제하여 사용자와 소통하는 수단이 된다.

**Widget 사용 패턴**
1. Layout에 사용하고자 하는 widget을 배치한다.
2. 이 때, activity가 실행되면 화면이 구성되고 화면에 배채된 모든 view들은 **객체**로 생성된다.
3. **객체**로 생성된 view 중에 필요한 widget들의 주소 값을 얻어와 코드로 이들을 통제한다.
4. 필요하다면 이벤트에 대한 코드를 구성하여 사용한다.

**Widget 객체의 주소 값 가져오기**

Activity 객체가 생성되면 화면에 배치된 모든 view는 객체로 생성된다. 이때 해당 객체를 사용하기 위해서는 객체의 주소값이 필요하다.
```kotlin
    val textView1 = findViewById<TextView>(R.id.textView1)
```
위와 같은 코드를 통해 생성된 view의 주소값을 얻어올 수 있다. <br>
여기서 코틀린으로 안드로이드를 개발하는 장점이 등장하는데, 코틀린은 view의 id와 같은 이름의 변수가 자동으로 생성되고 해당 변수는 객체의 주소를 갖는다.

### 3-1. TextView와 ImageView

- **사용자에게 전달하고자 하는 문자열과 이미지를 표시하는 view**

**안드로이드 프로젝트 이미지 저장 방식**

![edit text list]({{site.url}}/img/Android/imageView.png){:height="500"} 

프로젝트를 보면 res 폴더 아래 drawable, mipmap 폴더에 이미지가 있다. 

**Drawable vs Mipmap**

- 안드로이드에서 이미지르 넣은 폴더는 drawable 폴더이다.
- 안드로이드 버전이 변경되면서 mipmap이라는 폴더를 제공하는데, 이 폴더의 이미지는 비트맵이 아닌 벡터 방식으로 이미지를 그리게 된다.
- mipmap 폴더의 이미지는 런처 아이콘용 이미지를 넣는 폴더로 사용한다.

### 3-2. Button

- **사용자가 클릭(터치)하면 개발자가 만든 코드를 동작시켜 주는 view**
- Button은 문자열을 표시하는 Button과 이미지를 표시하는 ImageButton이 있다.

**Button의 이벤트**

- **OnClick:** 사용자가 Button을 Click하면 발생하는 이벤트

**EX) Button에 이벤트 리스너를 장착해 보기** <br>
상황: 버튼이 클릭되면 textView의 문자열을 "버튼이 클릭 되었습니다."로 변경한다.
```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            // 방법 1
            // 리스너 객체를 생성하고 버튼에 setOnClickListener를 통해 리스너를 장착한다.
            button.setOnClickListener(listener1)

            // 방법 2
            // 코틀린의 경우 고차함수를 이용해 리스너를 생성할 수 있다.
            button.setOnClickListener {
                textView.text = "버튼이 클릭 되었습니다."
            }
        }

        var listener1 = object : View.OnClickListener{
            override fun onClick(p0: View?) {
                textView.text = "버튼이 클릭 되었습니다."
            }
        }
    }

```

하나의 리스너에서 여러 view를 분기해서 처리하고 싶을때는 일반적으로 **방법1**을 사용하고, 각각의 view를 따로 처리하고 싶을 때는 **방법2**를 사용한다.

### 3-3. EditText

- **사용자에게 문자열 데이터를 입력 받을 때 사용하는 view**

![edit text list]({{site.url}}/img/Android/edittextlist.png){:height="300"} 

Plain Text부터 시작해 아래 밑줄 표시가 된 view가 모두 editText view이다. 모두 똑같은 editText이지만 목적에 따라 각각의 view가 설정값이 조금씩 다르다.

**EditText 주요 속성**

|속성|내용|
|---|---|
|text|표시할 문자열을 설정한다.|
|hint|입력된 값이 없을 경우 안내 문구를 설정한다.|
|inputType|입력 값에 대해 설정한다. 표시되는 형식, 나타나는 키보드 등에 영향을 준다.|
|imeOptions|나타나는 키보드의 enter 키 모양을 설정한다.|

**EX) text 속성을 이용해보자** <br>
상황: 버튼을 클릭했을 때 editText의 내용을 textView에 그대로 표시한다.
```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button3.setOnClickListener {
                textView2.text = editTextTextPersonName2.text

                // 키보드가 사라지게 하는 방법
                val imm = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
                imm.hideSoftInputFromWindow(editTextTextPersonName2.windowToken, 0)
            }
        }
    }
```

**EditText 이벤트**

- **TextWatcher:** 사용자가 입력한 내용을 실시간으로 감시한다.
- **EditorAction:** 키보드의 Enter 키를 눌럿을 때 발생하는 이벤트

**EX) EditText 이벤트를 이용해 보자** <br>
상황: 이벤트 리스너를 이용해 editText값이 변경되었을 때 textView값을 변경해 본다.
```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            editTextTextPersonName2.addTextChangedListener(listenr1)
        }

        val listenr1 = object : TextWatcher{
            // 문자열이 변경되기 전
            override fun beforeTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {

            }

            // 문자열 변경 작업이 완료되었을 때
            override fun onTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {

            }

            // 변경된 문자열이 화면에 반영되었을 때
            // 주로 이 메소드를 사용한다.
            override fun afterTextChanged(p0: Editable?) {
                textView2.text = p0
            }
        }
    }
```
실시간으로 editText 내용이 변경되는 것을 감시하는 watcher 이벤트 리스너의 경우 enter 키를 누르는 등의 액션이 필요없다. <br><br> 

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            editTextTextPersonName2.setOnEditorActionListener { textView, i, keyEvent ->
                textView2.text = editTextTextPersonName2.text
                false
            }
            // false를 반환하면 키보드가 내려간다.
        }
    }
```
EditAction 이벤트의 경우 리스너를 고차 함수로 구현할 수 있다. 하지만 TextWatcher의 경우 그렇지 않다. <br>
**두 이벤트의 차이는 이벤트 리스너가 오버라이딩 해야하는 메서드의 개수 차이다. 오버라이딩이 필요한 메서드가 1개보다 많으면 고차함수가 제공되지 않는다.**

### 3-4. TextInputLayout

- **EditText을 보완한 view**

![edit text list]({{site.url}}/img/Android/textinputlayout.png){:height="500"} 

**TextInputLayout 주요 속성**

|속성|내용|
|---|---|
|hint|입력된 값이 없을 경우 안내 문구를 설정한다. <br> editText와 달리 문자열을 입력하면 상단 부분으로 올라간다.|
|counterEnabled|입력한 글자의 수가 나타난다.|
|counterMaxLength|지정된 글자수를 넘으면 붉은 색으로 표시해준다.|

**EX) TextInputLayout 속성을 이용해 보자** <br>
상황: 버튼을 클릭했을 때 TextInputLayout의 값을 textView에 표시한다.
```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button2.setOnClickListener {
                textView.text = textInputLayout.editText?.text
                // textInputLayout 안에 있는 editText에 접근해 text를 가져와야 한다!
            }
        }
    }
```

![edit text list]({{site.url}}/img/Android/textinputlayout2.png){:height="300"} 

### 3-5. CheckBox와 RadioButton

**CheckBox**

- **선택할 수 있는 항목 들을 제공하고 체크를 통해 선택할 수 있도록 하는 view**
- 각 checkBox는 독립되어 있기 때문에, 다수의 checkBox를 동시에 선택할 수 있다. 

**RadioButton**

- **하나의 그룹 안에서 하나만 선택할 수 있도록 하는 view**
- RadioGroop 내부에서 사용할 수 있다.

**이벤트**

- **checkedChange:** 체크 상태가 변경되었을 때 발생하는 사건

### 3-6.Chip

- **버튼, 체크박스, 라디오 등의 기능을 가지고 있는 새로운 UI 요소**
- ChipGroup를 통해 RadioButton 처럼 구성할 수 있다.

**주요 속성**

|속성|내용|
|---|---|
|Theme|테마를 설정한다. 반드시 설정해야 한다.|
|Style|Chip의 스타일을 설정한다.|
|Checkable|체크 가능 여부를 설정한다.|
|Text|Chip에 표시할 문자열을 설정한다.|
|chipIcon|Chip에 표시할 아이콘을 설정한다.|
|chipIconVisiable|Chip 아이콘을 보여줄 것인가를 설정한다.|
|checkedIcon|선택되었을 때의 아이콘을 설정한다.|
|checkedIconVisiable|선택되었을 때의 아이콘을 보여줄 것인가를 설정한다.|
 
![chip]({{site.url}}/img/Android/chip.png){:height="500"}

### 3-7. 그 외 여러가지 Widget

1. **TogglButton:** 환경설정과 같은 화면에서 어플리케이션의 기능을 on/off 하는 기능을 제공하고자 할 때 사용하는 view
2. **Switch:** ON/OFF 상태를 좌우로 이동하면서 설정할 수 있는 view
3. **ProgressBar:** 오래 걸리는 작업이 있을 경우 작업 중임을 표시하는 view (로딩, 설치 ..)
4. **SeekBar:** ProgressBar와 매우 유사하지만 **유저**가 값을 직접 설정할 수 있는 기능이 있는 view
5. **RatingBar:** 별 점을 조절할 수 있는 view
6. **CardView:** 화면에 배치되는 view들을 그룹으로 묶어 관리하는 view로써, CardView 자체에 그림자를 두어 약간 공중에 떠 있는 듯한 모습을 보인다.
7. **ScrollView:** ScrollView는 배치되어 있는 view가 화면을 벗어날 경우 스크롤 할 수 있도록 제공되는 view

![etc widget]({{site.url}}/img/Android/etc-widget.png){:height="500"} 


---
layout: post
title: "[Android] 안드로이드 AdapterView"
excerpt: "안드로이드 AdapterView에 관해 알아본다."
categories: [Android]
comments: true
---

![android]({{site.url}}/img/Android/giphy.gif)

- 개발자는 화면의 다양한 view들을 배치해 화면을 구성한다.
- 대부분의 view들은 배치를 하면 기본적으로 정해진 속성에 따라 모양이 구성된다.
- 하지만 일부 view들은 스스로 결정할 수 없는 부분이 있어 개발자가 반드시 데이터를 설정해야만 구성이 가능하다.
- 이렇게 개발자가 반드시 설정해야 화면을 구성할 수 있는 view들을 가르켜 **Adapter View**라고 부른다.

**Adapter Class**

- Adapter View를 **개발자가 직접 설정**하기 위해서 사용하는 Class
- 일반적으로 **몇개의 항목**을 보여주고 **어떠한 형태와 데이터를** 보여줄 것인지 결정한다.

## <span style="color:#0f7b6c">1. ListView</span>

- **가장 대표적이고 가장 많이 사용하는 adapterView**

**ListView 이벤트**

- **ItemClick:** 항목을 터치하면 발생


**ArrayAdapter**

- **ListView를 구성하기 위해 사용하는 Adapter**

```
    class MainActivity : AppCompatActivity() {
        val data = arrayOf(
            "문자열1", "문자열2", "문자열3", "문자열4", "문자열5",
            "문자열6", "문자열7", "문자열8", "문자열9", "문자열10"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // 첫번째 : Context
            // 두번쩨 : 항목 하나를 구성하기 위해 사용할 layout 파일
            // 세번쩨 : 데이터
            val adapter1 = ArrayAdapter(this, android.R.layout.simple_list_item_1, data)
            list1.adapter = adapter1
            // context : 화면에 listView를 표현하기 위해서 this로 activity 객체를 전달
            // android.R.layout.simple_list_item_1 : 안드로이드가 기본적으로 제공하고 있는 listView의 layout으로써 문자열 1개를 표현하는 용도로 사용가능
        }
    }
```

**simple_list_item_1.xml**

- **안드로이드에서 기본적으로 제공하는 listView layout**
- 각 list에 하나의 텍스트를 넣고 싶을때 사용

![simple list]({{site.url}}/img/Android/simple-list.png){: height="400" width="700"}

보다시피 1개의 TextView만 가진다.

### 1-1. CustomListView (문자열이 1개 일 때)

- **안드로이드에서 기본적으로 제공하는 ListView의 layout을 사용하지 않을때 사용**

**step 1: layout 파일 생성**

![create layout step 1]({{site.url}}/img/Android/create-layout-step-1.png){: height="400" width="700"}

**step 2: layout 구성**

![create layout step 1]({{site.url}}/img/Android/create-layout-step-2.png){: height="400" width="700"}

**step 3: Adapter 생성 및 장착**
```
    class MainActivity : AppCompatActivity() {
        val data = arrayOf(
            "문자열1", "문자열2", "문자열3", "문자열4", "문자열5",
            "문자열6", "문자열7", "문자열8", "문자열9", "문자열10"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            var adapter1 = ArrayAdapter(this, R.layout.row, R.id.rowTextView ,data)
            list1.adapter = adapter1
        }
    }
```
custom list view도 일반적인 list view를 사용할때와 마찬가지로 ArrayAdapter를 사용하는데, 각 list에 표시할 문자열이 1개인 경우 일반 list와 custom list 상관 없이 ArrayAdapter를 사용하면 된다.

**step 4: 결과 확인**

![create layout step 1]({{site.url}}/img/Android/create-layout-step-4.png){: height="400"}

성공적으로 원하는 layout을 갖는 listView가 생성된 것을 볼 수 있다.

### 1-2. CustomListView (문자열이 1개가 아닐 때)

**step 1: layout 파일 생성**

**step 2: layout 구성**

![create layout step 1]({{site.url}}/img/Android/create-layout-step-22.png){: height="400" width="700"}

**step 3: Adapter 생성 및 장착**
```
    class MainActivity : AppCompatActivity() {
        val imgRes = arrayOf(
            R.drawable.imgflag1,R.drawable.imgflag2,R.drawable.imgflag3,R.drawable.imgflag4,
            R.drawable.imgflag5,R.drawable.imgflag6,R.drawable.imgflag7,R.drawable.imgflag8
        )

        val data1 = arrayOf(
            "토고", "프랑스", "스위스", "스페인", "일본", "독일", "브라질", "대한민국"
        )

        val data2 = arrayOf(
            "togo", "france", "swis", "spain", "japan", "german", "brazil", "korea"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // simple adapter에 셋팅할 데이터를 가지고 있을 ArrayList
            val dataList = ArrayList<HashMap<String, Any>>()

            for(i in imgRes.indices){
                // 항목 하나를 구성하기 위해 필요한 데이틀 담고 있는 HashMap
                val map = HashMap<String, Any>()
                map["img"] = imgRes[i]
                map["data1"]= data1[i]
                map["data2"]= data2[i]

                dataList.add(map)
            }

            // HashMap에 데이터를 저장했을 때 사용했던 이름 배열
            val keys = arrayOf("img", "data1", "data2")
            // 데이터를 셋팅할 view의 id 배열
            var ids = intArrayOf(R.id.rowImageView, R.id.rowTextView1, R.id.rowTextView2)

            val adapter1 = SimpleAdapter(this, dataList, R.layout.row, keys, ids)
            list1.adapter = adapter1
        }
    }
```
**step 4: 결과 확인**

![create layout step 1]({{site.url}}/img/Android/create-layout-step-44.png){: height="400"}

## <span style="color:#0f7b6c">2. Spinner</span>

- **사용자에게 항목을 주고 선택 하게 할 수 있는 Adapter view**
- 작은 스마트폰 화면을 효율적으로 사용할 수 있다는 장점을 가지고 있다.

```
    class MainActivity : AppCompatActivity() {
        val items = arrayOf(
            "스피너항목1","스피너항목2","스피너항목3","스피너항목4","스피너항목5"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // 어댑터를 생성

            // 1. 접혔을 때의 모습을 구성할 layout
            val adapter1 = ArrayAdapter(this, android.R.layout.simple_list_item_1, items)

            // 2. 펼쳤을 때의 모습을 구성할 layout
            adapter1.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item)

            spinner.adapter = adapter1
        }
    }
```

**Spinner 이벤트**

- **ItemSelected:** 사용자가 항목을 선택했을 때 발생. 이 이벤트의 리스너는 프로퍼티로 설정.(다른 이벤트는 메서드로 했었는데 여기서 살짝 차이가 존재!)

## <span style="color:#0f7b6c">3. GridView</span>

- **ListView와 거의 동일하며 항목을 그리드 형태로 보여 줄 수 있는 view**
- ListView를 구현하는 방식에서 속성 한 가지만 더 추가하면 된다.


```
    class MainActivity : AppCompatActivity() {
        val items = arrayOf(
            "그리드항목1","그리드항목2","그리드항목3","그리드항목4","그리드항목5",
            "그리드항목6","그리드항목7","그리드항목8","그리드항목9","그리드항목10"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val adapter1 = ArrayAdapter(this, android.R.layout.simple_list_item_1, items)

            grid.adapter = adapter1
        }
    }
```

**속성 값에 따른 GridView**

![grid view 1]({{site.url}}/img/Android/gridView.png){: height="400"}

numColumns라는 속성이 girdView의 핵심 속성이며, 속성값을 auto_fit로 하면 디바이스의 화면크기에 맞춰 자동으로 칸의 개수가 맞춰진다.

**GridView 이벤트**

- **ItemSelected:** 사용자가 항목을 선택했을 때 발생.

## <span style="color:#0f7b6c">4. AutoCompleteTextView</span>

- **EditText에 자동완성 기능을 추가한 view**
- 사용자가 문자열을 입력하면 설정한 문자열 항목을 통해 자동완성 리스트를 제공

```
    class MainActivity : AppCompatActivity() {
        val items = arrayOf(
            "abcd", "abce", "abcf", "abcq", "abce", "abcc",
            "bcde", "bcdr", "bcft", "bcfy"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val adapter1 = ArrayAdapter(this, android.R.layout.simple_dropdown_item_1line, items)
            // 다른 adapterView는 어댑터를 프로퍼티로 설정했다면 autoCompleteTextView는 setAdapter 메서드를 통해 설정한다.
            autoCompleteTextView.setAdapter(adapter1)
        }
    }
```

**속성 값에 따른 autoCompleteTextView**

![auto complete text vieew]({{site.url}}/img/Android/autoCompleteTextView.png){: height="400"}

completionThreshold 값에 따라 몇 글자를 입력했을 때 자동완성 리스트가 나타날지 설정

**autoCompleteTextView 이벤트**

- **ItemClick:** 제공되는 자동완성 리스트의 항목을 클릭할 경우 발생

### 4-1. MultiAutoCompleteTextView

- **AutoCompleteTextView와 거의 동일하며, 구분자를 활용해 여러 문자열을 동시에 입력 받을 수 있는 view**
- AutoCompleteTextView 생성 과정에서 구분자를 설정하는 단계만 추가하면 된다.

```
    class MainActivity : AppCompatActivity() {
        val items = arrayOf(
            "abcd", "abce", "abcf", "abcq", "abce", "abcc",
            "bcde", "bcdr", "bcft", "bcfy"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val adapter1 = ArrayAdapter(this, android.R.layout.simple_dropdown_item_1line, items)

            // 구분자
            multiAutoCompleteTextView.setTokenizer(MultiAutoCompleteTextView.CommaTokenizer())

            // 다른 adapterView는 어댑터를 프로퍼티로 설정했다면 autoCompleteTextView는 setAdapter 메서드를 통해 설정한다.
            multiAutoCompleteTextView.setAdapter(adapter1)
        }
    }
```

![MultiAutoCompleteTextView]({{site.url}}/img/Android/MultiAutoCompleteTextView.png){: height="400"}

## <span style="color:#0f7b6c">5. SingleChoiceListView</span>

- **다수의 항목을 제공하고 항목 중 하나를 선택할 수 있는 listView**
- ListView의 Mode를 변경하여 설정

```
    class MainActivity : AppCompatActivity() {
        val items = arrayOf(
            "항목1", "항목2", "항목3", "항목4", "항목5", "항목6"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val adapter1 = ArrayAdapter(this, android.R.layout.simple_list_item_single_choice, items)
            list1.adapter = adapter1
            list1.choiceMode = ListView.CHOICE_MODE_SINGLE
        }
    }
```

![SingleChoice]({{site.url}}/img/Android/singleChoice.png){: height="400"}

### 5-1. MultiChoiceListView

```
    class MainActivity : AppCompatActivity() {
        val items = arrayOf(
            "항목1", "항목2", "항목3", "항목4", "항목5", "항목6"
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val adapter1 = ArrayAdapter(this, android.R.layout.simple_list_item_multiple_choice, items)
            list1.adapter = adapter1
            list1.choiceMode = ListView.CHOICE_MODE_MULTIPLE
        }
    }
```

![multiChoice]({{site.url}}/img/Android/multiChoice.png){: height="400"}

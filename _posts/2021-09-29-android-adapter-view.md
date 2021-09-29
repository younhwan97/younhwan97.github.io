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
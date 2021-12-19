---
layout: post
title: "[Android] 안드로이드 MVC Pattern"
excerpt: "안드로이드 MVC Pattern에 관해 알아본다."
categories: [Android]
comments: true
---

- 안드로이드에서는 Fragment를 활용해 MVC 패턴을 구성할 수 있다.
- 눈에 보이는 모든 부분을 Fragment로 만들어 사용할 경우 Fragment를 관리하는 Activity가 Controller의 역할을 한다.
- 즉 Fragment는 MVC의 View를 담당하고, Activity는 Controller를 담당하게 된다.

> Activity는 어떤 Fragment를 보여 줄 건지 관리한다. <br><br> Activity를 통해 보여지는 모든 Fragment는 자신을 관리하는 activity에 접근할 수 있기 때문에, 변수는 activity에 정의하고 activity가 Fragment끼리 데이터를 주고받는 등의 작업을 관리한다.

> 지금까지 안드로이드에서 '눈이보이는 것'은 Activity로 취급했다. 하지만 지금부터는 안드로이드에서 MVC 패턴을 적용하기 위해 과감하게 눈에 보이는 영역은 모두 Fragment에게 맡기고 activity는 Controller의 역할로써 사용하고자 한다.

```kotlin
    class MainActivity : AppCompatActivity() {
        val inputFragment = InputFragment()
        val resultFragment = ResultFragment()
        
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            setFragment("input")
        }

        fun setFragment(str:String){
            val tran = supportFragmentManager.beginTransaction()
            when(str){
                "input"->{
                    tran.replace(R.id.container1, inputFragment)
                }
                "result"->{
                    tran.replace(R.id.container1, resultFragment)
                    tran.addToBackStack(null)
                }
            }
            tran.commit()
        }
    }
```

위 코드를 보면 MainActivity에서는 setFragment를 정의하고, 어떤 Fragment를 사용할 건지만 결정한다.

**Activity를 이용해 Fragment 사이 데이터를 관리하기**

```kotlin
    class MainActivity : AppCompatActivity() {
        val inputFragment = InputFragment()
        val resultFragment = ResultFragment()

        // Fragment에서 사용할 변수를 Activity에서 설정한다.
        // Activity가 관리하는 Fragment는 Activity에 접근할 수 있기 때문이다.
        var input_value1 = ""
        var input_value2 = ""

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            setFragment("input")
        }

        fun setFragment(str:String){
            val tran = supportFragmentManager.beginTransaction()
            when(str){
                "input"->{
                    tran.replace(R.id.container1, inputFragment)
                }
                "result"->{
                    tran.replace(R.id.container1, resultFragment)
                    tran.addToBackStack(null)
                }
            }
            tran.commit()
        }
    }
```

InputFragment.kt
```kotlin
    class InputFragment : Fragment(){
        override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
            val view = inflater.inflate(R.layout.fragment_input, null)
            return view
        }

        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)
            input_fragment_btn1.setOnClickListener {
                val Activity = activity as MainActivity
                Activity.input_value1 = input_fragment_edit1.text.toString()
                Activity.input_value2 = input_fragment_edit2.text.toString()
                Activity.setFragment("result")
            }
        }
    }
```

ResultFragment.kt
```kotlin
    class ResultFragment:Fragment() {
        override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
            val view = inflater.inflate(R.layout.fragment_result, null)

            return view
        }

        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)

            val Activity = activity as MainActivity
            result_fragment_text1.text = Activity.input_value1
            result_fragment_text2.text = Activity.input_value2
        }
    }
```

![android]({{site.url}}/img/Android/activity-controller.png){:height="500"}

MainActivity에서는 Fragment에서 사용할 변수를 선언한다. Activity가 관리하는 Fragment는 activity의 변수에 접근할 수 있기 때문에 activity에서 선언한다. <br>
각 Fragment에서는 activity를 MainActivity로 형변환 함으로써 MainActivity에 접근하였고, MainActivity를 호출함으로써 값을 세팅하거나 함수를 호출하여 Fragment는 오직 view만을 담당하고, 각 Fragment 사이 데이터 이동을 activity가 할 수 있도록 설정했다.
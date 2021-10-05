---
layout: post
title: "[Android] 안드로이드 Fragment"
excerpt: "안드로이드 Fragment에 관해 알아본다."
categories: [Android]
comments: true
---

![android]({{site.url}}/img/Android/giphy.gif)

- **여러 화면을 가지고 있는 어플리케이션은 여러 activity를 가지고 있는 어플리케이션을 의미한다.**
- Activity의 경우 독립된 실행단위로써 메모리 소비가 크다. 
- 독립된 실행단위가 아닌 화면만 필요한 경우 Activity보다 Fragment가 더 잘어울린다.
- Fragment는 activity내 작은 화면 조각으로 activity의 화면을 여러 영역으로 나누어 관리하는 목적으로 사용한다.

**step 1: Fragment 생성**

![test broad version]({{site.url}}/img/Android/create-fragment-step-1.png){:height="500"} 

빈 Fragment를 생성하기 위해 Kotlin 클래스 파일을 생성한다.

**step 2: Fragment layout 생성**

![test broad version]({{site.url}}/img/Android/create-fragment-step-2.png){:height="500"} 

**step 3: Fragment 파일 작성**

```kotlin
    class FirstFragment: Fragment() {
        override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
            val view = inflater.inflate(R.layout.fragment_first, null)
            return view
        }
    }
```

Fragment 클래스를 상속하는 custom Fragment를 생성한다. <br>
Fragment는 onCreateView에서 Fragment에 사용할 view를 생성하기 때문에, inflater를 사용해 view를 생성하고 반환하자.

**step 4: Fragment를 생성할 상황을 구성한다.** <br>
상황: activity에서 버튼을 클릭하면 fragment를 생성한다.

```kotlin
    class MainActivity : AppCompatActivity() {
        val first_fragment= FirstFragment()

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // Fragment 작업 시작
                val tran = supportFragmentManager.beginTransaction()
                // Fragment를 셋팅
                tran.replace(R.id.container1, first_fragment)
                tran.commit()
            }
        }
    }
```

**step 5: 결과 확인**

![test broad version]({{site.url}}/img/Android/create-fragment-step-5.png){:height="500"} 

## <span style="color:#0f7b6c">1. AddToBackStack</span>

- 안드로이드에서 Back Button의 경우 현재 activity를 종료한다.
- Fragment는 activity가 아니므로 back button으로 제거할 수 없는데, AddToBackStack 메서드를 통해 BackStack에 포함된 경우 back button으로 제거할 수 있다.
- 이를 통해 마치 이전화면으로 돌아가는 듯한 효과를 줄 수 있다.

```kotlin
    class MainActivity : AppCompatActivity() {
        val first_fragment= FirstFragment()

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // Fragment 작업 시작
                val tran = supportFragmentManager.beginTransaction()
                // Fragment를 셋팅
                tran.replace(R.id.container1, first_fragment)
                tran.addToBackStack(null)
                tran.commit()
            }
        }
    }
```

## <span style="color:#0f7b6c">2. Fragment 생명주기</span>

- Fragment도 생명주기를 가진다.
- 각 주기에 자동으로 호출되는 메서드가 제공되며 이 메서드를 통해 필요한 처리를 할 수 있다.

![test broad version]({{site.url}}/img/Android/fragment-lifecycle.png){:height="500"} 

```kotlin
    class FirstFragment: Fragment() {
        // Fragment와 activity가 연결될 때 호출된다.
        override fun onAttach(context: Context) {
            super.onAttach(context)
            Log.d("test","onAttch")
        }

        // Fragment가 생성될 때 호출된다.
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            Log.d("test","onCreate")
        }

        // Fragment를 통해 보여줄 view 객체를 생성하기 위해 호출된다.
        override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
            val view = inflater.inflate(R.layout.fragment_first, null)
            Log.d("test","onCreateView")
            return view
        }

        // Fragment를 통해 보여줄 view 객체가 생성되면 호출된다.
        override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
            super.onViewCreated(view, savedInstanceState)
            Log.d("test","onViewCreated")
        }

        // Activity에 보여줄 fragment가 완전히 생성되면 호출된다,
        override fun onActivityCreated(savedInstanceState: Bundle?) {
            super.onActivityCreated(savedInstanceState)
            Log.d("test","onActivityCreated")
        }

        // Fragment가 화면에 보여질 때 호출된다.
        override fun onStart() {
            super.onStart()
            Log.d("test","onStart")
        }

        // Fragment가 보여지고 나서 호출된다.
        override fun onResume() {
            super.onResume()
            Log.d("test","onResume")
        }

        // Fragment가 화면에서 사라질 때 호출된다.
        override fun onPause() {
            super.onPause()
            Log.d("test","onPause")
        }

        // Fragment가 화면에서 완전히 사라져서 더 이상 보여지지 않을 때 호출
        override fun onStop() {
            super.onStop()
            Log.d("test","onStop")
        }

        // Fragment가 제거 될 때 호출된다.
        override fun onDestroy() {
            super.onDestroy()
            Log.d("test","onDestory")
        }

        // Fragment와 activity의 연결이 완전히 끊기기 전에 호출된다.
        override fun onDetach() {
            super.onDetach()
            Log.d("test","onDetach")
        }
    }
```

**step 1: Fragment가 화면에 나타나기 까지**

![test broad version]({{site.url}}/img/Android/fragment-lifecycle-step-1.png){:height="500"} 

**step 2: 어플리케이션이 백그라운드에 위치할 때**

![test broad version]({{site.url}}/img/Android/fragment-lifecycle-step-2.png){:height="500"} 

**step 3: 다시 어플리케이션을 메인 화면으로 가져왔을 때**

![test broad version]({{site.url}}/img/Android/fragment-lifecycle-step-3.png){:height="500"} 

**step 4: remove 메서드를 통해 Fragment를 제거했을 때**

![test broad version]({{site.url}}/img/Android/fragment-lifecycle-step-4.png){:height="500"} 

**step 5: remove 메서드를 호출한 상태에서 back button을 클릭 했을 때**

![test broad version]({{site.url}}/img/Android/fragment-lifecycle-step-5.png){:height="500"} 

addToBackStack 메서드를 호출하지 않았을 경우 step 4에서 detach가 발생한다.

## <span style="color:#0f7b6c">3. Fragment 내의 view 제어</span>

Activity 내의 view를 제어할때는 onCreate 메서드 내부에서 view를 제어했다. 그 이유는 onCreate 메서드가 호출될 때 activity 내 view 객체들이 생성되어 있기 때문에 사용성을 보장받을 수 있다. <br>
Fragment 역시 Fragment 내부의 view를 제어하기 위해서는 모든 view가 생성된 이후에 해야한다. 때문에 Fragment는 view를 생성하기위해 호출되는 onCreateView가 아닌 onViewCreated 메서드에서 view를 제어한다.
---
layout: post
title: "[Android] 안드로이드 Activity"
excerpt: "안드로이드 activity에 관해 알아본다."
categories: [Android]
comments: true
---

![android]({{site.url}}/img/Android/giphy.gif)

![app start]({{site.url}}/img/Android/app-start.png){:height="400" width="800"} 

위 과정을 통해 첫 화면을 관리하는 Activity가 생성 되었다. 일명 MainActiviy라고 불리는 그 activity는 자신에게 부여된 임무인 **'main_activity 화면 관리'**라는 임무를 완수하기 위해 계속 힘을 쓸 것이다. 그렇다면 사용자에게 다른 화면을 보여주려면 어떻게 해야 할까? <br>
**-> 보여주고자 하는 화면을 관리하는 다른 activity가 필요하다.**

## <span style="color:#0f7b6c">1. Activity 생성</span>

메인 화면에서 다른 화면으로 전환을 하기 위해서는 새로운 화면을 관리할 activity가 필요하다. 때문에 새로운 activity를 기존에 사용하던 activity에서 생성하면 된다. 이때 기존의 activity가 새로운 activity를 그냥 생성하는 것은 불가능하다. 첫 activity의 경우 AndroidManifest.xml 파일을 읽어 자동으로 생성 되었다면, 새로운 activity는 intent 객체를 이용하여 생성해야 한다.

**Intent 👍**

- **안드로이드 4대 구성 요소들을 실행하기 위해서는 Intent라는 객체가 필요하다.**
- Intent는 실행하고자 하는 4대 구성 요소와 관련된 정보를 가지고 있다.
- 개발자는 실행하고자 하는 4대 구성 요소의 정보를 Intent에 담고 이를 안드로이드 OS에게 전달하면 안드로이드 OS에 의해 해당 구성 요소가 실행된다.

activity 뿐만 아니라 다른 4대 구성들도 마찬가지로 intent라는 객체를 통해 생성된다고 한다. 하지만 여기서는 activity만을 위주로 살펴보자.

**step 1: 새로운 Activity 파일 생성**

![create new activity step 1]({{site.url}}/img/Android/create-new-activity-1.png){:height="400" width="800"} 

**step 2: 새로운 Activity를 생성할 적절한 위젯, 기능 생성**

버튼을 클릭하면 새로운 activity가 나오도록 설정!

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val second_intent = Intent(this, Second::class.java)
                startActivity(second_intent)
            }

        }
    }
```
버튼을 클릭했을 때, Intent객체가 생성되고 안드로이드 OS에게 Intent 객체를 전달(startAcitivty 메서드를 통해)했다.

**step 3: 결과 확인**

![create new activity step 3]({{site.url}}/img/Android/create-new-activity-3.png){:height="400"}

왼쪽은 어플리케이션 실행시 자동으로 생성되는 MainActivity이고, 오른쪽은 새롭게 생성한 Activity다. 

### 1-1. BackStack

- Activity에서 다른 Activity를 실행하면 이전 Activity는 Back Stack에 담겨 **정지 상태**가 되고 새로 실행된 Activity가 활동하게 된다.
- 새로운 실행된 Activity가 **제거** 되면 Back Stack에 있던 Activity가 다시 활동하게 된다.

![back stack]({{site.url}}/img/Android/back-stack.png){:height="400" width="800"}

### 1-2. Activity 생명 주기

![activity life cycle]({{site.url}}/img/Android/activity-lifecycle.png){:height="500"}

1. Activity가 생성되면 자동으로 onCreate 메서드가 호출된다. 화면을 만들고 최초의 초기화 등의 작업이 주로 이뤄진다.
2. onStart과 onResume을 거쳐 Activity는 **running**상태가 된다. 이때 비로소 화면상에 화면이 나타나게 된다.
3. running 상태에 있는 activity에 작은 팝업창 등이 발생되면, onPause 메서드가 호출되고 activit는 일시정지가 된다.
4. 그 이후 팝업창이 사라지면 onResume을 호출하고 다시 activity는 running 상태가 된다.
5. activity가 일시 정지 상태에서 완전히 activity가 안보이게 되면 onStop 메서드가 호출된다. 여기서 다시 activity가 나타나면 onRestart 메서드를 호출하고, onStart와 onResume 메서드를 거쳐 다시 running 상태가 된다.
6. activity가 완전히 소멸되면 onDestory 메서드가 호출된다.

Running 상태의 activity에서 팝업이나 알림 등의 요소로 activity의 상태가 변할 수 있다. 이때 일시 정지 혹은 정지 상태까지 상태가 변하게 되는데 일시 정지 상태에서 다시 화면이 나타나면 onResume 메서드를 호출하고 running 상태로 돌아가고, 정지 상태에서 다시 activity가 나타나면 onStart와 onResume을 거쳐 다시 running 상태로 돌아간다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            Log.d("test1", "onCrete")
        }

        override fun onStart() {
            super.onStart()
            Log.d("test1", "onStart")
        }

        override fun onResume() {
            super.onResume()
            Log.d("test1", "onResume")
        }

        override fun onPause() {
            super.onPause()
            Log.d("test1", "onPause")
        }

        override fun onStop() {
            super.onStop()
            Log.d("test1", "onStop")
        }

        override fun onDestroy() {
            super.onDestroy()
            Log.d("test1", "onDestroy")
        }
    }
```

**step 1: 어플을 처음 실행했을 때 MainActivity**

![activity life cycle]({{site.url}}/img/Android/activity-lifecycle-step-1.png){:height="500"}

**step 2: 어플이 Background에서 실행되고 있을 때**

![activity life cycle]({{site.url}}/img/Android/activity-lifecycle-step-2.png){:height="500"}

**step 3: Background에서 어플을 다시 실행시켰을 때**

![activity life cycle]({{site.url}}/img/Android/activity-lifecycle-step-3.png){:height="500"}

**step 4: 다시 어플이 Background에서 실행되고 있을 때**

![activity life cycle]({{site.url}}/img/Android/activity-lifecycle-step-4.png){:height="500"}

**step 5: 어플을 종료했을 때**

![activity life cycle]({{site.url}}/img/Android/activity-lifecycle-step-5.png){:height="500"}

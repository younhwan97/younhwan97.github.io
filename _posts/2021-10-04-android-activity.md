---
layout: post
title: "[Android] 안드로이드 Activity"
excerpt: "안드로이드 activity에 관해 알아본다."
categories: [Android]
comments: true
---

![app start]({{site.url}}/img/Android/app-start.png){:height="400" width="800"} 

위 과정을 통해 첫 화면을 관리하는 Activity가 생성 되었다. 일명 MainActiviy라고 불리는 그 activity는 자신에게 부여된 임무인 **'main_activity 화면 관리'**라는 임무를 완수하기 위해 계속 힘을 쓸 것이다. 그렇다면 사용자에게 다른 화면을 보여주려면 어떻게 해야 할까? <br>
**-> 보여주고자 하는 화면을 관리하는 다른 activity가 필요하다.**

## <span style="color:#0f7b6c">1. Activity 생성</span>

메인 화면에서 다른 화면으로 전환을 하기 위해서는 새로운 화면을 관리할 activity가 필요하다. 때문에 새로운 activity를 기존에 사용하던 activity에서 생성하면 된다. 이때 기존의 activity가 새로운 activity를 그냥 생성하는 것은 불가능하다. 첫 activity의 경우 AndroidManifest.xml 파일을 읽어 자동으로 생성 되었다면, 새로운 activity는 intent 객체를 이용하여 생성해야 한다.

**Intent 👍**

- **안드로이드 4대 구성 요소들을 실행하기 위해서는 Intent라는 객체가 필요하다.**
- Intent는 실행하고자 하는 4대 구성 요소와 관련된 정보를 가지고 있다.
- 개발자는 실행하고자 하는 4대 구성 요소의 정보를 Intent에 담고 이를 **안드로이드 OS에게 전달**하면 안드로이드 OS에 의해 해당 구성 요소가 실행된다.

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

- Activity에서 다른 Activity를 실행하면 이전 Activity는 Back Stack에 담겨 **정지 상태(Stop)**가 되고 새로 실행된 Activity가 활동하게 된다.
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

## <span style="color:#0f7b6c">2. startActivityForResult</span>

- **Activity에서 다른 Activity를 실행하고 돌아왔을 때, 어떤 처리가 필요하다면 Activity를 생성할 때 startActivity가 아닌 startActivityForResult 메서드를 이용한다.**
- startActivityForResult 메서드를 이용해 Activity를 실행하고 돌아오면 자동으로 onActivityResult 메서드가 호출된다.
- 필요한 작업은 onActivityResult에서 처리하면 된다.

**EX) second activity에서 돌아왔을 때 text값을 변경**
```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                var second_intent = Intent(this, MainActivity2::class.java)
                startActivityForResult(second_intent,0)

            }
        }

        override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
            super.onActivityResult(requestCode, resultCode, data)
            textView2.text = "다른 activity에서 돌아 왔습니다."
        }

    }
```

![startActivityForResult]({{site.url}}/img/Android/startActivityForResult.png){:height="500"}

**EX) 생성할 수 있는 activity가 여러개 일 때** <br>
상황: second activty에서 돌아오면 text를 "second activity에서 돌아왔습니다."로 설정하고, third activty에서 돌아왔을 때는 "third activity에서 돌아왔습니다."로 설정한다.
```kotlin
    class MainActivity : AppCompatActivity() {
        val SECOND_ACTIVITY = 100
        val THIRD_ACTIVITY = 200
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                var second_intent = Intent(this, MainActivity2::class.java)
                startActivityForResult(second_intent,SECOND_ACTIVITY)

            }

            button2.setOnClickListener {
                var third_intent = Intent(this, MainActivity3::class.java)
                startActivityForResult(third_intent, THIRD_ACTIVITY)
            }
        }

        override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
            super.onActivityResult(requestCode, resultCode, data)
            when(requestCode){
                SECOND_ACTIVITY -> {
                    textView2.text ="second activty에서 돌아왔습니다."
                }
                THIRD_ACTIVITY -> {
                    textView2.text ="third activity에서 돌아왔습니다."
                }
            }
        }
    }
```
requestCode를 통해 분기할 수 있다.

![multipleActivityForResult]({{site.url}}/img/Android/multipleActivityForResult.png){:height="500"}

**EX) 결과 값을 가지고 돌아 올 때** <br>
상황: 생성된 activity에서 사용자의 행동에따라 ok, cancel 등의 결과를 가지고 돌아와서 해당 결과를 분기한다.
```kotlin
    class MainActivity : AppCompatActivity() {
        val SECOND_ACTIVITY = 100
        val THIRD_ACTIVITY = 200
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                var second_intent = Intent(this, MainActivity2::class.java)
                startActivityForResult(second_intent,SECOND_ACTIVITY)

            }

            button2.setOnClickListener {
                var third_intent = Intent(this, MainActivity3::class.java)
                startActivityForResult(third_intent, THIRD_ACTIVITY)
            }
        }

        override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
            super.onActivityResult(requestCode, resultCode, data)
            when(requestCode){
                SECOND_ACTIVITY -> {
                    textView2.text ="second activty에서 돌아왔습니다."
                }
                THIRD_ACTIVITY -> {
                    when(resultCode){
                        RESULT_OK->{
                            textView2.text = "third에서 ok 상태로 돌아왔습니다."
                        }
                        RESULT_CANCELED->{
                            textView2.text = "third에서 canceled 상태로 돌아왔습니다."
                        }
                        RESULT_FIRST_USER->{
                            textView2.text = "third에서 first user 상태로 돌아왔습니다."
                        }
                        RESULT_FIRST_USER+1->{
                            textView2.text = "third에서 first user + 1 상태로 돌아왔습니다."
                        }
                    }
                }
            }
        }
    }
```

<br>

```kotlin
    class MainActivity3 : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main3)

            button3.setOnClickListener {
                setResult(RESULT_OK)
                finish()
            }

            button4.setOnClickListener {
                setResult(RESULT_CANCELED)
                finish()
            }

            button5.setOnClickListener {
                setResult(RESULT_FIRST_USER)
                finish()
            }
            button6.setOnClickListener {
                setResult(RESULT_FIRST_USER+1)
                finish()
            }
        }
    }
```
![multipleActivityForResult]({{site.url}}/img/Android/resultcode.png){:height="500"}

thrid activty에서 RESULT_OK, RESULT_CANCELED, RESULT_FIRST_USER, RESULT_FIRST_USER+1 각각의 버튼을 클릭하면 결과 값이 각각 RESULT_OK, RESULT_CANCELED, RESULT_FIRST_USER, RESULT_FIRST_USER+1로 셋팅된다. 해당 결과 값은 MainActivity의 onActivityResult에서 resultCode로 사용할 수 있다.

## <span style="color:#0f7b6c">3. 데이터 전달</span>

- **Activity를 생성하기 위해 사용되는 intent 객체에 데이터를 저장할 수 있다.**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val second_intent = Intent(this, MainActivity2::class.java)

                second_intent.putExtra("value1", 1)
                second_intent.putExtra("value2", 11.11)
                second_intent.putExtra("value3","str")
                second_intent.putExtra("value4", true)

                startActivity(second_intent)
            }
        }
    }
```

<br>

```kotlin
    class MainActivity2 : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main2)

            // 현재 activity를 실행하기 위해 사용한 intent로 부터 데이터를 가져올 수 있다.
            val data1 = intent.getIntExtra("value1", 0)
            val data2 = intent.getDoubleExtra("value2", 0.0)
            val data3 = intent.getStringExtra("value3")
            val data4 = intent.getBooleanExtra("value4", false)

            textView2.text = "$data1\n"
            textView2.append("$data2\n")
            textView2.append("$data3\n")
            textView2.append("$data4")
        }
    }
```

putExtra 메서드를 이용해 intent객체에 데이터를 저장할 수 있고, intent객체를 이용해 새로 생긴 activity에서는 intent객체의 getXXXExtra 메서드를 통해 데이터를 전달 받을 수 있다.

![putExtra]({{site.url}}/img/Android/putExtra.png){:height="500"}

### 3-1. 종료할 때 기존 activity로 데이터 가져오기

- **Activity를 생성하는 쪽에서는 생성하는 activity의 결과로 데이터를 받아야 하므로 startActivityForResult를 사용한다.**

```kotlin
    class MainActivity : AppCompatActivity() {
        val SECOND_INTENT = 100

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val second_intent = Intent(this, MainActivity2::class.java)

                second_intent.putExtra("value1", 1)
                second_intent.putExtra("value2", 11.11)
                second_intent.putExtra("value3","str")
                second_intent.putExtra("value4", true)

                startActivityForResult(second_intent,SECOND_INTENT )
            }
        }

        override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
            super.onActivityResult(requestCode, resultCode, data)

            when(requestCode){
                SECOND_INTENT -> {
                    val result1 = data?.getIntExtra("result1", 0)
                    val result2= data?.getDoubleExtra("result2", 0.0)
                    val result3 = data?.getStringExtra("result3")
                    val result4 = data?.getBooleanExtra("result4", true)

                    textView.text= "${result1}\n"
                    textView.append("$result2\n")
                    textView.append("$result3\n")
                    textView.append("$result4\n")

                }
            }
        }
    }
```

<br>

```kotlin
    class MainActivity2 : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main2)

            // 현재 activity를 실행하기 위해 사용한 intent로 부터 데이터를 가져올 수 있다.
            val data1 = intent.getIntExtra("value1", 0)
            val data2 = intent.getDoubleExtra("value2", 0.0)
            val data3 = intent.getStringExtra("value3")
            val data4 = intent.getBooleanExtra("value4", false)

            textView2.text = "$data1\n"
            textView2.append("$data2\n")
            textView2.append("$data3\n")
            textView2.append("$data4")

            val result_intent = Intent()
            result_intent.putExtra("result1", 100)
            result_intent.putExtra("result2", 11.11)
            result_intent.putExtra("result3","result")
            result_intent.putExtra("result4",false)

            setResult(RESULT_CANCELED, result_intent)
        }
    }
```

![putExtraResult]({{site.url}}/img/Android/putExtraResult.png){:height="500"}

### 3-2. 객체 전달하기

- **Intent를 통해 객체를 전달할 때는 객체 직렬화를 해야 하는데 안드로이드는 Parcelable 인터페이스를 사용한다.**
- Parcelable 인터페이스는 전달 받은 쪽에서 객체를 복원할 때 필요한 정보를 가진 부분을 의미한다.

```kotlin
    class MainActivity : AppCompatActivity() {
        val SECOND_INTENT = 100

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val second_intent = Intent(this, MainActivity2::class.java)

                val t1 = TestClass(100,11.11)
                second_intent.putExtra("obj1",t1)

                startActivityForResult(second_intent, SECOND_INTENT)
            }
        }

        override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
            super.onActivityResult(requestCode, resultCode, data)

            when(requestCode){
                SECOND_INTENT -> {

                }
            }
        }
    }

    class TestClass(val a1:Int, val a2:Double): Parcelable{
        constructor(parcel: Parcel) : this(parcel.readInt(), parcel.readDouble()) {
        }

        override fun writeToParcel(parcel: Parcel, flags: Int) {
            parcel.writeInt(a1)
            parcel.writeDouble(a2)
        }

        override fun describeContents(): Int {
            return 0
        }

        companion object CREATOR : Parcelable.Creator<TestClass> {
            override fun createFromParcel(parcel: Parcel): TestClass {
                return TestClass(parcel)
            }

            override fun newArray(size: Int): Array<TestClass?> {
                return arrayOfNulls(size)
            }
        }

    }
```

<br>

```kotlin   
    class MainActivity2 : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main2)

            val obj1 = intent.getParcelableExtra<TestClass>("obj1")
            textView3.text= "${obj1?.a1}"
            textView3.append("\n${obj1?.a2}")

            button2.setOnClickListener {
                finish()
            }
        }
    }
```

![transmissionObj]({{site.url}}/img/Android/transmissionObj.png){:height="500"}

Parcelable 인터페이스를 구현한 객체를 이용해 데이터를 전달하는 것은 사실 그 객체 자체를 전달하는 것은 아니다. 객체에서 변수에 해당하는 부분만 추출해 전달하고, 전달받은 쪽에서 그 변수를 읽어와 다른 객체를 새롭게 생성하는 개념이다. <br> 즉, 전달 할 때 사용한 객체와 전달 받아서 사용하는 객체는 서로 다른 객체다. 단지 값만 복사해서 새롭게 생성한 것이다. 

## <span style="color:#0f7b6c">4. 다른 어플리케이션의 Activity 실행하기</span>

- 안드로이드 4대 구성요소는 모두 AndroidManifest.xml 파일에 기록되어야 한다.
- 이 때 다른 어플리케이션이 실행할 수 있도록 하고자 한다면, Intent filter를 이용해 이름을 설정해주면 된다.
- 어플리케이션이 단말기에 설치되면 안드로이드 OS는 지정된 Intent filter의 이름을 확인하여 정리하고 실행 요청을 받으면 이를 실행할 수 있다.

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="kr.co.younhwan.myapplication">

        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/Theme.MyApplication">
            <activity android:name=".MainActivity2" android:exported="true">
                <intent-filter>
                    <action android:name="kr.co.younhwan.test_acitivity" />
                    <category android:name="android.intent.category.DEFAULT" />
                </intent-filter>
            </activity>>
            <activity
                android:name=".MainActivity"
                android:exported="true">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />

                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        </application>

    </manifest>
```

MainActivity2라는 이름의 activity를 다른 어플리케이션에서 실행할 수 있도록 설정하기 위해서 AndroidManifest.xml 파일 내부에 위와 같은 설정을 추가했다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val other_intent = Intent("kr.co.younhwan.test_activity")
                startActivity(other_intent)
            }
        }
    }
```

다른 어플리케이션에서 intent filter에 등록된 이름을 이용해 실행했다.

![create other activity]({{site.url}}/img/Android/create-other-activity.png){:height="500"}

My Application2에서 My Application이 실행된 것을 볼 수 있다.
---
layout: post
title: "[Android] 안드로이드 Board Cast Receiver"
excerpt: "안드로이드 Board Cast Receiver에 관해 알아본다."
categories: [Android]
comments: true
---

![android]({{site.url}}/img/Android/giphy.gif)

- **Broad Cast Receiver는 안드로이드 OS에서 특정 상황에 발생하는 메시지를 받아 들여 동작하는 실행단위이다.**
- Broad Cast Receiver는 반드시 외부에서 접근을 하기위한 이름을 가져야 한다.
- 안드로이드 OS에서 어떤 사건이 발생하면 사건과 관련된 이름으로 지정된 Broad Cast Receiver를 찾아 동작 시킨다.

예를 들면 스팸 문자를 판별해주는 어플리케이션이 있다. 해당 어플은 문자가 왔을 때 해당 문자가 스팸인지 아닌지 구별해야 하기 때문에 해당 어플리케이션에는 '문자 수신'이라는 Broad Cast Receiver가 등록되어 있을 것 이다.

> Broad Cast Receiver는 수신하고자 하는 메시지에 한해서 안드로이드 OS 혹은 다른 어플리케이션이 발생시키는 메시지를 수신하는 수신기다.<br><br> 다른 어플리케이션에서 발생하는 메시지역시 해당 어플리케이션이 intent 객체를 이용해 안드로이드 OS에게 '이런 메시지를 방송해줘'라고 요청하는 것이다.

## <span style="color:#0f7b6c">1. 안드로이드 8.0 이전</span>

- **AndroidManifest.xml에 receiver에 이름을 등록하면 이름에 해당하는 메시지가 왔을때, receiver가 호출된다.**

**step 1: Broad Cast Receiver 생성**

![create broad cast receiver step 1]({{site.url}}/img/Android/create-broad-cast-step-1.png){:height="500"} 

**step 2: Receiver가 호출되었을 때 동작할 코드 작성**


```kotlin
    class testReceiver : BroadcastReceiver() {

        override fun onReceive(context: Context, intent: Intent) {
            // This method is called when the BroadcastReceiver is receiving an Intent broadcast.
            val str = "Broad Cast Receiver가 동작하였습니다."
            val t1 = Toast.makeText(context,str,Toast.LENGTH_SHORT)
            t1.show()
        }
    }
```

**step 3: 구성파일에 Receiver 등록**

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="kr.co.younhwan.brapp1">

        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/Theme.Brapp1">
            <receiver
                android:name=".testReceiver"
                android:enabled="true"
                android:exported="true">
                <intent-filter>
                    <action android:name="kr.co.younhwan.testbr"></action>
                </intent-filter>
            </receiver>

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

intent-filter를 이용해 receiver의 이름을 "kr.co.younhwan.testbr"로 등록했다. 이제 해당 receiver는 안드로이드가 "kr.co.younhwan.testbr"이라는 이름의 메시지를 생성할 때 동작할 것 이다.
(안드로이드는 어플리케이션을 설치할 때 해당 어플리케이션의 구성파일을 보고 미리 ~라는 이름의 리시버가 있다는 것을 파악해 뒀다가 해당 이름의 메시지가 호출되면 미리 정리된 리시버 목록을 이용해 해당 이름의 리시버를 호출한다.)

**step 4: 메시지를 발생시켜 리시버가 동작하는지 확인**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                var broad_cast_intent = Intent("kr.co.younhwan.testbr")
                sendBroadcast(broad_cast_intent)
            }
        }
    }
```

![test broad version]({{site.url}}/img/Android/test-broad-version.png){:height="500"} 

왼쪽은 안드로이드 6.0, 오른쪽은 11.0 이다. 안드로이드 6.0에서는 "create broad cast message"버튼을 클릭했을 때 정상적으로 안드로이드가 메시지 요청을 받고 해당 이름의 메시지를 발생시켰지만 안드로이드 11.0에서는 그렇지 않았다.

## <span style="color:#0f7b6c">2. 안드로이드 8.0 부터</span>

- **안드로이드 8.0 부터는 개발자가 만든 Broad Cast Receiver와 OS에서 제공하는 일부 Broad Cast Receiver는 코드를 통해서만 등록 가능하다.**
- 이는 보안상의 이유로 Broad Cast Receiver를 가진 어플리케이션 내부에서만 사용하기 위한 제약이다.

안드로이드가 더이상 리시버 목록을 가지고 있지 않는다. 어플리케이션이 실행될 때 리시버가 os에 등록되고, 리시버는 어플리케이션이 실행된 상태에서만 동작하게 되는 것 이다.

```kotlin
    class MainActivity : AppCompatActivity() {
        // 리시버 객체 생성
        var br = testReceiver()

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // 8.0 이상 부터는 코드를 통해 등록하고 해제 해야 한다.
            if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O){
                val filter = IntentFilter("kr.co.younhwan.testbr")
                registerReceiver(br, filter)
            }

            button.setOnClickListener {
                var broad_cast_intent = Intent("kr.co.younhwan.testbr")
                sendBroadcast(broad_cast_intent)
            }
        }

        override fun onDestroy() {
            super.onDestroy()
            // 8.0 이상 부터는 코드를 통해 등록하고 해제 해야 한다.
            if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O){
                unregisterReceiver(br)
            }
        }
    }
```

![test broad version]({{site.url}}/img/Android/test-broad-version-2.png){:height="500"} 
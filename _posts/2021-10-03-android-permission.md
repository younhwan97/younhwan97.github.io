---
layout: post
title: "[Android] 안드로이드 Permission"
excerpt: "안드로이드 권한에 관해 알아본다."
categories: [Android]
comments: true
---

- 안드로이드는 개인 정보, 센서, 카메라, 저장소 등의 개인 정보와 관련된 기능을 사용하기 위해 권한이 필요하다.
- 권한 등록은 사용자가 어플레케이션을 다운받거나 설치 후 어플리케이션 정보에서 확인 가능하다.
- 권한 등록의 목적은 사용자에게 어플리케이션이 어떠한 기능을 사용하는지 알려주눈 목적에서 사용한다.

> 권한 등록을 필요로 하는 기능을 사용할 때 권한을 등록하지 않으면 오류가 발생하여 개발자는 반드시 권한을 등록해야 한다. <br><br>
개인 정보와 관련된 권한은 등록 후 사용자에게 승인을 받는 과정까지 필요하다.   

**AndroidManifest.xml**
```xml
    <?xml version="1.0" encoding="utf-8"?>
    <manifest xmlns:android="http://schemas.android.com/apk/res/android"
        package="kr.co.younhwan.permission">

        <application
            android:allowBackup="true"
            android:icon="@mipmap/ic_launcher"
            android:label="@string/app_name"
            android:roundIcon="@mipmap/ic_launcher_round"
            android:supportsRtl="true"
            android:theme="@style/Theme.Permission">
            <activity
                android:name=".MainActivity"
                android:exported="true">
                <intent-filter>
                    <action android:name="android.intent.action.MAIN" />

                    <category android:name="android.intent.category.LAUNCHER" />
                </intent-filter>
            </activity>
        </application>
        
        <!-- 권한을 추가 -->
        <uses-permission android:name="android.permission.INTERNET"></uses-permission>
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"></uses-permission>
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"></uses-permission>
        <uses-permission android:name="android.permission.READ_CONTACTS"></uses-permission>
        <uses-permission android:name="android.permission.WRITE_CONTACTS"></uses-permission>
        <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"></uses-permission>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"></uses-permission>
    </manifest>
```
![enroll permission]({{site.url}}/img/Android/enroll-permission.png){:height="400"}

AndroidManifest.xml이라는 구성파일에 어플리케이션에서 사용하고자 하는 권한을 등록했고, 위 사진과 같이 잘 등록된 것을 볼 수 있다. <br>
하지만 권한이 등록되었다고 모두 사용할 수 있는것은 아니다. 앞서 언급했듯 개인정보에 관한 권한은 사용자에게 승인을 받아야 정상적으로 사용할 수 있다. 

![approval permission]({{site.url}}/img/Android/approval-permission.png){:height="400"}

### 사용자에게 권한 승인 받기
```kotlin
    class MainActivity : AppCompatActivity() {
        // 개인 정보 사용 문제로 확인이 필요한 권한을 명시한다.
        // 사실 모든 권한을 기재해도 '확인이 필요한 권한만' 알아서 선별하여 사용에게 권한 요청을 보낸다.
        val permission_list = arrayOf(
            android.Manifest.permission.INTERNET,
            android.Manifest.permission.ACCESS_FINE_LOCATION,
            android.Manifest.permission.ACCESS_COARSE_LOCATION,
            android.Manifest.permission.READ_CONTACTS,
            android.Manifest.permission.WRITE_CONTACTS,
            android.Manifest.permission.READ_EXTERNAL_STORAGE,
            android.Manifest.permission.WRITE_EXTERNAL_STORAGE
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            // 사용자에게 권한 요청을 보낸다.
            requestPermissions(permission_list, 0)
        }
    }    
```

![request permission]({{site.url}}/img/Android/request-permission.png){:height="400"}

**사용자에게 권한을 확인 받은 후 자동으로 무언가를 처리할 때**
```kotlin
    class MainActivity : AppCompatActivity() {
        // 개인 정보 사용 문제로 확인이 필요한 권한을 명시한다.
        // 사실 모든 권한을 기재해도 '확인이 필요한 권한만' 알아서 선별하여 사용에게 권한 요청을 보낸다.
        val permission_list = arrayOf(
            android.Manifest.permission.INTERNET,
            android.Manifest.permission.ACCESS_FINE_LOCATION,
            android.Manifest.permission.ACCESS_COARSE_LOCATION,
            android.Manifest.permission.READ_CONTACTS,
            android.Manifest.permission.WRITE_CONTACTS,
            android.Manifest.permission.READ_EXTERNAL_STORAGE,
            android.Manifest.permission.WRITE_EXTERNAL_STORAGE
        )

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            // 사용자에게 권한 요청을 보낸다.
            requestPermissions(permission_list, 0)
        }

        override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<out String>, grantResults: IntArray) {
            super.onRequestPermissionsResult(requestCode, permissions, grantResults)

            for(idx in grantResults.indices){
                if(grantResults[idx] == PackageManager.PERMISSION_GRANTED){
                    // idx 번째 권한이 사용자에 의해서 승인 되었을 때
                }else if(grantResults[idx] == PackageManager.PERMISSION_DENIED){
                    // idx 번째 권한이 사용자에 의해서 거절 되었을 때
                }
            }
        }
    }
```
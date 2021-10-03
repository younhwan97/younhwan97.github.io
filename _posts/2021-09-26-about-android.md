---
layout: post
title: "[Android] 안드로이드란?"
excerpt: "안드로이드에 관해 알아본다."
categories: [Android]
comments: true
---

![main image]({{site.url}}/img/Android/denny-muller-HfWA-Axq6Ek-unsplash.jpg){:height="500" width="800"}

## <span style="color:#0f7b6c">1. 안드로이드란?</span>

- 2008년 구글이 발표한 스마트폰 OS
- 운영체제와 미들웨어, 주요 어플리케이션을 포함 👉 **소프트웨어집합**
- 현재 개발언어로 Java와 Kotlin 둘 다 지원하고 있음

### 1-1. 안드로이드 특징

- 어플리케이션 프레임워크를 제공
- ART 가상 머신
- OPEN GL ES 3.x 기반 3D 그래픽 지원
- SQLite 데이터 베이스
- 다양한 미디어(이미지, 동영상, 사운드 등등) 지원
- Android Studio IDE 제공
- 센서 등 다양한 하드웨어 지원  

### 1-2. 안드로이드 구조

![안드로이드구조]({{site.url}}/img/Android/android-stack_2x.png){: height="550" width="600"}

- HAL: 리눅스 커널과 하드웨어 기기간의 인터페이스 부분으로 단말기 제조사가 드라이버를 구현할 수 있도록 제공되는 계층
- Android Runtime: 안드로이드는 어플리케이션을 구동하기 위한 가상머신 (5.0 이상은 ART를 사용)
- Native C/C++: 안드로이드 OS가 어플리케이션 및 기능들을 구동하기 위해 사용하는 라이브러리. (개발자가 Java나 Kotlin으로 만들어진 API를 이용하면 여기에 구현되어 있는 C코드가 동작)
- Java/Kotlin API: 개발자가 어플리케이션을 제작할 때 사용하는 라이브러리
- System Apps: 안드로이드 OS 내부에 내장되어 있는 어플리케이션으로 개발자가 어플리케이션을 개발할 때 일부 기능을 가져댜 사용할 수 있다. (google map 등등)

### 1-3. Android X 라이브러리
- 안드로이드는 지속적인 버전 업데이트를 통해 많은 변화를 이뤘다.
- 이에 하위 버전의 OS와 상위 버전의 OS간의 차이가 심하게 나타난다.
- 이에 상위 버전에 추가된 기능 중 일부를 하위 버전에서도 사용할 수 있도록 라이브러리가 어플리케이션에 추가되는데 이를 Support 라이브러리라고 부른다.
- 그런데 이 라이브러리도 버전 별로 너무 많이 나눠지게 되어 안드로이드 10 버전 부터는 Android X라는 이름의 라이브러리로 통합되었다.

## <span style="color:#0f7b6c">2. 안드로이드 개발 환경 구축</span>

**step 1: JDK 설치**

- 안드로이드는 Java와 Kotlin 언어로 개발이 가능하다.
- Java는 물론이고, **Kotlin도 Java Virtual Machine(JVM)에서 동작하는 프로그래밍 언어**이기 때문에 JDK가 필요하다.
- [https://www.oracle.com/java/technologies/downloads/](https://www.oracle.com/java/technologies/downloads/) <br>
    <span style="color:#808080"> ㄴ 자바 환경 변수 설정을 빼먹지 않도록 한다. </span>


**step 2: Android Studio 설치**

- [https://developer.android.com/studio](https://developer.android.com/studio)

## <span style="color:#0f7b6c">3. 안드로이드 프로젝트 생성</span>

**step 1: New Project 버튼 클릭**
![create project step 1]({{site.url}}/img/Android/create-project-step-1.png){:height="300" width="600"}

**step 2: Activity 선택**
![create project step 2]({{site.url}}/img/Android/create-project-step-2.png){:height="300" width="600"}

- Activity: 눈에 보이는 화면

**step 3: 프로젝트 설정**
![create project step 3]({{site.url}}/img/Android/create-project-step-3.png){:height="300" width="600"}

- Package name: 마켓에서 어플리케이션을 구분하는 이름 (중복 x, 일반적으로 회사 도메인을 뒤집어서 사용) <br>
- Language: 현재 Java와 Kotlin을 사용해 안드로이드 어플리케이션을 만들 수 있다. <br>

**step 4: AVD 생성**
![create project step 4]({{site.url}}/img/Android/create-project-step-4.png){:height="300" width="600"} <br><br>
![create project step 5]({{site.url}}/img/Android/create-project-step-5.png){:height="300" width="600"} <br><br>
![create project step 6]({{site.url}}/img/Android/create-project-step-6.png){:height="300" width="600"} 
- AVD: Android Virtual Device의 약자로서 안드로이드 에뮬레이터이다.

## <span style="color:#0f7b6c">4. 안드로이드 어플리케이션의 동작원리 👍</span>

### 4-1. 안드로이드 4대 구성 요소
- **Activity**: 눈에 보이는 화면을 관리하는 실행 단위
- **Service**: 화면을 가지지 않는 실행 단위 (= 백그라운드 프로세싱)
- **Broadcast Receiver**: OS가 메시지를 받으면 실행되는 실행 단위
- **Content Provider**: 저장된 데이터를 제공하기 위해 실행되는 실행 단위

> <span style="color:#8d7edc">**안드로이드 어플리케이션**</span>은 4대 구성 요소들을 통합 관리하는 번들 개념이다.<br><br>
각각의 구성요소가 모여 하나의 안드로이드 어플리케이션을 이룬다. 각 구성요소는 자신들이 실행될 적절한 상황이 왔을 때, 개발자가 작성한와 함께 실행된다.

![안드로이드 4대 구성요소]({{site.url}}/img/Android/android_4_components.png){:height="500" width="700"} 

### 4-2. 프로젝트의 구조
![안드로이드 프로젝트 구조]({{site.url}}/img/Android/project-structure.png){:height="300"} 

- AndroidManifest.xml: 안드로이드 어플리케이션에 관련된 설정 파일
- java 폴더: 개발자가 작성하는 소스 코드
- res 폴더: 이미지, 사운드, 데이터 등 어플리케이션에서 필요한 리소스

### 4-3. 동작원리

**어플리케이션 설치**
1. 제작된 어플리케이션은 apk라는 파일로 압축되어 마켓에 등록된다.
2. apk 파일을 단말기에 다운로드하게 되면 자동으로 설치가 이뤄진다.
3. 안드로이드 OS는 설치가 완료되면 **AndroidManifest.xml** 파일의 내용을 분석하게 된다.
4. 여기에서 안드로이드 4대 구성요소 중 어떤 것들이 있는지 파악하여 이를 정리하게 된다.

**어플리케이션 실행**
- 안드로이드 어플리케이션이 실행되면 안드로이드 OS는 첫 번째 화면을 사용자에게 보여주려고 한다.
- 이 때 **AndroidManifest.xml**에 있는 여러 구성 요소 중 activity를 찾는다.
- 이 activity 중에 다음과 같이 작성되어 있는 것을 첫 화면을 관리하는 요소로 판단하고 이를 실행시켜 준다.

```kotlin
    <activity
        android:name=".MainActivity"
        android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />

            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
```

- activity의 name 속성의 클래스의 객체를 생성한 후 onCreate 메서드를 호출한다.
- 이 때 setContentView 메서드에 관리할 화면을 지정하는데 res 폴더의 layout에 있는 xml 파일을 지정하게 된다.
- 이를 통해 화면을 구성하고 단말기 화면에 나타나게 된다.

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
```

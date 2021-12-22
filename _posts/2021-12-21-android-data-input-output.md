---
layout: post
title: "[Android] 안드로이드 Data Input/Output"
excerpt: "안드로이드 Data Input/Output에 관해 알아본다."
categories: [Android]
comments: true
---

## <span style="color:#0f7b6c">1. XML을 이용한 View 객체 생성하기</span>

**LayoutInflater**

- Activity가 처음 생성될 때 OS는 Layout 폴더에 있는 XML파일을 이용해 View를 생성한다.
- 만약 실행 중 View를 만들어 추가할 경우에는 코드를 통해 View를 만들어 추가해줘야 한다.
- LayoutInflater를 사용하면 XML로 만든 화면 모양을 View 객체로 만들어서 사용할 수 있다.

**EX) 버튼을 눌렀을 때 View 객체를 생성하여 추가**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val new_view = layoutInflater.inflate(R.layout.layout_sub, null)

            button.setOnClickListener {
                // container라는 id를 갖는 layout에 새롭게 생성한 view를 추가한다.
                container.addView(new_view)
            }
        }
    }
```

LayoutInflater를 통해 View를 생성할 때 첫번째 인자로 생성할 View의 XML파일을 넘겨 객체를 생성하고, addView 메서드를 통해 추가했다. 이때 두번째 인자로 추가하고자 하는 layout의 id를 전달하면 addView 메서드를 호출할 필요없이 객체가 생성되자마자 해당 layout에 추가된다. 단, 2번째 인자를 사용해 자동으로 추가 된 View의 경우 추가 후 다시 제거할 수 없다.

## <span style="color:#0f7b6c">2. 코드를 통한 View 객체를 생성하기</span>

- 코드를 통해 View 객체를 생성하여 layout에 추가할 수 있다.
- View 객체를 생성할 때는 생성자에 Context 객체를 설정해줘야 한다.
- Context는 어떠한 작업을 하기 위한 정보를 가지고 있는 객체를 통칭한다.
- 안드로이드에서는 Activity가 Context를 상속받고 있기 때문에 this를 넣어주면 되고 그 외에는 Context를 구하는 다양한 메서드를 통해 설정한다.

**EX) 버튼을 눌렀을 때 View 객체를 코드를 통해 생성하여 추가**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val new_btn = Button(this)
            new_btn.text = "추가된 버튼입니다."

            button.setOnClickListener {
                // container라는 id를 갖는 layout에 새롭게 생성한 view를 추가한다.
                container.addView(new_btn)
            }
        }
    }
```

일부 UI는 가로, 세로 길이를 제대로 지정해줘야 하는 경우도 있다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val new_btn = Button(this)
                val params = LinearLayout.LayoutParams(
                    LinearLayout.LayoutParams.WRAP_CONTENT,
                    LinearLayout.LayoutParams.WRAP_CONTENT
                )
                new_btn.text = "추가된 버튼입니다."
                new_btn.layoutParams = params
                container.addView(new_btn)
            }
        }
    }
```

## <span style="color:#0f7b6c">3. Application 클래스</span>

- 안드로이드 어플리케이션에 단 하나를 지정할 수 있는 객체이다.
- 이 객체는 같은 안드로이드 어플리케이션이라면 어디서든 주소 값을 가져올 수 있다.
- 이를 통해 안드로이드의 다양한 구성요소에서 공통적으로 사용하는 데이터를 관리할 수 있다.

데이터의 경우 여러 View들 사이에서 Intent 객체를 이용해 얼마든지 값을 전달하고 받을 수 있다. 하지만 대부분의 View에서 공통적으로 사용되는 데이터라면 매번 Intent 객체를 이용해 값을 전달하고 받는게 불편하기 때문에 Application 클래스를 사용하는게 좋다.

```kotlin
    class AppClass: Application() {
        var value1 = 0
        var value2 = ""

        fun method1(){
            Log.d("test","메소드1을 호출하였습니다.")
        }

    }
```

위와 같이 Application을 상속받는 클래스를 생성하고 manifest.xml 파일에 해당 클래스의 이름을 등록하면 된다.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="kr.co.younhwan.applicationclass">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.ApplicationClass"
        android:name=".AppClass"> <!-- Application 클래스 이름-->
        <activity
            android:name=".SecondActivity"
            android:exported="true" />
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

<br>

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val app = application as AppClass
                textView.text = "${app.value1}"
            }

        }
    }
```

Applicaion 값의 경우 위와 같이 형변환을 거쳐 가져오면 된다.

## <span style="color:#0f7b6c">4. 파일 입출력</span>

**안드로이드 저장소**

- 안드로이드는 어플리케이션이 데이터를 저장할 수 있는 저장소를 두 가지로 제공한다.
- 내부 저장소 : 어플리케이션을 통해서만 접근이 가능하다.
- 외부 저장소 : 단말기 내부의 공유 영역으로 모든 어플리케이션이 접근 가능하다. 단말기를 컴퓨터에 연결하면 탐색기를 통해 접근할 수 있는 영역을 의미한다.

**파일 입출력**

- 내부 저장소 : openFileOutput, openFileInput
- 외부 저장소 : FileOutputStream, FileInputStream

### 4-1. Scroped Storage

- 외부 저장소에 저장된 파일은 모든 어플리케이션이 자유롭게 접근할 수 있어서 보안상 문제가 발생했다.
- 이에 안드로이드 10에서는 외부 저장소에 제한을 두어 보안을 강화했고, 그러한 외부 저장소를 Scroped Storage라고 부른다.

**구성**

- 앱 데이터 폴더 : 읽고 쓰는데 권한이 필요가 없으며, 해당 어플리케이션만 접근이 가능하다. 어플리케이션 삭제 시 폴더도 같이 삭제된다.
- 미디어 파일들 : 사진, 동영상, 음원파일들을 저장하는 장소이다.
- 공용 파일들 : Downloads 폴더. 이 폴더에 저장된 파일은 모든 어플리케이션에서 접근할 수 있다. 단, 코드를 통한 직접 접근은 불가능하고 단말기에 설치된 파일관리 어플을 통해서만 가능하다.

**내부 저장소 읽기/쓰기**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // 내부 저장소에 파일 쓰기
            // Context.MODE_PRIVATE: 덮어 쓰기
            // Context.MODE_APPEND: 이어서 쓰기
            val fos = openFileOutput("data.dat", Context.MODE_PRIVATE)
            val dos = DataOutputStream(fos)

            dos.writeInt(100)
            dos.writeDouble(11.11)
            dos.writeBoolean(true)
            dos.flush()
            dos.close()

            // 내부 저장소 파일 읽기
            val fis = openFileInput("data.dat")
            val dis = DataInputStream(fis)

            textView.text = "${dis.readInt()}\n"
            textView.append("${dis.readDouble()}\n")
            textView.append("${dis.readBoolean()}\n")
        }
    }
```

![android-read-and-write-internal-storage]({{site.url}}/img/Android/android-read-and-write-internal-storage.png){: height="400"}

**외부 앱 데이터 폴더 읽기/쓰기**

```kotlin
    class MainActivity : AppCompatActivity() {

        // 외부 저장소의 앱 데이터 디렉토리의 경로
        lateinit var externalStoragePath: String

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // getExternalFilesDir 인자가 null 일 때 -> 앱 데이터 디렉토리 경로
            // getExternalFilesDir 인자가 Environment.DIRECTORY_종류 일 때 -> 해당 종류의 디렉토리 경로
            externalStoragePath = getExternalFilesDir(null).toString()

            // 외부 저장소 쓰기
            val fos = FileOutputStream("${externalStoragePath}/data2.dat")
            val dos = DataOutputStream(fos)

            dos.writeInt(11)
            dos.writeDouble(11.11)
            dos.writeBoolean(false)
            dos.writeUTF("HELLO WORLD")

            dos.flush()
            dos.close()

            // 외부 저장소 읽기
            val fis = FileInputStream("${externalStoragePath}/data2.dat")
            val dis = DataInputStream(fis)

            textView.text = "${dis.readInt()}\n"
            textView.append("${dis.readDouble()}\n")
            textView.append("${dis.readBoolean()}\n")
            textView.append("${dis.readUTF()}")

            dis.close()

        }
    }
```

![android-read-and-write-external-storage]({{site.url}}/img/Android/android-read-and-write-external-storage.png){: height="400"}

**Downloads 폴더 읽기/쓰기**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // 파일 관리앱의 Activity를 실행
                // Intent.ACTION_CREATE_DOCUMENT: 파일을 만들 때
                val fileIntent = Intent(Intent.ACTION_CREATE_DOCUMENT)
                fileIntent.addCategory(Intent.CATEGORY_OPENABLE)

                // mine type 지정
                fileIntent.type = "*/*" // 모든 파일
                fileIntent.type = "image/*" // 모든 이미지
                startActivityForResult(fileIntent, 100)
            }

            button2.setOnClickListener {
                // 파일을 읽기 위해서 파일 관리앱의 Activity를 실행
                val fileIntent2 = Intent(Intent.ACTION_OPEN_DOCUMENT)
                fileIntent2.type = "*/*" // 모든 파일
                startActivityForResult(fileIntent2, 200)
            }
        }

        override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
            super.onActivityResult(requestCode, resultCode, data)

            when(requestCode){
                100 -> {
                    if(resultCode == RESULT_OK){
                        // 사용자가 파일을 생성했을 때 data 객체 안에 파일의 경로가 담겨온다.
                        val des1 = contentResolver.openFileDescriptor(data?.data!!, "w")
                        val fos = FileOutputStream(des1?.fileDescriptor)
                        val dos = DataOutputStream(fos)

                        dos.writeInt(300)
                        dos.writeDouble(33.33)
                        dos.writeBoolean(true)
                        dos.writeUTF("hello")

                        dos.flush()
                        dos.close()
                    }
                }

                200 -> {
                    if(resultCode== RESULT_OK){
                        val des2 = contentResolver.openFileDescriptor(data?.data!!, "r")
                        val fis = FileInputStream(des2?.fileDescriptor)
                        val dis = DataInputStream(fis)

                        textView.text = "${dis.readInt()}\n"
                        textView.append("${dis.readDouble()}\n")
                        textView.append("${dis.readBoolean()}\n")
                        textView.append("${dis.readUTF()}")
                    }
                }
            }
        }
    }
```

![android-read-and-write-external-storage]({{site.url}}/img/Android/android-read-and-write-downloads-storage.png){: height="400"}

## <span style="color:#0f7b6c">5. raw 파일 읽어오기</span>

- raw 데이터는 가공되지 않은 원천 데이터를 의미
- 사운드나 동영상, 사진 등을 데이터 용량을 줄이기 위해 압축을 하게 되는데 이러한 가공을 거치지 않은 순수 데이터 들을 raw 데이터라고 부른다.
- 안드로이드에서 각종 데이터 파일이나 동영상, 사운드 등의 데이터를 사용할 때 주로 사용한다. 

**raw 폴더**

- 실행 중 다운받거나 생성된 데이터 파일은 내부 저장소나 외부 저장소에 저장해 두었다가 필요할 때 읽어오면 된다.
- 만약 데이터가 저장된 파일을 어플리케이션 내부에 포함 시키겠다면 raw 폴더에 저장하면 된다.
- raw 폴더에 저장된 파일은 스트림을 손쉽게 추출할 수 있다.
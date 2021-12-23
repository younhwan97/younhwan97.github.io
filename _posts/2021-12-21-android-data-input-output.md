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
- 안드로이드에서 각종 데이터 파일이나 동영상, 사운드 등의 데이터를 사용할 때 주로 사용한다. (안드로이드에서는 XML과 이미지 파일을 제외한 모든 파일을 raw 파일로 정의)

**raw 폴더**

- 실행 중 다운받거나 생성된 데이터 파일은 내부 저장소나 외부 저장소에 저장해 두었다가 필요할 때 읽어오면 된다.
- 만약 데이터가 저장된 파일을 어플리케이션 내부에 포함 시키겠다면 raw 폴더에 저장하면 된다.
- raw 폴더에 저장된 파일은 스트림을 손쉽게 추출할 수 있다.

**step 1: raw 폴더 생성**

![android-read-and-write-external-storage]({{site.url}}/img/Android/create-raw-dir-step-1.png){: height="400"}

![android-read-and-write-external-storage]({{site.url}}/img/Android/create-raw-dir-step-2.png){: height="400"}

raw 폴더 안에 들어가는 파일의 이름은 숫자, 언더바(_)와 영문 소문자만 사용할 수 있으며, 영문 소문자로 시작해야 한다.

**step 2: test 파일 생성**

![android-read-and-write-external-storage]({{site.url}}/img/Android/create-raw-dir-step-3.png){: height="400"}

**step 3: test 파일 읽어오기**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // Stream 추출
                val inputStream = resources.openRawResource(R.raw.data1)

                // Text 파일 처리를 위한 Buffered Reader 생성
                val isr = InputStreamReader(inputStream, "UTF-8")
                val br = BufferedReader(isr)

                var str:String? = null
                val sb = StringBuffer()

                do{
                    str = br.readLine()

                    if(str != null){
                        sb.append("${str}\n")
                    }
                }while (str != null)

                br.close()
                textView.text = sb.toString()
            }
        }
    }
```

![android-read-and-write-external-storage]({{site.url}}/img/Android/create-raw-dir-step-4.png){: height="400"}

### 5-1. 사운드 파일 읽어오기

```kotlin
    class MainActivity : AppCompatActivity() {

        var mp:MediaPlayer? = null

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            if(mp == null){
                mp = MediaPlayer.create(this, R.raw.song)
                mp?.start()
            }
        }
    }
```

### 5-2. 동영상 파일 읽어오기

video view를 추가해야 한다!

```kotlin
    class MainActivity : AppCompatActivity() {

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            if(videoView.isPlaying == false){
                // 영상 파일 경로
                val uri = Uri.parse("android:resource://${packageName}/raw/filename")
                videoView.setVideoURI(uri)
                videoView.start()
            }
        }
    }
```

## <span style="color:#0f7b6c">6. assets</span>

- raw 데이터 파일은 raw 폴더에 담으면 스트림을 손쉽게 추출할 수 있다는 장점이 있다.
- 허나 raw 폴더는 하위 폴더를 만드는 등 계층적으로 관리할 수 없다.
- 만약 파일들을 계층적인 폴더 구조로 만들어 관리하겠다면 assets 폴더를 사용한다.
- assets 폴더는 res 폴더 내부가 아니므로 리소스(R 클래스)로 관리할 수 없다.

**step 1: assets 폴더 생성**

![android-read-and-write-external-storage]({{site.url}}/img/Android/create-assets-step-1.png){: height="400"}

![android-read-and-write-external-storage]({{site.url}}/img/Android/create-assets-step-2.png){: height="400"}

위에서 언급했다시피 res 폴더가 아닌 외부에 폴더가 만들어 진 것을 볼 수 있다.

**step 2: 데이터 읽어오기**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val inputStream = assets.open("text/data1.txt")
            val isr = InputStreamReader(inputStream, "UTF-8")
            val br = BufferedReader(isr)

            var str : String? = null
            val sb = StringBuffer()

            do{
                str = br.readLine()
                if(str != null){
                    sb.append("$str\n")
                }
            }while (str != null)

            br.close()

            textView.text = sb.toString()
        }
    }
```

### 6-1. 폰트 사용하기

- assets 폴더에는 다양한 종류의 파일들을 담고 사용할 수 있다.
- 특히 폰트 파일을 손쉽게 사용할 수 있도록 클래스를 제공하고 있다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // 폰트 객체 생성
            val face = Typeface.createFromAsset(assets,"font/font_file_name")
            textView.typeface = face
        }
    }
```

## <span style="color:#0f7b6c">7. SQLite 데이터베이스</span>

- 안드로이드에서 사용하는 내장 데이터 베이스로 표준 SQL문을 사용하는 관계형 데이터 베이스이다.
- MySQL과 유사한 문법을 사용하고 있으며 일반적인 관계형 데이터 베이스가 가지고 있는 기능을 가지고 있다.

**동작 방식**

- SQLite 데이터베이스는 임베디드형 데이터베이스로써 데이터베이스를 사용하는 어플리케이션에 셋팅되는 데이터 베이스이다.
- 안드로이드는 안드로이드 OS에 내장되어 있으며 개발자가 만드는 어플리케이션은 안드로이드 OS에게 쿼리문을 전달하고 안드로이드 OS가 직접 데이터 베이스에 대한 처리를 하게된다.
- 즉 단말기 내부에 설치된 로컬 데이터베이스로써 어플리케이션이 관리하는 것이 아닌 운영체제에 의해 관리되고, 다른 단말기에서는 접근할 수 없다.

**작성 방식**

- 안드로이드에서의 SQLite 데이터베이스 사용은 쿼리문을 이용하는 방법과 제공되는 클래스를 이용하는 방법 두 가지가 있다.
- 쿼리문을 이용하는 방식은 일반적인 SQL문을 사용하며 MySQL과 유사한 문법을 지닌다.
- 클래스를 이용하는 방법은 개발자가 정해줘야 하는 몇 가지 정보를 제공하면 쿼리문이 생성되고 실행되는 구조이다.

**SQLite OpenHelper**

- 이름 그대로 안드로이드 SQLite 데이터베이스 오픈을 도와주는 클래스다.
- 안드로이드에서 SQLite 데이터베이스를 사용하려면 SQLiteOpenHelper를 상속받은 클래스를 만들어야한다.
- 이 클래스는 사용할 데이터베이스의 이름을 설정하는 것 뿐만 아니라 다음 기능들을 제공한다.
- 지정된 데이터베이스 파일을 사용하려고 할 때 파일이 없으면 파일을 만들고 onCreate 메서드를 호출한다. 이 메서드에서는 테이블을 만드는 쿼리를 실행해주면 된다.
- 어플리케이션을 서비스하다가 데이터베이스 구조를 변경하려면 데이터베이스의 버전을 변경하면 된다. 버전을 변경하면 onUpgrade 메서드가 호출되고 여기에서 테이블을 새로운 구조로 변경해주는 작업을 해주면 된다.

즉 onCreate 메서드에서는 **항상 최신의 테이블구조**를 만들 수 있는 쿼리문이, onUpgrade에서는 테이블의 구조가 **변경되기 전에서 새로운 구조로 변경**을 해주는 쿼리가 작성되면 된다.

**step 1: 클래스 생성**

![android-read-and-write-external-storage]({{site.url}}/img/Android/create-sql-lite-db-step-1.png){: height="400" width="700"}

```kotlin
    class dbHelper: SQLiteOpenHelper {

        constructor(context: Context): super(context, "Test.db", null, 1)

        // 데이터 베이스 파일이 없는 경우 파일을 만들고 자동으로 호출된다.
        // 어플리케이션 설치 후 최초로 접근시 호출
        // 최신 형태의 테이블을 생성하는 쿼리문을 작성한다.
        override fun onCreate(p0: SQLiteDatabase?) {
            TODO("Not yet implemented")
        }

        // 버전이 변경된 경우 호출
        // 기존에 앱을 사용하는 사용자를 위해 테이블의 구조를 최신 형태로 만들어주는 쿼리문을 작성한다.
        override fun onUpgrade(p0: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {
            when(oldVersion){
                1 -> {
                    // 1에서 2로 변경되었을 때
                }
                2 -> {
                    // 2에서 3로 변경되었을 때
                }
            }
        }
    }
```

**step 2: DB Open**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val helper = dbHelper(this)
            
        }
    }
```

**step 3: 테이블 생성**

```kotlin
    class dbHelper: SQLiteOpenHelper {

        constructor(context: Context): super(context, "Test.db", null, 1)

        // 데이터 베이스 파일이 없는 경우 파일을 만들고 자동으로 호출된다.
        // 어플리케이션 설치 후 최초로 접근시 호출
        // 최신 형태의 테이블을 생성하는 쿼리문을 작성한다.
        override fun onCreate(p0: SQLiteDatabase?) {
            val sql = """
                create table TestTable
                    (idx integer primary key autoincrement,
                    textData text not null,
                    intData integer not null,
                    floatData real not null,
                    dateData date not null)
            """.trimIndent()
            p0?.execSQL(sql)
        }

        // 버전이 변경된 경우 호출
        // 기존에 앱을 사용하는 사용자를 위해 테이블의 구조를 최신 형태로 만들어주는 쿼리문을 작성한다.
        override fun onUpgrade(p0: SQLiteDatabase?, oldVersion: Int, newVersion: Int) {
            when(oldVersion){
                1 -> {
                    // 1에서 2로 변경되었을 때
                }
                2 -> {
                    // 2에서 3로 변경되었을 때
                }
            }
        }
    }
```

**step 4: insert/select/upgrade/delete query**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            insertBtn.setOnClickListener {
                val helper = dbHelper(this)

                val query = """
                    insert into TestTable (textData, intData, floatData, dateData)
                    values (?, ?, ?, ?)
                """.trimIndent()

                // ?에 바인딩될 값
                val sdf = SimpleDateFormat("yyyy-MM-dd", Locale.getDefault())
                val now = sdf.format(Date())

                val arg1 = arrayOf("문자열 1", "100", "11.11", now) // 타입에 관련없이 문자열
                val arg2 = arrayOf("문자열 2", "200", "22.22", now)

                // 저장
                helper.writableDatabase.execSQL(query, arg1)
                helper.writableDatabase.execSQL(query, arg2)

                helper.writableDatabase.close()

                textView.text = "저장 완료"
            }

            selectBtn.setOnClickListener {
                val helper = dbHelper(this)

                val query = "select * from TestTable"

                // 쿼리 실행
                val cl = helper.writableDatabase.rawQuery(query, null)
                textView.text = ""

                while (cl.moveToNext()){ // 가져올 데이터가 남아있다면 moveToNext 메서드가 true를 반환
                    // 가져올 컬럼의 인덱스 번호를 추출
                    val idx1 = cl.getColumnIndex("idx")
                    val idx2 = cl.getColumnIndex("textData")
                    val idx3 = cl.getColumnIndex("intData")
                    val idx4 = cl.getColumnIndex("floatData")
                    val idx5 = cl.getColumnIndex("dateData")

                    // 데이터를 추출한다.
                    val idx = cl.getInt(idx1)
                    val textData = cl.getString(idx2)
                    val intData = cl.getInt(idx3)
                    val floatData = cl.getFloat(idx4)
                    val dateData = cl.getString(idx5)

                    textView.append("${idx}\n")
                    textView.append("${textData}\n")
                    textView.append("${intData}\n")
                    textView.append("${floatData}\n")
                    textView.append("${dateData}\n")
                }

                helper.writableDatabase.close()
            }

            updateBtn.setOnClickListener {
                val helper = dbHelper(this)

                val query = """
                    update TestTable set textData = ? where idx = ?
                """.trimIndent()

                // ?에 바인딩될 값 세팅
                val arg1 = arrayOf("문자열3", 1)

                helper.writableDatabase.execSQL(query, arg1)
                helper.writableDatabase.close()
            }

            deleteBtn.setOnClickListener {
                val helper = dbHelper(this)

                val query = """
                    delete from TestTable where idx = ? 
                """.trimIndent()

                // ?에 바인딩될 값 세팅
                val arg1 = arrayOf("1")

                helper.writableDatabase.execSQL(query,arg1)
                helper.writableDatabase.close()
            }
        }
    }
```

### 7-1. 제공되는 클래스를 이용해 SQLite 데이터베이스 사용

- 안드로이드는 SQLite 데이터베이스 사용 시 직접 직접 쿼리문을 작성하는 것 분만 아니라 제공되는 클래스를 이용하는 방법을 제공한다.
- 코드 구현은 대부분 비슷하며 쿼리문 작성 대신에 클래스를 사용하면 된다.

**ContentValue**

- 클래스를 이용하는 방법을 사용할 때 가장 중요한 클래스
- ContentValue 클래스는 값을 저장할 때 이름을 부여하는 클래스로써 값을 저장할 때 사용하는 이름은 테이블의 컬럼이름과 매칭된다.
- ContentValue에 저장한 데이터는 테이블의 칼럼과 매칭되어 insert, update 등에 사용된다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            insertBtn.setOnClickListener {
                val helper = DBHelper(this)

                // 컬럼에 저장될 데이터를 관리하는 객체
                val cv1 = ContentValues()
                cv1.put("textData","문자열1")
                cv1.put("intData",100)
                cv1.put("floatData", 11.11)

                helper.writableDatabase.insert("TestTable", null, cv1)

                val cv2 = ContentValues()
                cv2.put("textData", "문자열2")
                cv2.put("intData", 200)
                cv2.put("floatData", 22.22)

                helper.writableDatabase.insert("TestTable", null, cv2)

                helper.writableDatabase.close()
            }

            selectBtn.setOnClickListener {
                val helper = DBHelper(this)

                // 첫 번째: 가져올 데이터가 있는 테이블의 이름
                // 두 번째: 가져올 컬럼의 이름이 담겨져 있는 문자열 배열, null일 경우에는 모든 컬럼
                // 세 번째: 조건절 (idx = ? and name = ?), 조건절이 필요없으면 null
                // 네 번째: 조건절 ?에 바인딩 될 값 배열, 세 번째가 null이면 여기도 null
                // 다섯 번째: Group By 기준 컬럼
                // 여섯 번째: having 절에 들어갈 조건문
                // 일곱 번째: 정렬 기준
                val c1 = helper.writableDatabase.query("TestTable",null,null,
                    null,null,null,null)

                while (c1.moveToNext()){
                    val idx1 = c1.getColumnIndex("idx")
                    val idx2 = c1.getColumnIndex("textData")
                    val idx3 = c1.getColumnIndex("intData")
                    val idx4 = c1.getColumnIndex("floatData")

                    val idx = c1.getInt(idx1)
                    val textData = c1.getString(idx2)
                    val intData = c1.getInt(idx3)
                    val floatData = c1.getFloat(idx4)
                }

                helper.writableDatabase.close()
            }

            updateBtn.setOnClickListener {
                val helper = DBHelper(this)

                val cv = ContentValues()
                cv.put("textData", "문자열3")

                val where = "idx = ?"
                val args = arrayOf("1")

                // 테이블 명, ContentValues, 조건절, 조건절에 바인딩될 값
                helper.writableDatabase.update("TestTable", cv, where, args)
                helper.writableDatabase.close()
            }

            deleteBtn.setOnClickListener {
                val helper = DBHelper(this)

                val where = "idx = ?"
                val args = arrayOf("1")

                // 테이블 명, 조건절, 조건절에 바인딩될 값
                helper.writableDatabase.delete("TestTable", where, args)
                helper.writableDatabase.close()
            }
        }
    }
```

## <span style="color:#0f7b6c">8. Preferences</span>

- 안드로이드의 저장 방식 중 하나로 어플리케이션의 데이터를 간단하게 저장할 수 있는 수단이다.
- 많은 양의 데이터를 저장할 때는 SQLite를 사용하고 소규모의 데이터를 저장할 때 Preference를 사용할 수 있다.
- 일반적으로 SQLite 데이터베이스에는 학생들의 정보 등과 같은 다수의 매개체에 대한 데이터를 저장할 때 사용하고, 어플리케이션 설정 데이터와 같이 유일한 데이터들을 기록할 때 Preferences를 사용한다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // Preferences 객체를 추출
            // Context.MODE_APPEND: 기존에 저장된 데이터를 유지하고 새로운 데이터도 추가
            // Context.MODE_PRIVATE: 기존에 저장된 데이터를 초기화하고 새로운 데이터 추가
            val pref = getSharedPreferences("data", Context.MODE_PRIVATE)

            // 데이터 저장을 위한 객체 추출
            val editor = pref.edit()

            // 저장
            editor.putBoolean("data1", true)
            editor.putInt("data2", 100)
            editor.putFloat("data3", 11.11F)
            editor.putString("data4", "문자열")
            editor.putLong("data5", 100L)

            val set = HashSet<String>()
            set.add("문자열1")
            set.add("문자열2")
            set.add("문자열3")
            editor.putStringSet("data6", set)

            editor.commit()

            // Pref 데이터 읽어오기
            val pref2 = getSharedPreferences("data", Context.MODE_PRIVATE)

            val data1 = pref2.getBoolean("data1", false) // 2번째 인자로 기본값을 설정
            val data2 = pref2.getInt("data2", 0)
            val data3 = pref2.getFloat("data3", 0.0F)
            val data4 = pref2.getString("data4","")
            val data5 = pref2.getLong("data5", 0L)
            val data6 = pref2.getStringSet("data6", null)
        }
    }
```

### 8-1. Preferences Screen

- UI를 통해서 Preferences를 사용할 수 있도록 제공되는 개념 (ex 환경 설정 스크린을 보다 쉽게 만들 수 있도록)
- PreferenceFragment를 사용하며 저장 기능까지 모두 구현 되어 있다.


---
layout: post
title: "[Android] 안드로이드 Messaging"
excerpt: "안드로이드 Messaging에 관해 알아본다."
categories: [Android]
comments: true
---

![android]({{site.url}}/img/Android/giphy.gif)

## <span style="color:#0f7b6c">1. Toast</span>

- **일정 시간이 지나면 자동으로 사라지는 메시지로 간단한 메시지를 사용자에게 전달하는 용도로 주로 사용한다.**
- 화면과 관련 없이 안드로이드 OS에게 메시지 출력을 요청하고 안드로이드 OS에 의해 나타나는 메시지
- 단말기내의 모든 어플리케이션, 모든 구성요소가 요청할 수 있으며 어플리케이션에 관계없이 요청된 순서대로 메시지가 나타난다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // toast 객체를 생성
                val t1 = Toast.makeText(this, "기본 toast 입니다.", Toast.LENGTH_SHORT)
                t1.show()
            }
        }
    }
```

![basic toast]({{site.url}}/img/Android/basic-toast.png){:height="400"} 

**Callback**

- 안드로이드 11 부터 Toast 메시지에 Callback을 설정할 수 있다.
- onToastHidden: Toast 메시지가 사라질 때 호출된다.
- onToastShown: Toast 메시지가 나타날 때 호출된다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // toast 객체를 생성
                val t1 = Toast.makeText(this, "기본 toast 입니다.", Toast.LENGTH_SHORT)

                // callback
                if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.R){
                    // 디바이스가 안드로이드 11버전 이상인 경우
                    val callback = object : Toast.Callback(){
                        override fun onToastHidden() {
                            super.onToastHidden()
                            textView.text ="toast 메시지가 사라졌습니다."
                        }

                        override fun onToastShown() {
                            super.onToastShown()
                            textView.text ="toast 메시지가 나타났습니다."
                        }
                    }

                    t1.addCallback(callback)
                }

                t1.show()
            }
        }
    }
```

디바이스 버전을 확인해 toast callback을 지원하는 안드로이드 11버전 이상인 경우 callback 객체를 정의하고, addCallback 메서드를 호출해 toast에 callback을 적용시킨다.

![toast callback]({{site.url}}/img/Android/toast-callback.png){:height="400"} 

### 1-1. SnackBar

- **Toast의 업그레이드 버전이라고 불리기도 하는 메시징 도구이다.**
- Activity 위에 표시되며 하단에 나타나는 메시지이다.
- 안드로이드 11버전 부터 기본 Toast는 계속 사용 가능하고 커스터마이징 부분은 SnackBar를 이용하는 것을 권장한다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // snackBar 객체 생성
                val snack1 = Snackbar.make(it, "기본 스낵바", Snackbar.LENGTH_SHORT)
                snack1.show()
            }
        }
    }
```

![toast callback]({{site.url}}/img/Android/basic-snackbar.png){:height="400"} 

**Action 추가**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // snackBar 객체 생성
                val snack1 = Snackbar.make(it, "기본 스낵바", Snackbar.LENGTH_INDEFINITE)
                snack1.setAction("action"){
                    // onClickListener
                }
                snack1.show()
            }
        }
    }
```

![toast callback]({{site.url}}/img/Android/snackbar-action.png){:height="400"} 

SnackBar의 경우 1개의 액션 버튼을 둘 수 있다. (액션 버튼을 둘 경우 버튼을 누를 시간을 줘야하기 때문에 생성 시간을 길게하거나 계속 생성되어 있도록 설정하는게 일반적이다.)

**Callback**

- SnackBar가 나타나고 사라졌을 때 반응

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // snackBar 객체 생성
                val snack1 = Snackbar.make(it, "기본 스낵바", Snackbar.LENGTH_INDEFINITE)
                snack1.setAction("action"){
                    // to do
                }

                val callback1 = object : BaseTransientBottomBar.BaseCallback<Snackbar>(){
                    override fun onShown(transientBottomBar: Snackbar?) {
                        super.onShown(transientBottomBar)
                    }

                    override fun onDismissed(transientBottomBar: Snackbar?, event: Int) {
                        super.onDismissed(transientBottomBar, event)
                    }
                }
                snack1.addCallback(callback1)
                snack1.show()
            }
        }
    }
```

**SnackBar 커스터마이징**

- SnackBar는 새로운 view를 설정하는 메서드나 프로퍼티를 제공하지 않는다.
- SnackBar를 구성하기 위해 사용되는 layout을 추출해 view를 추가해서 처리한다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                // snackBar 객체 생성
                val snackbar = Snackbar.make(it, "custom SnackBar", Snackbar.LENGTH_LONG)

                // snackBar에 사용할 새로운 view 생성
                val snackView = layoutInflater.inflate(R.layout.custom_snackbar, null)

                // snackBar는 새로운 스낵바를 세팅하는 메서드나 프로퍼티가 없기 때문에 기존에 사용하던 view를 추출(get)
                val snackbarLayout = snackbar.view as Snackbar.SnackbarLayout
                // 새롭게 생성한 view를 추가
                snackbarLayout.addView(snackView)
                // 기존에 사용하던 view는 제거
                val oldSnackView = snackbarLayout.findViewById<TextView>(com.google.android.material.R.id.snackbar_text)
                oldSnackView.visibility = View.INVISIBLE

                snackbar.show()
            }
        }
    }
```

![toast callback]({{site.url}}/img/Android/custom-snackbar.png){:height="400"} 

## <span style="color:#0f7b6c">2. Dialog</span>

- **개발자가 필요할 때 사용자에게 메시지를 전달하는 용도로 사용하며 다이얼로그가 나타나 있을 때는 주변의 view를 사용할 수 없다.**
- 메시지 전달이나 입력 등의 용도로 사용

### 2-1. Basic Dialog

- **기본 다이얼로그는 메시지와 최대 3개의 버튼을 제공할 수 있다.**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                var builder = AlertDialog.Builder(this)

                builder.setTitle("basic dialog")
                builder.setMessage("기본 다이얼로그 입니다.")
                builder.setIcon(R.mipmap.ic_launcher)
                builder.setPositiveButton("positive", null)
                builder.setNeutralButton("Neutral", null)
                builder.setNegativeButton("Negative", null)
                builder.show()
            }
        }
    }
```

![toast callback]({{site.url}}/img/Android/basic-dialog.png){:height="400"} 

Positive, Negative, Neutral 3가지 버튼이 기본 다이얼로그에서는 사용 가능하다. 이때 무조건 Positive 버튼은 긍정의 의미로 Negative 버튼은 부정의 의미로 사용할 필요는 없다. 각 버튼의 차이는 버튼이 놓여지는 위치만 있다.

### 2-2. Custom Dialog

- 기본 다이얼로그에 view를 설정하면 다이얼로그에 표시되는 모양을 자유롭게 구성할 수 있다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val builder = AlertDialog.Builder(this)
                builder.setTitle("custom dialog")
                builder.setIcon(R.mipmap.ic_launcher)

                // dialog에서 사용할 view를 생성
                val custom_dialog_view = layoutInflater.inflate(R.layout.custom_dialog, null)

                // dialog에 새롭게 생성한 view를 적용
                builder.setView(custom_dialog_view)

                builder.show()
            }
        }
    }
```

![toast callback]({{site.url}}/img/Android/custom-dialog.png){:height="400"} 

### 2-3. DatePicker & TimePicker Dialog

**DatePicker**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val calendar = Calendar.getInstance()
                val year = calendar.get(Calendar.YEAR)
                val month = calendar.get(Calendar.MONTH)
                val day = calendar.get(Calendar.DAY_OF_MONTH)

                val picker_dialog = DatePickerDialog(this, null, year, month, day)
                picker_dialog.show()
            }
        }
    }
```

![toast callback]({{site.url}}/img/Android/picker-dialog.png){:height="400"} 

DataPickerDialog 메서드의 2번째 인자는 Dialog의 ok 버튼을 눌렀을 때 반응하는 리스너를 설정하는 인자이다.

**TimePicker**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            button.setOnClickListener {
                val calendar = Calendar.getInstance()
                val hour  = calendar.get(Calendar.HOUR)
                val mintue = calendar.get(Calendar.MINUTE)

                val time_picker_dialog = TimePickerDialog(this, null ,hour, mintue, true)
                time_picker_dialog.show()
            }
        }
    }
```

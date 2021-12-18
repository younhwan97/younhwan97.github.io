---
layout: post
title: "[Android] 안드로이드 Thread와 Service"
excerpt: "안드로이드 Thread와 Service에 관해 알아본다."
categories: [Android]
comments: true
---

- Thread는 여러 처리를 비 동기적으로 처리하기 위해 사용한다.
- 안드로이드는 비 동기적 처리 외에 네트워크에 관련된 코드는 전부 Thread로 운영해야 한다.

## <span style="color:#0f7b6c">1. Main Thread와 사용자 Thread</span>

- 안드로이드에서는 Activity 코드를 처리하기 위해 Thread를 실행시킨다.
- 이 때 발생되는 Thread를 Main Thread라 부르며, UI Thread라고 부르기도 한다.
- Main Thread가 어떠한 처리도 하지 않고 유휴 상태 일때만 화면 작업이 가능하다.
- 이 때문에 시간이 오래 걸리는 작업의 경우 별도의 Thread를 생성하여 처리하고 Main Thread를 항상 유휴 상태로 유지하게 해야 한다. 
- Main Thread가 별도의 작업을 처리하기 위해 끊임없이 사용되면 새롭게 화면이 갱신될 시점에 제대로 갱신을 하지 못해 유저에게 좋지않은 어플리케이션 사용경험을 줄 수 있다.

### 1-1. 화면 처리

- 오래 걸리는 별도의 작업은 사용자 Thread가 담당하고, 화면 처리의 경우 Main Thread가 담당한다.
- 안드로이드는 개발자가 발생시킨 '사용자 Thread'에서 화면에 대한 처리를 하면 오류가 발생한다. (작업이 어느정도 진행됐는지 나타내는 등의..)
- 현재 안드로이드 8.0 이상 부터는 개발자가 발생시킨 Thread에서 화면 처리가 가능하다.
- 허나 OS 버전이 변경되면서 상황은 달라질 수도 있고 하위버전을 위해 화면 처리는 Main Thread에서 하는게 좋다.

### 1-2. 사용자 Thread 생성

**Thread 상속 클래스 생성**

```kotlin
    class MainActivity : AppCompatActivity() {

        var isRunning = true // thread 상태를 담당하는 변수

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)


            val thread1 = object : Thread(){
                override fun run() {
                    super.run()
                    while (isRunning){
                        SystemClock.sleep(100)
                        val now = System.currentTimeMillis()
                        Log.d("time", "Thread : ${now}")
                    }
                }
            }
            thread1.start()
        }

        override fun onDestroy() {
            super.onDestroy()
            isRunning = false 
        }
    }
```

Activity가 종료되어도 Thread는 계속 동작하기 때문에 onDestroy에서 thread를 종료할 적절한 코드가 필요하다.

**thread 고차 함수를 사용**

```kotlin
    class MainActivity : AppCompatActivity() {

        var isRunning = true // thread 상태를 담당하는 변수

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
            
            thread {
                while (isRunning){
                    SystemClock.sleep(100)
                    val now = System.currentTimeMillis()
                    Log.d("time", "Thread: ${now}")
                }

                // 따로 start 메서드도 호출할 필요가 없다.
            }
        }

        override fun onDestroy() {
            super.onDestroy()
            isRunning = false
        }
    }
```

## <span style="color:#0f7b6c">2. Handler를 이용한 반복작업</span>

- Main Thread에서 처리하는 코드(Activity)중에 일정한 작업(=한 번 수행될 때 시간이 오래 걸리지 않은)을 계속 반복 처리해야할 경우가 있다.
- 이 때, while문을 이용하여 무한루프를 운영하면 Main Thread가 종료되지 않아 화면을 처리할 수 없다.
- 이러한 문제를 해결하기 위해 다양한 방법을 제공하고 있다.

즉, 한 번 실행될 때 오래걸리는 작업은 아니지만 어플리케이션이 실행되는 동안 꾸준히 실행되는 작업을 이야기한다. 이러한 경우에 while 무한루프를 사용하면 한 번 실행될 때 시간이 얼마 걸리지 않은 작업이라고 하더라도 작업이 종료되지 않기 때문에 바람직하지 않다. ex) 화면에 시간을 표시하는 작업

**Handler**

- Handler는 개발자의 작업 요청을 안드로이드 OS에게 전달한다.
- 개발자가 작업을 요청하면 안드로이드 OS는 다른 작업을 하지 않을 때 개발자가 요청한 작업을 처리한다.
- 이 처리는 Main Thread에서 처리한다.
- 5초 이상 걸리는 작업은 피하는 것이 좋다.
- 화면 처리도 가능하다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val handler = Handler(Looper.myLooper()!!)

            // '한 번의 처리'에 대한 작업을 구현
            val thread1 = object : Thread(){
                override fun run() {
                    super.run()
                    val now = System.currentTimeMillis()
                    Log.d("time", "thread: ${now}")

                    // handler.post(this)
                    // handler.postDelayed(this, 100)
                }
            }

            handler.post(thread1)
        }
    }
```

### 2-1. Handler를 이용한 화면 처리

```kotlin
    class MainActivity : AppCompatActivity() {
        var isRunning = true

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // 화면 처리를 위한 핸들러
            // 핸들러가 요청한 작업은 Main Thread가 처리!!!!
            val handler1 = object :Handler(Looper.myLooper()!!){
                override fun handleMessage(msg: Message) {
                    super.handleMessage(msg)

                    when(msg.what){
                        0 -> {
                            textView.text = "${msg.obj}"
                        }
                    }
                }
            }

            thread {
                while (isRunning){
                    val now = System.currentTimeMillis()

                    val message = Message()
                    message.what = 0
                    message.obj = "$now"

                    handler1.sendMessage(message)
                    SystemClock.sleep(5000)
                }
            }
        }

        override fun onDestroy() {
            super.onDestroy()
            isRunning = false
        }
    }
```

별로의 큰 작업을 처리하는 Thread에서는 작업을 처리하다가 핸들러 객체에게 메시지를 전달한다. 메시지를 전달받은 핸들러는 다시 그 메시지를 os에게 전달하여 화면처리를 Main Thread가 할 수 있도록 한다.

## <span style="color:#0f7b6c">3. RunOnUiThread</span>

- RunOnUiThread 메서드는 개발자가 발생시킨 사용자 Thread에서 코드 일부를 MainThread가 처리하도록 하는 메서드이다.
- 이를 이용해 Handler를 대신하여 사용자 Thread를 운영할 수 있다.

```kotlin
    class MainActivity : AppCompatActivity() {
        var isRunning = true

        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            thread {
                while (isRunning){
                    SystemClock.sleep(500)

                    val now = System.currentTimeMillis()

                    runOnUiThread(object :Thread(){
                        override fun run() {
                            super.run()
                            // Main Thread가 처리 할 코드
                            textView.text = "$now"
                        }
                    })
                }
            }
        }

        override fun onDestroy() {
            super.onDestroy()
            isRunning = false
        }
    }
```

## <span style="color:#0f7b6c">4. Service</span>

- 화면을 가지고 있지 않는 실행 단위. (ex 음악 재생)
- 안드로이드 4대 구성 요소중에 하나로 백그라운드 처리를 위해 사용되다.
- Activity는 화면을 가지고 있어 화면이 보이는 동안 동작하지만 Service는 화면을 가지고 있지 않아 **보이지 않는 동안에도** 동작하는 것을 의미한다.

### 4-1. 서비스 구성 방법

**step 1: 서비스 클래스 생성**

![create service step 1]({{site.url}}/img/Android/create-service-step-1.png){:height="400"} 

**step 2: 서비스 가동/중지 시 동작할 메서드 오버라이딩**

```kotlin
    class MyService : Service() {

        override fun onBind(intent: Intent): IBinder {
            TODO("Return the communication channel to the service.")
        }

        // 서비스가 가동될 때 호출되는 메서드
        override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {

            Log.d("service", "service 가동")

            return super.onStartCommand(intent, flags, startId)
        }

        // 서비스가 중지(=소멸)될 때 호출되는 메서드
        override fun onDestroy() {
            super.onDestroy()
            Log.d("service", "service 중지(=소멸)")
        }
    }
```

**step 3: 서비스 가동/중지 시킬 환경 구성**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            val serviceIntent = Intent(this, MyService::class.java)

            button.setOnClickListener {
                startService(serviceIntent)
            }

            button2.setOnClickListener {
                stopService(serviceIntent)
            }

        }
    }
```

### 4-2. 서비스와 Thread

### 4-3. Foreground Service
---
layout: post
title: "[Android] 안드로이드 Menu"
excerpt: "안드로이드 메뉴에 관해 알아본다."
categories: [Android]
comments: true
---

## <span style="color:#0f7b6c">1. Option Menu</span>

- 안드로이드에서 화면 하나당 하나씩 가질 수 있는 메뉴를 의미하며 현재 보이는 화면(Activity)의 **메인 메뉴**가 된다.

**step 1: Menu Layout 파일 생성**

![android]({{site.url}}/img/Android/create-option-menu-step-1.png){:height="400"}

![android]({{site.url}}/img/Android/create-option-menu-step-2.png){:height="400"}

res 폴더 하위에 새롭게 menu 폴더가 생성된 것을 볼 수 있다.

**step 2: Layout 파일 구성**

![android]({{site.url}}/img/Android/create-option-menu-step-3.png){:height="400"}

**step 3: 화면(Activity)에 메뉴 장착**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
        }

        override fun onCreateOptionsMenu(menu: Menu?): Boolean {
            // XML로 메뉴를 구성한다.
            menuInflater.inflate(R.menu.main_menu, menu)
            return true // false를 반환시 메뉴가 나타나지 않는다.
        }
    }
```

화면이 생성될 때 자동으로 onCreateOptionsMenu가 호출된다.

**step 4: 메뉴의 아이템에 이벤트 리스너 장착**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
        }

        override fun onCreateOptionsMenu(menu: Menu?): Boolean {
            // XML로 메뉴를 구성한다.
            menuInflater.inflate(R.menu.main_menu, menu)
            return true // false를 반환시 메뉴가 나타나지 않는다.
        }

        override fun onOptionsItemSelected(item: MenuItem): Boolean {
            // 메뉴 아이템의 id 별로 분기한다.
            when(item.itemId){
                R.id.item1 ->{
                    // 항목 1을 눌렀을 때 실행할 코드
                }
                R.id.item2 ->{
                    // 항목 2을 눌렀을 때 실행할 코드
                }
                R.id.item3 ->{
                    // 항목 3을 눌렀을 때 실행할 코드
                }
            }
            return super.onOptionsItemSelected(item)
        }
    }
```

## <span style="color:#0f7b6c">2. Context Menu</span>

- 화면에 배치된 view에 설정할 수 있는 메뉴
- 메뉴가 설정된 view를 길게 누르면 메뉴가 나타난다.

**step 1: Menu Layout 파일 생성**

![android]({{site.url}}/img/Android/create-context-menu-step-1.png){:height="400"}

**step 2: Layout 파일 구성**

![android]({{site.url}}/img/Android/create-context-menu-step-2.png){:height="400"}

**step 3: View에 메뉴 장착**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // View에 contextMenu를 등록한다.
            registerForContextMenu(textView)
        }

        override fun onCreateContextMenu(menu: ContextMenu?, v: View?, menuInfo: ContextMenu.ContextMenuInfo?) {
            super.onCreateContextMenu(menu, v, menuInfo)

            // contextMenu는 화면당 1개가 아니라 여러 view에 등록될 수 있기 때문에 적절히 분기해줘야 한다.
            when(v?.id){
                R.id.textView->{
                    menu?.setHeaderTitle("텍스트뷰의 메뉴")
                    menuInflater.inflate(R.menu.contextmenu1, menu)
                }
            }
        }
    }
```

![android]({{site.url}}/img/Android/create-context-menu-step-3.png){:height="400"}

**step 4: 메뉴의 아이템에 이벤트 리스너 장착**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // View에 contextMenu를 등록한다.
            registerForContextMenu(textView)
        }

        override fun onCreateContextMenu(menu: ContextMenu?, v: View?, menuInfo: ContextMenu.ContextMenuInfo?) {
            super.onCreateContextMenu(menu, v, menuInfo)

            // contextMenu는 화면당 1개가 아니라 여러 view에 등록될 수 있기 때문에 적절히 분기해줘야 한다.
            when(v?.id){
                R.id.textView->{
                    menu?.setHeaderTitle("텍스트뷰의 메뉴")
                    menuInflater.inflate(R.menu.contextmenu1, menu)
                }
            }
        }

        override fun onContextItemSelected(item: MenuItem): Boolean {
            // 메뉴 아이템의 id 값으로 분기한다.
            when(item.itemId){
                R.id.item1->{

                }

                R.id.item2->{

                }
            }
            return super.onContextItemSelected(item)
        }
    }
```

## <span style="color:#0f7b6c">3. Popup Menu</span>

- 개발자가 원할 때 원하는 곳에 띄울 수 있는 메뉴

**step 1: Menu Layout 파일 생성**

**step 2: Layout 파일 구성**

**step 3: View에 메뉴 장착**

상황: 버튼을 누르면 textView에 메뉴를 띄운다.

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)


            button.setOnClickListener {
                // PopupMenu 객체를 생성
                val pop = PopupMenu(this, textView)

                // 메뉴를 구성한다.
                menuInflater.inflate(R.menu.popupmenu, pop.menu)

                pop.show()
            }
        }
    }
```

![android]({{site.url}}/img/Android/create-popup-menu-step-1.png){:height="400"}

**step 4: 메뉴의 아이템에 이벤트 리스너 장착**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)


            button.setOnClickListener {
                // PopupMenu 객체를 생성
                val pop = PopupMenu(this, textView)

                // 메뉴를 구성한다.
                menuInflater.inflate(R.menu.popupmenu, pop.menu)

                // 메뉴의 아이템에 이벤트 리스너 장착
                pop.setOnMenuItemClickListener {
                    when(it.itemId){
                        R.id.item1->{
                            // item1을 눌렀을 때 실행할 코드
                        }

                        R.id.item2->{
                            // item12을 눌렀을 때 실행할 코드
                        }
                    }
                    
                    true
                }

                pop.show()
            }
        }
    }
```
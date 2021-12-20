---
layout: post
title: "[Android] 안드로이드 ActionBar와 Toolbar"
excerpt: "안드로이드 ActionBar에 관해 알아본다."
categories: [Android]
comments: true
---

![android-action-bar]({{site.url}}/img/Android/android-action-bar.png){:height="300"} 

## <span style="color:#0f7b6c">1. Option Menu의 item을 ActionBar 위로</span>

![android-action-bar]({{site.url}}/img/Android/android-option-menu-original-style.png){:height="300"} 

이때 item의 ShowAsAction이라는 속성을 이용해 각각의 item을 ActionBar위에 배치할 수 있다.

- None: 기본 값으로 ActionBar에 표시하지 않는다.
- Always: 무조건 ActionBar에 표시된다.
- ifRoom: 공간이 허락할 경우 ActionBar에 표시된다.
- Icon: ActionBar에 표시될 때 사용할 아이콘을 지정한다.
- withText: 공간이 허락될 경우 아이콘과 함께 문자열을 표시한다.

![android-action-bar]({{site.url}}/img/Android/android-action-bar-set-attribute.png){:height="300"}

item의 showAsAction 값을 Always로 설정하고 Icon을 설정했다.(설정하지 않을 경우 텍스트 값이 표시된다.)

## <span style="color:#0f7b6c">2. ActionView</span>

- ActionBar에 View를 배치하고 이를 접었다 폈다 할 수 있는 개념이다.
- 주로 검색 기능을 만들 때 사용한다.

**step 1: showAsAction 값 설정**

ActionView로 사용하고자 하는 item에서 showAsAction의 collapseActionView를 체크한다.

![android-action-bar]({{site.url}}/img/Android/create-action-view-step-1.png){:height="400"}

**step 2: item을 클릭했을 때 보여줄 view를 설정한다.**

```xml
    <item
        android:id="@+id/item1"
        android:icon="@android:drawable/ic_menu_search"
        android:title="Item1"
        app:showAsAction="always|collapseActionView"
        app:actionViewClass="androidx.appcompat.widget.SearchView"
        />
```

item의 코드를 변경함으로서 가능하다. 

![android-action-bar]({{site.url}}/img/Android/create-action-view-step-2.png){:height="300"}

**step 3: 안내 문구 변경**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
        }

        override fun onCreateOptionsMenu(menu: Menu?): Boolean {
            menuInflater.inflate(R.menu.main_menu, menu)

            // SearchView를 가지고 있는 메뉴 아이템 객체를 추출
            val item1 = menu?.findItem(R.id.item1)
            // SearchView 객체를 가지고 온다.
            val searchView1 = item1?.actionView as SearchView
            // 안내 문구를 설정한다.
            searchView1.queryHint = "검색어 입력"

            return true
        }
    }
```

**step 4: ActionView 이벤트 리스너 생성 및 적용**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
        }

        override fun onCreateOptionsMenu(menu: Menu?): Boolean {
            menuInflater.inflate(R.menu.main_menu, menu)

            // SearchView를 가지고 있는 메뉴 아이템 객체를 추출
            val item1 = menu?.findItem(R.id.item1)
            // SearchView 객체를 가지고 온다.
            val searchView1 = item1?.actionView as SearchView
            // 안내 문구를 설정한다.
            searchView1.queryHint = "검색어 입력"

            // 메뉴 아이템에 배치된 뷰가 접히거나 펼쳐질 때 반응하는 리스너
            val listener1 = object : MenuItem.OnActionExpandListener{
                // 접혔을 때 호출
                override fun onMenuItemActionCollapse(p0: MenuItem?): Boolean {
                    TODO("Not yet implemented")
                }

                // 펼쳐졌을 때 호출
                override fun onMenuItemActionExpand(p0: MenuItem?): Boolean {
                    TODO("Not yet implemented")
                }
            }

            item1.setOnActionExpandListener(listener1)

            return true
        }
    }
```

**step 5: SearchView 이벤트 리스너 생성 및 적용**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)
        }

        override fun onCreateOptionsMenu(menu: Menu?): Boolean {
            menuInflater.inflate(R.menu.main_menu, menu)

            // SearchView를 가지고 있는 메뉴 아이템 객체를 추출
            val item1 = menu?.findItem(R.id.item1)
            // SearchView 객체를 가지고 온다.
            val searchView1 = item1?.actionView as SearchView
            // 안내 문구를 설정한다.
            searchView1.queryHint = "검색어 입력"

            // SearchView의 리스너
            val listener1 = object : SearchView.OnQueryTextListener{
                // SearchView의 문자열을 입력할 때 마다 호출되는 메서드
                override fun onQueryTextChange(newText: String?): Boolean {
                    TODO("Not yet implemented")
                }

                // 키보드의 확인 버튼을 눌렀을 때 호출되는 메서드
                override fun onQueryTextSubmit(query: String?): Boolean {
                    TODO("Not yet implemented")
                }
            }

            searchView1.setOnQueryTextListener(listener1)
            
            return true
        }
    }
```

## <span style="color:#0f7b6c">3. ActionBar Navigation</span>

- ActionBar에 <- 아이콘을 배치하여 뒤로가기 기능을 구현할 수 있다.
- 아이콘을 표시한다고 해서 뒤로 가기 기능이 생기는 것은 아니지 때문에 직접 구현을 해야 한다.

```kotlin
    class SecondActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_second)

            // HomeButton 메뉴를 활성화한다.
            supportActionBar?.setHomeButtonEnabled(true)
            // HomeButton을 노출 시킨다.
            supportActionBar?.setDisplayHomeAsUpEnabled(true)
            // HomeButton의 아이콘을 설정한다.
            // supportActionBar?.setHomeAsUpIndicator()
        }

        override fun onOptionsItemSelected(item: MenuItem): Boolean {
            when(item.itemId){
                android.R.id.home -> { // id가 미리 설정돼 있다.
                    finish()
                }
            }

            return super.onOptionsItemSelected(item)
        }
    }
```

![android-action-bar]({{site.url}}/img/Android/android-action-bar-navigation.png){:height="300"}

## <span style="color:#0f7b6c">4. ActionBar 커스터마이징</span>

- 안드로이드에서 ActionBar는 개발자가 자유롭게 구성할 수도 있다.

**step 1: Custom ActionBar 레이아웃 생성 및 구성**

![android-action-bar]({{site.url}}/img/Android/create-custom-action-bar-step-1.png){:height="300" width="500"}

![android-action-bar]({{site.url}}/img/Android/create-custom-action-bar-step-2.png){:height="300" width="500"}

**step 2: Custom ActionBar 생성 및 장착**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // 기존 ActionBar를 사라지게 만든다.
            supportActionBar?.setDisplayShowCustomEnabled(true)
            supportActionBar?.setDisplayHomeAsUpEnabled(false)
            supportActionBar?.setDisplayShowTitleEnabled(false)
            supportActionBar?.setDisplayShowHomeEnabled(false)

            // layout을 통해 view를 생성
            val customActionBar = layoutInflater.inflate(R.layout.custom_actionbar, null)
            supportActionBar?.customView = customActionBar
        }
    }
```

![android-action-bar]({{site.url}}/img/Android/create-custom-action-bar-step-3.png){:height="400"}

## <span style="color:#0f7b6c">5. Toolbar</span>

- 안도르이드에서 ActionBar를 보다 자유롭게 사용할 수 있도록 Toolbar라는 view를 제공한다.
- Toolbar를 이용해 탭 등 다양한 기능을 이용할 수 있도록 제공하고 있으며 기본적인 부분은 ActionBar와 동일하다.

**step 1: Layout에 Toolbar를 배치**

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        tools:context=".MainActivity" >

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="?attr/colorPrimary"
            android:minHeight="?attr/actionBarSize"
            android:theme="?attr/actionBarTheme" />
    </LinearLayout>
```

![android-action-bar]({{site.url}}/img/Android/create-tool-bar-step-1.png){:height="400"}

**step 2: 기존 ActionBar 제거**

![android-action-bar]({{site.url}}/img/Android/create-tool-bar-step-2.png){:height="400"}

**step 3: Toolbar를 ActionBar 대신 사용할 수 있도록 만들기**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // toolbar를 actionBar 대신 사용하도록 설정
            setSupportActionBar(toolbar)
        }
    }
```

이제 기존에 ActionBar를 사용했던 것 처럼 사용할 수 있다.

### 5-1. Toolbar에서 ActionView 사용하기

**MainActivity**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // toolbar를 actionBar 대신 사용하도록 설정
            setSupportActionBar(toolbar)
        }

        override fun onCreateOptionsMenu(menu: Menu?): Boolean {
            menuInflater.inflate(R.menu.main_menu, menu)
            
            return true
        }
    }
```

**main_menu**

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <menu xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:android="http://schemas.android.com/apk/res/android">

        <item
            android:id="@+id/item1"
            android:icon="@android:drawable/ic_menu_search"
            android:title="Item1"
            app:showAsAction="always|collapseActionView"
            app:actionViewClass="androidx.appcompat.widget.SearchView"
            />
    </menu>
```

### 5-2. Toolbar에서 Navigation 사용 하기

- Toolbar도 ActionBar와 같은 방법으로 네비게이션 처리를 할 수 있다.
- Toolbar로 부터 ActionBar를 추출하고 이 후에는 ActionBar와 동일하게 처리해준다.

**MainActivity**

```kotlin
    class MainActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_main)

            // toolbar를 actionBar 대신 사용하도록 설정
            setSupportActionBar(toolbar)

            button.setOnClickListener {
                val second_intent = Intent(this, SecondActivity::class.java)
                startActivity(second_intent)
            }
        }


        override fun onCreateOptionsMenu(menu: Menu?): Boolean {
            menuInflater.inflate(R.menu.main_menu, menu)

            return true
        }
    }
```

**SecondActivity**

```kotlin
    class SecondActivity : AppCompatActivity() {
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            setContentView(R.layout.activity_second)

            setSupportActionBar(toolbar3)

            supportActionBar?.setHomeButtonEnabled(true)
            supportActionBar?.setDisplayHomeAsUpEnabled(true)
        }

        override fun onOptionsItemSelected(item: MenuItem): Boolean {
            when(item.itemId){
                android.R.id.home ->{
                    finish()
                }
            }

            return super.onOptionsItemSelected(item)
        }
    }
```

## <span style="color:#0f7b6c">6. Toolbar를 활용한 다양한 View 구성</span>

### 6-1. ViewPager2와 Toolbar

- 기존 ViewPager의 경우 View를 전환하는 개념이었다.
- ViewPager2의 경우 Fragment를 전환하는 개념이다.


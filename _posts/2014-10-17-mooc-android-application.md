---
layout: post
layout: post
title: "[Moocs]Programming Mobile Aplication on Android Platform(2) - Intent/Permission/Fragment"
description: ""
category: 
- 網路課程筆記 
- android安卓學習心得

tags: []

---

![image](../images/2014/Android_ICON.jpg)

上到第三周了，開始有一些重點出來．主要有提到Intent，Permission跟 Fragment．

**關於Intent:**

- Intetn可以分為 Implicit Intent 跟Explicit Intent，差別如下：
    - Implicit 是使用系統內建（或是有登陸的Activity/App)
    - Explicity 是可以讓你呼叫自己定義的Activity
- 可以有許多"動作"(Action)選擇，比如說 ACTION_DIAL... 等等， 使用SetAction()來呼叫
- URI 是幫助你傳遞一些需要的小資料，比如說: 電話號碼， 地圖經緯度.. 等等 使用SetData()來呼叫．
- Extra 可以指定為EXTRA_EMAIL，或是改為字串會是變數
    - 需要多加變數可以用  putExtra(KEY, VALUE)  跟 getExtras() 來傳遞    
- 關於OnActivityResult部分，要用setResult(Activity.RESULT_OK, Intent) 來傳遞
- 如果要跟系統註冊為一種 ImplicitAcitivity的瀏覽器就一定要在AndroidManifest.xml裡面加上：
<pre class="prettyprint">
            < intent-filter >
                < action android:name="android.intent.action.MAIN" />
                < category android:name="android.intent.category.LAUNCHER" />
            < /intent-filter>  
            < intent-filter>
                < action android:name="android.intent.action.VIEW" />
				< category android:name="android.intent.category.DEFAULT" />
                < data android:scheme="http" />
            < /intent-filter>    
</pre>
    
**關於Permission:**

- 我們比較熟悉的事<uses-permission>，但是其實可以建立自定的permission，利用 <permission>的tag
- 想知道更多參考 Android Permission [http://developer.android.com/guide/topics/security/permissions.html](http://developer.android.com/guide/topics/security/permissions.html)
- 如果有需要讀取瀏覽器的書簽需要加上以下的permission
    -更多的權限要看這裡 [http://developer.android.com/reference/android/Manifest.permission.html#WRITE_HISTORY_BOOKMARKS](http://developer.android.com/reference/android/Manifest.permission.html#WRITE_HISTORY_BOOKMARKS)
<pre class="prettyprint">
    	< uses-permission android:name="com.android.browser.permission.READ_HISTORY_BOOKMARKS" />
</pre>    
    
- 如果permission 已經被提升(granted)的話，就必須要移除App才能測試移除permission的部分．

<pre class="prettyprint">
    < permission >
        android:name="course.examples.permissionexample.TEST_PERM"
        android:description="@string/test_perm_string"
        android:label="@string/test_permission_label_string" >
    < /permission >
</pre>    

**關於Fragment**

- 使用的時機點:
    - 同一個App在tablet 與手機上，可以顯示不同畫面
    - Fragment 建立的方式有以下幾種:
        - Static Fragment:  將Fragment 定義在 layout裡面
        - Programmatically: 不用先將Fragment 寫死在Layout，而是動態讀取Fragment 然後貼在Frame上面．
        - Dynamic Fragment: 不僅僅可以用程式指定Fragment，更可以決定在某些狀況下才顯示Fragment
    - 一般而言，configuration change (手機直向或是橫向變化) 會Destroy Fragment然後重新建立，可以利用setRetainInstance(true) 去避免這個狀況．   
        - 如接下來的 Life Cycle 所顯示，如果有加上 setRetainInstance OnDestroy 與 OnDetach不會執行到，相對的也不會跑 OnAttach跟 OnCreate．只會跑OnCreateView． 所以要控制外觀的部分(比如說記錄selected list index)需要加在OnActivityCreate這裡．
    - Fragment Life cycle :
        - OnAttach 
        - OnCreate
        - OnCreateView  (handle Fragment UI)
        - OnActivityCreate
        - OnStart, OnResume ....  OnDestroy
        - OnDestroy
        - OnDetach
        - 可以參考這裡有更多解答 [http://developer.android.com/guide/components/fragments.html](http://developer.android.com/guide/components/fragments.html)
    - Fragment 可以是動態加入(形成原本是單個frame的界面，按下後變成兩個frame的界面)，並且利用加入onBackStackChanged來改變layout回單個frame
    - 一般而言，在旋轉手機的時候會觸發 configuration change 然後使得Fragment 重建．如果要保留Fragment的狀態需要加上setRetainInstance 
    - 想要一開始只顯示左邊list，並且在選取後可以出現詳細在右邊．就可以先選取一個Fragment ID，然後在ListSelected 上面去讀取另外一個Fragment在產生transaction即可．              
    
<pre class="prettyprint">    
FragmentTransaction fragmentTransaction = getFragmentManager().beginTransaction();
			
// Add the Fragment to the layout
fragmentTransaction.add(R.id.frag, mFragment);
			
// Add this FragmentTransaction to the backstack
fragmentTransaction.addToBackStack(null);
			
// Commit the FragmentTransaction
fragmentTransaction.commit();
			
// Force Android to execute the committed FragmentTransaction
// execute transaction now
getFragmentManager().executePendingTransactions();    

</pre>

**相關資料:** 

- Android Document about Intent
    - [http://developer.android.com/reference/android/content/Intent.html](http://developer.android.com/reference/android/content/Intent.html)    
-  Android Permission 
    -  [http://developer.android.com/guide/topics/security/permissions.html](http://developer.android.com/guide/topics/security/permissions.html)
-  User-Permission list
    - [http://blog.yam.com/jackiefu/article/37082465](http://blog.yam.com/jackiefu/article/37082465)     
- 關於Android Fragment
    - [http://developer.android.com/guide/components/fragments.html](http://developer.android.com/guide/components/fragments.html)
- Android Manigest Permission
    - [http://developer.android.com/reference/android/Manifest.permission.html](http://developer.android.com/reference/android/Manifest.permission.html)           

---
layout: post
layout: post
title: "[Android]關於JNI的學習筆記"
description: ""
category: 
- android
tags: []

---

![image](https://lh3.googleusercontent.com/-RogETFDNCSE/AAAAAAAAAAI/AAAAAAAAAIQ/oaIS5A2ZXMk/photo.jpg)



## 前言

上個禮拜在Android Studio 1.1 把JNI搞定之後，接下來就是把JNI的內容去完成．其中有許多小技巧與眉眉角角的地方需要紀錄一下．


## 筆記內容

### 關於Android.mk 與 Application.mk 的部分


####  (2015/10/21 update) 關於Android Studio與JNI的發生ld.exe crash

如果在Android Studio要去load其他的jni static library，在Windows上面有時候會發生ld.exe crash的問題．這時候只要打開"Build Variants"-> 把"Build All"，改成看你現在需要哪一種(Android 手機就是 arm-Debug，模擬器就是x86-Debug)． 就可以解決．


####  (2015/10/20 update) 關於STL部分

如果有用到STL的支援，主要都是修改`Application.mk`，不是改在`Android.mk`中． 這樣才能一次改到所有的檔案，避免A檔案過，B檔案不過．

		APP_STL := stlport_static

參考: [Using the STL with Android NDK C++](http://stackoverflow.com/questions/9458208/using-the-stl-with-android-ndk-c)

####  增加 C++ 與 CPP11 支援

新增以下到 `Application.mk`

        APP_STL := stlport_static
        APP_CPPFLAGS += -std=c++11


#### 讀取其他資料夾與一次讀取所有的cpp

新增以下到 Android.mk

    LOCAL_SRC_FILES := $(wildcard $(LOCAL_PATH)/*.cpp)
    LOCAL_SRC_FILES += $(wildcard $(LOCAL_PATH)/../../../../../../source/*.cpp)

### 關於JNI資料的轉換部分

#### C++ char* 與 JNI jstring 的轉換

**jstring to char* **


        char* jstringTostring(JNIEnv* env, jstring jstr)
        {        
          char* rtn = NULL;
          jclass clsstring = env->FindClass("java/lang/String");
          jstring strencode = env->NewStringUTF("utf-8");
          jmethodID mid = env->GetMethodID(clsstring, "getBytes", "(Ljava/lang/String;)[B");
          jbyteArray barr= (jbyteArray)env->CallObjectMethod(jstr, mid, strencode);
          jsize alen = env->GetArrayLength(barr);
          jbyte* ba = env->GetByteArrayElements(barr, JNI_FALSE);
          if (alen > 0)
          {
            rtn = (char*)malloc(alen + 1);
            memcpy(rtn, ba, alen);
            rtn[alen] = 0;
          }
          env->ReleaseByteArrayElements(barr, ba, 0);
          return rtn;
        }


**char* to jstring**

        jstring stoJstring(JNIEnv* env, const char* pat)
        {
            jclass strClass = env->FindClass("Ljava/lang/String;");
            jmethodID ctorID = env->GetMethodID(strClass, "<init>", "([BLjava/lang/String;)V");
            jbyteArray bytes = env->NewByteArray(strlen(pat));
            env->SetByteArrayRegion(bytes, 0, strlen(pat), (jbyte*)pat);
            jstring encoding = env->NewStringUTF("utf-8");
            return (jstring)env->NewObject(strClass, ctorID, bytes, encoding);
        } 

其他更多的部分可以參考[這裏](http://provista.iteye.com/blog/839703)..  


### 指標(pointer)的傳遞

如果需要使用到 native (也就是C++裡面建議的的記憶體區塊)

    //從C++的部分取得所建立的記憶體物件
    void* obj = NULL;
    jint ret_code = (jint) native.createObj(&obj);

    //改成jlong回傳 java並且處理（避免記憶體位置過長)
    jlong ret_addr = (jlong) obj;

之後要處理就傳進 jlong然後轉成 void*

    //input jlong jobj_addr
    void *obj = (void*) jobj_addr;


### 回傳native資料結構給java

很多時候 native C/C++裡面會回回一個資料結構，或者是說你需要多個回傳值而不僅僅只是string與integer的時候就需要用到． 首先要先了解流程如下:

- 在java端先建立相對應的class (或是使用已經建立的class)
- 在JNI曾透過 FindClass 與 FindMethod 去找到class與 method．
- 將資料寫成jobject 並且回傳jobject給java


首先根據[JNI Types and Data Structures](https://docs.oracle.com/javase/8/docs/technotes/guides/jni/spec/types.html) 要先了解java與native資料轉換如下列表:

**Java VM  Type Signatures **

Type Signature | Java Type
--- | --- | ---
Z | boolean
B | byte
C |	char
S |	short
I | int
J | long
F | float
D | double
L | fully-qualified-class ;	fully-qualified-class
[ type | type[]
( arg-types ) | ret-type	method type

假設你要建立的java資料結構如下

        class TwoFieldResult {
            public int mError_Code;
            public String mResponse;
            public CCS_TwoFieldResult(int inFirst, String inSecond) {
                mError_Code = inFirst; mResponse = inSecond;
            }
        }

那麼你需要對應到constructor的部分就是:

        env->GetMethodID(theReturnType,"<init>","(ILjava/lang/String;)V");

以下為部分程式碼:

        // Struct in Java
        // class TwoFieldResult {
        //     public int mError_Code;
        //     public String mResponse;
        //     public CCS_TwoFieldResult(int inFirst, String inSecond) {
        //         mError_Code = inFirst; mResponse = inSecond;
        //     }
        // }
         
        jobject PrepareReturnObject(JNIEnv* env, int error_code, const char *result_string) {
            jclass theReturnType(env->FindClass("com/example/evan/hellojni/TwoFieldResult"));
            if (!theReturnType) {
                LOGD("java exception thrown: FindClass");
                return NULL; // java exception thrown
            }
            jmethodID constructor = env->GetMethodID(theReturnType,"<init>","(ILjava/lang/String;)V");
            if (!constructor) {
                LOGD("java exception thrown: GetMethodID");
                return NULL; // java exception thrown
            }
            jstring output_jstring = env->NewStringUTF(result_string);
            return env->NewObject(theReturnType, constructor, error_code, output_jstring);
        }



如果要參考回傳陣列結構，可以參考[這一篇](http://andilyliao.iteye.com/blog/496816)

### 容易出現錯誤部分

#### env->NewObject執行後Crash

之前卡在這裡，不過這裡crash主要有幾個需要檢查的部分

- char * 轉 jstring會出現crash 
    - 這個部分想不到compiler不會出現錯誤，而是直接在執行的時候才會出現．所以千萬注意要做`jstring output_jstring = env->NewStringUTF(result_native_string);`的轉換．
- 透過[EnsureLoadCapacity] 來修復(http://stackoverflow.com/questions/19887763/how-to-fix-jni-crash-on-env-newobject)


#### 如何在 Android Studio 1.1 裡面加入 .so

- 其實非常簡單，只要把.so(包含目錄 `jniLibs`)檔案放在 `app/src/main` 就可以
- 如果從Eclipse的專案轉過來的，可能得稍微研究一下．目前我解法是依照這樣的擺法．

#### Android Studio 移動專案目錄名稱會發生app下面資料全部不見

不確定是不是bug，其實只要gradle重讀就好

- [Tools]->[Android]->[Sync PRoject with Gradle Files]


#### JNI裡面要讀取的Class 處理方式

- 需要宣告為`public class` 幫助在其他的package裡面可以處理．
- 建議使用new java class來產生．


#### NDK_TOOLCHAIN_VERSION 可能會造成的問題 [update 2015/11/04]

最近在整合一些跨平台的module到AS(Android Studio) 的時候，由於要編譯成很多個static library．所以我都是透過docker來編譯的．  

這時候Android Studio 1.4之後，就不支援編譯static library，所以需要的是透過NDK Console編譯好之後再給Android Studio來跑． 

但是不知道為什麼會一直遇到錯誤:

		failed: dlopen failed: unknown reloc type 160 ....

後來查了一下子，才發現跟我的某個static library的Application.mk設定有關：

		NDK_TOOKCHAIN	_VERSION=clang //要拿掉
		

拿掉之後就可以解決問題．		



## 參考資料

- [Java Programming Tutorial-Java Native Interface (JNI)](https://www3.ntu.edu.sg/home/ehchua/programming/java/JavaNativeInterface.html)
- [JNI Reference Example](https://thenewcircle.com/s/post/1292/jni_reference_example)
- [資料與數組轉換(轉)](http://provista.iteye.com/blog/839703)
- [Android Doc: JNI tips](http://developer.android.com/training/articles/perf-jni.html)
- [Accessing Java Strings in Native Methods](http://www.math.uni-hamburg.de/doc/java/tutorial/native1.1/implementing/string.html)
- [JNI Reference Example](https://thenewcircle.com/s/post/1292/jni_reference_example)
- [JNI错误总结](http://www.cnblogs.com/xingyun/archive/2012/08/03/2622410.html)
- [JNI Types and Data Structures](https://docs.oracle.com/javase/8/docs/technotes/guides/jni/spec/types.html)
- [JNI 返回结构体参数](http://andilyliao.iteye.com/blog/496816)

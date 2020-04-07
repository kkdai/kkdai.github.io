---
layout: post
layout: post
title: "[Android][JNI]如何由JNI thread/callback去呼叫Java Method"
description: ""
category: 
- android
tags: [android, jni]

---


![image](https://lh3.googleusercontent.com/-RogETFDNCSE/AAAAAAAAAAI/AAAAAAAAAIQ/oaIS5A2ZXMk/photo.jpg)


## 前言

在執行Android JNI的時候或多或少都需要在JNI中去呼叫Java的函式．（不論是只有Android才能拿到的資料，或是需要做callback) 

這裡就介紹如何在 JNI thread (或是 callback)中去呼叫 Java Method．

## 基礎觀念

先了解基礎概念，大部分而言會有兩種狀況需要由JNI呼叫Java．

1. Java -> Jni -> 更新另外一個Java 
2. Jni的Callback or Thread -> 更新 Java

會需要這種狀況，大部分都是(2)．當然也可能是(1)．由於在(2)的狀況下限制比較多而且比較麻煩，所以只講解(2)的部分． （當然，一樣的方式也可以適合給(1)使用．


### 先講解可能的限制

由於我們是從jni裡面的thread 或是 jni 去呼叫C/C++ module的callback．  所以..  你拿不到JNI Environment `JNIEnv` 跟原先呼叫或是你想要呼叫的Activity的`jobject`．

所以這裡要先透過某個 JNI main thread的function．舉例而言，最基本的Hello World JNI 都會有個 `stringFromJni()` 透過那個main thread的JNI function．

### 相關流程
1. 需要紀錄JVM (一般而言 JVM生命週期是每個App launch啟動）
2. 也必須要記錄你要呼叫的 Activity 的 jobject
3. 透過JVM取得目前的目前 JNIEnv  (透過[AttachCurrentThread](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/invocation.html))
4. 依照一般流程呼叫`CallVoidMethod `->`GetMethodID` -> `CallVoidMethod`(如果是呼叫void)
5. 透過[DetachCurrentThread](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/invocation.html)回收相關資源


### 程式碼:

<script src="https://gist.github.com/kkdai/d3ada20a9b368c054f64.js"></script>

### 容易出錯的部分:

#### 錯誤的jobject使用

這邊主要是提醒，由於JNI function 函式裡面的參數 `jobject thiz`的處理:

```
JNIEXPORT jstring Java_com_example_hellojni_stringFromJNI( JNIEnv *env, jobject thiz )
```

裡面的 `JNIEnv *env`  與  `jobject thiz`主要解釋如下:

- `JNIEnv *env` : 負責處理JNI的環境，千萬注意在thread裡面的`JNIEnv`會不同．所以要使用上面的[AttachCurrentThread](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/invocation.html)
- `jobject thiz`: 主要是處理該JNI library owner (Activity) 的Java Object． 
	- 這裡要主義的是這個jobject 主要是`System.loadLibrary("hello-jni");` 的Java Activity，而不是呼叫JNI function 的Java Object．


所以如果你有一個Java Class (ex:JNIJava)專門處理JNI，另外一個Java Class (ex: MainActivity)去建立 JNIJava然後去呼叫他，如果你需要呼叫回去MainActivity的函式，記得要把該Activity 傳進來． (不然就是要用FindClass找到正確的)


#### 沒有使用AttachCurrentThread，造成`CALL_TYPE` crash

如果在JNI native debugger 發現有`CALL_TYPE` error 在 `jni.h`．代表你再不是JNI main thread去使用main thread的`JNIEnv`．所以要注意．
	

## 相關鏈結

- [C++ callbacks into Java via JNI made easy(ier)](http://w01fe.com/blog/2009/05/c-callbacks-into-java-via-jni-made-easyier/)
- [Java Invocation](https://docs.oracle.com/javase/1.5.0/docs/guide/jni/spec/invocation.html)

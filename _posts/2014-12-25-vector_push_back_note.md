---
layout: post
layout: post
title: "[C][RUST][Python][Go]關於vector的push_back造成的記憶體參考(reference)問題"
description: ""
category: 
- RUST
- python
- vc++程式設計
- c++溫故知新
tags: []
---

![image](http://www.rust-lang.org/logos/rust-logo-blk.svg)

**前言:**

很基本的概念，是從[RUST官方網頁在介紹他的記憶體管理](http://doc.rust-lang.org/nightly/intro.html#ownership)的部分看到的．覺得是很有趣的vector與記憶體處理的問題，由於[RUST](http://www.rust-lang.org/)有比對它的程式語言設計架構，於是很好奇地把其他手邊常用的程式語言找了一下相關的範例．

**主要問題(C/C++/ObjectC)**

- 主要是講解，說在vector裡面的記憶體管理其實有一些技巧．尤其是在push_back的部分，每次的push_back如果目前的大小超過他的容量，就會放棄目前的記憶體位址，而去建立一個新的vector記憶體．而原本的記憶體位置就沒使用變成了garbage．
- 如此一來會造成原本去參考v[0]的記憶體位置變成了garbage，而會crash．
- 這邊可以參考[vector的push_back](http://en.cppreference.com/w/cpp/container/vector/push_back)說明．
- 這裏在ObjectC與C/C++結果不同，他不會crash但是會沒值．(ObjectC原本設定概念)
- **在C/C++/ObjectC解決方式**:
    - 其實只要先把記憶體保留起來，就可以解決這樣的問題． vector.reserve(specific_size); 

<pre class="prettyprint">  
#include <iostream>
#include <vector>
#include <string>

int main(int argc, const char * argv[]) {
    //Init new vector here
    std::vector<std::string> v;
    //v.reserve(128); could resolve this issue, 128 is arbitrarily value.
    
    //Put first element "hello" v: "Hello"
    v.push_back("Hello");
    // x refer to v[0] which is "Hello"
    std::string& x = v[0];
    
    //Push new element but size will over capacity (default is zero, after first push become 1)
    //So whole memory will deprence and create new memory size =2
    //Refer to http://en.cppreference.com/w/cpp/container/vector/push_back
    v.push_back("world");
    
    // Old reference memory aleady become to garbage, so app crash (depends OS)
    std::cout << x;    
    return 0;
}
</pre>

**在RUST方面**
    
- 根據[RUST官方網頁](http://www.rust-lang.org/)在介紹他的記憶體的管理方面，由於它定義兩種資料結構mutable 跟 let (這裏跟swift有點像)．所以根據以下的程式碼，會造成編譯的時候不成功．

<pre class="prettyprint">  
fn main() {
    let mut v = vec![];
    v.push("Hello");
    //let x = &v[0];  Cannnot compile. because the mutable variable cannot assign to static value.
    let x = v[0].clone();
    v.push("world");
    println!("{}", x);
}
</pre>


**在Python的部分**

- 沒有找到比較像vector的資料結構，我使用list來使用．我也有用codeskulptor來查看[視覺化(Viz mode)](http://www.codeskulptor.org/viz/index.html)的結果．
- 在python內有分成兩種方式來參照(reference)，如果你參照的是unmutable通常是使用copy，如果是參照mutable就會使用bind．而且整個list變大也不會影響參照的變數．
- 這邊主要是python varaible assignment比較不一樣，有比較多的討論可以看:
    - [How do I write a function with output parameters (call by reference)?](https://docs.python.org/3/faq/programming.html#how-do-i-write-a-function-with-output-parameters-call-by-reference)
    - [[stackoverflow]How do I pass a variable by reference?](http://stackoverflow.com/questions/986006/how-do-i-pass-a-variable-by-reference) 
        - 以下一段原文quote很值得了解: 
            - "the parameter passed in is actually a reference to an object (but the reference is passed by value)"
            - ""some data types are mutable, but others aren't"
        

<pre class="prettyprint">  

# Example in assign to first.
list_a = list()
list_a.append("hello")
item_b = list_a[0]   #copy value list_a first element to item_b, not point to.
list_a.append("world")
print item_b  #hello

# Example in bind to whole list.
list_a = list()
list_a.append("hello")
item_b = list_a    #item_b bind to list_a
list_a.append("world")   #in codeskulptor memory not relocate nd continue.
print item_b       #still "hello"
</pre>

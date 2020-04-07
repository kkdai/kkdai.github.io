---
layout: post
layout: post
title: "[Golang]Study note about 'Taking Out The Trash talk'"
description: ""
category: 
- golang
tags: []

---

![image](http://talks.golang.org/2014/go4java/img/fast.jpg)

## What is this talk about

Here is the talke [Taking Out the Trash: Great talk about optimizing memory allocation.﻿](https://www.youtube.com/watch?v=l3zHGMgtr2Y&feature=share) about Go memory optimize and GC.

I have read it, and note my understanding as follow:

##Note:

###How does Go allocate / deallocate memory?
- Use stack for local variable and cleanup when return.
- Use heap when large local variale or compiler cannot approve are not referenced. It cleanup by GC.
- More detail in [golangDoc- stack or heap](http://golang.org/doc/faq#stack_or_heap)
    
### Reserving Virtual Memmory
- Golang will reserve the virtual memory but not use it. If you want to know how much memory use by go need check "top" (check RES/RESIZE columns).

![image](http://dave.cheney.net/wp-content/uploads/2014/07/Screenshot-from-2014-07-11-144528.png)

(Refer to [Dave Cheney:Visualising the Go garbage collector](http://dave.cheney.net/2014/07/11/visualising-the-go-garbage-collector), you can see the system memory will not return by Go)


### When GC Run?

- Base on source code src/runtime/mgc0.c define the GOGC environment is 100. We could adjust it to change to GC period. 
- The more gabarge you make, means it need run more times of GC.


### Reuse your slice

Slice address will create and reserve once you use it, if some slice you will not use it anymore. We could recycle its address to reduce memory allocate time and reuse memory by GC.

<pre class="prettyprint"> 
package main

import "fmt"

func main() {
	a := []string{"Hello", "World"}
	//a=["Hello", "World"]

	b := a[:0]
	//b=[] but cap and address is the same with a

	fmt.Println(a, b, cap(a), cap(b), len(a), len(b))
	//[Hello World] [] 2 2 2 0
	//*note* The cap is the same, but length is different.

	b = append(b, "Change")
	//Appen one item to b, it will also change a because they share the same address.

	fmt.Printf("a content=%q, b content=%s, length(a)=%d, length(b)=%d, address(a)=%d, address(b)=%d\n", a, b, len(a), len(b), &a[0], &b[0])
	//a content=["Change" "World"], b content=[Change], length(a)=2, length(b)=1, address(a)=272851328, address(b)=272851328
}
</pre>

Play is [here](http://play.golang.org/p/AyVbUBZ-f_).  

For slice resource allocation, we could refer to [slice trick](https://github.com/golang/go/wiki/SliceTricks) in Golang wiki.



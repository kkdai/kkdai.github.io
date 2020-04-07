---
layout: post
layout: post
title: "[Golang] Enable HTTPS in martini using martini-contrib/secure"
description: ""
category: 
- golang
tags: []

---

![image](http://trading.divblocks.com/static/uploads/goimg1910.png)

實在有點懶得用中文來寫了... 讓我用英文寫這篇吧...


## Before this..

I am learning about golang on server backend side programming via [martini](https://github.com/go-martini/martini). I want to write a RESTful server communicate via JSON. Currently my framework as follow:

- Host on: [Heroku](https://www.heroku.com/)
- DB: [MongoDB](https://www.compose.io/)
- Web framework: [maritini](https://github.com/go-martini/martini)

Here is the note about how I add [martini-secure](https://github.com/martini-contrib/secure) HTTPS in martini.

## Working step with martini-secure

### 1. Create a SSL key using generate_cert.go

You need create your localhost SSL key (or purchase one from CA) using [generate_cert.go](http://golang.org/src/crypto/tls/generate_cert.go). 
I am not sure how to get this package, so I just download this source code and run it locally.

```
go run generate_cert.go --host="localhost"
```

You will get file "key.pem" and "cert.pem", just put in your web side source code.


### 2. Apply martini-secure in martini program.

It is very easy to add SSL as a plugin in martini, here is some code. At first I would like to host two services as follow:

- HTTP: port 8080 
- HTTPS: port 8443

```
m := martini.Classic()

// Make sure this enable, or it will get failed.
martini.Env = martini.Prod

    
m.Use(secure.Secure(secure.Options{
	SSLRedirect: false,
	SSLHost:     "localhost:8443",
}))

m.Get("/foo", func() string {
	return "bar"
})

go func() {
	if err := http.ListenAndServe(":8080", m); err != nil {
		fmt.Println(err)
	}
}()

// HTTPS, make sure "cert.pem" and "key.pem" files are exist.
if err := http.ListenAndServeTLS(":8443", "cert.pem", "key.pem", m); err != nil {
	fmt.Println(err)
}
```

### 3. Write a client to verity it.

If it build with luck, let's verify with some simple client code as follow:

- TLSClientConfig: true : Because we use custom SSL key, not signed by CA.

```
package main

import (
	"crypto/tls"
	"fmt"
	"io/ioutil"
	"net/http"
	"os"
)

func HttpsVerity(address string) {
	tr := &http.Transport{
		TLSClientConfig: &tls.Config{InsecureSkipVerify: true},
	}

	client := &http.Client{Transport: tr}

	response, err := client.Get(address)
	if err != nil {
		fmt.Printf("%s", err)
		os.Exit(1)
	} else {
		defer response.Body.Close()
		contents, err := ioutil.ReadAll(response.Body)
		if err != nil {
			fmt.Printf("%s", err)
			os.Exit(1)
		}
		fmt.Printf("%s\n", string(contents))
	}
}

func HttpVerity(address string) {
	client := &http.Client{}

	response, err := client.Get(address)
	if err != nil {
		fmt.Printf("%s", err)
		os.Exit(1)
	} else {
		defer response.Body.Close()
		contents, err := ioutil.ReadAll(response.Body)
		if err != nil {
			fmt.Printf("%s", err)
			os.Exit(1)
		}
		fmt.Printf("%s\n", string(contents))
	}
}

func main() {
	fmt.Println("HTTP")
	HttpVerity("http://localhost:8080/foo")
	fmt.Println("HTTPS")
	HttpsVerity("https://localhost:8443/foo")
}
```

## Problems you might occur

### tls: oversized record received with length 20527

- Root cause: 
    - Server side SSL configuration failed, might related to key files.    
- Solution:
    - You forget to add "http.ListenAndServeTLS" in your server side also check SSL key files if missing.
    
    
### Cannot connect original HTTP protocol

- Root cause:
    - martini-secure default enable SSL redirect, so original HTTP protocol will redirect to HTTPS.
- Solution:
    - Add following code in your server side.

```
m.Use(secure.Secure(secure.Options{
	SSLRedirect: false,
}))               	
```

### Show your web side is not secure when using web browser

- Root cause:
    - We are using custom SSL key which don't signed by CA. So it will show error when you using browser to browse your server.
- Solution:
    - Buy a offcial SSL key.

###  How to deploy HTTPS on heroku using martini

It is very easy to deploy HTTPS on Heroku, using [SSL Endpoint](https://devcenter.heroku.com/articles/ssl-endpoint). All you need as follow:

- Enable SSL Endpoint in Heroku ($20/per month)    
- Purchase a valid srt from a legal CA, and apply to Heroku.
- Assign your domain name to HTTPS host (such as XXXX..herokussl.com)
- Your application just handle HTTP request as well.
- And.. That's done.  :)
 


Hope you can enjoy martini as well. 

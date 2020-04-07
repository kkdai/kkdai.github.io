---
layout: post
layout: post
title: "[Java][Heroku][程式設計週記] 新年快樂...來寫code吧.. Go-martini跟 Java Srping Boot"
description: ""
category: 
- eclipse
- java
- Heroku
- 程式設計週記
tags: []

---

![image](http://golang.org/doc/gopher/project.png)

**前言:**

新年新希望.... 繼續寫程式啊....  之後要把Go Web Server逐漸到真的能使用的部分．

**筆記:**

- [Golang][Heroku]  將buildpack upgrate到Go1.4
    - 最近的需求，將Go拿出來重新的翻了一下．除了之前發現沒有更新到Go1.4之外．也發現原來之前透過martini寫出的REST server並不完全是JSON的格式．於是最近開始把Go-herokuo-server翻了一下．
    - 以下紀錄如何將你的heroku更新到1.4
        - 重新跑godep
        - 記得將godep目錄下的檔案全部一併更新上去
    - 或是可以參考[kr/heroku-buildpack-go](https://github.com/kr/heroku-buildpack-go)         

- [Golang]關於Json資料的Get/Post處理
    - 發現martini原先雖然有做REST API但是並沒有針對JSON資料來處理．而要處理JSON的資料其實在Go裡面還是有一些地方要處理．
    - 幾個值得注意的地方如下:
        - 從網頁抓來的資料可以由http.Request.Form或是http.request.Body抓來處理．
            - Request.Form 資料可以從 curl -X POST -d "DATA" "http://address"
            - Request.Body 資料不僅僅可以抓 curl的資料，也可以抓取http.NewRequest("POST", url, bytes.NewBuffer(jsonStr))來的資料．
            - 抓取來的資料都可以透過map interface來接，再慢慢去處理．
            - 如果JSON資料比較複雜類似 { A:a1 B:[ { B1: b1, B2:b2}]} 就透過以下邏輯來解決
                - 先透過第一層map拿到B內容 [ { B1: b1, B2:b2}]
                - 透過 array interface拿到 { B1: b1, B2:b2}，記住內縙 ary[0]
                - 再透過map接過來繼續解出其他的內容．              
    - 參考資料:
        - [傳送JSON POST資料方式](http://stackoverflow.com/questions/24455147/go-lang-how-send-json-string-in-post-request)
        - [透過Request.Body抓取資料](http://stackoverflow.com/questions/15672556/handling-json-post-request-in-go)
        - [Golang Example的JSON](https://gobyexample.com/json)     
        - [這邊有關於Request.Body部分的處理方式](http://shahalpk.name/post/71325851907/handling-json-post-request-data-in-golang)
    - 以下把部分程式碼擷取出來:
    
<pre class="prettyprint">  
//資料來源
//curl -X POST -d "{ \"name\"" : \"that\", \"num\": \"3\"}ttp://localhost:5000/fruits"

// 透過Request.Form的資料
r.ParseForm()

var dat map[string]interface{}

for key, _ := range r.Form {
	fmt.Println("r.form")
	fmt.Printf("key = ")
	fmt.Println(key)
	err := json.Unmarshal([]byte(key), &dat)
	if err != nil {
		fmt.Println(err.Error())
	}
}

for key, value := range dat {
	fmt.Println("Key:", key, "Value:", value)
}


// 透過Request.Body再把Json資料Unmarshall
body, err := ioutil.ReadAll(r.Body)
if err != nil {
	fmt.Println("read error")
}
fmt.Println(body)
fmt.Println(reflect.TypeOf(body))
fmt.Println(string(body))

var dat map[string]interface{}

err = json.Unmarshal(body, &dat)
if err != nil {
	fmt.Println("Unmarshal error")
}

//Handle Data Like
////Data: `{"name":"test1", "num":"2", "data": [{"data1": "value1", "data2": "value2" }] }`

err = json.Unmarshal(body, &dat)
if err != nil {
	fmt.Println("Unmarshal error")
}

//Get specific colume as array interface
//[{"data1": "value1", "data2": "value2" }]
map_body := dat["data"].([]interface{})

var dat2 map[string]interface{}
// Get first one from array (dat2 is map)
//{"data1": "value1", "data2": "value2" }
dat2 = map_body[0].(map[string]interface{})

//Handle dat2 as map to get rest data.
	
</pre>        


- [iOS] 寫了Server方面，當然要寫client的方式
    - 讀取很方便，NSURLConnection就可以處理，而且抓來的Json會直接放進map處理
    - 寫入有一點小複雜，必須寫使用Diectionary處理你要處理的資訊後，再透過NSURLConnection來啟動．

<pre class="prettyprint">  

- (IBAction)JsonSend:(id)sender {
    NSMutableURLRequest *request = [NSMutableURLRequest
                                    requestWithURL:[NSURL URLWithString:@"http://127.0.0.1:5000/fruits2"]];
    
    //透過NSDictionary放入資料，右邊是Key左邊是Value
    NSDictionary *requestData = [[NSDictionary alloc] initWithObjectsAndKeys:
                                 @"Banana", @"name",
                                 @"3", @"num",
                                 nil];
    NSError *error;
    NSData *postData = [NSJSONSerialization dataWithJSONObject:requestData options:0 error:&error];
    [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
    [request setHTTPMethod:@"POST"];
    [request setHTTPBody:postData];
    
    
    NSURLResponse *requestResponse;
    NSData *requestHandler = [NSURLConnection sendSynchronousRequest:request returningResponse:&requestResponse error:nil];
    
    NSString *requestReply = [[NSString alloc] initWithBytes:[requestHandler bytes] length:[requestHandler length] encoding:NSASCIIStringEncoding];
    NSLog(@"requestReply: %@", requestReply);
}

- (IBAction)JsonGet:(id)sender {

    NSURLRequest *request = [[NSURLRequest alloc] initWithURL:[NSURL URLWithString:@"http://127.0.0.1:5000/fruits/1"]];
    
    __block NSDictionary *json;
    [NSURLConnection sendAsynchronousRequest:request
                                       queue:[NSOperationQueue mainQueue]
                           completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
                               json = [NSJSONSerialization JSONObjectWithData:data
                                                                      options:0
                                                                        error:nil];
                               NSLog(@"Async JSON: %@ id=%@", json, json[@"id"]);
                           }];

}
</pre>
     
- [Eclipse][Java] 復原整個Java Srping Boot的開發環境
    - 前言:
        - 之前因為Eclipse ADT從22.x更新到了23，使得我的eclipse無法正確跑Android．看了許多人的建議．看來只能夠把Eclipse砍掉重裝．不過也讓我的Java Spring Boot整個開發環境就跑掉了．
        - 這一篇記錄一下會遇到哪些問題，並且把問題的解決方式也記錄一下．
    - 問題一： JRE環境跑掉  
        - 問題內容
            - 無法正確的編譯Java Spring的專案
        - 解決方法:
            - 由於之前在跑Spring專案的時候，有使用到Java8的一些函示．所以有裝Java8．JRE跑掉會有以下的狀況．
            - [Preference]->[Java]->[Installed JREs]  發現是空的
            - 按下 [Add]->[Stardard VM]->Directory 選擇 [Library/Java/JavaVirtualMachines/JDKversion]
    - 問題二: 無法執行junit
        - 問題內容:
            - 按下junit來跑測試的部分的時候，發現都會出現“An internal error occured during Launching”
        - 解決方式:
            - 這邊花了一點時間才解決問題． 
                - Go to help
                - Install New Software
                - Work with: Juno
                - Programming languages (expand it)
                - Install "**Eclipse** Java Development Tools""
                - Restart
                - 參考[這裡](http://stackoverflow.com/questions/1250505/junit4-eclipse-an-internal-error-occured-during-launching)                        
    - 順便在三地記錄一下，要如何跑Java Spring Boot專案
        - 專案:
            - [Run] -> [Java Application] -> Machining Item -> [Application]
        - 測試部分:
            - 指向該junit檔案(*.java) [Run]->[Junit Test]
            
                                                     

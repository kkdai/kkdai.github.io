---
layout: post
layout: post
title: "[Golang] 關於Golang Time 的處理"
description: ""
category: 
- golang
tags: []

---


![image](http://i.imgur.com/JBu6kZF.jpg)

### [Golang] 關於Golang Time 的處理

花了一點時間在處理timestamp．工作不外乎是儲存timestamp，解析timestamp並且比對timestamp． 找了很久的相關資料，卻發現處理上並沒有那麼直覺主要是在於以下幾個部分．

####格式: 永遠會困在格式之中

首先最重要的就是處理time的格式，根據[Golang Time Pkg](http://golang.org/pkg/time/)，裡面的[格式](http://golang.org/pkg/time/#pkg-constants)可以有ANSI, RFC822, RFC1123.... 許多種，這裡面大家所熟知的[ISO 8601](http://en.wikipedia.org/wiki/ISO_8601)其實沒有名列在其中．但是你可以使用它其中一個profile 那麼就是[RFC 3339](http://www.ietf.org/rfc/rfc3339.txt)

儲存格式決定你要如何存取時間的資料．

當然.... 如果大家都是丟time.Time 的格式.. 那麼就世界太平.... 如果不行.. 大家談一下如何parse..

####時區: 時間比對的重要參數

大家都知道，時區是一個濫觴．但是只要能夠正確的處理其實Golang裡面內建已經把時區處理好了．主要用來解析的是[Parse](http://)與[ParseInLocation](http://golang.org/pkg/time/#ParseInLocation) 

會需要使用到時區的時候，通常是在於接收資料從client或是其他的程式． 這時候比較建議的是:

- 統一使用UTC 時區，作為字串解析會統一點
- LoadLocation 是個好東西，可以幫助你轉換時區．前提是你已經拿到time.Time

以下為一個簡單範例去取得UTC的時間

        func GetUTCTime() time.Time {
        	t := time.Now()
        	local_location, err := time.LoadLocation("UTC")
        	if err != nil {
        		fmt.Println(err)
        	}
        	time_UTC := t.In(local_location)
        	return time_UTC
        }

如果你必須要parse一個time.Time不支援的格式，來轉換成time.Time格式．以下是一個簡單的範例．(當然前提資料是 UTC :) )

        func ParseCustomTimeString(time_string string) time.Time {
        	//"2015-03-10 23:58:23 UTC"
        	str_array := strings.Split(time_string, "UTC")
        	var year, mon, day, hh, mm, ss int
        	fmt.Sscanf(str_array[0], "%d-%d-%d %d:%d:%d ", &year, &mon, &day, &hh, &mm, &ss)
        	time_string_to_parse := fmt.Sprintf("%d-%02d-%02dT%02d:%02d:%02d+00:00", year, mon, day, hh, mm, ss)
        	url_create_time, _ := time.Parse(time.RFC3339, time_string_to_parse)
        	return url_create_time
        }

####時間的比對，善用Before/After
針對於時間的操作上面，其實[Golang Time Pkg](http://golang.org/pkg/time/) 提供了很方便的比較時間先後的方式． [Before](http://golang.org/pkg/time/#Time.Before)/[After](http://golang.org/pkg/time/#Time.After)

其實只要透過以下的方式，就可以比較不同時區，不同時間格式的先後順序．

        func IsWarrantyExpired(time_purchase time.Time) bool {
        	//time of product purchase store in DB which is time.time with ETC
        	time_Warranty := time_purchase.AddDate(1, 0, 0) //假設是一年保固
        	return time_Warranty.After(time.Now())          //如果保固期比現在還後面，代表還是保固期內．
        }

範例程式中， time.Time.AddDate() 是一個可以取出變更過的時間．比如說你需要取出現在這個時間一年後的timestamp，就可以透過這個．目前支援年，月，日． 如果要小時的話，可能就得要手動去改．

####在資料庫裡處理time.Time

其實不論是 MySQL或是 Mongodb都是支援 time_t的．只是要如何透過Golang把 time.Time放入資料庫．這裡有簡單的整理．Go-MySQL-Driver [本身已經支援time.Time](https://github.com/go-sql-driver/mysql#timetime-support)． 接下來會介紹如何在[MongoDB](http://docs.mongodb.org/manual/reference/bson-types/)裡面使用time.Time．

如果是MySQL 可以直接把time.Time 轉成[]byte 來操作，方式如下：

        //MySQL using "[]byte" to handle time_stamp
        //Refer time-stamp in golang
        func (t *Timestamp) MarshalJSON() ([]byte, error) {
        	ts := time.Time(*t).Unix()
        	stamp := fmt.Sprint(ts)
 
        	return []byte(stamp), nil
        }
 
        func (t *Timestamp) UnmarshalJSON(b []byte) error {
        	ts, err := strconv.Atoi(string(b))
        	if err != nil {
        		return err
        	}

如果是MongoDB 則可以直接操作．在MongoDB裡面會把欄位設定成[ISODate()](http://docs.mongodb.org/manual/reference/bson-types/)


    type Person struct {
    	ID        bson.ObjectId `bson:"_id,omitempty"`
    	Name      string
    	Phone     string
    	Timestamp time.Time
    }
    
    func main() {
    	session, err := mgo.Dial("127.0.0.1")
    	if err != nil {
    		panic(err)
    	}

	defer session.Close()

	// Collection People
	c := session.DB("DB").C("test")

	// Insert Datas
	err = c.Insert(&Person{Name: "fff", Phone: "+55 53 1234 4321", Timestamp: time.Now().AddDate(0, 0, -3)},
		&Person{Name: "gggg", Phone: "+66 33 1234 5678", Timestamp: time.Now().AddDate(0, 0, 4)})

	if err != nil {
		panic(err)
	}

    // Query All and order by timestamp descending
    	var results []Person
    	err = c.Find(bson.M{"name": bson.M{"$exists": true}}).Sort("-timestamp").All(&results)

	if err != nil {
		panic(err)
	}
	fmt.Println("Results All: ")
	for _, v := range results {
		fmt.Println(v.Name, ":", v.Timestamp.Format("2006-01-02T15:04:05Z07:00"))
	}


詳細的程式碼，可以參考[這裡](https://gist.github.com/border/3489566)．


### 相關資料

- [MongoDB bson timestamp type](http://docs.mongodb.org/manual/reference/bson-types/)
- [Golang: time pkg](http://golang.org/pkg/time)
- [Git: sample code to store timestamp in mongoDB with Go](https://gist.github.com/border/3489566)
- [Go-SQL-Driver support Timestamp](https://github.com/go-sql-driver/mysql#timetime-support)
- [UNIX time stamps in Golang](https://medium.com/coding-and-deploying-in-the-cloud/time-stamps-in-golang-abcaf581b72f)

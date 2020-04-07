---
layout: post
layout: post
title: "[MongoDB]More study about mongodb and mgo"
description: ""
category: 
- golang
- mongodb
tags: []

---


## Preface

After some study about mongodb in some web server usage, I occur some question and need to resolve it.

Here is three major questions I found here.

- Question 1: Auto increase number
- Question 2: Check field(column


### Auto increase number

This question comes from the usage of MySQL auto increase sequence number. I am wondering if there is similiar mechanism in MongoDB. There is a [official article](http://docs.mongodb.org/manual/tutorial/create-an-auto-incrementing-field/) about MongoDB implement auto-increasementing Sequence Field.

The simple idea, is to add extra record in your database to storage this field. It might impact as follow:

-  Your index might need to refere this field.
-  Your query all need filter(ignore) the sequence record. (refer to "check field if exist")


For the FindAndModify mechanism in MongoDB, the [mgo](http://godoc.org/labix.org/v2/mgo) has similar implement about "Apply".


<pre class="prettyprint">  

type DBSequence struct {
	ID  string `json:"id"`
	// ID is a string ID, in this case I will use collection.Name as it ID.
	SEQ int64  `json:"seq"`
}

func AutoIncDBSeq(col *mgo.Collection) (int64, error) {
	seq := DBSequence{ID: "seq", SEQ: 0}
	change := mgo.Change{
		Update:    bson.M{"$inc": bson.M{"seq": 1}},
		ReturnNew: true,
	}
	_, err := col.Find(bson.M{"id": col,Name}).Apply(change, &seq)
	if err != nil {
		log.Println("err log:", err.Error())
		return 0, err
	}
	log.Println(seq)
	return seq.SEQ, nil
}
</pre>


### Check field if exist

Some time it might easy to find empty field (column) in other database, but if you want MongoDB filter some record if some field is empty here is the sample code in mongoDB. From [stackoverflow](http://stackoverflow.com/questions/9694223/test-empty-string-in-mongodb-and-pymongo)

- db.collection.find({"lastname" : {"$exists" : true}})

<pre class="prettyprint">  

//Check get if index is exist
func GetAllData() []Data {
	var alldb []Data
	//It is multuple query mgo sample, you need two bson in this case
	err := col.Find(bson.M{"index": bson.M{"$exists": true}}).All(&alldb)
	if err != nil {
		log.Println("No user in DB....")
		return nil
	}
	return alldb
}
</pre>

In this case it also describe multiple query mgo, sample. 

    {"lastname" : {"$exists" : true}}
    -> bson.M{"lastname" : {"$exists" : true}}
    -> bson.M{"lastname" : bson.M{"$exists" : true}}

### Paginating

When you need paginating for user request and display. You can use Skip and Limit command in MongoDB. Here is the detail [howo](http://blog.mongodirector.com/fast-paging-with-mongodb/).

Ex: Paginating information every 20 record a page.

    var Alldb []ReturnData
    skipCount := page * 20
    limit := 20
    Col.Find(bson.M{}).Skip(skipCount).Limit(limit).All(&Alldb)


## Reference:

- [Mgo: Mongo driver on Golang](http://godoc.org/labix.org/v2/mgo)
- [MongoDB: Create an Auto-Incrementing Sequence Field](http://docs.mongodb.org/manual/tutorial/create-an-auto-incrementing-field/)
- [Stackoverflow](http://stackoverflow.com/questions/9694223/test-empty-string-in-mongodb-and-pymongo)

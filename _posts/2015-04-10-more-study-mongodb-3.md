---
layout: post
layout: post
title: "[MongoDB]More study about mongodb and mgo 3 - logical operator and like "
description: ""
category: 
- golang
- mongodb
tags: []

---



## Preface

When we trying to use MongoDB, the requirement comes more and more complex and diversity. Here is some note during my implement.

## Multiple condition in MongoDB Query

It is very easy to find data in MongoDB, but how about multiple condition such as "AND" and "OR" ?


### AND OR in MongoDB

It is very easy to [find](http://docs.mongodb.org/manual/reference/operator/query/and/) "AND" support in MongoDB, but how to apply in [mgo](https://labix.org/mgo) (MongoDB driver in Go)?

        // Find user name is John and Contry is US.
        var alldb []User
        UserCollection.Find(bson.M{"$and": []bson.M{bson.M{"name": "John"}, bson.M{"Contry": "US"}}}).All(&alldb)

Please note: the `$and` need combine an array of bson.M.

        // Find CONDITION_A and CONDITION_B
        bson.M{"$and": []bson.M{ CONDITION_A, CONDITION_B }}

So, it is similar with "OR" (`$or`), detail doc is [here](http://docs.mongodb.org/manual/reference/operator/query/or/).

        // Find CONDITION_A or CONDITION_B
        bson.M{"$or": []bson.M{ CONDITION_A, CONDITION_B }}

Make it more clear in code.

        // Find user name is John or Tom.
        var alldb []User
        UserCollection.Find(bson.M{"$or": []bson.M{bson.M{"name": "John"}, bson.M{"name": "Tom"}}}).All(&alldb)


### Using "like" or simple regular expression in MongoDB Query

When we trying to query data, sometime we need use "like". In MongoDB it is easy to find data as follow:

    db.users.find({name: /a/})  //like '%a%'

But the special charactor `"/"` will skip during Go programming, so we need change to use regular expression.

    Collection.Find(bson.M{"name": bson.RegEx{"a", ""}}).All(&result) //like `%a%`

###  Query array size over "n" (great than "n")

The major adanvantage of MongoDB is to store JSON directly. What if we want to find a array size which great than 2 ?

The first idea is to find if the array size great than 2 in the collection.
So the query string should be follow:

    Collection.Find(bson.M{"array": bson.M{"$size" : bson.M{"$gt" : 2}}}).All(&result) //like `%a%`

But it don't work? How, so I find here is a discussion [here](http://stackoverflow.com/questions/7811163/in-mongodb-how-do-i-find-documents-where-array-size-is-greater-than-1).

The major idea is to get array element and check if exist. The easy way to get array index 1 (which mean second element of array aka. `array[1]`)

    Collection.Find(bson.M{"array.1": bson.M{"$exists" : true} }).All(&result) //like `%a%`


So, if you want to find array size great than `5`, just change to `array.4`.    


## Reference

- [How to use logical operator query in MongoDB](http://stackoverflow.com/questions/26932298/mongodb-in-go-golang-with-mgo-how-to-use-logical-operators-to-query)
- [MongoDB Query "AND"](http://docs.mongodb.org/manual/reference/operator/query/and/)
- [Using "like" in MongoDB](http://stackoverflow.com/questions/3305561/how-to-query-mongodb-with-like)
- [MongoDB Doc: $size](http://docs.mongodb.org/manual/reference/operator/query/size/)
- [MongoDB Doc: $gt](http://docs.mongodb.org/manual/reference/operator/query/size/)
- [Stackoverflow: How to find array size over 2](http://stackoverflow.com/questions/7811163/in-mongodb-how-do-i-find-documents-where-array-size-is-greater-than-1)

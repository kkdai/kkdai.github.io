---
layout: post
title: "[iOS/Golang]Server-Side In-App-Purchase Verification in Apple Store"
description: ""
category: 
- golang
- iOS
tags: ["IAP", "Mobile", "go"]
---


### Steps

#### Connect to Apple iTune

```
// iOS Http code
NSMutableDictionary *parameters = [NSMutableDictionary dictionary];
[parameters addEntriesFromDictionary:[credentials dictionary]];

// receipt is an object of my own making, but base64String just returns an
// NSString representation of the receipt data.
parameters[PURCHASE_RECEIPT] = [receipt base64String];

NSURLRequest *request =
    [[AFHTTPRequestSerializer serializer]
        requestWithMethod:@"POST"
                URLString:urlString
                parameters:parameters];

AFHTTPRequestOperation *operation =
    [[AFHTTPRequestOperation alloc]
        initWithRequest:request];
operation.responseSerializer = [AFJSONResponseSerializer serializer];

<snip>

[operation start];

// Json data
{"status":0,
    "environment":"Sandbox",
    "receipt":
    {"receipt_type":"ProductionSandbox",
        "adam_id":0,
        "bundle_id":"<snip>",
        "application_version":"1.0",
        "download_id":0,
        "request_date":"2013-11-12 01:43:06 Etc\/GMT",
        "request_date_ms":"1384220586352",
        "request_date_pst":"2013-11-11 17:43:06 America\/Los_Angeles",
        "in_app":[
                  {"quantity":"1",
                      "product_id":"<snip>",
                      "transaction_id":"1000000092978110",
                      "original_transaction_id":"1000000092978110",
                      "purchase_date":"2013-11-12 01:36:49 Etc\/GMT",
                      "purchase_date_ms":"1384220209000",
                      "purchase_date_pst":"2013-11-11 17:36:49 America\/Los_Angeles",
                      "original_purchase_date":"2013-11-12 01:36:49 Etc\/GMT",
                      "original_purchase_date_ms":"1384220209000",
                      "original_purchase_date_pst":"2013-11-11 17:36:49 America\/Los_Angeles",
                      "is_trial_period":"false"}
                  ]
    }
}
```


### Reference

- [Validating Receipts With the App Store](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)
- [iOS7 - receipts not validating at sandbox - error 21002 (java.lang.IllegalArgumentException)](http://stackoverflow.com/questions/19222845/ios7-receipts-not-validating-at-sandbox-error-21002-java-lang-illegalargume/19963192#19963192)
- [Validate Mac App Store receipt server side](http://stackoverflow.com/questions/22957165/validate-mac-app-store-receipt-server-side)
- [Validating iOS In-App Purchases With Laravel](http://blog.goforyt.com/validating-ios-app-purchases-laravel/)
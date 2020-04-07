---
layout: post
layout: post
title: "[iOS] HealthKit的初體驗.. "
description: ""
category: 
- ios的程式開發
tags: []

---

![image](../images/2014/healthkit-icon.png)

**前言:**

最近開始仔細瞭解HealthKit，發現使用上並沒有那麼的直覺．(可能是我對於許多醫療單位沒有那麼清楚的關係 XD) 使用上有一些要注意的與需要瞭解的部分，稍微做一些紀錄．

**筆記:**

[基本概念]

- 到目前為止，HealthKit只有iPhone可以用，並且要有iOS8．所以iPad執行程式會失敗．
- 每一個資料有所屬的資料格式數量(HKQuantity)與該數量的格式(HKQuantityType)，而數量(Quantity)本身與單位的格式(HKUnit)有關．
- 必須要清楚地瞭解，存取的資料數量單位與單位的格式．
    - 比如說，身高要確認是用cm還是inch，體重是使用pound還是kg．
- 每一個存取都與使用者權限有關，去拿使用者不開放的資料，就會引發錯誤．
- 所有的動作都是Async的方式:
    - 這邊比較麻煩的是，如果需要抽象化的話可能要把async改成sync這邊可以參考[這邊的內容](http://stackoverflow.com/questions/8582761/wrapping-asynchronous-calls-into-a-synchronous-blocking-thread)．

[基本資料欄位]

- 關於資料欄位舉例而言，要敘述關於血壓的舒張壓，你就得要撰寫以下的部分:                 

<pre class="prettyprint">
    //敘述你的血壓單位是 mmag mullumeter of mercyry unit
    HKUnit *BPUnit = [HKUnit millimeterOfMercuryUnit];
    //透過這個HKUnit去建立單位 HKQuantity 
    HKQuantity *BPSystolicQuantity = [HKQuantity quantityWithUnit:BPUnit doubleValue:SIS];
    // 透過建立的單位與數值來建立這個單位類型(HKQuantityType)
    HKQuantityType *BPSystolicType = [HKQuantityType quantityTypeForIdentifier:HKQuantityTypeIdentifierBloodPressureSystolic];
</pre>

[取得使用者權限] 

- 上面的方式僅僅是拿來作為處理資料欄位而已，接下來要開始去跟HealthKit查詢資料．在查詢資料之前，由於HealthKit上的資料都是儲存在手機上面，屬於個人私密資料．需要取得使用者的權限．

<pre class="prettyprint">
    //取出體重單位格式
    HKQuantityType *weightType = [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierBodyMass];
    //取出身高單位格式
    HKQuantityType *heightType = [HKObjectType quantityTypeForIdentifier:HKQuantityTypeIdentifierHeight];
    // 將身高體重加入要求的權限集合，這裡有分"讀取"與"寫入"的集合
    NSSet *writeDataTypes = [NSSet setWithObjects:heightType, weightType, nil];
    NSSet *readDataTypes = [NSSet setWithObjects:heightType, weightType, nil];

    // 確認HealthKit 存不存在，這裡如果還是iPad 是不會有的，請注意．
    if ([HKHealthStore isHealthDataAvailable]) {        
        // 要求使用者同意
        [self.healthStore requestAuthorizationToShareTypes:writeDataTypes readTypes:readDataTypes completion:^(BOOL success, NSError *error) {
            if (!success) {
                NSLog(@"You didn't allow HealthKit to access these read/write data types. In your app, try to handle this error gracefully when a user decides not to provide access. The error was: %@. If you're using a simulator, try it on a device.", error);
                
                return;
            }
            
            //處理成功以後，可能需要的資料讀取部分．
        }];
    }
</pre>

[讀取資料]

- 關於讀取資料這邊，需要注意的就是必須要取得正確的單位．
- 還是一樣，讀到資料以後是async，所以可以用另外的thread來更新UI
- 以下提供取得"心跳(Heart Rate)"的方式

<pre class="prettyprint">
    //心跳屬於複合資料欄位，需要用字串來找到HKUnit．文件不好找到．
    HKUnit *dataUnit = [HKUnit unitFromString:@"count/min"];
    
    //取得心跳的單位
    HKQuantity *HRQuantity = [HKQuantity quantityWithUnit:dataUnit doubleValue:heartRate];
    
    //取得心跳單位格式，注意HKQuantityTypeIdentifierHeartRate
    HKQuantityType *HeartRateType = [HKQuantityType quantityTypeForIdentifier:HKQuantityTypeIdentifierHeartRate];

    //建立資料回傳的sort敘述    
    NSSortDescriptor *timeSortDescriptor = [[NSSortDescriptor alloc] initWithKey:HKSampleSortIdentifierEndDate ascending:NO];

    //建立查詢的指令，注意因為我們只需要最近的所以把limit設定成1
    HKSampleQuery *query = [[HKSampleQuery alloc] initWithSampleType:HeartRateType predicate:nil limit:1 sortDescriptors:@[timeSortDescriptor] resultsHandler:^(HKSampleQuery *query, NSArray *results, NSError *error) {
        if (!results) {
            //錯誤發生            
            return;
        }
        
        //取得心跳資料
        double usersHR = [mostRecentQuantity doubleValueForUnit:dataUnit];
    }];
    
    //執行查詢的指令
    [self.healthStore executeQuery:query];

</pre>            

[寫入資料]

- 其實跟讀取一樣，主要也是要把HKUnit，HKQuantity與HKQuantityType搞定．
- 這邊需要注意的是，儲存的資料都是有時間格式的．你必須要設定開始時間與結束時間．

<pre class="prettyprint">

    //建立心跳的單位
    HKUnit *dataUnit = [HKUnit unitFromString:@"count/min"];

    //建立心跳的數量單位
    HKQuantity *HRQuantity = [HKQuantity quantityWithUnit:dataUnit doubleValue:heartRate];

    //建立心跳的單位格式  
    HKQuantityType *HRType = [HKQuantityType quantityTypeForIdentifier:HKQuantityTypeIdentifierHeartRate];
    NSDate *now = [NSDate date];

    //建立數量取樣
    HKQuantitySample *HRSample = [HKQuantitySample quantitySampleWithType:HRType                                                                          
                                                   quantity:HRQuantity                                                                         
                                                   startDate:now                                                                           
                                                   endDate:now];
    
    //存取該資料
    [self.healthStore saveObject:HRSample withCompletion:^(BOOL success, NSError *error) {
        if (!success) {
            NSLog(@"An error occured saving the sample %@. In your app, try to handle this gracefully. The error was: %@.", dataQuantityType, error);
            abort();
        }
        
        // 成功了之後，可以去更新資料．
        [self some_UI_update];
    }];

</pre>    

---
layout: post
layout: post
title: "[Mooc][Android]Programming Mobile Aplication on Android Platform(week8) - Data Management"
description: ""
category: 
- android安卓學習心得
- 網路課程筆記
tags: []

---

![image](http://www.evanlin.com/images/2014/Android_ICON.jpg)

**前言:**

最後一週，分成兩個一個是Assignment．另外還有Peer Assignment，要努力堅持到最後．


**筆記：**

關於資料管理的部分:

- SharePreferences 屬於小量的資料存取，(iOS 使用的是NSUserDefault 與 pList來管理)
    - 讀取資料getInt getString  
    - 寫入資料必須先取得SharedPreferences.Editor 之後再setInt or setString
- PreferenceFragment 是一個UI Fragment可以存取與直接收到SharedPreferences 的變化．
    - 透過setContentView可以把一個表示資料與介面的xml檔案設定好，然後存取它所代表的資料．     
- File 可以使用 internal memory 或是external memory    
    - 其中比較需要注意的是MODE_PRIVATE會把檔案鎖定成這個App才能讀取或是只用同樣的user ID跟App．
    - 對於external storage需要注意以下的事項：
        - 對於external memory 的檔案需要在manifest上面增加相關的權限:
            - <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" >
    
        - 存取前要先注意是否有讀或寫的權限:
            - Environment
				.getExternalStorageState() 
- SQL (SQLlite)
    - 如果要繼承SQLiteOpenHelper來使用，必須要Override OnCreate跟OnUpgrade．
    - 資料庫的檔案在 /data/data/<package name>/databases/
    - 透過以下方式可以讀取SQL資料
        - adb -s emulator-5554 shell
        - sqlite3 /data/data
    -        
    
<pre class="prettyprint">
// SharedPreferences

final SharedPreferences prefs = getPreferences(MODE_PRIVATE);
//get data from SharedPreferences
int high_score = prefs.getInt("high_score", 0);

//set data to SharedPreferences
SharedPreferences.Editor editor = prefs.edit();
editor.putInt("high_score", 99);
editor.commit();

//File read sample (internal storage)
FileInputStream fis = openFileInput(fileName);
BufferedReader br = new BufferedReader(new InputStreamReader(fis));

String line = "";

while (null != (line = br.readLine())) {

	TextView tv = new TextView(this);
	tv.setTextSize(24);
	tv.setText(line);

	ll.addView(tv);

}
br.close();

//File write
FileOutputStream fos = openFileOutput(fileName, MODE_PRIVATE);

PrintWriter pw = new PrintWriter(new BufferedWriter(
		new OutputStreamWriter(fos)));

pw.println("Line 1: This is a test of the File Writing API");
pw.println("Line 2: This is a test of the File Writing API");
pw.println("Line 3: This is a test of the File Writing API");

pw.close();


//File read sample (external storage)
if (Environment.MEDIA_MOUNTED.equals(Environment
		.getExternalStorageState())) {

	File outFile = new File(
			getExternalFilesDir(Environment.DIRECTORY_PICTURES),
			fileName);
	
	if (!outFile.exists())
		copyImageToMemory(outFile);
	
	ImageView imageview = (ImageView) findViewById(R.id.image);
	imageview.setImageURI(Uri.parse("file://" + outFile.getAbsolutePath()));
		}
</pre>


關於內容提供者的部分(Content Provider)

- Content Provider提供可以存取有結構的資料，可以跨app來存取．
- Content Resolver可以讓你去存取content provider裡面的資料．其中透過URI來傳遞與定位資料的相關資訊．

<pre class="prettyprint">
// Sample to using ContentResolver and URI
// Contact data
String columnsToExtract[] = new String[] { Contacts._ID,
		Contacts.DISPLAY_NAME, Contacts.PHOTO_THUMBNAIL_URI };

// Get the ContentResolver
ContentResolver contentResolver = getContentResolver();

// filter contacts with empty names
String whereClause = "((" + Contacts.DISPLAY_NAME + " NOTNULL) AND ("
		+ Contacts.DISPLAY_NAME + " != '' ) AND (" + Contacts.STARRED
		+ "== 1))";

// sort by increasing ID
String sortOrder = Contacts._ID + " ASC";

// query contacts ContentProvider
Cursor cursor = contentResolver.query(Contacts.CONTENT_URI,
		columnsToExtract, whereClause, null, sortOrder);

// pass cursor to custom list adapter
setListAdapter(new ContactInfoListAdapter(this, R.layout.list_item,
		cursor, 0));
</pre>        
        
- 其中要利用content provider去變更你Google的通訊錄的範例需要注意以下項目:
    - 在manifest裡面，取得<uses-permission android:name="android.permission.GET_ACCOUNTS" />  
    - 透過Google Account Manager取得帳戶資訊來讀取資料．
    - 透過ContentProviderOperation 來新增一個系列的batch執行序列(batch operation)
    - 透過content resolver來執行執行序列 (getContentResolver().applyBatch())

<pre class="prettyprint">
// Get Account information
// Must have a Google account set up on your device
String mAccountList = AccountManager.get(this).getAccountsByType("com.google");
String mType = mAccountList[0].type;
String mName = mAccountList[0].name;

// Set up a batch operation on Contacts ContentProvider
ArrayList<ContentProviderOperation> batchOperation = new ArrayList<ContentProviderOperation>();

for (String name : mNames) {
int position = ops.size();

// First part of operation
batchOperation(ContentProviderOperation.newInsert(RawContacts.CONTENT_URI)
		.withValue(RawContacts.ACCOUNT_TYPE, mType)
		.withValue(RawContacts.ACCOUNT_NAME, mName)
		.withValue(Contacts.STARRED, 1).build());

// Second part of operation
batchOperation(ContentProviderOperation.newInsert(Data.CONTENT_URI)
		.withValueBackReference(Data.RAW_CONTACT_ID, position)
		.withValue(Data.MIMETYPE, StructuredName.CONTENT_ITEM_TYPE)
		.withValue(StructuredName.DISPLAY_NAME, name).build());
}

try {

	// Apply all batched operations
	getContentResolver().applyBatch(ContactsContract.AUTHORITY,
			batchOperation);

} catch (RemoteException e) {
	Log.i(TAG, "RemoteException");
} catch (OperationApplicationException e) {
	Log.i(TAG, "RemoteException");
}
</pre>

- 剛剛關於content provider的範例都是使用已經架設的好的content provider．如果要新增自己的個人的content provider需要透過以下的流程:
    - 新增相關權限在 mannifest裡面: 新增 < provider /> 在< application /> 內
    - 建立 ContentProvider 衍生類別
    - 設定好唯一的URI之後，就可以透過這個URI使用ContentResolver來取得資訊．


關於Service的部分:

- 關於Service類別:
    - 沒有使用者介面
    - 兩個主要用途
        - 可以執行背景程序     
        - 跨App間的互動部分
- 關於IntentService
    - 將Service像是Intent一樣使用，建立後會在App背景等待呼叫
    - 透過Intent 來設定，但是透過startService而不是startActivity來啟動
- 關於與遠端的service溝通方式:
    - 使用messenger
        - 透過messanger來與service傳遞與接受訊息
    - 使用AIDL (Android Interface Definition Language)
        - 先宣告interface在 .aidl file
        - 不論是service與user都需要有建立 .aidl 檔案
        - 在Server端:
            - 需要建立與stub相關的API
        - 在User端:
            - 在ServiceConnection取得Service stub本體
            - 在App使用stub本身去呼叫他的API
            - 結束的時候必須要使用onServiceDisconnected清除stub    
    - Android team有建議要小心使用AIDL如果是在multiple thread的部分下面，或許可以考慮使用Messenger[http://developer.android.com/guide/components/bound-services.html](http://developer.android.com/guide/components/bound-services.html)            

<pre class="prettyprint">

// Intent Service sampl
public class LoggingService extends IntentService {

	public static String EXTRA_LOG = "course.examples.Services.Logging.MESSAGE";
	private static final String TAG = "LoggingService";

	public LoggingService() {
		super(TAG);
	}

	@Override
	protected void onHandleIntent(Intent intent) {

		Log.i(TAG, intent.getStringExtra(EXTRA_LOG));

	}
}


// 設定Intent
Intent startServiceIntent = new Intent(getApplicationContext(),
		LoggingService.class);

// 把需要log的資訊透過 putExtra 傳給Intent
startServiceIntent.putExtra(LoggingService.EXTRA_LOG,
		"Log this message");

// 啟動service
startService(startServiceIntent);

</pre>    
    

**關於作業:(2014/11/22更新)**

- Cursor 做任何讀取的時候，需要知道他是不是empty
    - Cursor.moveToFirst 這時候才會傳回 cursor是不是 empty 直接檢查指標是不是null 是沒有用的
    - 參考: [http://developer.android.com/reference/android/database/Cursor.html](http://developer.android.com/reference/android/database/Cursor.html)
- 這次官方給的作業有bug，需要改manifest才會出現 action bar menu
    - 修改:
        - <activity android:theme="@style/Theme.AppCompat.Light" ... > 在Manifest
        - 這個讓我找了好久..... 
    - 參考: [http://developer.android.com/guide/topics/ui/actionbar.html](http://developer.android.com/guide/topics/ui/actionbar.html)                       
- 其實最困難的部分就是swapcursor的部分(笑)，我想是因為註解太多敘述了，其實就專心地把swapcursor該做的做就好．陷阱題.......      
         
        
**參考網址:**

- Android Storage Option(包含 SharedPreferences，Internal Storage，External Storage跟SQl)
    - [http://developer.android.com/guide/topics/data/data-storage.html](http://developer.android.com/guide/topics/data/data-storage.html) 
    
- Android about bound services
    - [http://developer.android.com/guide/components/bound-services.html](http://developer.android.com/guide/components/bound-services.html)     

- Android how to use cursor
    - [http://developer.android.com/reference/android/database/Cursor.html](http://developer.android.com/reference/android/database/Cursor.html) 
- Android about action bar menu
    - [http://developer.android.com/guide/topics/ui/actionbar.html](http://developer.android.com/guide/topics/ui/actionbar.html) 

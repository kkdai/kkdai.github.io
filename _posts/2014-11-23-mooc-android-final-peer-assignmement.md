---
layout: post
layout: post
title: "[Mooc][Android]Programming Mobile Aplication on Android Platform -Final Peer Assignmement"
description: ""
category: 
- android安卓學習心得
- 網路課程筆記
tags: []

---

![image](http://www.evanlin.com/images/2014/Android_ICON.jpg)

**前言:**

最後一個作業是要做出一個自拍管理的App透過Content Provider來管理資料，還附有通知．從無到有其實很有趣的．我相信也能學到更多的東西，我希望能夠全部寫完，加油了．


**筆記:**

- 關於使用App Action Bar會強制請你加入Android-support-V7的project．
    - 如果你是要讓Android V7~V10的App支援Action Bar，你必須要新增Android-support-V7的project並且新增相依專案．但是如果希望新增一個然後不要有這些的相依性問題的話，你需要利用以下設定建立．(同時符合大部分作業的需求在V18上)
        - Minimal Requirement V17
        - Target V19
        - Compitible with V18                 
    - 參考文章:
        - [http://oldgrayduck.blogspot.tw/2013/10/android-support-library-actionbar.html](http://oldgrayduck.blogspot.tw/2013/10/android-support-library-actionbar.html)
- 如果要找default camera  在action menu的icon
    - 其實Android SDK裡面就有，在ANDROID_SDK/platforms/android-XX/data/res/drawable-XXX
    - 參考: [http://stackoverflow.com/questions/8179653/where-can-i-find-standards-icons-for-actionbar](http://stackoverflow.com/questions/8179653/where-can-i-find-standards-icons-for-actionbar)
    
- 裡面主要透過Content Provider 的方式來達到資料的存取，並且增加了Notification的功能．
    - Alarm Manager 透過他呼叫起一個Intent然後來設定Notification．透過這個方式來達到，離開App之後，可以在固定時間後收到通知．

- 關於讀取camera所拍照回來的照片檔案位址:
    - Android無法直接取得相簿裡面的位址，比較容易的方法是在透過intent設定camera 的時候先設定好相關的參數．
    - 光是那個參數設定了好久才能正常把相片另存地方．getBaseContext().getFilesDir()這個位址會讓Intent回不來．    
    - 參考[http://stackoverflow.com/questions/15009581/camera-intent-picture-file-path](http://stackoverflow.com/questions/15009581/camera-intent-picture-file-path)

<pre class="prettyprint">
//指定時間作為檔名來另存新檔
PackageManager pm = getPackageManager();
if (pm.hasSystemFeature(PackageManager.FEATURE_CAMERA)) {
	Intent intent_camera = new Intent(android.provider.MediaStore.ACTION_IMAGE_CAPTURE);
	
	SimpleDateFormat dateFormat = new SimpleDateFormat("yyyyMMdd_HHmmss");
    String currentTimeStamp = dateFormat.format(new Date());
    File mFile = new File(Environment.getExternalStorageDirectory(), currentTimeStamp+".jpg");
    Uri outputFileUri = Uri.fromFile(mFile);
	intent_camera.putExtra(android.provider.MediaStore.EXTRA_OUTPUT, outputFileUri);
	Toast.makeText(getBaseContext(), "File URI is "+outputFileUri, Toast.LENGTH_LONG).show();
	startActivityForResult(intent_camera, 0);
}
else {
	Toast.makeText(getBaseContext(), "Camera is not available", Toast.LENGTH_LONG).show();
}
</pre>    

- 讀取縮圖並且直接設定到ListView裡面：
    - 關於這部分的我把它想得太複雜了，網路上不少作法推薦[直接把檔案放進content provider](http://dharmendra4android.blogspot.tw/2012/04/save-captured-image-to-applications.htm)裡面，但是這樣沒有辦法存取多個資料．
    - 最後其實就直接記住檔名（這裡檔名使用當下的時間，最後把圖片放進去BitmapDrawable就可以．
<pre class="prettyprint">
// 從content provide拿取檔案名稱
String photoContentUri = cursor.getString(cursor.getColumnIndex(DataContract.PIC_FILE_NAME));
// 與系統位置配對成完整檔案位置
File mFile = new File(Environment.getExternalStorageDirectory(), photoContentUri);

if (null != photoContentUri) {
    InputStream input = null;
    try {
    // 讀取縮圖
    	input = context.getContentResolver().openInputStream(Uri.fromFile(mFile));
    	if (input != null) {
    		photoBitmap = new BitmapDrawable(mApplicationContext.getResources(), input);
    		photoBitmap.setBounds(0, 0, mBitmapSize, mBitmapSize);
    	}
    } catch (FileNotFoundException e) {
    	Log.e(TAG, "FileNotFoundException");
    }
}
</pre>    
 
- 由於點下去還要顯示完整大圖，這邊我也想得太複雜了．我以為還需要再增加一個新的activity，其實只要使用Intent就可以了
<pre class="prettyprint">
    //抓取圖片後，顯示新的Activity
    File mFile = new File(Environment.getExternalStorageDirectory(), fileName);
    Intent intent = new Intent();
    intent.setAction(android.content.Intent.ACTION_VIEW);
    intent.setDataAndType(Uri.fromFile(mFile), "image/jpg");
    startActivity(intent);
</pre>

**參考鏈結:**

- Android Support V7 in Action Bar.
    - [http://oldgrayduck.blogspot.tw/2013/10/android-support-library-actionbar.html](http://oldgrayduck.blogspot.tw/2013/10/android-support-library-actionbar.html)
- Where to find default camera icon in Andorid SDK.
    - [http://stackoverflow.com/questions/8179653/where-can-i-find-standards-icons-for-actionbar](http://stackoverflow.com/questions/8179653/where-can-i-find-standards-icons-for-actionbar)
- 關於儲存文件的Content Provider
    - [http://ipjmc.iteye.com/blog/1447226](http://ipjmc.iteye.com/blog/1447226)
- Save capture image in Android
    - [http://dharmendra4android.blogspot.tw/2012/04/save-captured-image-to-applications.html](http://dharmendra4android.blogspot.tw/2012/04/save-captured-image-to-applications.html)
                  

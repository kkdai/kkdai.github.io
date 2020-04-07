---
layout: post
layout: post
author: kkdai
comments: true
date: 2014-02-25 01:15:31+00:00
slug: android-%e9%97%9c%e6%96%bc%e7%9b%b8%e6%a9%9f%e7%a8%8b%e5%bc%8f%e7%9a%84%e5%ad%b8%e7%bf%92%e6%95%b4%e7%90%861
title: '[Android] 關於相機程式的學習整理(1)'
wordpress_id: 1336
categories:
- Android安卓學習心得
- 學習文件
---

這裡整理了一些關於在Android寫相機App會產生的一些疑問，並且把自己寫好的程式整理出來放在Github上面．  
這裡產生的相機功能主要如下:






  * 利用 Camera class產生相機的控制部分


  * 預覽的視窗


  * 拍照後可以讓Android系統預設的相簿觀看 




**讓相機產生preview**




1.將需要授權的相關部分放在”AndroidManifest.xml"




<uses-permissionandroid:name="android.permission.CAMERA"/>




<uses-featureandroid:name="android.hardware.camera.autofocus"/>




<uses-featureandroid:name="android.hardware.camera"android:required="false"/>




<uses-permissionandroid:name="android.permission.WRITE_EXTERNAL_STORAGE"/>




2. 把需要的元件畫出來，需要一個SurfaceView跟一個Button，修改reslayoutactivity_main.xml




    <SurfaceView




        android:id="@+id/surfaceView1"




        android:layout_width="fill_parent"




        android:layout_height="fill_parent"




        android:layout_alignParentBottom="true"




        android:layout_alignParentLeft="true"




        android:layout_alignParentRight="true">




    </SurfaceView>




 




    <Button




        android:id="@+id/btn_capture"




        android:layout_width="wrap_content"




        android:layout_height="wrap_content"




        android:layout_alignRight="@+id/surfaceView1"




        android:layout_below="@+id/textView1"




        android:layout_marginRight="18dp"




        android:layout_marginTop="144dp"




        android:text="Capture"/>




3. 讓preview可以動作，這裡的code比較多，儘量寫清楚點




3.1 建立 相關原件




//Camera object




Camera mCamera;




//Preview surface




SurfaceView surfaceView;




//Preview surface handle for callback




SurfaceHolder surfaceHolder;




//Camera button




Button btnCapture;




//Note if preview windows is on.




booleanpreviewing;




3.2 連接原件修改原來的 OnCreate




@Override




  protected void onCreate(Bundle savedInstanceState) {




  super.onCreate(savedInstanceState);




  setContentView(R.layout.activity_main);




 




  btnCapture = (Button) findViewById(R.id.btn_capture);




 




  surfaceView = (SurfaceView) findViewById(R.id.surfaceView1);




  surfaceHolder = surfaceView.getHolder();




 




  surfaceHolder.addCallback(new SurfaceViewCallback());




  surfaceHolder.setType(SurfaceHolder.SURFACE_TYPE_PUSH_BUFFERS);




}




3.3 增加surface callback class讓surface自己來處理相關預覽的部分




private final class SurfaceViewCallback implements android.view.SurfaceHolder.Callback {   




public void surfaceChanged(SurfaceHolder arg0, int arg1, int arg2, int arg3) 




{




  if (previewing) {




    mCamera.stopPreview();




    previewing = false;




  }




 




  try {




    mCamera.setPreviewDisplay(arg0);




    mCamera.startPreview();




    previewing = true;




   } catch (Exception e) {}




}




  





public void surfaceCreated(SurfaceHolder holder) {




   mCamera = Camera.open();




   // get Camera parameters




   Camera.Parameters params = mCamera.getParameters();




 




   List<String> focusModes = params.getSupportedFocusModes();




   if (focusModes.contains(Camera.Parameters.FOCUS_MODE_AUTO)) {




    // Autofocus mode is supported




  }




}




 




public void surfaceDestroyed(SurfaceHolder holder) {




  mCamera.stopPreview();




  mCamera.release();




  mCamera = null;




  previewing = false;




  }




}




 




4. 這樣就可以看到完整的camera preview，接下來就要implement 拍照後的一些處理




4.1 實作出拍照的功能




  btnCapture.setOnClickListener(new Button.OnClickListener() {




     public void onClick(View arg0) {




     if (previewing)




         mCamera.takePicture(shutterCallback, rawPictureCallback,jpegPictureCallback);




   }




  });




4.2 實作camera.takePicutre 的callback function




 先接起來button click listener




btnCapture.setOnClickListener(new Button.OnClickListener() {




  public void onClick(View arg0) {




  if (previewing)




    mCamera.takePicture(shutterCallback, rawPictureCallback, jpegPictureCallback);




  }




});




要把相關的 shutterCallback, rawPictureCallback加進去，最後記得要把jpegPictureCallback 弄好




ShutterCallback shutterCallback = new ShutterCallback() {




     @Override




     public void onShutter() {




   }




};




 




PictureCallback rawPictureCallback = new PictureCallback() {




     @Override




     public void onPictureTaken(byte[] arg0, Camera arg1) {




     }




};




這裡要注意，在jpegPictureCallback 中有幾件重要的事情






  * 將抓下來的部分存成檔案



    * 檔案目錄與檔名的選取，記得使用Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DCIM)


    * 在檔案的存取方面，使用的是BufferedOutputStream來寫檔案～這裡大部分網路例子會先轉成 Bitmap 然後再轉成JPG，這裡要改



  * 利用 MediaScannerConnection 去連接MediaStore 來把你拍下來的照片通知給系統，讓系統的相簿可以抓到你的相片




 這部分的code 如下




PictureCallback jpegPictureCallback = new PictureCallback() {




@Override




   public void onPictureTaken(byte[] arg0, Camera arg1) {




      String fileName = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DCIM).toString()




                        + File.separator




                        + "PicTest_" + System.currentTimeMillis() + ".jpg";




      File file = new File(fileName);




      if (!file.getParentFile().exists()) {




          file.getParentFile().mkdir();




       }




 




      try {




             BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(file));




             bos.write(arg0);




             bos.flush();




             bos.close();




             scanFileToPhotoAlbum(file.getAbsolutePath());




             Toast.makeText(MainActivity.this, "[Test] Photo take and store in" + file.toString(),Toast.LENGTH_LONG).show();




        } catch (Exception e) {




                Toast.makeText(MainActivity.this, "Picture Failed" + e.toString(),Toast.LENGTH_LONG).show();




        }




    };




};




 




public void scanFileToPhotoAlbum(String path) {




 




        MediaScannerConnection.scanFile(MainActivity.this,




                new String[] { path }, null,




                new MediaScannerConnection.OnScanCompletedListener() {




 




                    public void onScanCompleted(String path, Uri uri) {




                        Log.i("TAG", "Finished scanning " + path);




                    }




                });




這樣就完成了，完整的程式放在[Github   ](https://github.com/kkdai/Android_Camera_Sample)




[https://github.com/kkdai/Android_Camera_Sample](https://github.com/kkdai/Android_Camera_Sample)

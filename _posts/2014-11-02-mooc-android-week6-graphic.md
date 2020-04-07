---
layout: post
layout: post
title: "[Mooc][Android]Programming Mobile Aplication on Android Platform(week6) - Graphic/Multiple Touch/Multiple Media"
description: ""
category: 
- android安卓學習心得
- 網路課程筆記
tags: []

---


![image](http://www.evanlin.com/images/2014/Android_ICON.jpg)

**前言：**

到了第六週，也只剩下三週，要好好努力了． 感覺這個禮拜的內容有點多，看來得仔細把筆記做一下，也希望都能熟悉各個元件的用法．


**筆記:**

- Graphic 有兩種畫的方式:
    - Draw on canvas: 比較複雜，需要變動的繪圖．
    - Draw on view:   比較簡單，不需要變動的部分．
- Draw on View:
    - 可以有兩種方式來達成，將所有需要的外觀用xml來敘述，或是寫在程式碼裡面的MainActivity的OnCreate也是可以．
    - xml 可以是引用其他的xml，比如說顏色敘述可以是另外一個xml
<pre class="prettyprint">
// In  main.xml
        android:background="@drawable/sq2"

// In drawable/sq2.xml
< ?xml version="1.0" encoding="utf-8"?>
< shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle" >
    < solid android:color="#FFFFFF00" />
    < stroke
        android:width="25dp"
        android:color="#FFFF0000" />
< /shape>
</pre>          

- Draw on Canvas:
    - 透過View 來達成: (比較少更新)
        - 透過Canvas 提供的View 來使用
        - 透過 OnDraw 來更新畫面
    - 透過SurfaceView 來達成： (需要常更新)
        - 具有比較好的效能來更新UI
        - 不是屬於UI Drawing Thread來更新畫面．
        - 更新方式: (透過Surface callback)
            - surfaceCreated()
            - surfaceChanged()
            - sufaceDestroyed()
        - 使用流程:
            - SufaceHolder.lockCanvas() 先把UI resource 鎖定
            - 透過類似CCanvas.drawBitmap 來更新畫面
            - SufaceHolder.unlockCanvasAndPost() 來歸還UI resource           
- View Animation:
    - TransitionDrawable:
        - 用於兩個Drawable資源的切換，不過僅支援fade in/out的過場特效
    - Animation:
        - 可以做許多不同的Animation，比如說Alpha 變換（也就是fade in/out) 或是 rotate ， translate與 scale(大小)．
    - ValueAnimator:
        - 可以透過onAnimationUpdate callback 來調整每個animation 要改變的數值
    - ViewPropertyAnimator:
        - 每個view本身都有一個 .animate() 的ViewPropertyAnimator可以取用
        - 可以透過 Runnable去串接下一個Runnable來達成連續Animation的效果
             

<pre class="prettyprint">
//  TransitionDrawable 
Drawable[] layers = new Drawable[2]; //Two drawable in list
layers[0] = new ColorDrawable(Color.TRANSPARENT);
layers[1] = new BitmapDrawable(bitmap);
TransitionDrawable drawable = new TransitionDrawable(layers);

image.setImageDrawable(drawable);

drawable.startTransition(300);

 
// Animation java code
    // Create animation
    mAnim = AnimationUtils.loadAnimation(this, R.anim.view_animation);
    // Set Animation in ImageView when get focus
    mImageView.startAnimation(mAnim);


// Animation xml
    < alpha   //fade in/out
        android:duration="3000"
        android:fromAlpha="0.0"
        android:interpolator="@android:anim/linear_interpolator"
        android:toAlpha="1.0" />

    < rotate //rotate
        android:duration="4000"
        android:fromDegrees="0"
        android:interpolator="@android:anim/accelerate_interpolator"
        android:pivotX="50%"
        android:pivotY="50%"
        android:startOffset="3000"
        android:toDegrees="720" />

// View PropertyAnimation
    fadeIn.run();  // Run first runnable

	Runnable fadeIn = new Runnable() {
		public void run() {
			mImageView.animate().setDuration(3000)
					.setInterpolator(new LinearInterpolator()).alpha(1.0f)
					.withEndAction(rotate);
					//Call next runnable rotate.
		}
	};

</pre>           

- Multiple Touch Event:
    - OnTouch event handle MotionEvent:
        - Pointer ID: 代表著每一個觸碰的點．當有multiple touch發生的時候，Pointer ID 就有複數個．
        - MotionEvent.ACTION_MOVE.ACTION_POINTER_UP:  也是可能有多個pointer ID依序傳入．
        - MotionEvent.ACTION_MOVE: 是一次傳入一個群組的 id，你需要一個個去拿來處理．
    - OnTouch event handle by GestureDetector:
        - GestureDetector可以幫你判別是何種手勢，並且依據支援的手勢來呼叫
        - 如果需要增加新的手勢，需要使用GestureBuilder並且把檔案複製到App
            - 檔案從 /mnt/sdcard/gestures
            - 到 /res/raw 目錄夾
    - 客製化手勢使用流程:
        - 先使用 GesutureLibraries 把客製化的手勢讀進來
        - 輸入新的手勢之後，會得到一個預測的手勢陣列．取出分數最高的來處理
        
                
                                 
<pre class="prettyprint">
/*
 *  Multiple touch handle by MotionEvent
 */
 
// Init frame layout object 
FrameLayout mFrame = (FrameLayout) findViewById(R.id.frame);

// Create and set on touch listener
mFrame.setOnTouchListener(new OnTouchListener() {

	@Override
	public boolean onTouch(View v, MotionEvent event) {

		switch (event.getActionMasked()) {

		// Show new MarkerView
		
		case MotionEvent.ACTION_DOWN:
		case MotionEvent.ACTION_POINTER_DOWN: {

			int pointerIndex = event.getActionIndex();
			int pointerID = event.getPointerId(pointerIndex);
            // Each pointerID represent one finger movement. ex: 2 fingers will call twice in this function.
            
            ...  // Do something about handle in each finger point down.
            
			break;
		}


        // Handle move as group.
        case MotionEvent.ACTION_MOVE: {
        
        	for (int idx = 0; idx < event.getPointerCount(); idx++) {
        
        		int ID = event.getPointerId(idx);
        
        		MarkerView marker = mActiveMarkers.get(ID);
        		if (null != marker) {
        			// Do something with marker
        			}
        		}
        	}
        
        	break;
        }


/*
* Multiple touch handle by Gestture Detector and Custom Gestures
*/

// Load custom gestures
    GestureLibrary mLibrary = GestureLibraries.fromRawResource(this, R.raw.gestures);
    if (!mLibrary.load()) {
    	finish();
    }
	public void onGesturePerformed(GestureOverlayView overlay, Gesture gesture) {

		// Get gesture predictions
		ArrayList<Prediction> predictions = mLibrary.recognize(gesture);

		// Get highest-ranked prediction
		if (predictions.size() > 0) {
			Prediction prediction = predictions.get(0);

			// Ignore weak predictions

			if (prediction.score > 2.0) { //limited prediction score must higher 2.0
			if (prediction.name.equals("CUSTOM_GESTURE_ONE")) {
			        // Do something.

				}
			}

</pre>

- Multiple Media Player
    - 啟動MediaPlayer流程:
        - setDataSource()
        - prepare()
        - start() 
    - 啟動MediaRecorder流程 (以錄音為例):
        - setAudioSource(MediaRecorder.AudioSource.MIC)
        - setOutputFormat(MediaRecorder.OutputFormat.XXX)
        - setOutputFile()
        - setAudioEncoder(MediaRecorder.AudioEncoder.XXX)
        - prepare()
        - start()       

<pre class="prettyprint">
// Start recording with MediaRecorder
private void startRecording() {

	mRecorder = new MediaRecorder();
	mRecorder.setAudioSource(MediaRecorder.AudioSource.MIC);
	mRecorder.setOutputFormat(MediaRecorder.OutputFormat.THREE_GPP);
	mRecorder.setOutputFile(mFileName);
	mRecorder.setAudioEncoder(MediaRecorder.AudioEncoder.AMR_NB);

	try {
		mRecorder.prepare();
	} catch (IOException e) {
		Log.e(TAG, "Couldn't prepare and start MediaRecorder");
	}

	mRecorder.start();
}


// Playback audio using MediaPlayer
private void startPlaying() {

	mPlayer = new MediaPlayer();
	try {
		mPlayer.setDataSource(mFileName);
		mPlayer.prepare();
		mPlayer.start();
	} catch (IOException e) {
		Log.e(TAG, "Couldn't prepare and start MediaPlayer");
	}

}
</pre>

**參考資源:**

- Android中圖片璇轉與縮放
    - [http://fecbob.pixnet.net/blog/post/36421426-android%E4%B8%AD%E5%AF%A6%E7%8F%BE%E5%9C%96%E7%89%87%E5%8F%8A%E5%8B%95%E7%95%AB%E7%9A%84%E7%B8%AE%E6%94%BE%E5%92%8C%E6%97%8B%E8%BD%89(%E8%BD%89)](http://fecbob.pixnet.net/blog/post/36421426-android%E4%B8%AD%E5%AF%A6%E7%8F%BE%E5%9C%96%E7%89%87%E5%8F%8A%E5%8B%95%E7%95%AB%E7%9A%84%E7%B8%AE%E6%94%BE%E5%92%8C%E6%97%8B%E8%BD%89(%E8%BD%89))
- About TransitionDrawable 
    - [http://jason1peng.blogspot.tw/2013/01/android-ui-1-cross-fading.html](http://jason1peng.blogspot.tw/2013/01/android-ui-1-cross-fading.html)
- TransitionDrawable fade in/out
    - [http://givemepass.blogspot.tw/2012/03/xml.html](http://givemepass.blogspot.tw/2012/03/xml.html)
               

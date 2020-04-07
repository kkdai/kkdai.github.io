---
layout: post
layout: post
title: "[Mooc][Android]Programming Mobile Aplication on Android Platform(week7) - Sensor/Location/Map"
description: ""
category: 
- android安卓學習心得
- 網路課程筆記
tags: []

---


![image](http://www.evanlin.com/images/2014/Android_ICON.jpg)

**前言:**

最近開始比較忙碌了，也希望趕快把兩個課程結束．不然每次都得半夜熬夜想作業．尤其Python作業還搞到週末才弄完．


**筆記：**

- 關於Sensor的部分：
    - 有各種的Sensor可以使用(動態(motion)，動作(action)，還有環境(environment))．
    - 使用： 
        - 透過 getSystemService(context.SENSOR_SERVICE)
        - 夠過註冊onSensorChanged() 與 onAccuracyChanged()來接收變更．
        - 如果發現精準度(Accuracy)增加了，對於這樣得資料改變需要處理        
        - 關於座標系統(Coordinate System):
            - X: 左到右
            - Y: 下到上
            - Z: 前面到後面      
            - **手機的橫放(landscape)或是直放(potrait)不會影嚮座標系統**
        - 關於動態資訊與磁力座標(magnetic)資訊處理:
            - getRotationMatrix: 先透過手機的資訊轉換實際世界的資訊，主要是往北方的資訊
            - getOrientation: 拿到手機orientation資訊，裡面資訊是弧度(radians)
            - 轉換弧度(radians)到角度(degree)

<pre class="prettyprint">
@Override
	public void onSensorChanged(SensorEvent event) {

		// Acquire accelerometer event data
		
		if (event.sensor.getType() == Sensor.TYPE_ACCELEROMETER) {

			mGravity = new float[3];
			System.arraycopy(event.values, 0, mGravity, 0, 3);

		} 
		
		// Acquire magnetometer event data
		
		else if (event.sensor.getType() == Sensor.TYPE_MAGNETIC_FIELD) {

			mGeomagnetic = new float[3];
			System.arraycopy(event.values, 0, mGeomagnetic, 0, 3);

		}

		// If we have readings from both sensors then
		// use the readings to compute the device's orientation
		// and then update the display.

		if (mGravity != null && mGeomagnetic != null) {

			float rotationMatrix[] = new float[9];

			// Users the accelerometer and magnetometer readings
			// to compute the device's rotation with respect to
			// a real world coordinate system

			boolean success = SensorManager.getRotationMatrix(rotationMatrix,
					null, mGravity, mGeomagnetic);

			if (success) {

				float orientationMatrix[] = new float[3];

				// Returns the device's orientation given
				// the rotationMatrix

				SensorManager.getOrientation(rotationMatrix, orientationMatrix);

				// Get the rotation, measured in radians, around the Z-axis
				// Note: This assumes the device is held flat and parallel
				// to the ground

				float rotationInRadians = orientationMatrix[0];

				// Convert from radians to degrees
				mRotationInDegress = Math.toDegrees(rotationInRadians);            
</pre>             
            
    

- 關於地理位置(location)方面
    - 需要先邀請使用者提供以下兩種權限：
        - ACCESS_COARSE_LOCATION:
            - 提供網路所提供的地理資訊(較不精確，省電) 
        - ACCESS_FINE_LOCATION
            - 提供GPS資訊以及網路所綜合出比較精確的資訊(精確，較耗電)    
    - GPS 提供者：
        - GPS sensor(比較準，但是比較慢)
        - 網路提供(比較快，但是比較不準)
        - Passive (不會一直可以使用)
    - 關於位置(location)的判斷程序:
        - 先判斷是否上一次判斷結果是不是最佳的位置 ( 根據位置與精確度)，其中精確度是不斷會改變的．
        - 透過精確度來判斷現在的地理資訊是不是最佳的位置．
        - 更新最佳的地理資訊

- 關於地圖方面：
    - 之前已經玩過Google Map API所以這裡不再提了... 

**關於作業:**

- 關於虛擬位置選項的開啟
    - 由於這次是要撰寫取得位置的App，程式中有使用虛擬位置(mockLocation)，一開始我發現這個部分會一直導致我的App Crash但是一直找不到原因(我使用Galaxy Nexus)．（就算什麼都不加也一樣)，最後才發現跟選項有關
    - 方法：
        - 實體手機選項 [設定]->[開發者人員選項]->[允許模擬位置] 打勾
        - 我發現使用[GenyMotion]裡面來模擬Galaxy Nexus 也是一樣，不啟動這個部分會有crash的錯誤．
        - [教學文章](http://www.android-hk.com/applications/fake-gps-location-apps/)
        - 還是聽不懂或是找不到嗎？ 這裏有[影片教你](https://www.youtube.com/watch?v=a_Hq3A-GTJs)
        


**參考資料：**

- Sensor Overview
    - [http://developer.android.com/guide/topics/sensors/sensors_overview.html](http://developer.android.com/guide/topics/sensors/sensors_overview.html)
- Sensor Manager
    - [http://developer.android.com/reference/android/hardware/SensorManager.html](http://developer.android.com/reference/android/hardware/SensorManager.html)
- Sensor Motion
    - [http://developer.android.com/guide/topics/sensors/sensors_motion.html](http://developer.android.com/guide/topics/sensors/sensors_motion.html)          
- Enable "Allow MockLocation"
    - [http://www.android-hk.com/applications/fake-gps-location-apps/](http://www.android-hk.com/applications/fake-gps-location-apps/)
- Youtube to teach you how to enable "Allow Mocklocation"             
    - [https://www.youtube.com/watch?v=a_Hq3A-GTJs](https://www.youtube.com/watch?v=a_Hq3A-GTJs) 

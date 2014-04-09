<a name="top" />
<img src="./Android/images/android-intro1.png" width="640px" /><br></br>

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - If you are using the "InApp Plus" SDK, you MUST integrate at least the Splash, the Slider and the Interstitial
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)

<br></br>
<a name="step1" />
##Step 1, Adding the SDK JAR to Your Eclipse Project
Copy the SDK jar file from the SDK zip to the “libs” directory of your project.

[Back to top](#top)

<a name="step2" />
##Step 2, Updating Your AndroidManifest.xml File
Under the main \<manifest\> element, add the following permissions:
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

//These permissions are only required for showing the ad when pressing the Home button
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
<uses-permission android:name="android.permission.GET_TASKS"/>
```

Under the \<application\> element, add your new activities:
```xml
<activity android:name="com.startapp.android.publish.list3d.List3DActivity"
          android:taskAffinity="package_name.AppWall"
          android:theme="@android:style/Theme" />

<activity android:name="com.startapp.android.publish.AppWallActivity"
          android:theme="@android:style/Theme.Translucent"
          android:taskAffinity="package_name.AppWall"   		  
          android:configChanges="orientation|keyboardHidden" />
```

> **NOTE:** Replace _**package_name**_ with your actual package name as declared in your manifest in both activities. For example, if your package name is "com.test.example", replace _**package_name**_ with "com.test.example.AppWall".


[Back to top](#top)

<a name="step3" />
##Step 3, Initialization
In your main activity, go to the ``OnCreate`` method and before calling ``setContentView()`` call the static functions:

```java
StartAppAd.init(this, "Your Developer Id", "Your App ID");
```

Replace __"Your Developer Id"__ and  __"Your App ID"__ with your own values provided in the [developers’ portal](http://developers.startapp.com).<br></br>
After logging in, your developer ID will be at the top right-hand corner of the page:
<img src="./Android/images/android-devId.png" />

To find your application ID, click on the <img src="./Android/images/dash2.jpg" align="middle"/> at the top of the main screen and then choose the relevant ID from your app list:<br></br>
<img src="./Android/images/android-appId.png" width="350px" />

[Back to top](#top)

<a name="step4" />
##Step 4, Showing Banners
Add the following View inside your Activity layout XML:
```java
<com.startapp.android.publish.banner.Banner 
          android:id="@+id/startAppBanner"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_centerHorizontal="true"/>
```

> **NOTE:**
> This code places a View inside your Activity. You also have the option to add additional attributes for placing it in the desired location in your Activity.

**If you wish to add a specific type of banner, please refer to the [Advanced Usage](android-advanced-usage)**.

[Back to top](#top)

<a name="step5" />
##Step 5, Showing Interstitial Ads
Interstitial Ads are displayed before or after a certain content page or action, such as upon entering a stage, between stages, while waiting for an action, upon exiting the application and more. 

####Initializing the StartApp Ad Object
In your Activity, create a member variable, as follows:
```java
private StartAppAd startAppAd = new StartAppAd(this);
```

Override the ```onResume()``` method and add the method ```startAppAd.onResume()``` AFTER the method ```super.onResume()```:
```java
@Override
public void onResume() {
    super.onResume();
    startAppAd.onResume();
}
```

####Showing Exit Ads
Add the following code to show an ad upon exiting your application.

To show an ad when pressing the 'Back' button, override the ```onBackPressed()``` method and add the method ```startAppAd.onBackPressed()``` BEFORE the method ```super.onBackPressed()```:
```java
@Override
public void onBackPressed() {
    startAppAd.onBackPressed();
    super.onBackPressed();
}
```

To show an ad when pressing the 'Home' button, override the ```onPause()``` method and add the method ```startAppAd.onPause()``` AFTER the method ```super.onPause()```:
```java
@Override
public void onPause() {
    super.onPause();
    startAppAd.onPause();
}
```

####Showing Interstitials
Add the following code to the appropriate place(s) in the activity in which you would like to show the Ad:
```java
startAppAd.showAd(); // show the ad
startAppAd.loadAd(); // load the next ad
```

> **NOTE:**
> ```loadAd()``` must be called immediately after ```showAd()```. This will load the next Ad.

The following is an example of showing an Interstitial Ad between Activities:
```java
public void btnOpenActivity (View view){
    startAppAd.showAd();
    startAppAd.loadAd();
    Intent nextActivity = new Intent(this, NextActivity.class);
    startActivity(nextActivity);
}
```

[Back to top](#top)

<a name="step6" />
##Step 6, Showing a Splash Ad
A Splash Ad is a full-page ad that is displayed immediately after the application is launched.
A Splash Ad first displays a full page splash screen that you define (as described below) followed by a full page ad. 
StartApp In-Ad provides two modes for displaying Splash screens:

**Splash Screen Mode** | **Description**
---------------------- | ---------------
Template Mode          | StartApp In-Ad provides a pre-defined template in which you can place your own creatives, such as application name, logo and loading animation.
User-Defined Mode      | Please refer to the [Advanced Usage](android-advanced-usage)

####Adding the Splash Screen 
In the ```OnCreate``` method of your Activity, after calling ```StartAppAd.init``` and before ```setContentView```, call the following static function:
```java
StartAppAd.showSplash(this, savedInstanceState);
```
Apply the following parameters:
- ***this***: The context (Activity)
- ***savedInstanceState***: The Bundle parameter passed to your ```onCreate(Bundle savedInstanceState)``` method

**If you wish to customize or use a different splash screen, please refer to the [Advanced Usage](android-advanced-usage).**

[Back to top](#top)

<a name="step7" />
##Step 7, Integrating the Slider
After calling ```setContentView()```, in the ```OnCreate()``` method of your main activity, call the static function:
```java
StartAppAd.showSlider(this);
```

If you would like the Slider to appear in additional activities, repeat this step in each one of the activities you would like it to show in. The Slider cannot be implemented in activities with a _Dialog Theme_: ```(android:theme="@android:style/Theme.Dialog")```

> **NOTE:**
> for better user experience, and in order to avoid reload of the Slider when rotating the phone, it is recommended to go back to your manifest file and add the following attribute to any \<activity\> element that you added the Slider to:  ```android:configChanges="orientation|screenSize"```

[Back to top](#top)

<a name="step8" />
##Step 8, Obfuscation (Optional)
Obfuscation protects an application from reverse-engineering or modification by making it harder for a third-party to access your source (decompiled) code.

**StartApp In-Ad is already obfuscated!** Therefore, if you did not obfuscate your application using ProGuard™, then you can skip this step. If you have obfuscated your application using ProGuard, then use the following in the ProGuard configuration file:
```
-keep class com.startapp.** {
      *;
}

-keepattributes Exceptions, InnerClasses, Signature, Deprecated, SourceFile,
LineNumberTable, *Annotation*, EnclosingMethod
-dontwarn android.webkit.JavascriptInterface
-dontwarn com.startapp.**
```

[Back to top](#top)

<a name="Native" />
##Native Ads
A "Native Ad" is a raw representation of an ad without any pre-defined wrapping UI, which gives you the freedom to design and control the ad exactly as you want. Using Native Ads, you can design an ad experience that perfectly fits your application's scene, content and functionality.

For a full integration guide, please refer to the ["Using Native Ads"](android-advanced-usage#using-native-ads) section under the "Advanced Usage" page.

[Back to top](#top)

<a name="Demographic" />
##Enjoy Higher eCPM with Demographic-Targeted Ads
If you know your user's gender or age, StartApp can use it to serve better-targeted ads which can increase your eCPM and revenue significantly.

**Example**
```java
@Override
public void onResume() {
    super.onResume();
    startAppAd.loadAd(new AdPreferences()
                      .setAge(18)
                      .setGender("Male"));
    startAppAd.onResume();
}
```
**1**	In your ``onResume()`` method, use the **AdPreferences** object instead of just calling ``startAppAd.onResume()`` as described above. Use ``setAge()`` with your user's real age, and ``setGender()`` with your user's real gender – *"Male"* or *"Female"*.

**2** 	Do the same for each ``loadAd()`` call in your project.

[Back to top](#top)

<a name="SampleProject" />
##Sample Project
StartApp provides a sample integration project available on [GitHub](https://github.com/StartApp-SDK/StartApp_InApp_SDK_Example)

[Back to top](#top)

<a name="AdvancedUsage" />
##Advanced Usage
For advanced usage, please refer to the ["Advanced Usage"](android-advanced-usage)

[Back to top](#top)

<a name="SearchBox-SDK-Removal-Procedure" />
##SearchBox SDK Removal Procedure
If you are upgrading from our old SearchBox SDK, please refer to the ["SearchBox SDK Removal Procedure"](SearchBox-SDK-Removal-Procedure).

[Back to top](#top)


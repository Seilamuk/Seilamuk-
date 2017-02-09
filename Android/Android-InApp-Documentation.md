<a name="top" />
**Last version: 3.5.5**  
<br></br>
<img src="./Android/images/important_note.png" hspace="18" /><br></br>
<img src="./Android/images/android-intro1.png" width="640px" /><br></br>

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - Please notice that steps 1-3 are mandatory
> - A basic video tutorial is available <a href="https://www.youtube.com/watch?v=PNpJdfB8mUQ" target="_blank">here</a>
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)

<br></br>
<a name="gettingStarted" />
##Getting Started 
> **IMPORTANT:** This is a mandatory step

<a name="AddingSDK" />
###Step 1, Adding the SDK to Your Project
Copy the StartAppInApp-x.x.x.jar file from the SDK zip to the “libs” directory of your project.

<a name="manifest" />
###Step 2, Updating Your AndroidManifest.xml File

<a name="Activities" />
Add the following permissions under the main _\<manifest\>_ element:  
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
<uses-permission android:name="android.permission.BLUETOOTH" />
```

> The last five permissions are optional, but highly recommended for better performance.  
> StartApp SDK doesn't request location updates proactively but only uses the last known location.  
 
<a name="Activities & Service" />
Add the following activites under the _\<application\>_ element:
```xml
<activity android:name="com.startapp.android.publish.ads.list3d.List3DActivity"
          android:theme="@android:style/Theme" />

<activity android:name="com.startapp.android.publish.adsCommon.activities.OverlayActivity"
          android:theme="@android:style/Theme.Translucent"
          android:configChanges="orientation|keyboardHidden|screenSize" />

<activity android:name="com.startapp.android.publish.adsCommon.activities.FullScreenActivity"
          android:theme="@android:style/Theme"
          android:configChanges="orientation|keyboardHidden|screenSize" />
```

Add the following service receiver under the _\<application\>_ element:
```xml
<service android:name="com.startapp.android.publish.common.metaData.PeriodicMetaDataService" />
<receiver android:name="com.startapp.android.publish.common.metaData.BootCompleteListener" >
	<intent-filter>
		<action android:name="android.intent.action.BOOT_COMPLETED" />
	</intent-filter>
</receiver>
```

<a name="Initialization" />
###Step 3, Initialization
In your main activity, go to the ``OnCreate`` method and before calling ``setContentView()`` call the static function:

```java
StartAppSDK.init(this, "Your App ID", true);
```

Replace __"Your App ID"__ with your own value provided in the [developers’ portal](https://portal.startapp.com/#/signin).<br></br>

To find your application ID, click on the "Apps and Sites" tab on the left pane and choose the relevant ID from your app list:<br></br>
<img src="./Android/images/android-appId.png" />

The last ``true`` parameter enables ["Return Ads"](#return). If you want to disable this feature, simply pass ``false`` instead.

Please notice - if you initialize the SDK in a service, you must do it on the service's main thread.    

[Back to top](#top)

<a name="splash" />
##Splash Ad (recommended)
> **StartApp Splash Ad is a top performing ad unit, presenting the industry's highest CPM's**  

A Splash Ad is a full-page ad that is displayed immediately after the application is launched. A Splash Ad first displays a full page splash screen that you define (as described below) followed by a full page ad.   

The Splash Ad is enabled by default. If you want to disable it simply call ```StartAppAd.disableSplash()``` after calling ```StartAppSDK.init```.

By default, your application will be using a pre-defined splash screen designed by StartApp. If you want to customize this screen or if you already have your own splash screen and want to use it, please refer to the [Advanced Usage](android-advanced-usage#CustomizingSplashScreen).

[Back to top](#top)

<a name="return" />
##Return Ad
The **Return Ad** is a new ad unit which is displayed once the user returns to your application after a certain period of time.  To minimize the intrusiveness, short time periods are ignored. For example, the Return Ad won't be displayed if the user leaves your application to take a short phone call before returning. 

Return ads are enabled and activated by default. If you want to disable this feature, simply pass "false" as the 3th parameter of the ``StartAppSDK.init`` method:
 ```java
StartAppSDK.init(this, "Your App ID", false);
```

[Back to top](#top)



<a name="interstitial" />
##Interstitial Ads
Interstitial Ads are full page ads, displayed before or after a certain content page or action, such as upon entering a stage, between stages, while waiting for an action, upon exiting the application and more. There are three ways of integrating Interstitial Ads:    
**Exit Ads** - show an ad upon exiting your application  
**Standard Interstitial Ads** - show an ad at a specific location(s) in your application   
**Autostitial Ads** - show an ad automatically between activities  

###Exit Ads
To show an ad upon exiting your application when pressing the 'Back' button, override the ```onBackPressed()``` method and add the method ```StartAppAd.onBackPressed(this)``` BEFORE the method ```super.onBackPressed()``` (```this``` is the activity/application context):
```java
@Override
public void onBackPressed() {
	StartAppAd.onBackPressed(this);
	super.onBackPressed();
}
```

###Standard Interstitials
Use this method to show an Interstitial Ad at a specific location inside your app.    
Call ```StartAppAd.showAd(this)``` in the appropriate place(s) in the activity where you would like to show the Ad. The ```showAd``` method returns true in case the ad was displayed successfully, or false if not (for example, if an ad isn't ready yet).  

The following is an example of showing an Interstitial Ad between Activities:
```java
public void btnOpenActivity (View view){
    Intent nextActivity = new Intent(this, NextActivity.class);
    startActivity(nextActivity);
    StartAppAd.showAd(this);
}
```

Please notice - If you want to use Autostitial Ads and yet to show an Interstitial in a specific location, call ```StartAppAd.disbleAutoInterstitial();``` before calling ```StartAppAd.showAd```, otherwise two ads might be displayed together. Remember to call ```StartAppAd.enableAutoInterstitial();``` afterthat to reanable Autostitial Ads. 

> **IMPORTANT:** Loading an ad might take a few seconds. In case you call showAd() while the ad hasn't been successfully loaded yet, nothing will be displayed. If you want to show an ad when your application is launched, use our ["Splash Ad"](#splash). You can also implement your interstitial ad as an object and use the "onReceiveAd" callback which is triggered when an ad was loaded and ready to use. See ["Interstitial Ads"](android-advanced-usage#InterstitialAsAnObject) under the "Advanced Usage" section.


###Autostitials
"Autostitial" stands for "Auto Interstitial"; use this integration to show an Interstitial Ad each time an activity is changed.  
Simply call ```StartAppAd.enableAutoInterstitial();``` after calling ```StartAppSDK.init```.    
You can gain more control over the frequency of Autostitial Ads using two methods: time frequency and activity frequency.  
####time frequency
You can set a minimum time interval between consecutive Autostitial Ads.   
For example, set a 1 minute interval between two consecutive ads (time in seconds):
```java
StartAppAd.setAutoInterstitialPreferences(
                  new AutoInterstitialPreferences()
                  .setSecondsBetweenAds(60)                  
           );
``` 
####activity frequency
You can set a minimum number of activities between consecutive Autostitial Ads.   
For example, show an Autostitial after each 3 activities:
```java
StartAppAd.setAutoInterstitialPreferences(
                  new AutoInterstitialPreferences()
                  .setActivitiesBetweenAds(3)                  
           );
```

Time frequency and activity frequency can be used together.  

[Back to top](#top)


<a name="banners" />
##Banners
Add the following View inside your Activity layout XML:
```java
<com.startapp.android.publish.ads.banner.Banner 
          android:id="@+id/startAppBanner"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_centerHorizontal="true"/>
```

> **NOTE:**
> This code places a View inside your Activity. You also have the option to add additional attributes for placing it in the desired location in your Activity.

For adding a banner programmatically, please refer to the [Advanced Usage](android-advanced-usage#AddBannerProgrammatically).

[Back to top](#top)

<a name="Native" />
##Native Ads
A "Native Ad" is a raw representation of an ad without any pre-defined wrapping UI, which gives you the freedom to design and control the ad exactly as you want. Using Native Ads, you can design an ad experience that perfectly fits your application's scene, content and functionality.

For a full integration guide, please refer to the ["Using Native Ads"](android-advanced-usage#using-native-ads) section under the ["Advanced Usage"](android-advanced-usage#using-native-ads) page.

<img src="./Android/images/native.jpg" width="640px" /><br></br>

[Back to top](#top)

<a name="obfuscation" />
##Obfuscation (Optional)
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
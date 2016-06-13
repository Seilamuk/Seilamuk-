<a name="top" />
**Last version: 3.4.2**  
<br></br>
<img src="./Android/images/important_note.png" hspace="18" /><br></br>
<img src="./Android/images/android-intro1.png" width="640px" /><br></br>

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - Please notice that steps 1-3 are mandatory
> - A basic video tutorial is available <a href="https://www.youtube.com/watch?v=PNpJdfB8mUQ" target="_blank">here</a>
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)

<br></br>
<a name="step1" />
##Step 1, Adding the SDK to Your Project
> **IMPORTANT:** This is a mandatory step

Copy the StartAppInApp-x.x.x.jar file from the SDK zip to the “libs” directory of your project.

[Back to top](#top)

<a name="step2" />
##Step 2, Updating Your AndroidManifest.xml File
> **IMPORTANT:** This is a mandatory step

####Permissions
Under the main \<manifest\> element, add the following permissions.

Mandatory Permissions:
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

Optional Permissions (allow StartApp to show higher eCPM Geo-targeted ads):
```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

> StartApp SDK doesn't request location updates proactively but only uses the last known location. 

<a name="Activities" />
####Activities
Under the \<application\> element, add the following activities:
```xml
<activity android:name="com.startapp.android.publish.list3d.List3DActivity"
          android:theme="@android:style/Theme" />

<activity android:name="com.startapp.android.publish.OverlayActivity"
          android:theme="@android:style/Theme.Translucent"
          android:configChanges="orientation|keyboardHidden|screenSize" />

<activity android:name="com.startapp.android.publish.FullScreenActivity"
          android:theme="@android:style/Theme"
          android:configChanges="orientation|keyboardHidden|screenSize" />
```

[Back to top](#top)

<a name="step3" />
##Step 3, Initialization
> **IMPORTANT:** 
> - This is a mandatory step

In your main activity, go to the ``OnCreate`` method and before calling ``setContentView()`` call the static function:

```java
StartAppSDK.init(this, "Your App ID", true);
```

Replace __"Your App ID"__ with your own value provided in the [developers’ portal](https://portal.startapp.com/#/signin).<br></br>

To find your application ID, click on the "Apps and Sites" tab on the left pane and choose the relevant ID from your app list:<br></br>
<img src="./Android/images/android-appId.png" />

The last ``true`` parameter enables the ["Return Ads"](#step4) feature as explained in the next section. If you want to disable this feature, simply pass ``false`` instead.

Please notice - if you initialize the SDK in a service, you must do it on the service's main thread.    

[Back to top](#top)

<a name="step4" />
##Return Ads
The **Return Ad** is a new ad unit which is displayed once the user returns to your application after a certain period of time.  To minimize the intrusiveness, short time periods are ignored. For example, the Return Ad won't be displayed if the user leaves your application to take a short phone call before returning. 

Return ads are enabled and activated by default. If you want to disable this feature, simply pass "false" as the 3th parameter of the ``StartAppSDK.init`` method:
 ```java
StartAppSDK.init(this, "Your App ID", false);
```

[Back to top](#top)

<a name="step5" />
##Showing Banners
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

<a name="step6" />
##Showing Interstitial Ads
Interstitial Ads are displayed before or after a certain content page or action, such as upon entering a stage, between stages, while waiting for an action, upon exiting the application and more. 

####Initializing the StartApp Ad Object
In your Activity, create a member variable, as follows:
```java
private StartAppAd startAppAd = new StartAppAd(this);
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
    Intent nextActivity = new Intent(this, NextActivity.class);
    startActivity(nextActivity);
    startAppAd.showAd();
    startAppAd.loadAd();
}
```

####Showing Rewarded Video Ads
Rewarded Ads are interstitial video ads that provide a reward to the user in exchange for watching an entire video ad. The reward might be in-app goods, virtual currency or any premium content provided by the application. Because users actually opt-in to watch a rewarded video and are granted with something valuable in return, Rewarded Ads are an effective and clean monetization solution for stronger user retention and keeping users engaged in your application for a longer amount of time.

> **IMPORTANT**    
> 1. Rewarded Video Ads are supported from API level 16 and above  
> 2. Please make sure to update your manifest file with the "FullScreenActivity" as described [here](#Activities)  

In order to show a Rewarded Ad, pass the following AdMode parameter when calling the ``loadAd()`` method:
```java
startAppAd.loadAd(AdMode.REWARDED_VIDEO);
```

Implement the following listener in order to get a callback when the user completes watching the video and is eligible for getting the reward:  
```java
startAppAd.setVideoListener(new VideoListener() {
     @Override
     public void onVideoCompleted() {
          // Grant user with the reward
     }
});
```

> **IMPORTANT:** Loading an ad might take a few seconds especially in the case of a video, so it's important not to show the ad immediately after loading it. In case you call showAd() while the ad hasn't been successfully loaded yet, nothing will be displayed. It is recommended to use the "onReceiveAd" callback which is triggered when an ad was loaded and ready to use (see [Adding a Callback when an Interstitial Ad is loaded](android-advanced-usage#adding-a-callback-when-an-interstitial-ad-is-loaded)).

[Back to top](#top)

<a name="step7" />
##Showing a Splash Ad (recommended)
A Splash Ad is a full-page ad that is displayed immediately after the application is launched.
A Splash Ad first displays a full page splash screen that you define (as described below) followed by a full page ad. 

> **StartApp Splash Ad is a top performing ad unit, presenting the industry's highest CPM's**

StartApp In-Ad provides two modes for displaying Splash screens:

**Splash Screen Mode** | **Description**
---------------------- | ---------------
Template Mode          | StartApp In-Ad provides a pre-defined template in which you can place your own creatives, such as application name, logo and loading animation.
User-Defined Mode      | Please refer to the [Advanced Usage](android-advanced-usage#CustomizingSplashScreen)

####Adding the Splash Screen 
In the ```OnCreate``` method of your Activity, after calling ```StartAppAd.init``` and before ```setContentView```, call the following static function:
```java
StartAppAd.showSplash(this, savedInstanceState);
```
Apply the following parameters:
- ***this***: The context (Activity)
- ***savedInstanceState***: The Bundle parameter passed to your ```onCreate(Bundle savedInstanceState)``` method

**If you wish to customize or use a different splash screen, please refer to the [Advanced Usage](android-advanced-usage#CustomizingSplashScreen).**

[Back to top](#top)


<a name="step9" />
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

<a name="Native" />
##Native Ads
A "Native Ad" is a raw representation of an ad without any pre-defined wrapping UI, which gives you the freedom to design and control the ad exactly as you want. Using Native Ads, you can design an ad experience that perfectly fits your application's scene, content and functionality.

For a full integration guide, please refer to the ["Using Native Ads"](android-advanced-usage#using-native-ads) section under the ["Advanced Usage"](android-advanced-usage#using-native-ads) page.

<img src="./Android/images/native.jpg" width="640px" /><br></br>

[Back to top](#top)

<a name="Demographic" />
##Enjoy Higher eCPM with Demographic-Targeted Ads
If you know your user's gender or age, StartApp can use it to serve better-targeted ads which can increase your eCPM and revenue significantly.

####Set Age and Gender
Upon initialization, after providing your DevId and AppId, pass the **SDKAdPreferences** object with its data:
```java
StartAppSDK.init(this,  
                 "Your App ID", 
                 new SDKAdPreferences()
                      .setAge(35)
                      .setGender(Gender.FEMALE));
}
```
+ ``setAge`` can take an integer.
+ ``setGender`` can take one of the following values: Gender.FEMALE or Gender.MALE.


####Set Location
If you included the ACCESS_COARSE_LOCATION or the ACCESS_FINE_LOCATION optional permissions, you can skip this step. 

The location of the user is a dynamic property which is changed constantly. Hence, you should provide it every time you load a new Ad:

```java
@Override
public void onResume() {
    super.onResume();
    startAppAd.loadAd(new AdPreferences()
                           .setLatitude(31.776719)
                           .setLongitude(35.234508));
    startAppAd.onResume();
}
```

In your ``onResume()`` method, use the **AdPreferences** object instead of just calling ``startAppAd.onResume()`` as described above. Do the same for each ``loadAd()`` call in your project.

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
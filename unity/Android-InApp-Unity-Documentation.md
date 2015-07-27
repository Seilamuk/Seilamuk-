<a name="top" />

**Latest version: 3.1.1**

<img src="./unity/images/unity-android-intro.png" width="640px" /><br></br>

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - Please notice that steps 1-3 are mandatory
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)

<br></br>
<a name="step1" />
##Step 1, Adding the SDK package to your Unity project
In order to add StartApp SDK to your application please follow the following steps:

**1.** Unzip the SDK files to a temporary folder<br></br>
**2.** Import _StartAppAndroidUnitySDK-x.x.x.unitypackage_ as a Custom Package (or drag and drop it to your "Assets" folder under the "Project" Window)

<img src="./unity/images/unity-android-import-package.png" />

**3.** Import all the package's items by pressing "All" and then "Import".   
<img src="./unity/images/unity-android-import-all.png" />

[Back to top](#top)


<br></br>
<a name="step2" />
##Step 2, Updating your Manifest File
Update the _manifest.xml_ (in the _Android_ folders) as follow:
  
**1.** Make sure the following activities are declared under the \<application\> element:

```xml
<activity android:name="com.startapp.android.publish.list3d.List3DActivity"
                        android:hardwareAccelerated="true"
                        android:theme="@android:style/Theme" />
<activity android:name="com.startapp.android.publish.OverlayActivity"
                        android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen"
                        android:hardwareAccelerated="true"
                        android:windowSoftInputMode="stateHidden"
                        android:configChanges="orientation|keyboardHidden|screenSize" />
<activity android:name="com.startapp.android.publish.FullScreenActivity"
                        android:hardwareAccelerated="true"
                        android:theme="@android:style/Theme"
                        android:configChanges="orientation|keyboardHidden|screenSize" />
```

**2.** Make sure the meta-data parameter named `unityplayer.ForwardNativeEventsToDalvik` is set to `true`:
```xml
<meta-data android:name="unityplayer.ForwardNativeEventsToDalvik" android:value="true" />
```

**3.** Add the following mandatory permissions
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

**4.** Optional - add the following permissions (allow StartApp to show higher eCPM Geo-targeted ads):
```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

[Back to top](#top)


<br></br>
<a name="step3" />
##Step 3, Updating your StartApp data file
Update the _StartAppData.txt_ (in the Assets/Resources folders) as follows:

**1.** Add your StartApp Account ID after ``accountId=`` <br></br>
**2.** Add your StartApp Application ID after ``applicationId=`` <br></br>

You can find your Account and Application IDs in the [developers’ portal](https://portal.startapp.com/#/signin).<br></br>
After logging in, your account ID will be displayed at the top right-hand corner of the page:
<img src="./Android/images/accountId.png" />

To find your application ID, click on the "Apps and Sites" tab on the left pane and choose the relevant ID from your app list:<br></br>
<img src="./Android/images/android-appId.png" />

[Back to top](#top)

<a name="splash" />
##Showing a Splash Ad (recommended)
A Splash Ad is a full-page ad that is displayed immediately after the application is launched.
A Splash Ad first displays a full page splash screen that you define (as described below) followed by a full page ad. 

> **StartApp Splash Ad is a top performing ad unit, presenting the industry's highest CPM's**  
> **supported for Unity v4.2 and above**    

####Adding the Splash Screen 
First, import the sdk namespace
```csharp
using StartApp;
```

Then, in the ``OnGUI()`` method of your script, call the following static function:
```csharp
void OnGUI () {
#if UNITY_ANDROID
StartAppWrapper.showSplash();
#endif
}
```

**If you wish to customize the splash screen, please refer to the [Advanced Usage](unity-android-advanced-usage#CustomizingSplashScreen).**

[Back to top](#top)


<a name="return" />
##Showing Return Ads
The **Return Ad** is a new ad unit which is displayed once the user returns to your application after a certain period of time.  To minimize the intrusiveness, short time periods are ignored. For example, the Return Ad won't be displayed if the user leaves your application to take a short phone call before returning. 

Return ads are enabled and activated by default. If you want to disable this feature, simply change "returnAds" value to "false" in your StartApp data file (see "Step 3" above):
```csharp
returnAds=false;
```

[Back to top](#top)


<br></br>
<a name="exit" />
##Adding an exit ad to your project

Use the *StartAppBackPlugin* in components where you would like the user to press 'back' to exit the application. The plugin will show an ad and then exit the application.

**1.** Drag the StartAppBackPlugin.cs to the components which you would like pressing 'back' to exit the application <br></br>
**2.** Do not implement exit on the 'back' in this components <br></br>

> **NOTE:** there is no need to implement exit on the 'back' button in these components as the plugin will exit the application after showing the ad.

[Back to top](#top)

<a name="interstitial" />
##Showing Interstitial Ads

You can choose to show the interstitial ad in several locations within your application.
This could be upon entering (``Start()``), between stages, while waiting for an action and more.

We do, however, recommend showing the ad upon exiting the application by using the ‘Back’ button, as explained in the previous section.

Add the following code to the appropriate place or places within your activities in which you would like to show the ad:

**1.** Import the sdk namespace
```csharp
using StartApp;
```

**2.** Load the ad
```csharp
void Start () {
#if UNITY_ANDROID
StartAppWrapper.loadAd();
#endif
}
```

**3.** Show the ad
```csharp
#if UNITY_ANDROID
StartAppWrapper.showAd();
StartAppWrapper.loadAd();
#endif
```

####Showing Rewarded Video Ads
Rewarded Ads are interstitial video ads that provide a reward to the user in exchange for watching an entire video ad. The reward might be in-app goods, virtual currency or any premium content provided by the application. Because users actually opt-in to watch a rewarded video and are granted with something valuable in return, Rewarded Ads are an effective and clean monetization solution for stronger user retention and keeping users engaged in your application for a longer amount of time.

In order to show a Rewarded Ad, follow the following steps:  
**1.** Inside your ``Start()`` method, declare a video listener and load the rewarded video ad:
```csharp
#if UNITY_ANDROID
videoListener = new VideoListenerImplementation ();
StartAppWrapper.setVideoListener (videoListener);
StartAppWrapper.loadAd(StartAppWrapper.AdMode.REWARDED_VIDEO);
```

**2.** 
Implement the ``videoListener`` in order to get a callback when the user completes watching the video and is eligible for getting the reward:  
```csharp
#if UNITY_ANDROID
public class VideoListenerImplementation : StartAppWrapper.VideoListener {
   public void onVideoCompleted() {
      // Grant user with the reward
   }
}
#endif
```

**3.** Show the ad
```csharp
#if UNITY_ANDROID
StartAppWrapper.showAd();
StartAppWrapper.loadAd(StartAppWrapper.AdMode.REWARDED_VIDEO);
#endif
```
 
> **IMPORTANT:** Loading an ad might take a few seconds especially in the case of a video, so it's important not to show the ad immediately after loading it. In case you call showAd() while the ad hasn't been successfully loaded yet, nothing will be displayed. It is recommended to use the "onReceiveAd" callback which is triggered when an ad was loaded and ready to use (see [Adding a Callback when an Interstitial Ad is loaded](https://github.com/StartApp-SDK/Documentation/wiki/unity-android-advanced-usage#load-callback)).

[Back to top](#top)

<a name="banners" />
##Showing Banners
To add a banner to your application add the following code in the appropriate place:

**1.** Import the SDK namespace
```csharp
using StartApp;
```

**2.** Display the banner
```csharp
void Start () {
     #if UNITY_ANDROID
     StartAppWrapper.addBanner( 
           StartAppWrapper.BannerType.AUTOMATIC,
	       StartAppWrapper.BannerPosition.BOTTOM);
     #endif
}
```
**Parameters**

**BannerPosition** - position of the banner. Can receive one of the following:
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerPosition.BOTTOM
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerPosition.TOP

For advanced usage, such as showing a specific banner type or hiding the banner, please refer to the [Advanced Usage](unity-android-advanced-usage#banners).

[Back to top](#top)


<a name="AdvancedUsage" />
##Advanced Usage
For advanced usage, please refer to the ["Advanced Usage"](unity-android-advanced-usage) section.

[Back to top](#top)
<a name="top" />
**Last version: 3.2.0**
<img src="./Android/images/android-intro1.png" width="640px" /><br></br>

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - Please notice that steps 1-3 are mandatory
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)

<br></br>
<a name="step1" />
##Step 1, Adding the B4A Wrapper JAR to Your Project
> **IMPORTANT:** This is a mandatory step

In order to add StartApp SDK for B4A to your application please follow the following steps:

**1.** Unzip the SDK files to your B4A working folder<br></br>
**2.** Add the folder path to your project's “Additional Libraries”, under "Tools->Configure Paths"

<img src="./B4A/images/path_configuration.jpg" />

**3.** Click the "Libs" tab of your project and check the “StartAppInAppB4AWrapper-X.X.XX” lib.  
<img src="./B4A/images/Libs.png" />

[Back to top](#top)

<a name="step2" />  
##Step 2, Updating Your Manifest File  
> **IMPORTANT:** This is a mandatory step

Open the "Manifest Editor" ("Project->Manifest Editor") and add the following permissions and activities at the end of your manifest file. 

####Permissions
Mandatory Permissions:
```vb
AddPermission(android.permission.INTERNET)
AddPermission(android.permission.ACCESS_WIFI_STATE)
AddPermission(android.permission.ACCESS_NETWORK_STATE)
```

Optional Permissions (allow StartApp to show higher eCPM Geo-targeted ads):
```vb
AddPermission(android.permission.ACCESS_COARSE_LOCATION)
AddPermission(android.permission.ACCESS_FINE_LOCATION)
```

> StartApp SDK doesn't request location updates proactively but only uses the last known location. 

####Activities
```vb
AddApplicationText(
<activity android:name="com.startapp.android.publish.list3d.List3DActivity"
          android:theme="@android:style/Theme" />

<activity android:name="com.startapp.android.publish.OverlayActivity"
          android:theme="@android:style/Theme.Translucent"
          android:configChanges="orientation|keyboardHidden|screenSize" />

<activity android:name="com.startapp.android.publish.FullScreenActivity"
          android:theme="@android:style/Theme"
          android:configChanges="orientation|keyboardHidden|screenSize" /> )
```  

<img src="./B4A/images/Manifest.png" />

[Back to top](#top)

<a name="step3" />
##Step 3, Initialization
> **IMPORTANT:** 
> - This is a mandatory step

**1.** Add the following line to the ``Sub Globals`` section
```vb
Dim sdk As StartAppSDK
```

**2.** Call the SDK init method under the ``Sub Activity_Create(FirstTime As Boolean)`` method
```vb
sdk.init("Your App ID", True)
```

Replace __"Your App ID"__ with your own value provided in the [Publishers’ Portal](https://portal.startapp.com/#/signin).<br></br>

To find your application ID, click on the "Apps and Sites" tab on the left pane and choose the relevant ID from your app list:<br></br>
<img src="./Android/images/android-appId.png" />

The last ``True`` parameter enables the ["Return Ads"](#return) feature as explained in the next section. If you want to disable this feature, simply pass ``False`` instead.

[Back to top](#top)

<a name="return" />
##Showing Return Ads
The **Return Ad** is a new ad unit which is displayed once the user returns to your application after a certain period of time.  To minimize the intrusiveness, short time periods are ignored. For example, the Return Ad won't be displayed if the user leaves your application to take a short phone call before returning). 

Return ads are enabled and activated by default. If you want to disable this feature, simply pass "False" as the 2th parameter of the ``sdk.init`` method:
 ```vb
sdk.init("Your App ID", False)
```

[Back to top](#top)

<a name="splash" />
##Showing a Splash Ad (recommended)
A Splash Ad is a full-page ad that is displayed immediately after the application is launched.
A Splash Ad first displays a full page splash screen that you define (as described below) followed by a full page ad. 

> **StartApp Splash Ad is a top performing ad unit, presenting the industry's highest CPM's**   

####Adding the Splash Screen 
**1.** Add the following line to the ``Sub Globals`` section
```vb
Dim startAppSplash As StartAppAd
```

**2.** Initialize and create the splash screen under ``Activity_Create(FirstTime As Boolean)``:
```vb
startAppSplash.initialize
startAppSplash.showSplash(FirstTime)
```

**If you wish to customize the splash screen, please refer to the [Advanced Usage](B4A-android-advanced-usage#CustomizingSplashScreen).**

[Back to top](#top)


<a name="exit" />
##Showing Exit Ads
StartApp Exit ad is displayed when the user leaves your application (usually by pressing the BACK button on the app's main activity).  

####Adding an exit ad to your project
**1.** Add the following line to the ``Sub Globals`` section
```vb
Dim startAppInterstitial As StartAppAd
```

**2.** Create the following method inside your main activity
```vb
Sub Activity_KeyPress(KeyCode As Int) As Boolean
    If KeyCode = KeyCodes.KEYCODE_BACK Then
       startAppInterstitial.onBackPressed
    End If
End Sub
```

[Back to top](#top)

<a name="interstitial" />
##Showing Interstitial Ads

You can choose to show the interstitial ad in several locations within your application.
This could be upon entering the app, between stages, when pressing a button, while waiting for an action and more.

We do, however, recommend showing the ad upon exiting the application by using the ‘Back’ button, as explained in the previous section.

**1.** Add the following line to the ``Sub Globals`` section
```vb
Dim startAppInterstitial As StartAppAd
```

**2.** Update your ``Activity_Resume`` and ``Activity_Pause``
```vb
Sub Activity_Resume
 startAppInterstitial.onResume
End Sub

Sub Activity_Pause (UserClosed As Boolean)
 startAppInterstitial.onPause
End Sub
```

**3.** Initialize and create the ad under ``Activity_Create(FirstTime As Boolean)``:
```vb
startAppInterstitial.initialize
startAppInterstitial.loadAd
```

**4.** Show the ad where you want it to be displayed
```vb
startAppInterstitial.showAd
```

[Back to top](#top)


####Showing Rewarded Video Ads
Rewarded Ads are interstitial video ads that provide a reward to the user in exchange for watching an entire video ad. The reward might be in-app goods, virtual currency or any premium content provided by the application. Because users actually opt-in to watch a rewarded video and are granted with something valuable in return, Rewarded Ads are an effective and clean monetization solution for stronger user retention and keeping users engaged in your application for a longer amount of time.

In order to show a Rewarded Ad, follow the following steps:  
**1.** add the following line to the ``Sub Globals`` section
```vb
Dim startAppRewardedVideo As StartAppAd
```

**2.** Under ``Activity_Create(FirstTime As Boolean)``, initialize a video listener and load the rewarded video ad:
```vb
Dim rewardedAd As AdMode
startAppRewardedVideo.initialize
startAppRewardedVideo.setVideoListener("VideoListener")
startAppRewardedVideo.loadAdWithAdMode(rewardedAd.REWARDED_VIDEO)
```

**3.** Implement the ``VideoListener`` in order to get a callback when the user completes watching the video and is eligible for getting the reward:  
```vb
Sub VideoListener_VideoCompleted
 'Grant user with the reward
End Sub
```

**4.** Show the ad where you want it to be displayed
```vb
startAppRewardedVideo.showAd
```
 
> **IMPORTANT:** Loading an ad might take a few seconds especially in the case of a video, so it's important not to show the ad immediately after loading it. In case you call showAd() while the ad hasn't been successfully loaded yet, nothing will be displayed. It is recommended to use the "onReceiveAd" callback which is triggered when an ad was loaded and ready to use (see [Adding a Callback when an Interstitial Ad is loaded](B4A-android-advanced-usage#load-callback)).

[Back to top](#top)


<a name="banners" />
##Showing Banners
**1.** Add the following line to the ``Sub Globals`` section
```vb
Dim autoBanner As Banner
Dim bannerPositionToSet As BannerPosition
```

**2.** Create the banner in the appropriate location, for example under ``Activity_Create(FirstTime As Boolean)``
```vb
autoBanner.addBannerWithPosition(bannerPositionToSet.BOTTOM)
```

**Parameters**  

**BannerPosition** - position of the banner. Can receive one of the following:  

<img src="./iOS/images/V.png" hspace="15px" width="12px" /> bannerPositionToSet.BOTTOM  
<img src="./iOS/images/V.png" hspace="15px" width="12px" /> bannerPositionToSet.TOP    

For advanced usage, such as showing a specific banner type or hiding the banner, please refer to the [Advanced Usage](B4A-android-advanced-usage#banner-type).  

[Back to top](#top)

<a name="Demographic" />
##Enjoy Higher eCPM with Demographic-Targeted Ads
If you know your user's gender or age, StartApp can use it to serve better-targeted ads which can increase your eCPM and revenue significantly.

####Set Age and Gender
**1.** Add the following line to the ``Sub Globals`` section
```vb
Dim sdkPrefs As SDKAdPreferences
```

**2.** Under ``Activity_Create(FirstTime As Boolean)``, set the user's age and gender and pass it to the ``initWithSDKPreferences()`` method (which replaces the original ``init()`` method)
```vb
Dim genderToSet As Gender
sdkPrefs.initialize
sdkPrefs.setGender(genderToSet.MALE)
sdkPrefs.setAge(18)
sdk.initWithSDKPreferences("Your App ID", sdkPrefs, True)
```

+ ``setAge`` can take an integer
+ ``setGender`` can take one of the following values: *genderToSet.FEMALE* or *genderToSet.MALE*


####Set Location
The location of the user is a dynamic property which is changed constantly. Hence, you should provide it every time you load a new Ad:

```vb
Dim adPrefs As AdPreferences
adPrefs.initialize
adPrefs.setLatitude(31.776719)  'replace with the actual latitude
adPrefs.setLongitude(35.234508) 'replace with the actual longitude
startAppInterstitial.loadAdWithAdPreferences(adPrefs)
```

[Back to top](#top)

<a name="SampleProject" />
##Sample Project
StartApp provides a sample integration project available on [GitHub](https://github.com/StartApp-SDK/StartApp_InApp_SDK_B4A_Example)

[Back to top](#top)

<a name="AdvancedUsage" />
##Advanced Usage
For advanced usage, please refer to the ["Advanced Usage"](B4A-android-advanced-usage)

[Back to top](#top)
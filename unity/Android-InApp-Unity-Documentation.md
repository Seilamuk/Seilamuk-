<a name="top" />

**Latest version: 2.4.13**

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

**3.** Import all the package's items by pressing "All" and then "Import". If you already have an AndroidManifest.xml file, uncheck it before pressing "Import". 
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
          android:theme="@android:style/Theme.NoTitleBar.Fullscreen"/>

<activity android:name="com.startapp.android.publish.AppWallActivity" 
          android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" 
          android:hardwareAccelerated="true"
          android:configChanges="orientation|keyboardHidden|screenSize" />
```

**2.** Make sure the meta-data parameter named `unityplayer.ForwardNativeEventsToDalvik` is set to `true`:
```xml
<meta-data android:name="unityplayer.ForwardNativeEventsToDalvik" android:value="true" />
```

[Back to top](#top)


<br></br>
<a name="step3" />
##Step 3, Updating your StartApp data file
Update the _StartAppData.txt_ (in the Assets/Resources folders) as follows:

**1.** Add your StartApp Account ID after ``developerId=``.  
For example: ``developerId=103343523``  
**2.** Add your StartApp Application ID after ``applicationId=``.   
For example: ``applicationId=204346543``  

You can find your Account and Application IDs in the [developers’ portal](http://developers.startapp.com).<br></br>
After logging in, your account ID will be displayed at the top right-hand corner of the page:
<img src="./Android/images/accountId.png" />

To find your application ID, click on the "Apps and Sites" tab on the left pane and choose the relevant ID from your app list:<br></br>
<img src="./Android/images/android-appId.png" />


[Back to top](#top)


<br></br>
<a name="step4" />
##Adding an exit ad to your project

Use the *StartAppBackPlugin* in components where you would like the user to press 'back' to exit the application. The plugin will show an ad and then exit the application.

**1.** Drag the StartAppBackPlugin.cs to the components which you would like pressing 'back' to exit the application <br></br>
**2.** Do not implement exit on the 'back' in this components <br></br>

> **NOTE:** there is no need to implement exit on the 'back' button in these components as the plugin will exit the application after showing the ad.

[Back to top](#top)


<br></br>
<a name="step5" />
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


<br></br>
<a name="step6" />
##Showing Interstitial Ads

You can choose to show the interstitial ad in several locations within your application.
This could be upon entering (onCreate), between stages, while waiting for an action and more.

We do, however, recommend showing the ad upon exiting the application by using the ‘Back’ button or the ‘Home’ button, as explained in step 4 above.

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

[Back to top](#top)


<a name="AdvancedUsage" />
##Advanced Usage
For advanced usage, please refer to the ["Advanced Usage"](unity-android-advanced-usage) section.

[Back to top](#top)

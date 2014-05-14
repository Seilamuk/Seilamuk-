<a name="top" />
<img src="./unity/images/unity-android-intro.png" width="640px" /><br></br>

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - This SDK is designed to work across all Android devices. It supports all versions but activates only on Android OS 2.1 and above. In lower versions, the SDK will not be active.
> - Please notice that steps 1-3 are mandatory
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)

<br></br>
<a name="step1" />
##Step 1, Adding the SDK files to your Unity project
In order to add StartApp SDK to your application please follow the following steps:

**1.** Copy the _StartAppWrapper.cs_ to the Assets folder<br></br>
**2.** Right click on your _Assets_ folder in Unity<br></br>
<img src="./unity/images/assets.png" /><br></br>
**3.** Create a _Plugins_ folder if one does not exist<br></br>
**4.** Right click on your _Plugins_ folder<br></br>
**5.** Create an _Android_ folder if one does not exist<br></br>
**6.** Copy the following files from the SDK zip _Assets/Plugins/Android_ folder to the _Android_ folder:
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" />_AndroidManifest.xml_
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" />_StartAppInApp-2.2.0.jar_
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" />_StartAppInAppUnityWrapper-2.1.1.jar_
<br></br><img src="./unity/images/files.png" />

**7.** Copy the content of the _StreamingAssets_ from the SDK zip into the same folder in your _Assets/StreamingAssets_ folder (create one if it does not exists).<br></br>
**8.** Copy the content of the _Resources_ from the SDK zip into the same folder in your _Assets/Resources_ folder (create one if it does not exists).<br></br>

[Back to top](#top)


<br></br>
<a name="step2" />
##Step 2, Updating your Manifest File
Update the _manifest.xml_ (in the _Android_ folders) as follow:

**1.** Under the 'manifest' node place your \<YOUR_PACKAGE_NAME\> within package attribute.<br></br>
**2.** Under the 'AppWall' and 'List3DActivity' activities replace \<YOUR_PACKAGE_NAME\> with the name of your package as declared in your manifest.<br></br>

```xml
<activity android:name="com.startapp.android.publish.list3d.List3DActivity" 
          android:taskAffinity="<YOUR_PACKAGE_NAME>.StartApp"
          android:theme="@android:style/Theme" />

<activity android:name="com.startapp.android.publish.AppWallActivity"
          android:configChanges="orientation|keyboardHidden"
          android:taskAffinity="<YOUR_PACKAGE_NAME>.StartApp" 
          android:theme="@android:style/Theme.Translucent" />
```

[Back to top](#top)


<br></br>
<a name="step3" />
##Step 3, Updating your StartApp data file
Update the _StartAppData.txt_ (in the Assets/Resources folders) as follows:

**1.** Add your StartApp Developer ID after ``"developerId="`` <br></br>
**2.** Add your StartApp Application ID after ``"applicationId="`` <br></br>

You can find your Developer and Application IDs in the [developers’ portal](http://developers.startapp.com).<br></br>
After logging in, your developer ID will be at the top right-hand corner of the page:
<img src="./Android/images/android-devId.png" />

To find your application ID, click on the <img src="./Android/images/dash2.jpg" align="middle"/> at the top of the main screen and then choose the relevant ID from your app list:<br></br>
<img src="./Android/images/android-appId.png" width="350px" />

[Back to top](#top)


<br></br>
<a name="step4" />
##Step 4, Adding an exit ad to your project

Add the StartApp 'Back' plugin to your Unity project. This plugin will show an ad when a user presses the 'back' button

**1.** Copy the StartAppBackPlugin.cs to the _Assets_ folder <br></br>
**2.** Drag the StartAppBackPlugin.cs to a components in your scene <br></br>

> **NOTE:** :  There is no need to implement exit on the 'back' button in these components as the plugin will exit the application after showing the ad. 

[Back to top](#top)


<br></br>
<a name="step5" />
##Step 5, Showing Banners
To add a banner to your application add the following code in the appropriate place:

**1.** Import the SDK namespace
``` java
using StartApp;
```

**2.** Display the banner
```java
void Start () {
     StartAppWrapper.addBanner( 
           StartAppWrapper.BannerType.AUTOMATIC,
	       StartAppWrapper.BannerPosition);
}
```
**Parameters**

**BannerPosition** - position of the banner. Can receive one of the following:
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerPosition.BOTTOM
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerPosition.TOP

If you wish to add a specific type of banner, please refer to the [Advanced Usage](unity-NH-android-advanced-usage#banners).

[Back to top](#top)


<br></br>
<a name="step6" />
##Step 6, Showing Interstitial Ads

You can choose to show the interstitial ad in several locations within your application.
This could be upon entering (onCreate), between stages, while waiting for an action and more.

We do, however, recommend showing the ad upon exiting the application by using the ‘Back’ button or the ‘Home’ button, as explained in step 4 above.

Add the following code to the appropriate place or places within your activities in which you would like to show the ad:

**1.** Import the sdk namespace
```java
using StartApp;
```

**2.** Load the ad
```java
void Start () {
StartAppWrapper.loadAd();
}
```

**3.** Show the ad
```java
StartAppWrapper.showAd();
StartAppWrapper.loadAd();
```

[Back to top](#top)


<a name="AdvancedUsage" />
##Advanced Usage
For advanced usage, please refer to the ["Advanced Usage"](unity-NH-android-advanced-usage) section.

[Back to top](#top)
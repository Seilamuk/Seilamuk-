<a name="top" />
<img src="./unity/images/unity-android-intro.png" width="640px" /><br></br>

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)
> - This SDK is designed to work across all Android devices. It supports all versions but activates only on Android OS 2.1 and above. In lower versions, the SDK will not be active.

<br></br>
<a name="step1" />
##Step 1, Adding the SDK files to your Unity project
In order to add StartApp SDK to your application please follow the following steps:

*a.* Copy the StartAppWrapper.cs to the Assets folder
*b.* Right click on your Assets folder in Unity
<img src="./unity/images/assets.png" />
*c.* Create a Plugins folder if one does not exist
*d.* Right click on your Plugins folder
*e.* Create an Android folder if one does not exist
*f.* Copy the following files from the SDK zip Assets/Plugins/Android folder to the Android folder:
<br></br><img src="./iOS/images/V.png" width="12px" /> AndroidManifest.xml
<br></br><img src="./iOS/images/V.png" width="12px" /> StartAppInApp-2.2.0.jar
<br></br><img src="./iOS/images/V.png" width="12px" /> StartAppInAppUnityWrapper-2.1.1.jar
<img src="./unity/images/files.png" />

*g.* Copy the content of the StreamingAssets from the SDK zip into the same folder in your Assets/StreamingAssets folder (create one if it does not exists).
*h.* Copy the content of the Resources from the SDK zip into the same folder in your Assets/Resources folder (create one if it does not exists).

[Back to top](#top)


<br></br>
<a name="step2" />
##Step 1, Updating your AndroidManifest.xml File
Update the manifest.xml (in the Android folders) as follow:

*a.* Under the 'manifest' node place your <YOUR_PACKAGE_NAME> within package attribute.
*b.* Under the 'AppWall' and 'List3DActivity' activities replace <YOUR_PACKAGE_NAME> with the name of your package as declared in your manifest.

```xml
<activity
android:name="com.startapp.android.publish.list3d.List3DActivity" android:taskAffinity="<YOUR_PACKAGE_NAME>.StartApp"
android:theme="@android:style/Theme" />
<activity
android:name="com.startapp.android.publish.AppWallActivity"
android:configChanges="orientation|keyboardHidden"
android:taskAffinity="<YOUR_PACKAGE_NAME>.StartApp" android:theme="@android:style/Theme.Translucent" />
```

[Back to top](#top)


<br></br>
<a name="step3" />
##Step 3, Updating your StartApp data file
Update the StartAppData.txt (in the Assets/Resources folders) as follows:
a. Add your StartApp Developer ID after "developerId="
b. Add your StartApp Application ID after "applicationId="

You can find your Developer and Application IDs in the [developers’ portal](http://developers.startapp.com).<br></br>
After logging in, your developer ID will be at the top right-hand corner of the page:
<img src="./Android/images/android-devId.png" />

To find your application ID, click on the <img src="./Android/images/dash2.jpg" align="middle"/> at the top of the main screen and then choose the relevant ID from your app list:<br></br>
<img src="./Android/images/android-appId.png" width="350px" />

[Back to top](#top)


<br></br>
<a name="step4" />
##Step 3, Adding an exit ad to your project

Use the 'StartAppBackAndHomePlugin' in components where you would like the user to press 'home' or 'back' to exit the application. The plugin will show an ad and then exit the application.

*a.* Copy the StartAppBackAndHomePlugin.cs to the Assets folder
*b.* Drag the StartAppBackAndHomePlugin.cs to the components which you would like pressing 'home' or 'back' to exit the application
*c.* Do not implement exit on the 'back' in this components

> **NOTE:** there is no need to implement exit on the 'back' button in these components as the plugin will exit the application after showing the ad.
For additional usage options, please refer to [Extended Usage](Android-InApp-Unity-Documentation#extended-usage) section.

[Back to top](#top)


<br></br>
<a name="step5" />
##Step 3, Adding a banner to your project

There are 3 different types of banners:

**Banner Type** | **Description**
---------------------- | ---------------
Automatic Banner **(Recommended)**  | Automatic selects the most suitable banner of the two listed below
Standard (2D) Banner  | A standard (two dimensional) banner
3D Banner   | A three dimensional rotating banner

We highly recommend adding an Automatic banner, which automatically selects whether to display a Standard banner or a 3D banner. The banner remains displayed throughout the entire Activity life-cycle. 

To add a banner to your application add the following code in the appropriate place:
*a.* Import the SDK namespace
``` java
using StartApp;
```

*b.* Display the banner
```java
void Start () {
StartAppWrapper.addBanner ( StartAppWrapper.BannerType,
							StartAppWrapper.BannerPosition);
}
```

**Parameters:**

*1.* The first parameter is the type of banner, which can receive one of the following:
<br></br><img src="./iOS/images/V-blue.png" width="12px" /> StartAppWrapper.BannerType.AUTOMATIC
<br></br><img src="./iOS/images/V-blue.png" width="12px" /> StartAppWrapper.BannerType.STANDARD
<br></br><img src="./iOS/images/V-blue.png" width="12px" /> StartAppWrapper.BannerType.THREED
*2.* Second parameter is the position of banner, which can receive one of the following:
<br></br><img src="./iOS/images/V-blue.png" width="12px" /> StartAppWrapper.BannerPosition.BOTTOM
<br></br><img src="./iOS/images/V-blue.png" width="12px" /> StartAppWrapper.BannerPosition.TOP

[Back to top](#top)


<br></br>
<a name="step5" />
##Step 3, Showing Interstitial Ads

You can choose to show the interstitial ad in several locations within your application.
This could be upon entering (onCreate), between stages, while waiting for an action and more.

We do, however, recommend showing the ad upon exiting the application by using the ‘Back’ button or the ‘Home’ button, as explained in step 4 above.

Add the following code to the appropriate place or places within your activities in which you would like to show the ad:

*a.* Import the sdk namespace
```java
using StartApp;
```

*b.* Load the ad
```java
void Start () {
StartAppWrapper.loadAd();
}
```

*c.* Show the ad
```java
StartAppWrapper.showAd();
StartAppWrapper.loadAd();
```

[Back to top](#top)


<a name="AdvancedUsage" />
##Advanced Usage
For advanced usage, please refer to the ["Advanced Usage"](unity-android-advanced-usage) section.

[Back to top](#top)

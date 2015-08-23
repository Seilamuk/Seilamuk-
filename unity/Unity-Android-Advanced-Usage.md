<a name="top" />

######This section describes advanced usage and personal customization options and is not mandatory for the integration.

<a name="banner-type" />
##Customizing your Banner
You can show a specific banner type, or hide the banner in a specific screen or during a specific scene.

###Selecting Banner Type
There are 3 different types of banners:

**Banner Type** | **Description**
---------------------- | ---------------
Automatic Banner **(Recommended)**  | Automatic selects the most suitable banner of the two listed below
Standard (2D) Banner  | A standard (two dimensional) banner
3D Banner   | A three dimensional rotating banner

We highly recommend adding an Automatic banner, which automatically selects whether to display a standard banner or a 3D banner. The banner remains displayed throughout the entire activity life-cycle. 

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

_BannerType_ - type of banner. Can receive one of the following:
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerType.AUTOMATIC
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerType.STANDARD
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerType.THREED

_BannerPosition_ - position of the banner. Can receive one of the following:
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerPosition.BOTTOM
<br></br><img src="./iOS/images/V.png" hspace="15px" width="12px" /> StartAppWrapper.BannerPosition.TOP

###Hiding the Banner
In order to hide an already displayed banner, use the ``removeBanner()`` method. Simply pass the position of the banner you want to hide. 

**For example:**  
```csharp
#if UNITY_ANDROID
StartAppWrapper.removeBanner(StartAppWrapper.BannerPosition.BOTTOM);
#endif
```

In order to show the banner again, simply use the addBanner() method as described in ["Showing Banners"](Android-InApp-Unity-Documentation#banners).

[Back to top](#top)

<a name="load-callback" />
##Adding a callback when Ad has been loaded

``StartAppWrapper.loadAd()`` can get an implementation of *AdEventListener* as a parameter. In case you want to get a callback for the ad load, pass the object which implements *StartAppWrapper.AdEventListener* as a parameter to the method. This object should implement the following methods:
```csharp
public void onReceiveAd(){
}
public void onFailedToReceiveAd(){
}
```

<a name="show-callback" />
##Adding a callback when Ad has been displayed or clicked

``StartAppWrapper.showAd()`` can get an implementation of *AdDisplayListener* as a parameter. In case you want to get a callback for the ad show, pass the object which implements *StartAppWrapper.AdDisplayListener* as a parameter of the method. This object should implement the following methods:
```csharp
public void adHidden(){
}
public void adDisplayed(){
}
public void adClicked(){
}
```

[Back to top](#top)


<a name="CustomizingSplashScreen" />
##Customizing the Splash Screen
StartApp In-Ad provides a few pre-defined templates of splash screens in which you can place your own creatives, such as application name, logo and loading animation, as described below.

####Customizing the Splash Screen
In the ``OnGUI()`` method of your script, call the following static function:
```csharp
#if UNITY_ANDROID
StartAppWrapper.showSplash(splashConfig);
#endif
```

Apply the following parameters:
+ **``splashConfig``**: Optional object that can be used to customize some of your template's properties to suit your needs, such as your application name, logo and theme (see the example below). For a full description of the SplashConfig API, please refer to [SplashConfig API](#SplashConfig-API).

**Example:** the following is an example of a custom template with an OCEAN theme, modified application name, logo and landscape orientation:
```csharp
#if UNITY_ANDROID
StartAppWrapper.showSplash(new StartAppWrapper.SplashConfig()
				.setTheme(StartAppWrapper.SplashConfig.Theme.OCEAN)
				.setAppName("Your Application Name")
				.setLogo("Your logo")
				.setOrientation(StartAppWrapper.SplashConfig.Orientation.LANDSCAPE)				
);
#endif
```
> **NOTE:** for optimal appearance of your Splash screen on all device densities, provide a logo of 360x360px and place it in the Resources folder of your project. 
> If you do not provide a logo, then StartApp In-App uses the default application icon (as declared in the Manifest) and stretches it to 360x360px.

[Back to top](#top)

<a name="SplashConfig-API" />
##SplashConfig API
The following describes the methods that you can use for customizing the Splash screen displayed in a StartApp In-App Splash screen Ad.

####► Set the Splash screen mode
> **```public SplashConfig setTheme(StartAppWrapper.SplashConfig.Theme theme)```**

Sets the Splash theme.
Use one of the options below to select a design theme for your splash screen.  

**Parameters**<br></br>
*SplashConfig.Theme.DEEP_BLUE* (default)<br></br>
*SplashConfig.Theme.SKY*<br></br>
*SplashConfig.Theme.ASHEN_SKY*<br></br>
*SplashConfig.Theme.BLAZE*<br></br>
*SplashConfig.Theme.GLOOMY*<br></br>
*SplashConfig.Theme.OCEAN* <br></br>

####► Set the application name
> **```public SplashConfig setAppName(String appName)```**<br></br>

Sets the application name to be used in the Template mode.

**Parameters**<br></br>
*String* (default is the application name from the manifest).

####► Set the logo
> **```public SplashConfig setLogo(String logo)```**<br></br>

Sets the logo to be displayed in the Template mode.

**Parameters**<br></br>
*logo* the logo resource name under the Resources folder (default is the icon resource from the manifest).

> **NOTE:** if you provide a custom logo, please follow these steps:  
> 1. Click your logo image in the Resources folder  
> 2. On the menu on the right, under 'Texture Type' choose 'Advanced'  
> 3. Check 'Read/Write Enabled'  
> 4. Under 'Format' choose "RGB 24 bit"  
> 5. Click "Apply"  

####► Set the orientation
> **```public SplashConfig setOrientation(StartAppWrapper.SplashConfig.Orientation orientation)```**<br></br>

Sets the orientation to be used.

**Parameters**<br></br>
*StartAppWrapper.SplashConfig.Orientation.PORTRAIT* (default)<br></br>
*StartAppWrapper.SplashConfig.Orientation.LANDSCAPE*<br></br>
*StartAppWrapper.SplashConfig.Orientation.AUTO* (use the device's orientation upon entering the application)<br></br>

[Back to top](#top)
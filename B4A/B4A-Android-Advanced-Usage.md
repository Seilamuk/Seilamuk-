<a name="top" />

######This section describes advanced usage and personal customization options and is not mandatory for the integration.

<a name="banner-type" />
##Customizing your Banner
You can show a specific banner type as well as hiding and showing it in run time.

###Selecting Banner Type
There are 3 different types of banners:

**Banner Type** | **Description**
---------------------- | ---------------
Automatic Banner **(Recommended)**  | Automatic selects the most suitable banner of the two listed below
Standard (2D) Banner  | A standard (two dimensional) banner
3D Banner   | A three dimensional rotating banner

We highly recommend adding an Automatic banner, which automatically selects whether to display a standard banner or a 3D banner. The banner remains displayed throughout the entire activity life-cycle. 

To add a banner to your application add the following code in the appropriate place:

**1.** add the following line to the ``Sub Globals`` section
```vb
Dim bannerAuto As Banner
Dim bannerPositionToSet As BannerPosition
Dim bannerTypeToSet As BannerType
```

**2.** create the banner in the appropriate location, for example under ``Activity_Create(FirstTime As Boolean)``:
```vb
bannerAuto.addBannerWithPositionAndType(bannerPositionToSet.BOTTOM, 
										bannerTypeToSet.AUTOMATIC)
```

**Parameters**

_BannerPosition_ - position of the banner. Can receive one of the following:  
<img src="./iOS/images/V.png" hspace="15px" width="12px" /> bannerPositionToSet.BOTTOM  
<img src="./iOS/images/V.png" hspace="15px" width="12px" /> bannerPositionToSet.TOP   

_BannerType_ - type of banner. Can receive one of the following:  
<img src="./iOS/images/V.png" hspace="15px" width="12px" /> bannerTypeToSet.AUTOMATIC  
<img src="./iOS/images/V.png" hspace="15px" width="12px" /> bannerTypeToSet.STANDARD  
<img src="./iOS/images/V.png" hspace="15px" width="12px" /> bannerTypeToSet.THREED  


###Hiding the Banner
In order to hide an already displayed banner, use the ``removeBannerFromPosition()`` method. Simply pass the position of the banner you want to hide. 

**For example:**  
```vb
bannerAuto.removeBannerFromPosition(bannerPositionToSet.TOP)
```

In order to show the banner again, simply use the ``addBannerWithPosition()`` method as described in ["Showing Banners"](B4A-InApp-Documentation#banners).

[Back to top](#top)

<a name="load-callback" />
##Adding a callback when an Interstitial Ad is loaded

In case you want to get a callback when an ad has been loaded or failed to load, call startAppInterstitial.loadAdWithListener("AdEventListener") and implement the following events:
```vb
Sub AdEventListener_ReceiveAd(ad1 As Ad)
 'place your code here
End Sub

Sub AdEventListener_FailedToReceiveAd(ad1 As Ad)
 'place your code here
End Sub
```

Please note - please don't change the suffix of the methods above ("_ReceiveAd" and "_FailedToReceiveAd"). The prefix should be identical to the string passed to the loadAdWithListener method.

<a name="show-callback" />
##Adding a callback when an Interstitial Ad is displayed or clicked

In case you want to get a callback when an ad has been displayed or clicked, call startAppInterstitial.showAdWithListener("AdDisplayListener") and implement the following events:
```vb
Sub AdDisplayListener_AdHidden(ad1 As Ad)
 'place your code here
End Sub 

Sub AdDisplayListener_AdDisplayed(ad1 As Ad)
 'place your code here
End Sub 

Sub AdDisplayListener_AdClicked(ad1 As Ad)
 'place your code here
End Sub 

Sub AdDisplayListener_AdNotDisplayed(ad1 As Ad)
 'place your code here
End Sub
```

Please note - please don't change the suffix of the methods above ("_AdHidden", "_AdDisplayed", etc.). The prefix should be identical to the string passed to the loadAdWithListener method.

[Back to top](#top)


<a name="CustomizingSplashScreen" />
##Customizing the Splash Screen
StartApp provides a few pre-defined templates of splash screens in which you can place your own creatives, such as application name and logo, as described below.

####Customizing the Splash Screen
initialize and create the splash screen under ``Activity_Create(FirstTime As Boolean)``:
```vb
startAppSplash.initialize
Dim splashConf As SplashConfig
startAppSplash.showSplashWithSplashConfig(FirstTime, splashConf)
```

Apply the following parameters:
+ **``splashConfig``**: Optional object that can be used to customize some of your template's properties to suit your needs, such as your application name, logo and theme (see the example below). For a full description of the SplashConfig API, please refer to [SplashConfig API](#SplashConfig-API).

**Example:** the following is an example of a custom template with an OCEAN theme, modified application name, logo and landscape orientation:
```vb
Dim splashConf As SplashConfig
Dim orientationToSet As Orientation
Dim themeToSet As Theme
Dim logoToSet As Bitmap
logoToSet.Initialize(File.DirAssets, "your logo") 'Should be in [project folder]\Files
 
splashConf.initialize
splashConf.setAppName("Your Application Name")
splashConf.setOrientation(orientationToSet.PORTRAIT)
splashConf.setTheme(themeToSet.OCEAN)
splashConf.setLogo(logoToSet)

startAppSplash.showSplashWithSplashConfig(FirstTime, splashConf)
```

> **NOTE:** for optimal appearance of your Splash screen on all device densities, provide a logo of 360x360px and place it in the [project folder]\Files folder. 
> If you do not provide a logo, then StartApp In-App uses the default application icon (as declared in the Manifest) and stretches it to 360x360px.

[Back to top](#top)

<a name="SplashConfig-API" />
##SplashConfig API
The following describes the methods that you can use for customizing the Splash screen displayed in a StartApp In-App Splash screen Ad.

####► Set the Splash screen mode
> **```public SplashConfig setTheme(Theme theme)```**

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
*logo* the logo resource name under the [project folder]\Files folder (default is the icon resource from the manifest).

####► Set the orientation
> **```public SplashConfig setOrientation(Orientation orientation)```**<br></br>

Sets the orientation to be used.

**Parameters**<br></br>
*Orientation.PORTRAIT* (default)<br></br>
*Orientation.LANDSCAPE*<br></br>
*Orientation.AUTO* (use the device's orientation upon entering the application)<br></br>

[Back to top](#top)

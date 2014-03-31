<a name="top" />
######This section describes advanced usage and personal customization and is not mandatory for the integration.

<a name="SelectBanner" />
##Selecting Banner Type
Three types of banners are provided, as follows:

**Banner Type** | **Description**
---------------------- | ---------------
Automatic Banner **(Recommended)**  | Automatic selects the most suitable banner of the two listed below
Standard (2D) Banner  | A standard (two dimensional) banner
3D Banner   | A three dimensional rotating banner

We highly recommend adding an Automatic banner, which automatically selects whether to display a Standard banner or a 3D banner. The banner remains displayed throughout the entire Activity life-cycle. 
To add the automatic banner, please refer to [Step 4, Showing Banners](Android-InApp-Documentation#step4). If you do not wish to add the Automatic Banner, use one of the following options:

####Loading a Standard Banner
Add the following View inside your Activity layout .XML:
```java
<com.startapp.android.publish.banner.bannerstandard.BannerStandard
    	android:id="@+id/startAppStandardBanner"
    	android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"/>
```

####Loading a 3D Banner
Add the following View inside your Activity layout .XML:
```java
<com.startapp.android.publish.banner.banner3d.Banner3D 
    	android:id="@+id/startApp3DBanner"
    	android:layout_width="wrap_content"
        android:layout_height="wrap_content" 
        android:layout_centerHorizontal="true"/>
```

> **NOTE:**
> This code replaces a View inside your Activity. You also have the option to add additional attributes for placing it in the desired location in your Activity.

[Back to top](#top)

<a name="SelectInterstitial" />
##Selecting Interstitial Ad Type
We highly recommend using our Automatic mode, which automatically selects the best Interstitial Ad to display, meaning the type of Ads that will generate the most revenue for you.  
To add an automatic Interstitial Ad, please refer to [Step 5, Showing Interstitial Ads](Android-InApp-Documentation#step5). 
If you do not wish to use the automatic mode, ``startAppAd.loadAd()`` can be directed to load specific Ads to be shown later using the AdMode parameter. The options for the AdMode parameter are:

**Parameter Name** | **Description** | **Specific Ad Load Example**
---------------------- | ---------------------- | ---------------------- 
AUTOMATIC **(Recommended)** | Auto-selection of the best next Interstitial Ad to display, meaning the type of Ads that will generate the most revenue for you. This is the default | ``startAppAd.loadAd(AdMode.AUTOMATIC)``
FULLPAGE  | A full-page Interstitial Ad | ``startAppAd.loadAd(AdMode.FULLPAGE)``
OFFERWALL | Auto-selection of a Standard 2D full screen Offer Wall or a 3D Offer Wall | ``startAppAd.loadAd(AdMode.OFFERWALL)``
OVERLAY   | An overlay Interstitial Ad is a full page Ad that runs on top of your application | ``startAppAd.loadAd(AdMode.OVERLAY)``

When using this mode, the following additional methods must be implemented in the Activity’s lifecycle:

**1**     Override the ``onSaveInstanceState(Bundle outState)`` method and add a call to ``startAppAd.onSaveInstanceState(outstate)``.

> **NOTE:**
> Add this method immediately after the ``super.onSaveInstanceState(outState)`` method.

**Example**
```java
@Override
protected void onSaveInstanceState (Bundle outState){
   super.onSaveInstanceState(outState);
   startAppAd.onSaveInstanceState(outState);
}
```

**2**    Override the ``onRestoreInstanceState(Bundle savedInstanceState)`` method and add a call to ``startAppAd.onRestoreInstanceState(savedInstanceState)``.

> **NOTE** 
> Add this method immediately before the method ``super.onRestoreInstanceState(savedInstanceState)``.

**Example**
```java
@Override
protected void onRestoreInstanceState (Bundle savedInstanceState){
   startAppAd.onRestoreInstanceState(savedInstanceState);
   super.onRestoreInstanceState(savedInstanceState);
}
```

[Back to top](#top)
	
<a name="CloseInterstitial" />
##Explicitly Closing an Interstitial Ad
You can explicitly close an interstitial Ad by calling ``startAppAd.close()``. This closes the Ad and returns control to the calling Activity. You can use this when implementing a timeout for an Ad.

> **NOTE** 
> Keep in mind that the user can close the Ad before timeout expires

[Back to top](#top)

<a name="AddingInterstitialCallbacks" />
##Adding Interstitial Callbacks
####Adding a Callback when an Interstitial Ad is loaded
``startAppAd.loadAd()`` can get an implementation of ``AdEventListener`` as a parameter.
To get a callback when an Ad is loaded, pass the object that implements ``AdEventListener`` (this may be your Activity) as a parameter to the ``loadAd`` method. This object must implement the following methods:
```java
@Override
public void onReceiveAd(Ad ad) {
}
@Override
public void onFailedToReceiveAd(Ad ad) {
}
```

**Example**
```java
startAppAd.loadAd (new AdEventListener() {
    @Override
    public void onReceiveAd(Ad ad) {
    }
    @Override
    public void onFailedToReceiveAd(Ad ad) {
    }
});
```

####Adding a Callback when an Interstitial Ad is shown
``startAppAd.showAd()`` can get a parameter implementation of ``AdDisplayListener``.
To get a callback when an Ad is shown, pass the object that implements ``AdDisplayListener`` (this may be your Activity) as a parameter of the method. This object must implement the following methods:
```java
@Override
public void adHidden(Ad ad) {
}

@Override
public void adDisplayed(Ad ad) {
}
```

**Example**
```java
startAppAd.showAd(new AdDisplayListener() {
    @Override
    public void adHidden(Ad ad) {
    }
    @Override
    public void adDisplayed(Ad ad) {
    }
});
```

[Back to top](#top)

<a name="CustomizingSplashScreen" />
##Customizing the Splash Screen
StartApp In-Ad provides two modes for displaying splash screens - Template and User-defined. The template splash screen is a pre-defined template in which you can place your own creatives, such as application name, logo and loading animation, as described below. If you want to use your own splash screen, you can provide it as a layout, using the user-defined mode.

####Customizing the Template Splash Screen
In the ``OnCreate`` method of your Activity, after calling ``StartAppAd.init`` and before ``setContentView``, call the following static function:
```java
StartAppAd.showSplash(this, savedInstanceState, splashConfig);
```

Apply the following parameters:
+ **``this``**: The context (Activity)
+ **``savedInstanceState``**: The Bundle parameter passed to your ``onCreate(Bundle savedInstanceState)`` method.
+ **``splashConfig``**: Optional object that can be used to customize some of your template's properties to suit your needs, such as your application name, logo and theme (see the example below). For a full description of the SplashConfig API, please refer to [SplashConfig API](SplashConfig-API).

**Example:** the following is an example of a custom template with an OCEAN theme, modified application name, logo and landscape orientation:
```java
StartAppAd.showSplash(this, savedInstanceState, 
          new SplashConfig()
                 .setTheme(SplashConfig.Theme.OCEAN)
                 .setAppName("Yout Application Name")  
                 .setLogo(R.drawable.your_360x360_logo)   // resource ID
                 .setOrientation(SplashConfig.Orientation.LANDSCAPE)
);
```
> **NOTE:** for optimal appearance of your Splash screen on all device densities, provide a logo of 360x360px and place it in the __drawable__ folder of your project. If this folder does not exist, then create it.

> <img src="./Android/images/drawable.png" /><br></br>
> If you do not provide a logo, then StartApp In-App uses the default application icon (as declared in the Manifest) and stretches it to 360x360px.

####Adding a User-Defined Splash Screen 
Use the following option if you already have a Splash screen for your application or if you want to design your own custom layout for your Splash screen.
**1** 	Set a **SplashConfig** object with a specific layout resource ID.
**2** 	Pass on the **SplashConfig** object to the ```showSplash``` static function. 
For a full description of the SplashConfig API, please refer to [SplashConfig API](SplashConfig-API).

**Example**
```java
StartAppAd.showSplash(this, savedInstanceState, 
     new SplashConfig()
            .setTheme(SplashConfig.Theme.USER_DEFINED)
            .setCustomScreen(R.layout.your_splash_screen_layout_id)
);
```

[Back to top](#top)

##SplashConfig API
The following describes the methods that you can use for customizing the Splash screen displayed in a StartApp In-App Splash screen Ad.

####► Set the Splash screen mode
> **```public SplashConfig setTheme(SplashConfig.Theme theme)```**

Sets the Splash theme to Template mode or User-defined mode. 
Use one of the first five options below to specify a design theme for the Template mode. The last option sets the mode to User-Defined. You may refer to [Customizing the Splash Screen](CustomizingSplashScreen) for more information about Splash screen modes.

**Parameters**<br></br>
*SplashConfig.Theme.DEEP_BLUE* (default)<br></br>
*SplashConfig.Theme.SKY*<br></br>
*SplashConfig.Theme.ASHEN_SKY*<br></br>
*SplashConfig.Theme.BLAZE*<br></br>
*SplashConfig.Theme.GLOOMY*<br></br>
*SplashConfig.Theme.OCEAN* <br></br>
*SplashConfig.Theme.USER_DEFINED* – user-defined mode<br></br>

####► Set a Custom screen
> **```public SplashConfig setCustomScreen(int resource)```**<br></br>

Sets the splash layout to Custom mode. This is mandatory if you are using SplashConfig.Theme.USER_DEFINED.

**Parameters**<br></br>
*Layout Resource ID*

####► Set the application name
> **```public SplashConfig setAppName(String appName)```**<br></br>

Sets the application name to be used in the Template mode.

**Parameters**<br></br>
*String* (default is the application name from the manifest).

####► Set the logo
> **```public SplashConfig setLogo(int resource)```**<br></br>

Sets the logo to be displayed in the Template mode.

**Parameters**<br></br>
*Drawable resource ID* (default is the icon resource from the manifest).

####► Set the orientation
> **```public SplashConfig setOrientation(SplashConfig.Orientation orientation)```**<br></br>

Sets the orientation to be used in the Template or User-defined mode.

**Parameters**<br></br>
*SplashConfig.Orientation.PORTRAIT* (default)<br></br>
*SplashConfig.Orientation.LANDSCAPE*<br></br>
*SplashConfig.Orientation.AUTO* (use the device's orientation upon entering the application)<br></br>

[Back to top](#top)



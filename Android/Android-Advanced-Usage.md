<a name="top" />
######This section describes advanced usage and personal customization options and is not mandatory for the integration.

<a name="HideBanner" />
##Hiding the Banner
In order to hide an already displayed banner, find the banner's view and use the ``hideBanner()`` method. 

**For example:**  
```java
Banner banner = (com.startapp.android.publish.banner.Banner) findViewById(R.id.startAppBanner); 
banner.hideBanner();
```

Where ``R.id.startAppBanner`` is the banner's id from the layout XML file.

In order to show the banner again, simply use the showBanner() method.

[Back to top](#top)

<a name="SelectBanner" />
##Selecting Banner Type
Three types of banners are provided, as follows:

**Banner Type** | **Description**
---------------------- | ---------------
Automatic Banner **(Recommended)**  | Automatic selects the most suitable banner of the two listed below
Standard (2D) Banner  | A standard (two dimensional) banner
3D Banner   | A three dimensional rotating banner

We highly recommend adding an Automatic banner, which automatically selects whether to display a Standard banner or a 3D banner. The banner remains displayed throughout the entire Activity life-cycle. 
To add the automatic banner, please refer to [Banners](Android-InApp-Documentation#banners). If you do not wish to add the Automatic Banner, use one of the following options:

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

<a name="AddBannerProgrammatically" />
##Adding a banner programmatically 
You can add a banner programmatically, instead of using the layout XML.

For example, this is a basic example of adding a center-aligned banner to the bottom of the layout:
```java
// Get the Main relative layout of the entire activity
RelativeLayout mainLayout = (RelativeLayout)findViewById(R.id.mainLayout);   
// Define StartApp Banner
Banner startAppBanner = new Banner(context);
RelativeLayout.LayoutParams bannerParameters =
			new RelativeLayout.LayoutParams(
						RelativeLayout.LayoutParams.WRAP_CONTENT,
						RelativeLayout.LayoutParams.WRAP_CONTENT);
bannerParameters.addRule(RelativeLayout.CENTER_HORIZONTAL);
bannerParameters.addRule(RelativeLayout.ALIGN_PARENT_BOTTOM);    
// Add to main Layout
mainLayout.addView(startAppBanner, bannerParameters);
```

[Back to top](#top)


<a name="BannerCallbacks" />
##Adding Banner Callbacks
If you implemented the banner via the Activity Layout XML, obtain the banner's view (using ``findViewById()``, for example) and pass an implementation of a ``BannerListener`` to its ``setBannerListener()`` method. The ``BannerListener`` must implement the following methods:

```java
@Override
public void onReceiveAd(View banner) {
}
@Override
public void onFailedToReceiveAd(View banner) {
}
@Override
public void onClick(View banner) {
}
```

If you implemented the banner programmatically, simply pass an implementation of a ``BannerListener`` to the banner's constructor:
```java
Banner startAppBanner = new Banner(context, new BannerListener() {
	@Override
	public void onReceiveAd(View banner) {
	}
	@Override
	public void onFailedToReceiveAd(View banner) {
	}
	@Override
	public void onClick(View banner) {
	}
});
```

> **IMPORTANT:**
> Do not call loadAd() from within onFailedToReceiveAd(). The SDK will automatically try to reload an ad upon a failure. 

[Back to top](#top)


<a name="InterstitialAsAnObject" />
##Interstitial Ads
Implement interstitial ads with objects for gaining more control over your ads, like using callbacks or using multiple ads with different properties.   

To initialize an interstitial ad object, create a member variable in your activity as follows:
```java
private StartAppAd startAppAd = new StartAppAd(this);
```

You can now use this object to perform the following operations:

###Showing Exit Ads
To show an ad upon exiting your application when pressing the 'Back' button, override the ```onBackPressed()``` method and add the method ```startAppAd.onBackPressed()``` BEFORE the method ```super.onBackPressed()```:
```java
@Override
public void onBackPressed() {
    startAppAd.onBackPressed();
    super.onBackPressed();
}
```

###Showing Interstitials
Call ```showAd()``` in the appropriate place(s) in the activity where you would like to show the Ad:
```java
startAppAd.showAd(); // show the ad
```

The following is an example of showing an Interstitial Ad between Activities:
```java
public void btnOpenActivity (View view){
    Intent nextActivity = new Intent(this, NextActivity.class);
    startActivity(nextActivity);
    startAppAd.showAd();
}
```

<a name="SelectInterstitial" />
###Selecting Interstitial Ad Type
We highly recommend using our Automatic mode, which automatically selects the best Interstitial Ad to display, meaning the type of Ads that will generate the most revenue for you.  
To add an automatic Interstitial Ad, please refer to [Interstitial Ads](Android-InApp-Documentation#interstitial). 
If you do not wish to use the automatic mode, ``startAppAd.loadAd()`` can be directed to load specific Ads to be shown later using the AdMode parameter. The options for the AdMode parameter are:

**Parameter Name** | **Description** | **Specific Ad Load Example**
---------------------- | ---------------------- | ---------------------- 
AUTOMATIC **(Recommended)** | Auto-selection of the best next Interstitial Ad to display, meaning the type of Ads that will generate the most revenue for you. This is the default | ``startAppAd.loadAd(AdMode.AUTOMATIC)``
OFFERWALL | Auto-selection of a Standard 2D full screen Offer Wall or a 3D Offer Wall | ``startAppAd.loadAd(AdMode.OFFERWALL)``

When using this mode, the following additional methods must be implemented in the Activity’s life-cycle:

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

> **NOTE:** 
> Add this method immediately before the method ``super.onRestoreInstanceState(savedInstanceState)``.

**Example**
```java
@Override
protected void onRestoreInstanceState (Bundle savedInstanceState){
   startAppAd.onRestoreInstanceState(savedInstanceState);
   super.onRestoreInstanceState(savedInstanceState);
}
```
	
<a name="CloseInterstitial" />
###Explicitly Closing an Interstitial Ad
You can explicitly close an interstitial Ad by calling ``startAppAd.close()``. This closes the Ad and returns control to the calling Activity. You can use this when implementing a timeout for an Ad.

> **NOTE:** 
> Keep in mind that the user can close the Ad before timeout expires

[Back to top](#top)

<a name="AddingInterstitialCallbacks" />
###Adding Interstitial Callbacks
####Adding a Callback when an Interstitial Ad is loaded
``startAppAd.loadAd()`` can be called before showing the ad, and get an implementation of ``AdEventListener`` as a parameter.
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

> **IMPORTANT:**
> Do not call loadAd() from within onFailedToReceiveAd(). The SDK will automatically try to reload an ad upon a failure. 


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

@Override
public void adClicked(Ad ad) {
}

@Override
public void adNotDisplayed(Ad ad) {
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
    @Override
    public void adClicked(Ad ad) {
    }
    @Override
    public void adNotDisplayed(Ad ad) {
    }
});
```

<a name="RewardedVideo" />
###Showing Rewarded Video Ads
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
+ **``splashConfig``**: Optional object that can be used to customize some of your template's properties to suit your needs, such as your application name, logo and theme (see the example below). For a full description of the SplashConfig API, please refer to [SplashConfig API](#SplashConfig-API).

**Example:** the following is an example of a custom template with an OCEAN theme, modified application name, logo and landscape orientation:
```java
StartAppAd.showSplash(this, savedInstanceState, 
          new SplashConfig()
                 .setTheme(SplashConfig.Theme.OCEAN)
                 .setAppName("Your Application Name")  
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

For a full description of the SplashConfig API, please refer to [SplashConfig API](#SplashConfig-API).

**Example**
```java
StartAppAd.showSplash(this, savedInstanceState, 
     new SplashConfig()
            .setTheme(SplashConfig.Theme.USER_DEFINED)
            .setCustomScreen(R.layout.your_splash_screen_layout_id)
);
```

[Back to top](#top)

<a name="SplashConfig-API" />
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


<a name="using-native-ads" />
##Integrating Native Ads
####Initializing and Loading a StartAppNativeAd Object
In your Activity, create a member variable, as follows:
```java
private StartAppNativeAd startAppNativeAd = new StartAppNativeAd(this);
```
To load your native ad, call the loadAd() method with a NativeAdPreferences object:
```java
startAppNativeAd.loadAd(new NativeAdPreferences());
```

**NativeAdPreferences** can be used to customize some of the native ad properties to suit your needs, such as the number of ads to load, the image size of the ad, or whether the image should be pre-cached or not. For a full description of the **NativeAdPreferences**, please refer to [NativeAdPreferences API](#NativeAdPreferencesAPI).

> **NOTE:** By default, **StartAppNativeAd** retrieves the image URL of the ad. The SDK is also capable of auto-loading the image as a BITMAP object. This feature is turned off by default. For enabling it, set _``autoBitmapDownload``_ in **NativeAdPreferences** to true (please refer to [Ad's image configuration](#image-conf)).

You can register your **startAppNativeAd** object for callbacks by passing an **AdEventListener** object to the ``loadAd()`` method:
```java
startAppNativeAd.loadAd(new NativeAdPreferences(), new AdEventListener() {
	  @Override
	  public void onReceiveAd(Ad arg0) {
			// Native Ad Received
	  }
	  
	  @Override
	  public void onFailedToReceiveAd(Ad arg0) {
			// Native Ad failed to receive
	  }
});
```

####Using the Native Ad Object
After initializing and  loading your  **startAppNativeAd** object, use the ``getNativeAds()`` method to obtain an array of **NativeAdDetails** objects for all returning ads. The **NativeAdDetails** object provides access to each ad's details, such as the ad's title, description, image, etc.  This object also provides methods for firing an impression once the ad is displayed, and for executing the user's click on the ad. For a full description of the **NativeAdDetails** object, please refer to [NativeAdDetails API](#NativeAdDetailsAPI).

**Example:** the following is an example of how to load 3 native ads with a pre-cached images of 150x150 pixels size, and logging their details once ready (using callbacks)

```java
// Declare Native Ad Preferences
NativeAdPreferences nativePrefs = new NativeAdPreferences()
										  .setAdsNumber(3)                // Load 3 Native Ads
										  .setAutoBitmapDownload(true)    // Retrieve Images object
										  .setPrimaryImageSize(2);        // 150x150 image

// Declare Ad Callbacks Listener
AdEventListener adListener = new AdEventListener() {     // Callback Listener
	  @Override
	  public void onReceiveAd(Ad arg0) {              
			// Native Ad received
			ArrayList<NativeAdDetails> ads = startAppNativeAd.getNativeAds();    // get NativeAds list
			
			// Print all ads details to log
			Iterator<NativeAdDetails> iterator = ads.iterator();
			while(iterator.hasNext()){
				  Log.d("MyApplication", iterator.next().toString());
			}
	  }
	  
	  @Override
	  public void onFailedToReceiveAd(Ad arg0) {
			// Native Ad failed to receive
			Log.e("MyApplication", "Error while loading Ad");
	  }
};

// Load Native Ads
startAppNativeAd.loadAd(nativePrefs, adListener);
```

> **Note:** It is possible to get less ads than you requested. It is also possible that no ad will be returned. In this case you will receive an empty array.

####Showing and Clicking a Native Ad
+ Once you decide to actually show a native ad, you must call the ``NativeAdDetails.sendImpression()`` method.
+ Once the user clicks on the ad, you must call ``NativeAdDetails.sendClick()`` method.

<a name="NativeAdPreferencesAPI" />
####NativeAdPreferences API:

#####► Set the number of Native ads to retrieve
> **```public NativeAdPreferences setAdsNumber(int adsNumber)```**<br></br>

set number of native ads to be received from the server.

######Parameters<br></br>
*adsNumber* - integer of the ads number<br></br>

######Return Value<br></br>
*NatvieAdPreferences* – current object

<a name="image-conf" />
#####► Ad's image configuration
> **``public NativeAdPreferences setAutoBitmapDownload(boolean autoBitmapDownload)``**<br></br>

You can choose between two options to obtain the ad's image: 

1. get the image pre-cached as a BITMAP. <br></br>
2. get the image URL only.<br></br>

######Parameters<br></br>
*autoBitmapDownload* - Boolean:
+ true – native ad object will be loaded automatically with bitmap object
+ false – native ad wont load the image automatically

######Return Value<br></br>
*NatvieAdPreferences* – current object


#####► Set Ad's image size
> **``public NativeAdPreferences setPrimaryImageSize(int imageSize)``**<br></br>

Set the image size of the ad to be retrieved.

######Parameters<br></br>
*imageSize* - imageSize can get the following values: <br></br>
+ 0 – for image size 72px X 72px <br></br>
+ 1 – for image size 100px X 100px <br></br>
+ 2 – for image size 150px X 150px <br></br>
+ 3 – for image size 340px X 340px <br></br>
+ 4 – for image size 1200px X 628px <br></br>

######Return Value<br></br>
*NatvieAdPreferences* – current object

#####► Set Ad's secondary icon size
> **``public NativeAdPreferences setSecondaryImageSize(int imageSize)``**<br></br>

Set a secondary icon size of the ad to be retrieved.

######Parameters<br></br>
*imageSize* - imageSize can get the following values: <br></br>
+ 0 – for image size 72px X 72px <br></br>
+ 1 – for image size 100px X 100px <br></br>
+ 2 – for image size 150px X 150px <br></br>
+ 3 – for image size 340px X 340px <br></br>
+ 4 – for image size 1200px X 628px <br></br>

######Return Value<br></br>
*NatvieAdPreferences* – current object

<a name="NativeAdDetailsAPI" />
####NativeAdDetails API:
#####► Get the Ad's title
> **``public String getTitle()``**<br></br>

######Return Value: String

#####► Get the Ad's description
> **``public String getDescription()``**<br></br>

######Return Value: String

#####► Get the Ad's rating
> **``public String getRating()``**<br></br>

Get the rating of the ad in the Google Play store. The rating range is 1-5.

######Return Value: Float

#####► Get the Ad's image URL
> **``public String getImageUrl()``**<br></br>

Get the image URL of the ad, according to the selected size.

######Return Value: String


#####► Get the Ad's image bitmap
> **``public Bitmap getImageBitmap()``**<br></br>

Get the image of the ad as a pre-cached bitmap, if requested using the NativeAdPreferences.setAutoBitmapDownload() method.

######Return Value: Bitmap


#####► Get the Ad's secondary icon URL
> **``public String getSecondaryImageUrl()``**<br></br>

Get the secondary icon URL of the ad, according to the selected size.

######Return Value: String


#####► Get the Ad's secondary icon bitmap
> **``public Bitmap getSecondaryImageBitmap()``**<br></br>

Get the secondary icon of the ad as a pre-cached bitmap, if requested using the NativeAdPreferences.setAutoBitmapDownload() method.

######Return Value: Bitmap


#####► Get the Ad's installs numbers
> **``public String getInstalls()``**<br></br>

Get the amount of installs in Google Play store.

######Return Value: String


#####► Get the Ad's category
> **``public String getCategory()``**<br></br>

Get the category of the ad in the Google Play store.

######Return Value: String

#####► Get the Ad's package name
> **``public String getPackacgeName()``**<br></br>

Get the ad's package name in the Google Play store (for example, "com.startapp.quicksearchbox").

######Return Value: String

#####► To be called when you actually show the ad 
> **``public void sendImpression(Context context)``**<br></br>

Call this method when you show the ad in your application. 

######Parameters<br></br>
*context* – the context of the host activity

#####► To be called when the user clicks on the ad
> **``public void sendClick(Context context)``**<br></br>

Call this method when the user clicks on the ad.  

######Parameters<br></br>
*context* – the context of the host activity

[Back to top](#top)

<a name="Demographic" />
##Enjoy Higher eCPM with Demographic-Targeted Ads
If you know your user's gender or age, StartApp can use it to serve better-targeted ads which can increase your eCPM and revenue significantly.  

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


[Back to top](#top)


<a name="top">

<a name="hide-banner" />
###Hiding your banner
You can hide and show your banner in run time, using ``showBanner`` and ``hideBanner`` methods:
```objectivec
startappiOS* startAppBridge =  startappiOS().sharedInstance();
startAppBridge->hideBanner();
startAppBridge->showBanner();
startAppBridge->isVisible();
```

[Back to top](#top)

<a name="ControllingBannerSize" />
###Controlling the size of your banner
The size of the banner is determined by the second parameter which can receive one of the following values

Value | Size | Best fits for
--- | --- | ---
*`STA_AutoAdSize`* | Auto-size (recommended) | detects the width of the device's screen in its current orientation, and provides the optimal banner for this size
*`STA_PortraitAdSize_320x50`* | 320x50 | iPhone/iPod touch in portrait mode
*`STA_LandscapeAdSize_480x50`* | 480x50 | iPhone/iPod touch in landscape mode
*`STA_PortraitAdSize_768x90`* | 768x90 | iPad in portrait mode
*`STA_LandscapeAdSize_1024x90`* | 1024x90 | iPad in landscape mode

For example:
```cpp
startAppBridge->loadBanner(startappiOS::STAAdOrigin_Bottom, 
						   startappiOS::STA_PortraitAdSize_320x50);
```

You can load a new size for an existing banner, by using the ``setSTABannerSize`` method:
```cpp
startAppBridge->setSTABannerSize(startappiOS::STA_PortraitAdSize_320x50);
```

[Back to top](#top)

<a name="UsingFixedOriginBanner" />
###Using a fixed origin for your banner
If you choose to locate the banner in a fixed origin rather than the view's top or bottom, simply pass the required origin point (x,y) upon initialization as explain in the following example

Locating your banner 100 pixels above the view's bottom:
```cpp
startAppBridge->setOrigin(0, self.view.frame.size.height - 100);
```

You can change the origin of an existing banner, by using the ``setSTABannerSize``/``setSTAAutoOrigin`` methods:
```cpp
startAppBridge->setSTAAutoOrigin(startappiOS::STAAdOrigin_Top);
```

or
```cpp
startAppBridge->setOrigin(0, 50);
```

[Back to top](#top)

<a name="UsingBannerDelegates" />
###Using banner delegates
Set your view controller as a delegate so it is able to receive callbacks from the banner ad.

1. Add the STABannerDelegateProtocol to the header file
 ```objectivec
 @interface YourViewController : UIViewController <STABannerDelegateProtocol>
 {
     STABannerView* bannerView;  
 } 
 ```

2. Use "withDelegate:self" when initializing the STABannerView object:
 ```objectivec
 bannerView = [[STABannerView alloc] initWithSize:STA_AutoAdSize 
                                      autoOrigin:STAAdOrigin_Top                 
                                      withView:self.view 
                                      withDelegate:self];
 ```

3. Implement the following functions:
 ```objectivec
- (void) didDisplayBannerAd:(STABannerView*)banner;
- (void) failedLoadBannerAd:(STABannerView*)banner withError:(NSError *)error;
- (void) didClickBannerAd:(STABannerView*)banner;
 ```

[Back to top](#top)


<a name="UsingInterstitialDelegate" />
###Using Interstitial delegates
Set your view controller as a delegate so it is able to receive callbacks from the interstitial ad

1. Add the STADelegateProtocol to the header file
 ```objectivec
 @interface YourViewController : UIViewController <STADelegateProtocol>
 {
     STAStartAppAd* startAppAd;    
 } 
 ```

2. Use "withDelegate:self" when calling the loadAd function:
 ```[startAppAd loadAd:STAAdType_Automatic withDelegate:self]```

3. Implement the following functions:
 ```objectivec
 - (void) didLoadAd:(STAAbstractAd*)ad;
 - (void) failedLoadAd:(STAAbstractAd*)ad withError:(NSError *)error;
 - (void) didShowAd:(STAAbstractAd*)ad;
 - (void) failedShowAd:(STAAbstractAd*)ad withError:(NSError *)error;
 - (void) didCloseAd:(STAAbstractAd*)ad;
 - (void) didClickAd:(STAAbstractAd*)ad;
 ```

[Back to top](#top)

<a name="CustomizingSplashScreen" />
##Customizing your Splash Screen
You can customize the appearance of your splash screen using the ``STASplashPreferences`` object, as describes below. In order to use splash preferences, use the ``showSplashAdWithPreferences`` method when initializing the splash screen in your _AppDelegate_ class.

For example - using splash preferences to choose template mode:
```objectivec
STASplashPreferences *splashPreferences = [[STASplashPreferences alloc] init];
splashPreferences.splashMode = STASplashModeTemplate;
[sdk showSplashAdWithPreferences:splashPreferences];
```

###Splash Preferences API
The following API describes all customization options available for the splash screen.

####►Splash screen mode
Decide whether to use user-defined or template mode.

**Parameter:** _splashMode_

**Values:**   
_STASplashModeUserDefined_  
_STASplashModeTemplate_  

**Usage:**  
``splashPreferences.splashMode = STASplashModeTemplate;``

####►Change splash image (for user-defined mode)
Change the splash screen image, instead of using the default one. 

**Parameter:** _splashUserDefinedImageName_  

**Usage:**  
``splashPreferences.splashUserDefinedImageName = @"MyImage";``  

####►Choosing splash template (for template mode)
Choose of of 6 pre-designed templates.

**Parameter:** _splashTemplateTheme_  

**Values:**   
_STASplashTemplateThemeDeepBlue_  
_STASplashTemplateThemeSky_  
_STASplashTemplateThemeAshenSky_  
_STASplashTemplateThemeBlaze_  
_STASplashTemplateThemeGloomy_  
_STASplashTemplateThemeOcean_  

**Usage:**  
``splashPreferences.splashTemplateTheme = STASplashTemplateThemeBlaze;``  

####►Changing template's icon and title (for template mode)
The SDK uses your default application's name and icon. You can choose however to use your own assets.

**Parameters:**   
_splashTemplateIconImageName_   
_splashTemplateAppName_    

**Usage:**  
```objectivec
splashPreferences.splashTemplateIconImageName = @"MyIcon";
splashPreferences.splashTemplateAppName = @"MyAppName";
```


####►Enable/Disable loading indicator (for user-defined mode)
Choose whether to display a loading indicator on the splash screen.

**Parameter:** _isSplashLoadingIndicatorEnabled_  

**Values:**   
_YES_  
_NO_  

**Usage:**  
``splashPreferences.isSplashLoadingIndicatorEnabled = YES;``  

####►Choose loading indicator's type (for user-defined and template modes)
Choose which loading indicator type to display: iOS default activity indicator or a "dots" loading indicator

**Parameter:** _splashLoadingIndicatorType_  

**Values:**   
_STASplashLoadingIndicatorTypeIOS_  
_STASplashLoadingIndicatorTypeDots_  

**Usage:**  
``splashPreferences.splashLoadingIndicatorType = STASplashLoadingIndicatorTypeDots;``  

####►Change loading indicator's position (for user-defined mode)
The loading indicator is displayed by default on the center of the screen. You can choose however to set a custom position.

**Parameter:** _splashLoadingIndicatorCenterPoint_  

**Values:**   
_CGPointMake(x, y)_  

**Usage:**  
``splashPreferences.splashLoadingIndicatorCenterPoint = CGPointMake(100, 100);``  


####►Force landscape orientation (for user-defined and template modes)
The SDK display the splash screen using the orientation supported by the application and the device real orientation. You can choose however to force landscape orientation.

**Parameter:** _isLandscape_  

**Values:**   
_YES_  
_NO_  

**Usage:**  
``splashPreferences.isLandscape = YES;``  

[Back to top](#top)


<a name="using-native-ads" />
##Integrating Native Ads
###Initializing and Loading Native Ads  
First, import the StartApp SDK in your view controller and add the following lines to the header file for each view in which you would like to use StartApp Native ad.
```objectivec
// YourViewController.h

#import <StartApp/StartApp.h>

@interface YourViewController : UIViewController 
{
    STAStartAppNativeAd *startAppNativeAd;    // ADD THIS LINE
} 
```

In your view controller, initialize **STAStartAppNativeAd** within the ``viewDidLoad()`` method and load the ad with your selected preferences.
```objectivec
// YourViewController.m 

- (void)viewDidLoad 
{
    [super viewDidLoad];
    startAppNativeAd = [[STAStartAppNativeAd alloc] init];
    [startAppNativeAd loadAd];
}
```

You can check if the ad has been loaded, using  ``startAppNativeAd.adIsLoaded;``.

###Using Native Ad delegates
Set your view controller as a delegate so it is able to receive callbacks from the native ad.

1. Add the STADelegateProtocol to the header file
 ```objectivec
 @interface YourViewController : UIViewController <STADelegateProtocol>
 {
     STAStartAppNativeAd *startAppNativeAd;    
 } 
 ```

2. Implement the following functions
 ```objectivec
 - (void) didLoadAd:(STAAbstractAd*)ad;
 - (void) failedLoadAd:(STAAbstractAd*)ad withError:(NSError *)error;
 ```

3. Load an ad using ``loadAdWithDelegate``
 ```objectivec
 [startAppNativeAd loadAdWithDelegate:self];
 ```

###Using Native Ad Preferences
**STANativeAdPreferences** can be used to customize some of the native ad properties to suit your needs, such as the number of ads to load, the image size of the ad, or whether the image should be pre-cached or not. For a full description of the NativeAdPreferences object, please refer to [NativeAdPreferences API](#NativeAdPreferencesAPI).

In order to use the STANativeAdPreferences object, simply use it when loading the ad:
```
[startAppNativeAd loadAdWithNativeAdPreferences:pref];
```

Example: load 4 native ads with 100x100 icons, send lat/lon and register for callbacks:  
```objectivec
// YourViewController.m 

- (void)viewDidLoad 
{
    [super viewDidLoad];
    startAppNativeAd = [[STAStartAppNativeAd alloc] init];
    STANativeAdPreferences *pref = [[STANativeAdPreferences alloc]init];
    pref.adsNumber = 4; // Select ads number
    pref.bitmapSize = SIZE_100X100;     //Select image quality
    pref.autoBitmapDownload = YES;    // When set to NO no images will be downloaded by the SDK
	
    // You can pass longitude/latitude if available 
	pref.userLocation.latitude = 31.776719;
    pref.userLocation.longitude = 35.234508;
	
    [startAppNativeAd loadAdWithDelegate:self withNativeAdPreferences:pref]; 
}
```

###Using the Native Ad Object
After initializing and loading your **STAStartAppNativeAd**  object, use the **STANativeAdDetails** object to get details of all returning ads. The STANativeAdDetails object provides access to each ad's details, such as the ad's title, description, image, etc. This object also provides methods for firing an impression once the ad is displayed, and for executing the user's click on the ad. For a full description of the **STAStartAppNativeAd** object, please refer to [NativeAdDetails API](#NativeAdDetailsAPI).

Example: get some details of the 3rd ad.
```objectivec
STANativeAdDetails *adDetails = [startAppNativeAd.adsDetails objectAtIndex:3];
titleLabel.text=[[startAppNativeAd.adsDetails objectAtIndex:3] title];
descriptionLabel.text=[[startAppNativeAd.adsDetails objectAtIndex:3] description];
imageView.image=[[startAppNativeAd.adsDetails objectAtIndex:3] imageBitmap];
```

> **Note:** It is possible to get less ads than you requested. It is also possible that no ad will be returned. In this case you will receive an empty array.

###Showing and Clicking a Native Ad
Once you decide to actually show a native ad, you must call the ``[adDetails sendImpression]`` method.  
  
Once the user clicks on the ad, you must call ``[adDetails sendClick]`` method.  

<a name="NativeAdPreferencesAPI" />
###NativeAdPreferences API
Parameter name | Description | Values
--- | --- | ---
*`adsNumber `* | number of native ads to be retrieved | a number between 1-10
*`bitmapSize `* | the size of the icon's bitmap to be retrieved | SIZE72X72, SIZE100X100, SIZE150X150, SIZE340X340
*`autoBitmapDownload `* | Select the method for retrieving the ad's icon. You can get the icon's URL only, or pre-cache it into a bitmap object | "YES"=pre cached, "NO"=URL only
*`userLocation.latitude`* | the device's latitude  | latitude
*`userLocation.longitude`* | the device's longitude | longitude

<a name="STANativeAdDetailsAPI" />
###STANativeAdDetails API
Parameter name | Description | Return value
--- | --- | ---
*`title`* | Get the Ad's title | NSString
*`description`* | Get the Ad's description | NSString
*`imageBitmap`* | Get the actual icon of the ad, according to the selected size (if autoBitmapDownload="YES") | UIImage
*`imageUrl`* | Get the icon URL of the ad, according to the selected size (if autoBitmapDownload="NO") | NSString
*`category`* | Get the category of the ad in the App Store | NSString 
*`adId`* | Get the ad's package name in the App Store | NSString
*`rating`* | Get the rating of the ad in the App Store. The rating range is 1-5 | NSNumber 


[Back to top](#top)






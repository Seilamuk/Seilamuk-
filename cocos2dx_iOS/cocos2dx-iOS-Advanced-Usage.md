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

<a name="UsingDelegates" />
###Using banner delegates
You can get various callbacks from StartApp Ads, by implementing a callback method and pass it when loading the ad.

1. Add the following line to your header file where you would like to catch callbacks:
 ```cpp
 static void STACallbacks(const char *eventName , const char *eventError);
 ```

2. Then, add the following method to the relevant cpp file:
 ```cpp
 void HelloWorld::STACallbacks(const char *eventName , const char *eventError)
 {
    if(0==strcmp(eventName, "\<CALLBACK_NAME\>")){
        ...
    }	
 }
 ```

 Where <CALLBACK_NAME> is one of the supported callbacks listed in the next section.
 
3. Pass _STACallbacks_ when loading the ad:
 **For Splash:** 
 ```cpp
 startAppBridge->showSplashAd(STACallbacks);
 ```
 **For Interstitial:** 
 ```cpp
 startAppBridge->loadAd(STACallbacks);
 ```
 **For Banner:** 
 ```cpp
 startAppBridge->loadBanner(startappiOS::STAAdOrigin_Bottom, 
							startappiOS:: STA_AutoAdSize, 
							STACallbacks);
 ```
 
###Callbacks List  
####Splash callbacks  
<img src="./iOS/images/V.png" width="12px" /> didLoadSplashAd  
<img src="./iOS/images/V.png" width="12px" /> failedLoadSplashAd  
<img src="./iOS/images/V.png" width="12px" /> didShowSplashAd  
<img src="./iOS/images/V.png" width="12px" /> failedShowSplashAd  
<img src="./iOS/images/V.png" width="12px" /> didCloseSplashAd  
<img src="./iOS/images/V.png" width="12px" /> didClickSplashAd  

####Interstitial callbacks  
<img src="./iOS/images/V.png" width="12px" /> didLoadAd  
<img src="./iOS/images/V.png" width="12px" /> failedLoadAd  
<img src="./iOS/images/V.png" width="12px" /> didShowAd  
<img src="./iOS/images/V.png" width="12px" /> failedShowAd  
<img src="./iOS/images/V.png" width="12px" /> didCloseAd  
<img src="./iOS/images/V.png" width="12px" /> didClickAd  

####Banner callbacks  
<img src="./iOS/images/V.png" width="12px" /> didDisplayBannerAd  
<img src="./iOS/images/V.png" width="12px" /> failedLoadBannerAd  
<img src="./iOS/images/V.png" width="12px" /> didClickBannerAd  

[Back to top](#top)

<a name="CustomizingSplashScreen" />
##Customizing your Splash Screen
You can customize the appearance of your splash screen using the ``STASplashPreferences`` object, as describes below. After setting your required preferences, pass it to the ``showSplashAd`` method when initializing the splash screen in your _AppDelegate_ class.

For example - using splash preferences to choose template mode:
```cpp
startappiOS::STASplashPreferences *splashPreferences = new startappiOS::STASplashPreferences();
splashPreferences->splashMode = startappiOS::STASplashModeTemplate;
startAppBridge->showSplashAd(splashPreferences);
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
``splashPreferences->splashMode = startappiOS::STASplashModeTemplate;``

####►Change splash image (for user-defined mode)
Change the splash screen image, instead of using the default one. 

**Parameter:** _splashUserDefinedImageName_  

**Usage:**  
``splashPreferences->splashUserDefinedImageName = @"MyImage";``  

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
``splashPreferences->splashTemplateTheme = startappiOS::STASplashTemplateThemeBlaze;``  

####►Changing template's icon and title (for template mode)
The SDK uses your default application's name and icon. You can choose however to use your own assets.

**Parameters:**   
_splashTemplateIconImageName_   
_splashTemplateAppName_    

**Usage:**  
```cpp
splashPreferences->splashTemplateIconImageName = "MyIcon";
splashPreferences->splashTemplateAppName = "MyAppName";
```

####►Enable/Disable loading indicator (for user-defined mode)
Choose whether to display a loading indicator on the splash screen.

**Parameter:** _isSplashLoadingIndicatorEnabled_  

**Values:**   
_YES_  
_NO_  

**Usage:**  
``splashPreferences->isSplashLoadingIndicatorEnabled = true;``  

####►Choose loading indicator's type (for user-defined and template modes)
Choose which loading indicator type to display: iOS default activity indicator or a "dots" loading indicator

**Parameter:** _splashLoadingIndicatorType_  

**Values:**   
_STASplashLoadingIndicatorTypeIOS_  
_STASplashLoadingIndicatorTypeDots_  

**Usage:**  
``splashPreferences->splashLoadingIndicatorType = startappiOS::STASplashLoadingIndicatorTypeDots;``  

####►Change loading indicator's position (for user-defined mode)
The loading indicator is displayed by default on the center of the screen. You can choose however to set a custom position.

**Parameter:** _splashLoadingIndicatorCenterPoint_  

**Values:**   
_CGPointMake(x, y)_  

**Usage:**  
```cpp
splashPreferences->splashLoadingIndicatorCenterPoint.x = 100;
splashPreferences->splashLoadingIndicatorCenterPoint.y = 100;
```


####►Force landscape orientation (for user-defined and template modes)
The SDK display the splash screen using the orientation supported by the application and the device real orientation. You can choose however to force landscape orientation.

**Parameter:** _isLandscape_  

**Values:**   
_YES_  
_NO_  

**Usage:**  
``splashPreferences->isLandscape = true;``  

[Back to top](#top)


<a name="top" />

###### This section describes advanced usage and personal customization options and is not mandatory for the integration.

<a name="hide-banner" />

### Hiding your banner
You can hide and show your banner in run time, using ``showBanner`` and ``hideBanner`` methods:
```csharp
#if UNITY_IPHONE
StartAppWrapperiOS.hideBanner();               
StartAppWrapperiOS.showBanner();
#endif  
```

You can check if your banner is currently visible by using the ``bannerIsVisible()`` method:
```csharp
#if UNITY_IPHONE
StartAppWrapperiOS.bannerIsVisible();
#endif  
```

[Back to top](#top)

<a name="interstitial-customizations" />

## Customizing your Interstitial Ad
You can choose to show a specific type of interstitial ad, as well as getting callbacks events, using the ``STAInterstitialProperties`` object.

### Using Interstitial Callbacks
Use one of your GameObjects as a delegate to get callbacks from the interstitial ad. Even an empty GameObject you create for this purpose will do.

1. Pass the GameObject name on the ``STAInterstitialProperties.delegateName`` member to the ``loadAd()`` method
 ```csharp
 void Start () {
        #if UNITY_IPHONE
		StartAppWrapperiOS.STAInterstitialProperties adProp = new StartAppWrapperiOS.STAInterstitialProperties();
		adProp.delegateName ="Main Camera";
		StartAppWrapperiOS.loadAd(adProp);
        #endif    
 }
 ```

2. Implement the following callbacks in your GameObject script
 ```csharp
public class StartAppGameObject : MonoBehaviour {
    // Use this for initialization
    void Start () {
        #if UNITY_IPHONE
		StartAppWrapperiOS.STAInterstitialProperties adProp = new StartAppWrapperiOS.STAInterstitialProperties();
		adProp.delegateName ="Main Camera";
		StartAppWrapperiOS.loadAd(adProp);
        #endif    
    }
    
   void didLoadAd() {
        #if UNITY_IPHONE
        Debug.Log("didLoadAd");
        #endif    
    }
    void failedLoadAd(string Error) {
        #if UNITY_IPHONE
        Debug.Log("failedLoadAd");
        #endif
    }
    void didShowAd() {
        #if UNITY_IPHONE
        Debug.Log("didShowAd");
        #endif    
    }
    void failedShowAd(string Error) {
        #if UNITY_IPHONE
        Debug.Log("failedShowAd");
        #endif    
    }
    void didCloseAd() {
        #if UNITY_IPHONE
        Debug.Log("didCloseAd");
        #endif    
    }
    void didClickAd() {
        #if UNITY_IPHONE
        Debug.Log("didClickAd");
        #endif    
    }
}
```

### Checking if ad is ready
You can check if your ad is loaded and ready to use, by using the ``isAdReady()`` method:
```csharp
#if UNITY_IPHONE
StartAppWrapperiOS.isAdReady();
#endif 
```

[Back to top](#top)

<a name="banner-customizations" />

## Customizing your Banner Ad
You can control the position of your banner as well as loading a specific size and getting callbacks events, using the ``STABannerProperties`` object.

### Positioning your banner
You can show your banner at the bottom or top of your screen, or use a fixed position.

##### Use a top banner
```csharp
#if UNITY_IPHONE  
StartAppWrapperiOS.STABannerProperties bannerProp = new StartAppWrapperiOS.STABannerProperties();
bannerProp.position = StartAppWrapperiOS.BannerPosition.TOP;
StartAppWrapperiOS.addBanner(bannerProp);
#endif
```

##### Use a bottom banner
```csharp
#if UNITY_IPHONE  
StartAppWrapperiOS.STABannerProperties bannerProp = new StartAppWrapperiOS.STABannerProperties();
bannerProp.position = StartAppWrapperiOS.BannerPosition.BOTTOM;
StartAppWrapperiOS.addBanner(bannerProp);
#endif
```

##### Use a fixed position banner
```csharp
#if UNITY_IPHONE  
StartAppWrapperiOS.STABannerProperties bannerProp = new StartAppWrapperiOS.STABannerProperties();
bannerProp.useFixedPosition = true;
bannerProp.fixedPosition.x = 0;
bannerProp.fixedPosition.y = 100;
StartAppWrapperiOS.addBanner(bannerProp);
#endif
```

### Controlling the size of your banner
The size of the banner is determined by the "size" parameter which can receive one of the following values

Value | Size | Best fits for
--- | --- | ---
*`STA_AutoAdSize`* **(recommended)** | Auto-size | detects the width of the device's screen in its current orientation, and provides the optimal banner for this size
*`STA_PortraitAdSize_320x50`* | 320x50 | iPhone/iPod touch in portrait mode
*`STA_LandscapeAdSize_480x50`* | 480x50 | iPhone/iPod touch in landscape mode
*`STA_PortraitAdSize_768x90`* | 768x90 | iPad in portrait mode
*`STA_LandscapeAdSize_1024x90`* | 1024x90 | iPad in landscape mode

**Example - loading 320x50 top banner**
```csharp
#if UNITY_IPHONE  
StartAppWrapperiOS.STABannerProperties bannerProp = new StartAppWrapperiOS.STABannerProperties();
bannerProp.position = StartAppWrapperiOS.BannerPosition.TOP;
bannerProp.size = StartAppWrapperiOS.BannerSize.STA_PortraitAdSize_320x50;
StartAppWrapperiOS.addBanner(bannerProp);
#endif
```

### Using Banner Callbacks
Use one of your GameObjects as a delegate to get callbacks from the banner ad. Even an empty GameObject you create for this purpose will do.

1. Pass the GameObject name on the ``STABannerProperties.delegateName`` member to the ``addBanner()`` method
 ```csharp
 void Start () {
        #if UNITY_IPHONE
		StartAppWrapperiOS.STABannerProperties bannerProp = new StartAppWrapperiOS.STABannerProperties();
		bannerProp.delegateName ="Main Camera";
		StartAppWrapperiOS.addBanner(bannerProp);
        #endif    
 }
 ```

2. Implement the following callbacks in your GameObject script
 ```csharp
 public class StartAppGameObject : MonoBehaviour {
     // Use this for initialization
     void Start () {
         #if UNITY_IPHONE    
		 StartAppWrapperiOS.STABannerProperties bannerProp = new StartAppWrapperiOS.STABannerProperties();
	     bannerProp.delegateName ="Main Camera";
		 StartAppWrapperiOS.addBanner(bannerProp);
         #endif    
     }
     
     void didDisplayBannerAd() {
         #if UNITY_IPHONE
         Debug.Log("didDisplayBannerAd");
         #endif    
     }
     void failedLoadBannerAd(string Error) {
         #if UNITY_IPHONE
         Debug.Log("failedLoadBannerAd");
         #endif    
     }
     void didClickBannerAd() {
         #if UNITY_IPHONE
         Debug.Log("didClickBannerAd");
         #endif    
     }
 }
 ```

[Back to top](#top)


<a name="CustomizingSplashScreen" />

## Customizing your Splash Screen
You can customize the appearance of your splash screen using the ``STASplashPreferences`` object, as describes below. In order to use splash preferences, use the ``showSplashAd`` with an initialized ``STASplashPreferences`` object.

For example - using splash preferences to choose a template mode with a "blaze" theme:
```csharp
StartAppWrapperiOS.STASplashPreferences splashPreferences = new StartAppWrapperiOS.STASplashPreferences();
splashPreferences.mode = StartAppWrapperiOS.STASplashMode.STASplashModeTemplate;
splashPreferences.templateTheme = StartAppWrapperiOS.STASplashTemplateTheme.STASplashTemplateThemeBlaze;
StartAppWrapperiOS.showSplashAd(splashPreferences);
```

### Splash Preferences API
The following API describes all customization options available for the splash screen.

#### ►Splash screen mode
Decide whether to use user-defined or template mode.

**Parameter:** _mode_

**Values:**   
_STASplashModeUserDefined_  
_STASplashModeTemplate_  

**Usage:**  
``splashPreferences.mode = StartAppWrapperiOS.STASplashMode.STASplashModeTemplate;``

#### ►Choosing splash template (for template mode)
Choose of of 6 pre-designed templates.

**Parameter:** _templateTheme_  

**Values:**   
_STASplashTemplateThemeDeepBlue_  
_STASplashTemplateThemeSky_  
_STASplashTemplateThemeAshenSky_  
_STASplashTemplateThemeBlaze_  
_STASplashTemplateThemeGloomy_  
_STASplashTemplateThemeOcean_  

**Usage:**  
``splashPreferences.templateTheme = STASplashTemplateThemeBlaze;``  


#### ►Changing template's icon and title (for template mode)
The SDK uses your default application's name and icon. You can choose however to use your own assets.

**Parameters:**   
_templateIconImageName_  
_templateAppName_     

**Usage:**  
```objectivec
splashPreferences.templateIconImageName = "MyIcon";
splashPreferences.templateAppName = "MyAppName";
```


#### ►Choose loading indicator's type (for user-defined and template modes)
Choose which loading indicator type to display: iOS default activity indicator or a "dots" loading indicator

**Parameter:** _loadingIndicatorType_  

**Values:**   
_STASplashLoadingIndicatorTypeIOS_  
_STASplashLoadingIndicatorTypeDots_  

**Usage:**  
``splashPreferences.loadingIndicatorType = STASplashLoadingIndicatorTypeDots;``  

#### ►Change loading indicator's position (for user-defined mode)
The loading indicator is displayed by default on the center of the screen. You can choose however to set a custom position.

**Parameter:**   
_loadingIndicatorCenterPoint.x_  
_loadingIndicatorCenterPoint.y_  
 
**Usage:**  
```
splashPreferences.loadingIndicatorCenterPoint.x = 30;
splashPreferences.loadingIndicatorCenterPoint.y = 30;
```

[Back to top](#top)



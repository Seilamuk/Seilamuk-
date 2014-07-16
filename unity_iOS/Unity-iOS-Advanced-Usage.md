<a name="top" />

######This section describes advanced usage and personal customization options and is not mandatory for the integration.

<a name="interstitial-customizations" />
##Customizing your Interstitial Ad
You can choose to show a specific type of interstitial ad, as well as getting callbacks events, using the ``STAInterstitialProperties`` object.

###Selecting Interstitial Ad Type
When calling an interstitial ad, the ad type with the best performance will be automatically selected. If you would like to explicitly choose the type of ad, use the ``STAInterstitialProperties.type`` object when calling ``loadAd()``. 

The options for this parameter are:

Constant Name | Description
--- | --- 
*`StartAppWrapperiOS.AdType.STAAdType_Automatic`* **(recommended)** | Automatic selects the most suitable banner of the two listed below
*`StartAppWrapperiOS.AdType.STAAdType_FullScreen`* | A full-page ad 
*`StartAppWrapperiOS.AdType.STAAdType_OfferWall`* | A full page offerwall 
*`StartAppWrapperiOS.AdType.STAAdType_Overlay`* | An overlay Ad is a full page Ad that runs on top of your application

We highly recommend using the *Automatic* type, which automatically selects the best ad type to display. 

**Example - loading an OfferWall ad**  
```csharp
#if UNITY_IPHONE
StartAppWrapperiOS.STAInterstitialProperties adProp = new StartAppWrapperiOS.STAInterstitialProperties();
adProp.type = StartAppWrapperiOS.AdType.STAAdType_Overlay;
StartAppWrapperiOS.loadAd(adProp);
#endif
```

###Using Interstitial Callbacks
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
}
```

[Back to top](#top)

<a name="banner-customizations" />
##Customizing your Banner Ad
You can control the position of your banner as well as loading a specific size and getting callbacks events, using the ``STABannerProperties`` object.

###Positioning your banner
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

###Controlling the size of your banner
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

###Using Banner Callbacks
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
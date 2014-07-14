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

In order to show the banner again, simply use the addBanner() method as described in ["Showing Banners"](Android-InApp-Unity-Documentation#step5).

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
##Adding a callback when Ad has been shown

``StartAppWrapper.showAd()`` can get an implementation of *AdDisplayListener* as a parameter. In case you want to get a callback for the ad show, pass the object which implements *StartAppWrapper.AdDisplayListener* as a parameter of the method. This object should implement the following methods:
```csharp
public void adHidden(){
}
public void adDisplayed(){
}
```

[Back to top](#top)

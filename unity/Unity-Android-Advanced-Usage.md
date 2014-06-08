<a name="top" />

######This section describes advanced usage and personal customization options and is not mandatory for the integration.

<a name="banner-type" />
##Selecting Banner Type
There are 3 different types of banners:

**Banner Type** | **Description**
---------------------- | ---------------
Automatic Banner **(Recommended)**  | Automatic selects the most suitable banner of the two listed below
Standard (2D) Banner  | A standard (two dimensional) banner
3D Banner   | A three dimensional rotating banner

We highly recommend adding an Automatic banner, which automatically selects whether to display a standard banner or a 3D banner. The banner remains displayed throughout the entire activity life-cycle. 

To add a banner to your application add the following code in the appropriate place:

**1.** Import the SDK namespace
``` java
using StartApp;
```

**2.** Display the banner
```java
void Start () {
     StartAppWrapper.addBanner( 
           StartAppWrapper.BannerType,
	       StartAppWrapper.BannerPosition);
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

[Back to top](#top)

<a name="load-callback" />
##Adding a callback when Ad has been loaded

``StartAppWrapper.loadAd()`` can get an implementation of *AdEventListener* as a parameter. In case you want to get a callback for the ad load, pass the object which implements *StartAppWrapper.AdEventListener* as a parameter to the method. This object should implement the following methods:
```java
public void onReceiveAd(){
}
public void onFailedToReceiveAd(){
}
```

<a name="show-callback" />
##Adding a callback when Ad has been loaded

``StartAppWrapper.showAd()`` can get an implementation of *AdDisplayListener* as a parameter. In case you want to get a callback for the ad show, pass the object which implements *StartAppWrapper.AdDisplayListener* as a parameter of the method. This object should implement the following methods:
```java
public void adHidden(){
}
public void adDisplayed(){
}
```

[Back to top](#top)

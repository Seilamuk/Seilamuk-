<a name="top" />
######This section describes advanced usage and personal customization options and is not mandatory for the integration.

<a name="home-back" />
##Using Home and Back separately
You can use the exit ad with home or back separately, just do one of the following:

<br></br><img src="./iOS/images/V.png" width="12px" /> Add the StartApp 'home' plugin to your Unity project:
The plugin will show an ad when a user presses the 'home' button
*1.* Copy the _StartAppHomePlugin.cs_ to the Assets folder.
*2.* Drag the _StartAppHomePlugin.cs_ to a components in your scene.
<br></br><img src="./iOS/images/V.png" width="12px" /> Add the StartApp 'back' plugin to your Unity project:
The plugin will show an ad when a user presses the 'back' button
*1.* Copy the _StartAppBackPlugin.cs_ to the Assets folder.
*2.* Drag the _StartAppBackPlugin.cs_ to a components in your scene.

> **NOTE:**
> There is no need to implement exit on the 'back' button in these places as the plugin will exit the application after showing the ad.

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
<a name="top">

**Last SDK Version: 2.2.0**

<a name="hide-banner" />
###Hiding your banner
You can hide and show your banner in run time, using ``showBanner`` and ``hideBanner`` methods:
```objectivec
[bannerView showBanner];
[bannerView hideBanner];
```

[Back to top](#top)

<a name="ControllingBannerSize" />
###Controlling the size of your banner
The size of the banner is determined by the "size" parameter which can receive one of the following values

Value | Size | Best fits for
--- | --- | ---
*`STA_AutoAdSize`* | Auto-size (recommended) | detects the width of the device's screen in its current orientation, and provides the optimal banner for this size
*`STA_PortraitAdSize_320x50`* | 320x50 | iPhone/iPod touch in portrait mode
*`STA_LandscapeAdSize_480x50`* | 480x50 | iPhone/iPod touch in landscape mode
*`STA_PortraitAdSize_768x90`* | 768x90 | iPad in portrait mode
*`STA_LandscapeAdSize_1024x90`* | 1024x90 | iPad in landscape mode

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

<a name="UsingFixedOriginBanner" />
###Using a fixed origin for your banner
If you choose to locate the banner in a fixed origin rather than the view's top or bottom, simply pass the required origin point (x,y) upon initialization as explain in the following example

Locating your banner 100 pixels above the view's bottom:
```objectivec
bannerView = [[STABannerView alloc] initWithSize:STA_AutoAdSize 
                                    origin:CGPointMake(0, self.view.frame.size.height - 100) 
                                    withView:self.view 
                                    withDelegate:nil];
```
To use a different banner origin in a specific layout, call the "setOrigin"/"setStartAppAutoOrigin" with the new origin value.

[Back to top](#top)

<a name="ChangingBanner" />
###Changing the banner size and origin upon rotation
If you choose to manually control the banner's size & origin upon rotation, you can do it in the didRotateFromInterfaceOrientation function. 

Example:
```objectivec
// YourViewController.m

- (void)didRotateFromInterfaceOrientation:(UIInterfaceOrientation)fromInterfaceOrientation {
    if (UIInterfaceOrientationIsPortrait(self.interfaceOrientation)) {
         [bannerView setOrigin:CGPointMake(0, 0)];
    } else {	
        [bannerView setOrigin:CGPointMake(0, 300)];
    }
    [super didRotateFromInterfaceOrientation:fromInterfaceOrientation];
}
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

<a name="SelectInterstitialType" />
###Explicitly selecting the type of interstitial ad to load
When calling an interstitial ad, the ad type with the best performance will be automatically selected. If you would like to explicitly choose the type of ad, use the "withType" parameter when calling loadAd. 

The options for this parameter are:

Constant Name | Description | Specific Ad Load Example
--- | --- | ---
*`STAAdType_FullScreen`* | A full-page ad | `[startAppAd loadAd:STAAdType_FullScreen ];`
*`STAAdType_OfferWall`* | A full page offerwall | `[startAppAd loadAd:STAAdType_OfferWall ];`
*`STAAdType_Overlay`* | An overlay Ad is a full page Ad that runs on top of your application  | `[startAppAd loadAd:STAAdType_Overlay ];`

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
The SDK display the splash screen using the orientation supported by the application and the device real orientation. If you choose however to force landscape orientation.

**Parameter:** _isLandscape_  

**Values:**   
_YES_  
_NO_  

**Usage:**  
``splashPreferences.isLandscape = YES;``  

[Back to top](#top)



<a name="table-view" />
###Showing banners in UITableView
If you would like to load a banner into a UITableView instead of a general UIView, follow these instructions:

1. Declare an **STABannerView** instance variable in your UITableView class

 ```objectivec
 @interface YourViewController ()
 {
     STABannerView* bannerview;
 }
 @end
 ```
 
2. Override the ``cellForRowAtIndexPath`` method, and add the required code:

 ```objectivec
  - (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
     static NSString *CellIdentifier = @"Cell";
     UITableViewCell *cell=nil;
     cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier];
     
     if (cell == nil)  {
         cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault  reuseIdentifier:CellIdentifier]; 
     }
    
    
     // ADD THE FOLLOWING LINES
     if(bannerview == nil)
     {
         bannerview = [[STABannerView alloc] initWithSize:STA_AutoAdSize autoOrigin:STAAdOrigin_Top  withView:cell withDelegate:self]; 
     }
    
     [bannerview addSTABannerToCell:cell withIndexPath:indexPath atIntexPathRow:2 repeatEach:8];
    
     return cell;
 }
```

 Use the ``addSTABannerToCell`` method to set the banner's position and frequency:
 + ``atIntexPathRow`` - set the cell where you want to show the banner
 + ``repeatEach`` - set repetition frequency

 In the above example, the banner will be displayed at the second cell, and will be repeated each 8 cells.

[Back to top](#top)



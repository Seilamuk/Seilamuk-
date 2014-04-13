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

<a name="UsingBannerDelegates" />
###Using banner delegates
Set your view controller as a delegate so it is able to receive callbacks from the banner ad by adding the <STABannerDelegateProtocol> to the header file

1. Use "withDelegate:self" when initializing the STABannerView object:
 ```objectivec
 bannerView = [[STABannerView alloc] initWithSize:STA_AutoAdSize 
                                      autoOrigin:STAAdOrigin_Top                 
                                      withView:self.view 
                                      withDelegate:self];
 ```

2. Implement the following functions:
 ```objectivec
- (void) didDisplayBannerAd:(STABannerView*)banner;
- (void) failedLoadBannerAd:(STABannerView*)banner withError:(NSError *)error;
- (void) didClickBannerAd:(STABannerView*)banner;
 ```

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

<a name="ChangingBanner" />
###Changing the banner size and origin upon rotation
If you choose to manually control the banner's size & origin upon rotation, you can do it in the didRotateFromInterfaceOrientation function. 

Example:
```objectivec
// YourViewController.h

- (void)didRotateFromInterfaceOrientation:(UIInterfaceOrientation)fromInterfaceOrientation {
    if (UIInterfaceOrientationIsPortrait(self.interfaceOrientation)) {
         [bannerView setOrigin:CGPointMake(0, 0)];
    } else {	
        [bannerView setOrigin:CGPointMake(0, 300)];
    }
    [super didRotateFromInterfaceOrientation:fromInterfaceOrientation];
}
```

<a name="UsingInterstitialDelegate" />
###Using Interstitial delegates
Set your view controller as a delegate so it is able to receive callbacks from the interstitial ad by adding the <STADelegateProtocol> to the header file

1. Use "withDelegate:self" when calling the loadAd function:
 ```[startAppAd loadAd:STAAdType_Automatic withDelegate:self]```

2. Implement the following functions:
 ```objectivec
- (void) didLoadAd:(STAAbstractAd*)ad;
- (void) failedLoadAd:(STAAbstractAd*)ad withError:(NSError *)error;
- (void) didShowAd:(STAAbstractAd*)ad;
- (void) failedShowAd:(STAAbstractAd*)ad withError:(NSError *)error;
- (void) didCloseAd:(STAAbstractAd*)ad;
```

<a name="SelectInterstitialType" />
###Explicitly selecting the type of interstitial ad to load
When calling an interstitial ad, the ad type with the best performance will be automatically selected. If you would like to explicitly choose the type of ad, use the "withType" parameter when calling loadAd. 

The options for this parameter are:

Constant Name | Description | Specific Ad Load Example
--- | --- | ---
*`STAAdType_FullScreen`* | A full-page ad | `[startAppAd loadAd:STAAdType_FullScreen ];`
*`STAAdType_OfferWall`* | A full page offerwall | `[startAppAd loadAd:STAAdType_OfferWall ];`
*`STAAdType_Overlay`* | An overlay Ad is a full page Ad that runs on top of your application  | `[startAppAd loadAd:STAAdType_Overlay ];`




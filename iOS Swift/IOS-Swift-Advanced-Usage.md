<a name="top">

**Last SDK Version: 2.1.0**

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

1. Add the *STABannerDelegagteProtocol* to the class declaration
 ```objectivec
class ViewController: UIViewController, STABannerDelegagteProtocol
 ```

2. Use ``withDelegate:self`` when initializing the *STABannerView* object:
 ```objectivec
startAppBanner = STABannerView(size: STA_AutoAdSize, 
                               autoOrigin: STAAdOrigin_Bottom, 
                               withView: self.view, 
                               withDelegate: self);
 ```

3. Implement the following functions:
 ```objectivec
func didDisplayBannerAd(banner: STABannerView)
func failedLoadBannerAd(banner: STABannerView,  withError error: NSError)    
func didClickBannerAd(banner: STABannerView)
```

[Back to top](#top)

<a name="UsingFixedOriginBanner" />
###Using a fixed origin for your banner
If you choose to locate the banner in a fixed origin rather than the view's top or bottom, simply pass the required origin point (x,y) upon initialization as explain in the following example

Locating your banner 100 pixels above the view's bottom:
```objectivec
startAppBanner = STABannerView(size: STA_AutoAdSize, 
                               origin: CGPointMake(0, self.view.frame.size.height - 100), 
                               withView: self.view, 
                               withDelegate: nil)
```
To use a different banner origin in a specific layout, call the "setOrigin"/"setSTAAutoOrigin" with the new origin value.

[Back to top](#top)

<a name="ChangingBanner" />
###Changing the banner size and origin upon rotation
If you choose to manually control the banner's size & origin upon rotation, you can do it in the didRotateFromInterfaceOrientation method. 

Example:
```objectivec
override func didRotateFromInterfaceOrientation(fromInterfaceOrientation:UIInterfaceOrientation) {
 startAppBanner!.didRotateFromInterfaceOrientation(fromInterfaceOrientation)

   if (UIInterfaceOrientationIsPortrait(self.interfaceOrientation)) {
      startAppBanner!.setOrigin(CGPointMake(0, 0))
      } else {
      startAppBanner!.setOrigin(CGPointMake(0, 200))
      }
   super.didRotateFromInterfaceOrientation(fromInterfaceOrientation)
   }
```

[Back to top](#top)

<a name="UsingInterstitialDelegate" />
###Using Interstitial delegates
Set your view controller as a delegate so it is able to receive callbacks from the interstitial ad

1. Add the *STADelegateProtocol* to the class declaration
 ```objectivec
class ViewController: UIViewController, STADelegateProtocol
 ```

2. Use "withDelegate:self" when calling the loadAd function:
 ```startAppAd!.loadAd(STAAdType_Automatic, withDelegate: self)```

3. Implement the following functions:
 ```objectivec
func didLoadAd(ad: STAAbstractAd) 
func failedLoadAd(ad: STAAbstractAd, withError error: NSError)
func didShowAd(ad: STAAbstractAd)
func failedShowAd(ad: STAAbstractAd, withError error: NSError)
func didCloseAd(ad: STAAbstractAd)
 ```

[Back to top](#top)

<a name="SelectInterstitialType" />
###Explicitly selecting the type of interstitial ad to load
When calling an interstitial ad, the ad type with the best performance will be automatically selected. If you would like to explicitly choose the type of ad, specify it when calling loadAd. 

The options for this parameter are:

Constant Name | Description | Specific Ad Load Example
--- | --- | ---
*`STAAdType_FullScreen`* | A full-page ad | `startAppAd!.loadAd(STAAdType_FullScreen)`
*`STAAdType_OfferWall`* | A full page offerwall | `startAppAd!.loadAd(STAAdType_OfferWall)`
*`STAAdType_Overlay`* | An overlay Ad is a full page Ad that runs on top of your application  | `startAppAd!.loadAd(STAAdType_Overlay)`

[Back to top](#top)

<a name="table-view" />
###Showing banners in UITableView
If you would like to load a banner into a UITableView instead of a general UIView, follow these instructions:

1. Make sure that the size of your cell is at least as the size of the banner you would like to show
2. Declare an *STABannerView* instance variable in your *UITableView* class
 
 ```objectivec
 var startAppBanner: STABannerView?
 ```
 
2. Override the cellForRowAtIndexPath function, and add the required code:

 ```objectivec
override func tableView(tableView: UITableView?, cellForRowAtIndexPath indexPath: NSIndexPath!) -> UITableViewCell? {
    
        let reuseIdentifier = "Cell"
    
        var cell:UITableViewCell? = tableView?.dequeueReusableCellWithIdentifier(reuseIdentifier) as? UITableViewCell
        
        if !cell {
            cell = UITableViewCell(style: UITableViewCellStyle.Default, reuseIdentifier: reuseIdentifier)
        }
    
        
        if (!startAppBanner) {
          startAppBanner = STABannerView(size: STA_AutoAdSize, autoOrigin: STAAdOrigin_Top, withView: cell, withDelegate: self)
        }
        
        startAppBanner!.addSTABannerToCell(cell, withIndexPath: indexPath, atIntexPathRow: 2, repeatEach: 8)
        
        cell!.textLabel.text = "some text"
        cell!.textLabel.backgroundColor = UIColor.clearColor() 
	
        return cell
    }

 ```

 Use the ``addSTABannerToCell`` method to set the banner's position and frequency:
 + ``atIntexPathRow`` - set the cell where you want to show the banner
 + ``repeatEach`` - set repetition frequency

 In the above example, the banner will be displayed at the second cell, and will be repeated each 8 cells.

[Back to top](#top)


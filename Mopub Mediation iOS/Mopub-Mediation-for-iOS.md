<a name="top" />

<img src="./Mopub%20Mediation%20iOS/images/mopub_logo.png" />   

This document describes the procedure for serving StartApp Ads in your iOS application using MoPub mediation network

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)


<br></br>
<a name="step1" />
##Step 1, Getting Started
The following instructions assume you are already familiar with the MoPub mediation network and have already integrated the MoPub iOS SDK into your application. Otherwise, please start by visiting MoPub site and reading the instructions on how to add MoPub mediation code into your app.

- MoPub site: <a href="http://www.mopub.com/products/ad-network-mediation" target="_blank">mopub.com/products/ad-network-mediation</a>
- MoPub instructions: <a href="https://github.com/mopub/mopub-ios-sdk/wiki" target="_blank">github.com/mopub/mopub-ios-sdk/wiki</a>

[Back to top](#top)

<br></br>
<a name="step2" />
##Step 2, Adding Your Application to Your StartApp Developer's Account
1. Login into your <a href="https://portal.startapp.com/#/signin" target="_blank">StartApp developer's account</a>
2. Add your application and get its App ID
3. Download the StartApp In-App SDK

For any questions or difficulties during this process, please contact us via [support@startapp.com](mailto:support@startapp.com)

[Back to top](#top)

<br></br>
<a name="step3" />
##Step 3, Integrating the MoPub Mediation Adapter
Add the required AdMob adapter classes from the zip file to your project:  
<img src="./Mopub%20Mediation%20iOS/images/V.png" width="12px" />  STAMoPubCustomEventBanner   
<img src="./Mopub%20Mediation%20iOS/images/V.png" width="12px" />  STAMoPubCustomEventInterstitial  

Make sure to check the "Copy items if needed" checkbox.  
  
[Back to top](#top)

<br></br>
<a name="step4" />
##Step 4, Integrating StartApp In-App SDK
Integrate the StartApp SDK by implementing steps 1-3 <a href="https://github.com/StartApp-SDK/Documentation/wiki/iOS-InApp-Documentation" target="_blank">from the integration manual.</a> 
You can ignore all the following steps unless you want to use StartApp Ads directly instead of via AdMob mediation network.

In step 3 of the integration, call ``[sdk disableReturnAd]`` after initializing your appID and devID:
```objectivec
sdk.devID = @"your account id";
sdk.appID = @"your app Id";

[sdk disableReturnAd];  // Add this line to disable return ads
```
This extra line will disable StartApp "Return Ads" feature as it's not an integral part of AdMob mediation. You can still enjoy this attractive ad unit directly by omitting this line. In this case Return Ads will be activated and display StartApp direct ads, outside of the AdMob Mediation network. 

[Back to top](#top)

<br></br>
<a name="step5" />
##Step 5, Adding a Custom Event

1. Login into your MoPub account  
2. Navigate to "Networks" tab and click "Add a Network"  
<img src="./Mopub%20Mediation%20iOS/images/mopub-add-ad-network.png" />   
3. Under "Additional Networks" click "Custom Native Network"  
<img src="./Mopub%20Mediation%20iOS/images/mopub-choose-network.png" />   
4. Under "Setup Custom Native Network", fill in "StartApp" in the "Title" field
<img src="./Mopub%20Mediation%20iOS/images/mopub-network-title.png" />   
5. Under "Set Up Your Inventory", select the Ad Units where you want to show StartApp ads, and fill in the appropriate class name under the "CUSTOM EVENT CLASS" box:  
 <img src="./Mopub%20Mediation%20iOS/images/V1.png" width="16px" /> **Interstitial:** ``STAMoPubCustomEventInterstitial``   
 <img src="./Mopub%20Mediation%20iOS/images/V1.png" width="16px" /> **Banner:** ``STAMoPubCustomEventBanner``    
 <img src="./Mopub%20Mediation%20iOS/images/V1.png" width="16px" /> **Banner:** ``STAMoPubCustomEventNative``     

 <img src="./Mopub%20Mediation%20iOS/images/mopub-inventory.png" />   
6. For Native only - you can get a specific ad size, fill it in the appropriate class data under "CUSTOM EVENT CLASS DATA" of your Banner ad unit using the following format: 
 ```
 {"primaryImageSize" : primary_size, 
  "secondaryImageSize" : secondary_size}
 ```
Where primary_size and secondary_zise can get one of the following values:
 + 0 – for image size of 72px X 72px   
 + 1 – for image size of 100px X 100px   
 + 2 – for image size of 150px X 150px   
 + 3 – for image size of 340px X 340px   
 + 4 – for image size of 1200px X 628px  

 For example: ```{"primaryImageSize":4, "secondaryImageSize":2}``` will return a primary image of 1200x628 and a secondary icon of 150x150.  
 <img src="./Mopub%20Mediation%20iOS/images/mopub-custom-event-data-native.png" />     
7. Click "Save and Continue"
8. Click the "StartApp" network and make sure the "Run" button is active  
<img src="./Mopub%20Mediation%20iOS/images/mopub-run-network.png" />    

[Back to top](#top)

<br></br>
<a name="step6" />
##Step 6, Testing Your Application
Congratulation - that's it! You may now run your app and see StartApp ads in action.  

[Back to top](#top)
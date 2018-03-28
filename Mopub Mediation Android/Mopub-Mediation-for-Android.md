<a name="top" />

<img src="./Mopub%20Mediation%20Android/images/mopub_logo.png" />   

This document describes the procedure for serving StartApp Ads in your application using MoPub mediation network

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)


<br></br>
<a name="step1" />

## Step 1, Getting Started
The following instructions assume you are already familiar with the MoPub mediation network and have already integrated the MoPub Android SDK into your application. Otherwise, please start by visiting MoPub site and reading the instructions on how to add MoPub mediation code into your app.

- MoPub site: <a href="http://www.mopub.com/products/ad-network-mediation" target="_blank">mopub.com/products/ad-network-mediation</a>
- MoPub instructions: <a href="https://github.com/mopub/mopub-android-sdk/wiki/Getting-Started" target="_blank">github.com/mopub/mopub-android-sdk/wiki/Getting-Started</a>

[Back to top](#top)

<br></br>
<a name="step2" />

## Step 2, Adding Your Application to Your StartApp Publisher's Account
1. Login into your <a href="https://portal.startapp.com/#/signin" target="_blank">StartApp Publisher's account</a>
2. Add your application and get its App ID
3. Download the StartApp In-App SDK

For any questions or difficulties during this process, please contact us via [support@startapp.com](mailto:support@startapp.com)

[Back to top](#top)

<br></br>
<a name="step3" />

## Step 3, Integrating the MoPub Mediation Adapter
Copy the java source files from the zip to your project's source code.

[Back to top](#top)

<br></br>
<a name="step4" />

## Step 4, Integrating StartApp In-App SDK
Integrate the StartApp SDK by implementing steps 1-3 <a href="https://github.com/StartApp-SDK/Documentation/wiki/Android-InApp-Documentation" target="_blank">from the integration manual.</a>   
If you have obfuscated your application using ProGuard, follow the <a href="https://github.com/StartApp-SDK/Documentation/wiki/Android-InApp-Documentation#obfuscation-optional" target="_blank">Obfuscation</a> section as well.  
You can ignore all other steps unless you want to use StartApp Ads directly instead of via MoPub mediation network.

In step 3 of the integration, use the following line to initialize the SDK:
```java
StartAppSDK.init(this, "Your App ID", false);
```
The extra ``false`` parameter will disable StartApp "Return Ads" feature as it's not an integral part of MoPub mediation. You can still enjoy this attractive ad unit directly by omitting the ``false`` parameter. In this case Return Ads will be activated and display StartApp direct ads, outside of the MoPub Mediation network. 

[Back to top](#top)

<br></br>
<a name="step5" />

## Step 5, Adding a Custom Event

1. Login into your MoPub account  
2. Navigate to "Networks" tab and click "Add a Network"  
<img src="./Mopub%20Mediation%20Android/images/mopub-add-ad-network.png" />   
3. Under "Additional Networks" click "Custom Native Network"  
<img src="./Mopub%20Mediation%20Android/images/mopub-choose-network.png" />   
4. Under "Setup Custom Native Network", fill in "StartApp" in the "Title" field
<img src="./Mopub%20Mediation%20Android/images/mopub-network-title.png" />   
5. Under "Set Up Your Inventory", select the Ad Units where you want to show StartApp ads, and fill in the appropriate class name under the "CUSTOM EVENT CLASS" box:  
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> 

**Interstitial:** ``com.mopub.mobileads.StartAppCustomEventInterstitial``   
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **Banner:** ``com.mopub.mobileads.StartAppCustomEventBanner``    
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **Native:** ``com.mopub.nativeads.StartAppCustomEventNative``    

 <img src="./Mopub%20Mediation%20Android/images/mopub-inventory.png" />   
 
6. For Banners only - if you want to show banner with a different size rather than the standard 320x50,  fill it in the appropriate class data under "CUSTOM EVENT CLASS DATA" of your Banner ad unit using the following format: {"adWidth":"<width>", "adHeight":"<height>"}. For example, {"adWidth":"480", "adHeight":"50"}  
 <img src="./Mopub%20Mediation%20Android/images/mopub-custom-event-data-banners.png" />     
7. Optional - if you want to show a specific Interstitial ad type, fill in the appropriate *class data* under "CUSTOM EVENT CLASS DATA" of your Interstitial ad unit:  
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> 

**Overlay Ad:** ``{ "adMode" : "AdMode.OVERLAY" }``  
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **Full Page:** ``{ "adMode" : "AdMode.FULLPAGE" }``  
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **App Wall:** ``{ "adMode" : "AdMode.OFFERWALL" }``  
 
 <img src="./Mopub%20Mediation%20Android/images/mopub-custom-event-data.png" />   

 > **NOTE:** we highly recommend leaving this field empty in order to use the automatic mode which selects the best ad to load.    

8. Click "Save and Continue"
9. Click the "StartApp" network and make sure the "Run" button is active  
<img src="./Mopub%20Mediation%20Android/images/mopub-run-network.png" />    

[Back to top](#top)

<br></br>
<a name="step6" />

## Step 6, Testing Your Application
Congratulation - that's it! You may now run your app and see StartApp ads in action.  

[Back to top](#top)
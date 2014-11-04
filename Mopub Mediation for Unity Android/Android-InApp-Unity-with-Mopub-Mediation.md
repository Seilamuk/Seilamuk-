<a name="top" />

<img src="./images/unity+mopub.png" />   

This document describes the procedure for serving StartApp Ads in your Unity Android application using MoPub mediation network

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)


<br></br>
<a name="step1" />
##Step 1, Getting Started
The following instructions assume you are already familiar with the MoPub mediation network and have already integrated the MoPub Android SDK into your application. Otherwise, please start by visiting MoPub site and reading the instructions on how to add MoPub mediation code into your app.

- MoPub site: <a href="http://www.mopub.com/products/ad-network-mediation" target="_blank">mopub.com/products/ad-network-mediation</a>
- MoPub instructions: <a href="https://github.com/mopub/mopub-android-sdk/wiki/Getting-Started" target="_blank">github.com/mopub/mopub-android-sdk/wiki/Getting-Started</a>

[Back to top](#top)

<br></br>
<a name="step2" />
##Step 2, Adding Your Application to Your StartApp Developer's Account
1. Login into your <a href="https://developers.startapp.com/General/Login.aspx" target="_blank">StartApp developer's account</a>
2. Add your application and get its App ID
3. Download the StartApp In-App SDK

For any questions or difficulties during this process, please contact us via [support@startapp.com](mailto:support@startapp.com)

[Back to top](#top)

<br></br>
<a name="step3" />
##Step 3, Integrating the MoPub Mediation Adapter
Copy the adapter jar file from the zip to the “Assets/Plugins/Android” directory of your project.

[Back to top](#top)

<br></br>
<a name="step4" />
##Step 4, Integrating StartApp In-App SDK
Integrate the StartApp SDK by implementing steps 1-3 <a href="https://github.com/StartApp-SDK/Documentation/wiki/Android-InApp-Unity-Documentation" target="_blank">from the integration manual.</a> 
You can ignore all the following steps unless you want to use StartApp Ads directly instead of via MoPub mediation network.

After completing Step 3 of the integration, add the following line to your code. Make sure this code is executed before using Mopub API:
```java
StartAppWrapper.init(false);
```
The extra ``false`` parameter will disable StartApp "Return Ads" feature as it's not an integral part of MoPub mediation. You can still enjoy this attractive ad unit directly by omitting the ``false`` parameter and just call the ``init()`` method. In this case Return Ads will be activated and display StartApp direct ads, outside of the MoPub Mediation network. 

[Back to top](#top)

<br></br>
<a name="step5" />
##Step 5, Adding a Custom Event

1. Login into your MoPub account  
2. Navigate to "Networks" tab and click "Add a Network"  
<img src="./Mopub%20Mediation%20Android/images/mopub-add-ad-network.png" />   
3. Under "Additional Networks" click "Custom Native Network"  
<img src="./Mopub%20Mediation%20Android/images/mopub-choose-network.png" />   
4. Under "Setup Custom Native Network", fill in "StartApp" in the "Title" field
<img src="./Mopub%20Mediation%20Android/images/mopub-network-title.png" />   
5. Under "Set Up Your Inventory", select the Ad Units where you want to show StartApp ads, and fill in the appropriate class name under the "CUSTOM EVENT CLASS" box:  
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **Interstitial:** ``com.mopub.mobileads.StartAppCustomEventInterstitial``   
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **Banner:** ``com.mopub.mobileads.StartAppCustomEventBanner``    
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **Native:** ``com.mopub.nativeads.StartAppCustomEventNative``    

 <img src="./Mopub%20Mediation%20Android/images/mopub-inventory.png" />   
6. Optional - if you want to show a specific Interstitial ad type, fill in the appropriate *class data* under "CUSTOM EVENT CLASS DATA" of your Interstitial ad unit:  
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **Overlay Ad:** ``{ "adMode" : "AdMode.OVERLAY" }``  
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **Full Page:** ``{ "adMode" : "AdMode.FULLPAGE" }``  
 <img src="./Mopub%20Mediation%20Android/images/V1.png" width="16px" /> **App Wall:** ``{ "adMode" : "AdMode.OFFERWALL" }``  
 
 <img src="./Mopub%20Mediation%20Android/images/mopub-custom-event-data.png" />   

 > **NOTE:** we highly recommend leaving this field empty in order to use the automatic mode which selects the best ad to load.    

7. Click "Save and Continue"
8. Click the "StartApp" network and make sure the "Run" button is active  
<img src="./Mopub%20Mediation%20Android/images/mopub-run-network.png" />    

[Back to top](#top)

<br></br>
<a name="step6" />
##Step 6, Testing Your Application
Congratulation - that's it! You may now run your app and see StartApp ads in action.  

[Back to top](#top)
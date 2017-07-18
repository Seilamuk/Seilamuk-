<a name="top" />

<img src="./Admob%20Mediation%20Android/images/admob_logo.png" />   

This document describes the procedure for serving StartApp Ads in your application using AdMob mediation network

> **NOTES:**
> - The code samples in this document can be copy/pasted into your source code
> - If you have any questions, contact us via [support@startapp.com](mailto:support@startapp.com)


<br></br>
<a name="step1" />

## Step 1, Getting Started
The following instructions assume you are already familiar with the AdMob Mediation Network and have already integrated the Google Mobile Ads SDK into your application. Otherwise, please start by reading the following articles for a walk-through explanation of what mediation is, how to use the AdMob Mediation UI, and instructions on how to add AdMob mediation code into your app.

- Mediation Overview: <a href="https://support.google.com/admob/answer/2413211" target="_blank">support.google.com/admob/answer/2413211</a>
- Instructions: <a href="https://developers.google.com/mobile-ads-sdk/docs/admob/mediation#android" target="_blank">https://developers.google.com/admob/android/mediation</a>

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

## Step 3, Integrating the AdMob Mediation Adapter
Copy the java source files from the zip to your project's source code.

[Back to top](#top)

<br></br>
<a name="step4" />

## Step 4, Integrating StartApp In-App SDK
Integrate the StartApp SDK by implementing steps 1-3 <a href="https://github.com/StartApp-SDK/Documentation/wiki/Android-InApp-Documentation" target="_blank">from the integration manual.</a> 
You can ignore all the following steps unless you want to use StartApp Ads directly instead of via AdMob mediation network.

In step 3 of the integration, use the following line to initialize the SDK:
```java
StartAppSDK.init(this, "Your App ID", false);
```
The extra ``false`` parameter will disable StartApp "Return Ads" feature as it's not an integral part of AdMob mediation. You can still enjoy this attractive ad unit directly by omitting the ``false`` parameter. In this case Return Ads will be activated and display StartApp direct ads, outside of the AdMob Mediation network. 

[Back to top](#top)

<br></br>
<a name="step5" />

## Step 5, Adding a Custom Event

1. Login into your AdMob account
2. Navigate to "Monetize" tab and choose your application
3. Find your "Ad Unit" and then click "Edit Mediation"  
<img src="./Admob%20Mediation%20Android/images/admob-edit-mediation.png" />   
4. Choose "New Ad Network" 
<img src="./Admob%20Mediation%20Android/images/admob-add-ad-network.png" />  
5. Click "Custom event", and fill in the following fields:  
  *  Class Name: *com.startapp.android.mediation.admob.StartAppCustomEvent*
  *  Label: *StartApp*
  *  Parameter (for Interstitials only): leave empty for automatic mode, or use one of the following options:     
  <img src="./iOS/images/V.png" width="12px" />  AdMode.FULLPAGE  
  <img src="./iOS/images/V.png" width="12px" />  AdMode.OFFERWALL  

  <img src="./Admob%20Mediation%20Android/images/admob-add-custom-event.png" />  

  > **NOTE:** If not a must, we highly recommend leaving this field empty to use the automatic mode which selects the best ad to load. 

6. Click "Continue"

[Back to top](#top)

<br></br>
<a name="step6" />

## Step 6, Testing Your Application
Congratulation - that's it! You may now run your app and see StartApp ads in action.  

[Back to top](#top)
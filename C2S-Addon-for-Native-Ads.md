<a name="using-native-ads" />
##Integrating Native Ads
####Initializing and Loading a StartAppNativeAd Object
In your Activity, create a member variable, as follows:
```java
private StartAppNativeAd startAppNativeAd = new StartAppNativeAd(this);
```
To load your native ad, call the loadAd() method with a NativeAdPreferences object:
```java
startAppNativeAd.loadAd(new NativeAdPreferences());
```

**NativeAdPreferences** can be used to customize some of the native ad properties to suit your needs, such as the number of ads to load, the image size of the ad, or whether the image should be pre-cached or not. For a full description of the **NativeAdPreferences**, please refer to [NativeAdPreferences API](#NativeAdPreferencesAPI).

> **NOTE:** By default, **StartAppNativeAd** retrieves the image URL of the ad. The SDK is also capable of auto-loading the image as a BITMAP object. This feature is turned off by default. For enabling it, set _``autoBitmapDownload``_ in **NativeAdPreferences** to true (please refer to [Ad's image configuration](#image-conf)).

You can register your **startAppNativeAd** object for callbacks by passing an **AdEventListener** object to the ``loadAd()`` method:
```java
startAppNativeAd.loadAd(new NativeAdPreferences(), new AdEventListener() {
	  @Override
	  public void onReceiveAd(Ad arg0) {
			// Native Ad Received
	  }
	  
	  @Override
	  public void onFailedToReceiveAd(Ad arg0) {
			// Native Ad failed to receive
	  }
});
```

####Using the Native Ad Object
After initializing and  loading your  **startAppNativeAd** object, use the ``getNativeAds()`` method to obtain an array of **NativeAdDetails** objects for all returning ads. The **NativeAdDetails** object provides access to each ad's details, such as the ad's title, description, image, etc.  This object also provides methods for firing an impression once the ad is displayed, and for executing the user's click on the ad. For a full description of the **NativeAdDetails** object, please refer to [NativeAdDetails API](#NativeAdDetailsAPI).

**Example:** the following is an example of how to load 3 native ads with a pre-cached images of 150x150 pixels size, and logging their details once ready (using callbacks)

```java
// Declare Native Ad Preferences
NativeAdPreferences nativePrefs = new NativeAdPreferences()
										  .setAdsNumber(3)                // Load 3 Native Ads
										  .setAutoBitmapDownload(true)    // Retrieve Images object
										  .setImageSize(NativeAdBitmapSize.SIZE150X150);

// Declare Ad Callbacks Listener
AdEventListener adListener = new AdEventListener() {     // Callback Listener
	  @Override
	  public void onReceiveAd(Ad arg0) {              
			// Native Ad received
			ArrayList<NativeAdDetails> ads = startAppNativeAd.getNativeAds();    // get NativeAds list
			
			// Print all ads details to log
			Iterator<NativeAdDetails> iterator = ads.iterator();
			while(iterator.hasNext()){
				  Log.d("MyApplication", iterator.next().toString());
			}
	  }
	  
	  @Override
	  public void onFailedToReceiveAd(Ad arg0) {
			// Native Ad failed to receive
			Log.e("MyApplication", "Error while loading Ad");
	  }
};

// Load Native Ads
startAppNativeAd.loadAd(nativePrefs, adListener);
```

> **Note:** It is possible to get less ads than you requested. It is also possible that no ad will be returned. In this case you will receive an empty array.

####Showing and Clicking a Native Ad
+ Once you decide to actually show a native ad, you must call the ``NativeAdDetails.sendImpression()`` method.
+ Once the user clicks on the ad, you must call ``NativeAdDetails.sendClick()`` method.

<a name="NativeAdPreferencesAPI" />
####NativeAdPreferences API:

#####► Set the number of Native ads to retrieve
> **```public NativeAdPreferences setAdsNumber(int adsNumber)```**<br></br>

set number of native ads to be received from the server.

######Parameters<br></br>
*adsNumber* - integer of the ads number<br></br>

######Return Value<br></br>
*NatvieAdPreferences* – current object

<a name="image-conf" />
#####► Ad's image configuration
> **``public NativeAdPreferences setAutoBitmapDownload(boolean autoBitmapDownload)``**<br></br>

You can choose between two options to obtain the ad's image: 

1. get the image pre-cached as a BITMAP. <br></br>
2. get the image URL only.<br></br>

######Parameters<br></br>
*autoBitmapDownload* - Boolean:
+ true – native ad object will be loaded automatically with bitmap object
+ false – native ad wont load the image automatically

######Return Value<br></br>
*NatvieAdPreferences* – current object


#####► Set Ad's image size
> **``public NativeAdPreferences setImageSize(NativeAdBitmapSize bitmapSize)``**<br></br>

Set the image size of the ad to be retrieved.

######Parameters<br></br>
*bitmapSize* - NativeAdBitmapSize for selecting image size. The NativeAdBitmapSize can get the following values: <br></br>
+ SIZE72X72 – for image size 72px X 72px <br></br>
+ SIZE100X100 – for image size 100px X 100px <br></br>
+ SIZE150X150 – for image size 150px X 150px <br></br>
+ SIZE340X340 – for image size 340px X 340px <br></br>

######Return Value<br></br>
*NatvieAdPreferences* – current object

<a name="NativeAdDetailsAPI" />
####NativeAdDetails API:
#####► Get the Ad's title
> **``public String getTitle()``**<br></br>

######Return Value: String

#####► Get the Ad's description
> **``public String getDescription()``**<br></br>

######Return Value: String

#####► Get the Ad's rating
> **``public String getRating()``**<br></br>

Get the rating of the ad in the Google Play store. The rating range is 1-5.

######Return Value: Float

#####► Get the Ad's image URL
> **``public String getImageUrl()``**<br></br>

Get the image URL of the ad, according to the selected size.

######Return Value: String


#####► Get the Ad's image URL
> **``public Bitmap getImageBitmap()``**<br></br>

Get the image of the ad as a pre-cached bitmap, if requested using the NativeAdPreferences.setAutoBitmapDownload() method.

######Return Value: Bitmap

#####► Get the Ad's installs numbers
> **``public String getInstalls()``**<br></br>

Get the amount of installs in Google Play store.

######Return Value: String


#####► Get the Ad's category
> **``public String getCategory()``**<br></br>

Get the category of the ad in the Google Play store.

######Return Value: String

#####► Get the Ad's package name
> **``public String getPackacgeName()``**<br></br>

Get the ad's package name in the Google Play store (for example, "com.startapp.quicksearchbox").

######Return Value: String

#####► To be called when you actually show the ad 
> **``public void sendImpression(Context context)``**<br></br>

Call this method when you show the ad in your application. 

######Parameters<br></br>
*context* – the context of the host activity

#####► To be called when the user clicks on the ad
> **``public void sendClick(Context context)``**<br></br>

Call this method when the user clicks on the ad.  

######Parameters<br></br>
*context* – the context of the host activity

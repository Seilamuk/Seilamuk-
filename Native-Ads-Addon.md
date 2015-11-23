<a name="using-native-ads" />
##Integrating Native Ads
####Initializing and Loading a StartAppNativeAd Object
**1.** In your activity, create a member variable, as follows:
```java
private StartAppNativeAd startAppNativeAd = new StartAppNativeAd();
```

**2.** Prepare your native ad parameters:
```java
NativeAdParams adRequestParams = new NativeAdParams();
		adRequestParams.setPartner("123456789");
		adRequestParams.setProd("987654321");
		adRequestParams.setAdw(1200);
		adRequestParams.setAdh(628);
		adRequestParams.setAdsNum(5);
```

**Parameters:**
+ **``setPartner``**: Your Partner/Publisher Id
+ **``setProd``**: Your Product/application Id
+ **``setAdw``**: Your ad width
+ **``setAdh``**: Your ad height
+ **``setAdsNum``**: Number of native ads to load

**3.** Create a **NativeAdListener** object for getting callbacks:
```java
NativeAdListener nativeAdListener = new NativeAdListener() {
	  @Override
	  public void onReceiveAd(NativeAdResponse arg0) {
			// Native Ad Received
	  }
	  
	  @Override
	  public void onFailedToReceiveAd(NativeAdResponse arg0) {
			// Native Ad failed to receive
	  }
};
```

**4.** To load your native ad, call the loadAd() method with the ApplicationContext and adRequestParams object:
```java
startAppNativeAd.loadAd(getApplicationContext(), adRequestParams, nativeAdListener);
```


####Using the Native Ad Object
After initializing and  loading your  **startAppNativeAd** object, use the ``getAds()`` method to obtain an array of **NativeAdDetails** objects for all returning ads. The **NativeAdDetails** object provides access to each ad's details, such as the ad's title, description, image, etc.  This object also provides methods for firing an impression once the ad is displayed, and for executing the user's click on the ad. For a full description of the **NativeAdDetails** object, please refer to [NativeAdDetails API](#NativeAdDetailsAPI).

**Example:** the following is an example of how to load 5 native ads with icon size of 1200x628 pixels size, and logging their details once ready (using callbacks)

```java
// Create your StartAppNativeAd object
private StartAppNativeAd startAppNativeAd = new StartAppNativeAd();

// Declare Native Ad Preferences
NativeAdParams adRequestParams = new NativeAdParams();
		adRequestParams.setPartner("123456789");
		adRequestParams.setProd("987654321");
		adRequestParams.setAdw(1200);
		adRequestParams.setAdh(628);
		adRequestParams.setAdsNum(5);

// Declare Ad Callbacks Listener
NativeAdListener adListener = new NativeAdListener() {
			
	@Override	
	public void onReceiveAd(NativeAdResponse response) {			
			// Native Ad received
			List<NativeAdDetails> nativeAds = new ArrayList<NativeAdDetails>();
			nativeAds = response.getAds();
				
			// Print all ads details to log
	            Iterator<NativeAdDetails> iterator = nativeAds.iterator();
	            while(iterator.hasNext()){
	                  Log.d("MyApplication", iterator.next().getTitle());
					  Log.d("MyApplication", iterator.next().getDesc());
					  Log.d("MyApplication", iterator.next().getImg());
					  Log.d("MyApplication", iterator.next().getIcon());
					  Log.d("MyApplication", iterator.next().getRate().toString());
					  Log.d("MyApplication", iterator.next().getInstalls());
					  Log.d("MyApplication", iterator.next().getCat());
					  Log.d("MyApplication", iterator.next().getPck());					  					  
	            }
			}
			
			@Override
			public void onFailedToReceiveAd(NativeAdResponse response) {
				// Native Ad failed to receive
	            Log.e("MyApplication", "Error while loading Ad");
			}
		};

startAppNativeAd.loadAd(getApplicationContext() ,adRequestParams, adListener);
```

> **Note:** It is possible to get less ads than you requested. It is also possible that no ad will be returned. In this case you will receive an empty array.

####Showing and Clicking a Native Ad
+ Once you decide to actually show a native ad, you must call the ``NativeAdDetails.sendImpression()`` method.
+ Once the user clicks on the ad, you must call ``NativeAdDetails.sendClick()`` method.


<a name="NativeAdDetailsAPI" />
####NativeAdDetails API:
#####► Get the Ad's title
> **``public String getTitle()``**<br></br>

######Return Value: String

#####► Get the Ad's description
> **``public String getDesc()``**<br></br>

######Return Value: String

#####► Get the Ad's rating
> **``public String getRating()``**<br></br>

Get the rating of the ad in the Google Play store. The rating range is 1-5.

######Return Value: Float

#####► Get the Ad's image URL
> **``public String getImg()``**<br></br>

Get the image URL of the ad, according to the selected size.

######Return Value: String

#####► Get the Ad's icon URL
> **``public String getIcon()``**<br></br>

Get the icon URL of the ad

######Return Value: String


#####► Get the Ad's installs numbers
> **``public String getInstalls()``**<br></br>

Get the amount of installs in Google Play store.

######Return Value: String


#####► Get the Ad's category
> **``public String getCat()``**<br></br>

Get the category of the ad in the Google Play store.

######Return Value: String

#####► Get the Ad's package name
> **``public String getPck()``**<br></br>

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

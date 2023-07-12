# Stylitics Data SDK

The Stylitics Data SDK enables a host app to use Stylitics services. Currently, the Data SDK provides services to fetch outfits, item replacements, and send tracking events.

## Dependency

In App level `.build.gradle` file, to access the Data SDK APIs, please add the dependency:

```jsx
implementation files('libs/styliticsdatarelease.aar')
```

*Note: check name of the .aar file and use the provided version if different from above.*

## SDK Configuration

Integrator App must provide an application context and a valid string value for `clientName`. All other configurations have a default value, which can be customized if required by the Integrator app.


- `timeout` - default value is 60 (seconds)
- `enableDebugLogs` - default set to `false`
- `dataApisHost `- default host(Production Environment)
- `trackingApisHost` - default host(Production Environment)
- `clientName` - should always be provided. It is used by Data SDK to fetch Stylitics Experience Configs to identify which features are enabled for the client. Stylitics will provide this value to you.
- `context` - should always be provided
- `locale` - default value is null


***Notes*** :

- If the integrator app tries to access the outfits API
    - Before configuring the Data SDK with a valid client name, the Data SDK throws a runtime error with the message “*Client name not configured*”.
    - By configuring SDK without context, it will throw an exception with the message - *“Application context is required, please initialise it using StyliticsData.configure(this) method.”*
- This configuration should be provided only once before using SDK APIs. If you try to configure it again, the Data SDK throws a runtime error with the message *“Already configured, cannot be initialised again”*.

### Configuration Examples

Below are a few samples of how Sample/Integrator app can use the configuration API.

To continue with default configurations:

```kotlin
StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC"))
```

To enable Retrofit and Stylitics SDK logs in logcat:

```kotlin
StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", enableDebugLogs = true))
```

To change the timeout value for Stylitics Data SDK APIs:

```kotlin
StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", timeout = 70))
```

### Locale config

To enable locale for Stylitics Data SDK:

```kotlin
StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName = "ABC", locale = "en-gb"))
```

### Hosts config

For **Data APIs**, the SDK supports Production, Staging, and Custom hosts. The syntax for setting up a staging and custom host for Data APIs is as follows: (*Note: Since production is default, no need to configure it.)*

**Staging**

```kotlin
StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", dataApisHost = DataApisHost.Staging))
```

**Custom**

```kotlin
StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", dataApisHost = DataApisHost.Custom("xyz.stylitics.com")))
```

For **Tracking APIs**, the SDK supports Production, Staging, and Custom hosts. The syntax for setting up a staging and custom host for tracking APIs is as follows: (*Note : Since production is default no need to configure it*.)

**Staging**

```kotlin
StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", trackingApisHost = TrackingApisHost.Staging))
```

**Custom**

```kotlin
StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", trackingApisHost = TrackingApisHost.Custom("xyz.stylitics.com")))
```

To update multiple or all configuration values for Stylitics Data SDK APIs, use the below syntax:

```kotlin
StyliticsData.configure(    
	MyApplication.getAppContext(), 
	config = StyliticsConfig(
		clientName =  "ABC", 
		enableDebugLogs = BuildConfig.DEBUG,
		dataApisHost = DataApisHost.Staging,
		trackingApisHost = TrackingApisHost.Staging,
		timeout = 90,
        locale = "en-gb"
	)
)
```

## Mix and Match

If the client has the Mix and Match Experience enabled and if the Sample/Integrator app sends `shouldEnableMixAndMatch` as true in the Outfits API call, Mix and Match feature is enabled.

### Experiences API  /  in-app config flag behavior

| Experience config API, MnM enabled? | shouldEnableMixAndMatch flag | Result |
|-------------------------------------|------------------------------|--------|
| true                                | true                         | true   |
| true                                | false                        | false  |
| false                               | true                         | false  |
| false                               | false                        | false  |

*Note : Experience API permissions are fetched by Data SDK using provided `clientName` during SDK configuration.*

### Example to enable Mix and Match from Sample/Integrator App

*Note: assumes per the above table that MnM is enabled server-side, and reported as such by the Experiences API*

```kotlin
val filterParams = mapOf<String, Any>("username" to "xyz", "item_number" to "123456")

StyliticsData.outfits(filterParams, shouldEnableMixAndMatch = true) { 
	response ->    
		when (response) {
	    is NetworkResponse.Success -> {
				println("Success : isMixAndMatchEnabled ${response.body.isMixAndMatchEnabled}")         
			}         
			is NetworkResponse.ApiError -> {             
				println("API error : ${response.error}")         
			}         
			is NetworkResponse.NetworkError -> {             
				println("Network error : ${response.throwable}")         
			}    
	}
}
```

## Stylitics Data API Call Examples

After configuring the Stylitics Data SDK, the Integrator App can invoke the APIs

### Fetch Outfits using item number

*Note*: *More than 1 item number can be passed as a comma separated string.*

```kotlin
val filterParams = mapOf<String, Any>("username" to "xyz", "item_number" to "123456")

StyliticsData.outfits(filterParams) { response ->    
	when (response) {     
		is NetworkResponse.Success -> {
        println("Success ${response.body}")    
		}     
		is NetworkResponse.NetworkError -> { 
       println("Network error : ${response.throwable}")
    }     
		is NetworkResponse.ApiError -> {
       println("API error : ${response.error}")   
	  }   
  }
}
```

### Fetch Outfits using tags

*Note : More than 1 tag can be passed as a comma separated string.*

```kotlin
val filterParams = mapOf<String, Any>("username" to "xyz", "tags" to "abc")
StyliticsData.outfits(filterParams) { response ->    
	when (response) {     
		is NetworkResponse.Success -> {
			println("Success ${response.body}")
		}     
		is NetworkResponse.NetworkError -> {
			println("Network error : ${response.throwable}")
		}     
		is NetworkResponse.ApiError -> {
			println("API error : ${response.error}")
		}    
	}
}
```

### Fetch Outfits using item number and locale

*Note : If the Integrator App has provided a locale value in the global config, the locale value provided during an API call will be overridden.*

```kotlin
val filterParams = mapOf<String, Any>("username" to "xyz", "item_number" to "123456", "locale" to "en-gb")

StyliticsData.outfits(filterParams) { response ->
  when (response) {
    is NetworkResponse.Success -> {
      println("Success ${response.body}")
    }
    is NetworkResponse.NetworkError -> {
      println("Network error : ${response.throwable}")
    }
    is NetworkResponse.ApiError -> {
      println("API error : ${response.error}")
    }
  }
}
```

### Fetch Replacements

*Note: More than 1 id can be passed as a comma separated string.*

```kotlin
val optionsInfo = mapOf("ids" to "123456")
StyliticsData.replacements(optionsInfo) { 
	response ->    
		when (response) {         
			is NetworkResponse.Success -> {
			  println("Success ${response.body}")
			}         
			is NetworkResponse.NetworkError -> {            
				println("Network error : ${response.throwable}")         
			}         
			is NetworkResponse.ApiError -> {
				println("API error : ${response.error}")        
		 }    
	}
}
```

## Tracking API Examples

*Note: The `position` is 0 index-based, where referenced*

### Jumplink clicked event

This event should be triggered when the user clicks on the “See How To Wear It” button, using below syntax.

*Note: This must be triggered by the host app, even if the UI SDK is present, because the “jumplink” is always implemented at the host app level.*

```kotlin
val engagementsTrackingInfo = EngagementsTrackingInfo(Event.JUMPLINK)
Engagements.track(engagementsTrackingInfo)
```

### Outfit view event

This event is already triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits. If Integrator App uses its own custom view to display Outfits, please refer to the below information on triggering the Outfit view event. This event should be triggered when Outfits come into the viewport and are visible. *This event should be triggered only once per Outfit page session, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*

```kotlin
val engagementsTrackingInfo = EngagementsTrackingInfo(Event.VIEW, EngagementInfo.OutfitData(outfit = outfit, position = index))

StyliticsData.engagements(engagementsTrackingInfo)
```

### Outfit click event

This event is already triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits. If Integrator App uses its own custom view to display Outfits, please refer to the below example of triggering the Outfit click event.

```kotlin
val engagementsTrackingInfo = EngagementsTrackingInfo(Event.CLICK, EngagementInfo.OutfitData(outfit = outfit, position = index))

StyliticsData.engagements(engagementsTrackingInfo)
```

### OutfitItem view event

This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items. If Integrator App uses its own custom view to display OutfitItem, this event should be triggered when the user clicks on an outfit and is shown a list of items. Send the outfitItem view event when the item comes into the viewport and is visible, using below syntax.

```kotlin
val engagementsTrackingInfo = EngagementsTrackingInfo(Event.VIEW, EngagementInfo.OutfitItemData(outfitItem = outfitItem, position = index))

StyliticsData.engagements(engagementsTrackingInfo)
```

### OutfitItem click event

This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items. If Integrator App uses its own custom view to display OutfitItem, this event should be triggered when a user clicks on an item, using below syntax.

```kotlin
val engagementsTrackingInfo = EngagementsTrackingInfo(Event.CLICK, EngagementInfo.OutfitItemData(outfitItem = outfitItem, position = index))

StyliticsData.engagements(engagementsTrackingInfo)
```

### Item add-to-cart event

This event should be triggered when the user adds a Stylitics data item to the cart, using below syntax.

```kotlin
val engagementsTrackingInfo = EngagementsTrackingInfo(Event.ADD_TO_CART, EngagementInfo.OutfitItemData(outfitItem = outfitItem, position = index))

StyliticsData.engagements(engagementsTrackingInfo)
```

### Purchase event

This event should be triggered by Integrator/Sample app when user purchases any item provided by Stylitics data. It will help Stylitics provide better recommendations for the user.

Setting up the purchased items data:

```kotlin
val itemInfoList:PurchasedItemInfo = PurchasedItemInfo(itemId = 1234, price = 12.34)
```

When order id is not available:

```kotlin
val purchasedItems = PurchasedItems(currency = Constants.CURRENCY, itemInfoList = itemInfoList)
```

When order id is available:

```kotlin
val purchasedItems = PurchasedItems(currency= Constants.CURRENCY, orderId= orderId, itemInfoList= itemInfoList)
```

Then (in either case):

```kotlin
StyliticsData.purchases(purchasedItems) {  
	if (it is NetworkResponse.NetworkError) {
		println("Network error : ${it.throwable}")  
	} else if (it is NetworkResponse.ApiError) {
		println("API error: ${it.error}, Header: ${it.headers}")  
	}
}
```

## Stylitics API Documentation

Go through the below link to learn more about the Data and Tracking APIs and their input parameters.

### Data API

[https://widget-api-staging.stylitics.com/index.html#/](https://widget-api-staging.stylitics.com/index.html#/)

### Tracking API

[http://datastream-staging.stylitics.com/api/api-docs/](http://datastream-staging.stylitics.com/api/api-docs/)

## Android Versioning Support

- Minimum required android API level to access features of SDK is - 21(Android 5.0)
- Target android API level is 32(Android 13/ Beta)

## Technologies Used

- Android Studio as IDE(**v** Chipmunk | 2021.2.1 Patch 1)
- Kotlin as programming language(**v** 1.6.21)

## Permissions

Please ensure that the Internet permission is provided in the Manifest file of Integrator App.

```
 <uses-permission android:name="android.permission.INTERNET"/>
```

## License

Copyright © 2023 Stylitics

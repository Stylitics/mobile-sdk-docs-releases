# Stylitics Data SDK

Data SDK enables Sample/Integrator app to use Stylitics services. Currently, the Data SDK provides services to fetch outfits, replacements, and send tracking events.

## Features

It offers below features to the Integrator app.

* Data API Services -
  1. Outfits - Using the provided filter parameters, It will retrieve the appropriate outfits.
  2. Replacements -
      1. It should be used only when the Mix and Match feature is enabled.</br>
      2. It retrieves the replacement items for the item numbers provided in the request.

* Tracking API Services -
  1. Purchases - It is used to track the purchase event for items received through the Stylitics Data APIs.
  2. Engagements - It is used to track the user interaction with Stylitics data.

## Technologies Used

Below technologies are used for implementing SDK

* Android Studio as IDE(**v** Chipmunk | 2021.2.1 Patch 1)
* Kotlin as programming language(**v** 1.6.21)

## Dependency

*_**Assumption**_* : Data SDK is already added into the project.<br />
In App level .build.gradle file, to access the Data SDK APIs, please add below dependency:

```javascript
implementation files('libs/styliticsdatarelease.aar')
```

## SDK Configuration

Integrator App has to provide an application context and a valid string value for clientName. All other configurations have a default value, which can be customised if required by the Integrator app.

         1. timeout(in seconds) - default value is 60

         2. enableDebugLogs - default set to false

         3. dataApisHost - default host(Production Environment)

         4. trackingApisHost - default host(Production Environment)

         5. clientName - should always be provided. It is used by Data SDK to fetch Stylitics Experience Configs to identify which features are enabled for the client.

         6. context - should always be provided  

*_**Note**_* :</br>
1. If the integrator app tries to access the outfits API 
   1. Before configuring the Data SDK with a valid client name, the Data SDK throws a runtime error with the message *_**"Client name not configured"**_*.</br>
   2. By configuring SDK without context, it will throw an exception with the message - *_**"Application context is required, please initialise it using StyliticsData.configure(this) method."**_*
2. This configuration should be provided only once before using SDK APIs. If you try to configure it again, the Data SDK throws a runtime error with the message *_**"Already configured, cannot be initialised again"**_*.

Below are a few samples of how Sample/Integrator app can use the configuration API.

* To continue with default configurations, use the below syntax:

```kotlin
        StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC"))
```

* To enable Retrofit and Stylitics SDK logs in logcat, use the below syntax:

```kotlin
        StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", enableDebugLogs = true))
```

* To change the timeout value for Stylitics Data SDK APIs, use the below syntax:

```kotlin
        StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", timeout = 70))
```

* For Data APIs, the SDK supports Production, Staging, and Custom hosts. The syntax for setting up a staging and custom host for Data APIs is as follows:<br />
  *_**Note**_* : Since production is default no need to configure it.

```kotlin 
        Staging : 
        StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", dataApisHost = DataApisHost.Staging))
        
        Custom :
        StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", dataApisHost = DataApisHost.Custom("xyz.stylitics.com")))
```

* For tracking APIs, the SDK supports Production, Staging, and Custom hosts. The syntax for setting up a staging and custom host for tracking APIs is as follows:<br />
  *_**Note**_* : Since production is default no need to configure it.

```kotlin 
        Staging : 
        StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", trackingApisHost = TrackingApisHost.Staging))
        
        Custom :
        StyliticsData.configure(MyApplication.getAppContext(), config = StyliticsConfig(clientName =  "ABC", trackingApisHost = TrackingApisHost.Custom("xyz.stylitics.com")))
```

* To update multiple or all configuration values for Stylitics Data SDK APIs, use the below syntax:

```kotlin
        StyliticsData.configure(
            MyApplication.getAppContext(), config = StyliticsConfig(
                clientName =  "ABC",
                enableDebugLogs = BuildConfig.DEBUG,
                dataApisHost = DataApisHost.Staging,
                trackingApisHost = TrackingApisHost.Staging,
                timeout = 90
            )
        )
```

## Mix and Match

If the client has the Mix and Match Experience enabled and if the Sample/Integrator app sends shouldEnableMixAndMatch as true in the Outfits API call, Mix and Match feature is enabled.

#### Below table explains Experience config to Flag mapping
| Mix and Match Experience | shouldEnableMixAndMatch flag | Result |
| ---- | ---- | ---- |
| true | true | true |
| true | false | false |
| false | true | false |
| false | false | false |

*_**Note**_* : Mix and Match Experience is fetched by Data SDK using provided clientName during SDK configuration.

#### Example to enable Mix and Match from Sample/Integrator App

```kotlin
        val filterParams = mapOf<String, Any>("username" to "xyz", "item_number" to "123456")    
                StyliticsData.outfits(filterParams, shouldEnableMixAndMatch = true) { response ->
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

After configuring the Stylitics Data SDK, the Integrator App can invoke the APIs using the below syntax.

### 1. Data API Examples

#### Example to Fetch Outfits using item number
*_**Note**_* : More than 1 item number can be passed as a comma separated string.

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

#### Example to Fetch Outfits using tags<br />
*_**Note**_* : More than 1 tag can be passed as a comma separated string.

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

#### Example to Fetch Replacements
*_**Note**_* : More than 1 ids can be passed as a comma separated string.

```kotlin
        val optionsInfo = mapOf("ids" to "123456")
               StyliticsData.replacements(optionsInfo) { response ->
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

### 2. Tracking API Examples

#### API Call to Track Engagement Events

*_**Note**_* : The position is 0 index-based, and is applicable for all events.

Following are the list of events to track user interaction with the app.

* #### Jumplink clicked event
  This event should be triggered when the user clicks on the "See How To Wear IT" button, using below syntax.
```kotlin
        val engagementsTrackingInfo = EngagementsTrackingInfo(Event.JUMPLINK)
        Engagements.track(engagementsTrackingInfo)
```

Refer below image:</br>
</br>![Image1](Screenshots/pdp_screen_top.png)


* #### Outfit view event
1. This event is already triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits.</br>
2. If Integrator App uses its own custom view to display Outfits, please refer below information to trigger the Outfit view event.</br>
    1. This event should be triggered when Outfits come into the viewport and are visible.</br>
    2. This event should be triggered only once per Outfit page session, using below syntax.</br>

```kotlin
         val engagementsTrackingInfo = EngagementsTrackingInfo(Event.VIEW, EngagementInfo.OutfitData(outfit = outfit, position = index))
         StyliticsData.engagements(engagementsTrackingInfo)
```

* #### Outfit click event
1. This event is already triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits.</br>
2. If Integrator App uses its own custom view to display Outfits, please refer below information to trigger the Outfit click event.</br>
    1. This event should be triggered when the user selects an outfit from the list, using below syntax.</br>

```kotlin
         val engagementsTrackingInfo = EngagementsTrackingInfo(Event.CLICK, EngagementInfo.OutfitData(outfit = outfit, position = index))
         StyliticsData.engagements(engagementsTrackingInfo)
```

* #### OutfitItem view event
1. This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items.</br>
2. If Integrator App uses its own custom view to display OutfitItem, please refer below information to trigger the OutfitItem view event.</br>
    1. This event should be triggered when the user clicks on an outfit, it shows a list of items. Send the outfitItem view event when the item comes into the viewport and is visible, using below syntax.

```kotlin
         val engagementsTrackingInfo = EngagementsTrackingInfo(Event.VIEW, EngagementInfo.OutfitItemData(outfitItem = outfitItem, position = index))
         StyliticsData.engagements(engagementsTrackingInfo)
```

* #### OutfitItem click event
1. This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items.
2. If Integrator App uses its own custom view to display OutfitItem, please refer below information to trigger the OutfitItem click event.</br>
    1. This event should be triggered when a user clicks on an item, send an item click event, using below syntax.

```kotlin
         val engagementsTrackingInfo = EngagementsTrackingInfo(Event.CLICK, EngagementInfo.OutfitItemData(outfitItem = outfitItem, position = index))
         StyliticsData.engagements(engagementsTrackingInfo)
```

* #### Item add-to-cart event
  This event should be triggered when the user adds a Stylitics data item to the cart, using below syntax.

```kotlin
         val engagementsTrackingInfo = EngagementsTrackingInfo(Event.ADD_TO_CART, EngagementInfo.OutfitItemData(outfitItem = outfitItem, position = index))
         StyliticsData.engagements(engagementsTrackingInfo)
```

#### API Call to Track Purchase event
* #### Purchase event
  This event should be triggered by Integrator/Sample app when user purchases any item provided by Stylitics data. It will help Stylitics server to provide better recommendations for the user. Below is the syntax for triggering it.

```kotlin
        val itemInfoList:PurchasedItemInfo = PurchasedItemInfo(itemId = 1234, price = 12.34)
```

When order id is not available:
```kotlin
        val purchasedItems = PurchasedItems(currency = Constants.CURRENCY, itemInfoList = itemInfoList)
```

When order id is available:
```kotlin

        val purchasedItems = PurchasedItems(currency= Constants.CURRENCY,
                                            orderId= orderId,
                                            itemInfoList= itemInfoList)

        StyliticsData.purchases(purchasedItems) {
              if (it is NetworkResponse.NetworkError) {
                    println("Network error : ${it.throwable}")
              } else if (it is NetworkResponse.ApiError) {
                    println("API error : ${it.error}, Header : ${it.headers}")
              }
        }
```

## Stylitics API Documentation

Go through the below link to learn more about the Data and Tracking APIs and their input parameters.

#### Data API

https://widget-api.stylitics.com/index.html#/

#### Tracking API

http://datastream-staging.stylitics.com/api/api-docs/

## Android Versioning Support

* Minimum required android API level to access features of SDK is - 21(Android 5.0)

* Target android API level is 32(Android 13/ Beta)

## Permissions

Please ensure, Internet permission is provided in the Manifest file of Integrator App.

     <uses-permission android:name="android.permission.INTERNET"/>

## License

copyright © 2023 Stylitics
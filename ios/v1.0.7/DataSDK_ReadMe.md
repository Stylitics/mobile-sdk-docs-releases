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

* Xcode as IDE
* Swift as programming language
* SwiftLint to enforce Swift style and conventions

## Dependency

*_**Assumption**_* : Data SDK is already added into the project.<br />
        In any file, to access the Data SDK APIs, please import the SDK using below syntax:
```swift
        import StyliticsData
```


## SDK Configuration

Integrator App has to provide a valid string value for clientName. All other configurations have a default value, which can be customised if required by the Integrator app.

         1. timeout(in seconds) - default value is 60
    
         2. enableDebugLogs - default set to false
    
         3. dataApisHost - default host(Production Environment)
    
         4. trackingApisHost - default host(Production Environment)
         
         5. clientName - should always be provided. It is used by Data SDK to fetch Stylitics Experience Configs to identify which features are enabled for the client.

*_**Note**_* :</br>
    1. If the integrator app tries to access the outfits API before configuring the Data SDK with a valid client name, the Data SDK throws a runtime error with the message "Client name not configured".</br>
    2. This configuration should be provided only once before using SDK APIs. If you try to configure it again, the Data SDK throws a runtime error with the message "Already configured, cannot be initialised again". 

Below are a few samples of how Sample/Integrator app can use the configuration API.

* To continue with default configurations, use the below syntax:

```swift
        do {
            try StyliticsDataApis.configure(config: StyliticsConfig(clientName: "ABC"))
        } catch {
            print("error \(error)")
        }
```

* To enable Stylitics SDK logs in console, use the below syntax:

```swift
        do {
            try StyliticsDataApis.configure(config: StyliticsConfig(enableDebugLogs: true,
                                                                    clientName: "ABC"))
        } catch {
            print("error \(error)")
        }
```

* To change the timeout value for Stylitics Data SDK APIs, use the below syntax:
```swift
        do {
            try StyliticsDataApis.configure(config: StyliticsConfig(timeoutInSecs: 70,
                                                                    clientName: "ABC"))
        } catch {
            print("error \(error)")
        }
```

* For Data APIs, the SDK supports Production, Staging, and Custom hosts. The syntax for setting up a staging and custom host for Data APIs is as follows:<br />
    *_**Note**_* : Since production is default no need to configure it.

```swift 
        Staging : 
        do {
            try StyliticsDataApis.configure(config: StyliticsConfig(dataApisHost: .staging,
                                                                    clientName: "ABC"))
        } catch {
            print("error \(error)")
        }

        Custom :
        do {
            try StyliticsDataApis.configure(config: StyliticsConfig(dataApisHost: .custom("xyz.stylitics.com"),
                                                                    clientName: "ABC"))
        } catch {
            print("error \(error)")
        }
```

* For tracking APIs, the SDK supports Production, Staging, and Custom hosts. The syntax for setting up a staging and custom host for tracking APIs is as follows:<br />
    *_**Note**_* : Since production is default no need to configure it.

```swift
        Staging : 
        do {
            try StyliticsDataApis.configure(config: StyliticsConfig(trackingApisHost: .staging,
                                                                    clientName: "ABC"))
        } catch {
            print("error \(error)")
        }

        Custom :
        do {
            try StyliticsDataApis.configure(config: StyliticsConfig(trackingApisHost: .custom("xyz.stylitics.com"),
                                                                    clientName: "ABC"))
        } catch {
            print("error \(error)")
        }
```

* To update multiple or all configuration values for Stylitics Data SDK APIs, use the below syntax:

```swift
        do {
            try StyliticsDataApis.configure(config: StyliticsConfig(timeoutInSecs: 90,
                                                                    enableDebugLogs: true,
                                                                    dataApisHost: .staging,
                                                                    trackingApisHost: .staging,
                                                                    clientName: "ABC"))
        } catch {
            print("error \(error)")
        }
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

```swift
        let filterInfo = ["username": "xyz", "item_number": "123456"]

        do {
            try StyliticsDataApis.outfits(filterInfo: filterInfo,
                                          shouldEnableMixAndMatch: true) { response in
                if let error = response.error {
                    print("error \(error)")
                } else if let outfits = response.data {
                    print("Success, isMixAndMatchEnabled \(outfits.isMixAndMatchEnabled)")
                }
            }
        } catch {
            print("error \(error)")
        }
```

## Stylitics Data SDK API Call Examples

After configuring the Stylitics Data SDK, the Integrator App can invoke the APIs using the below syntax.

### 1. Data API Examples

#### Example to Fetch Outfits using item number
*_**Note**_* : More than 1 item number can be passed as a comma separated string.

```swift
        let filterInfo = ["username": "xyz", "item_number": "123456"]

        do {
            try StyliticsDataApis.outfits(filterInfo: filterInfo) { response in
                if let error = response.error {
                    print("error \(error)")
                } else if let outfits = response.data {
                    print("Success \(outfits)")
                }
            }
        } catch {
            print("error \(error)")
        }
```

#### Example to Fetch Outfits using tags<br />
*_**Note**_* : More than 1 tag can be passed as a comma separated string.

```swift
        let filterInfo = ["username": "xyz", "tags": "abc"]

        do {
            try StyliticsDataApis.outfits(filterInfo: filterInfo) { response in
                if let error = response.error {
                    print("error \(error)")
                } else if let outfits = response.data {
                    print("Success \(outfits)")
                }
            }
        } catch {
            print("error \(error)")
        }
```

#### Example to Fetch Replacements
*_**Note**_* : More than 1 ids can be passed as a comma separated string.

```swift
        let optionsInfo = ["ids": "123456"]
    
        StyliticsDataApis.replacements(optionsInfo: optionsInfo) { [weak self] response in
            if let error = response.error {
                print("error \(error)")
            } else if let replacements = response.data {
                print("Success \(replacements)")
            }
        } 
```
### 2. Tracking API Examples

#### API Call to Track Engagement Events

*_**Note**_* : The position is 0 index-based, and is applicable for all events.

Following are the list of events to track user interaction with the app. 

* #### Jumplink clicked event
  This event should be triggered when the user clicks on the "See How To Wear IT" button, using below syntax.
```swift
        let engagementTrackingInfo = EngagementsTrackingInfo(event: .jumplink)
        StyliticsDataApis.engagement(trackingInfo: engagementTrackingInfo)
```
Refer below image:</br> 
![ALt seeHowToWearIt](Screenshots/seeHowToWearIt.png)


* #### Outfit view event
1. This event is already triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits.</br>
2. If Integrator App uses its own custom view to display Outfits, please refer below information to trigger the Outfit view event.</br>
    1. This event should be triggered when outfits come into the viewport and are visible.</br>
    2. This event should be triggered only once per Outfit page session, using below syntax.</br>

```swift
        let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                              engagementInfo: .outfit(outfit: outfit,
                                                                                      position: index))
        StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
            if let error = response.error {
                print("Error in Engagements api response \(error)")
            }
        }
```
* #### Outfit click event
1. This event is already triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits.</br>
2. If Integrator App uses its own custom view to display Outfits, please refer below information to trigger the Outfit click event.</br>
    1. This event should be triggered when the user selects an outfit from the list, using below syntax.</br>
```swift
        let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                              engagementInfo: .outfit(outfit: outfit,
                                                                                      position: index))
        StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
            if let error = response.error {
                print("Error in Engagements api response \(error)")
            }
        }
```

* #### OutfitItem view event
1. This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items.</br>
2. If Integrator App uses its own custom view to display OutfitItem, please refer below information to trigger the OutfitItem view event.</br>
    1. This event should be triggered when the user clicks on an outfit, it shows a list of items. Send the outfitItem view event when the item comes into the viewport and is visible, using below syntax.</br>
```swift
        let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                              engagementInfo: .outfitItem(outfitItem: outfitItem,
                                                                                          position: index))
        StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
            if let error = response.error {
                print("Error in Engagements api response \(error)")
            }
        }
```

* #### OutfitItem click event
1. This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items.
2. If Integrator App uses its own custom view to display OutfitItem, please refer below information to trigger the OutfitItem click event.</br>
    1. This event should be triggered when a user clicks on an item, send an item click event, using below syntax.</br>
```swift
        let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                              engagementInfo: .outfitItem(outfitItem: outfitItem,
                                                                                          position: index))
        StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
            if let error = response.error {
                print("Error in Engagements api response \(error)")
            }
        }
```

* #### Item add-to-cart event
    This event should be triggered when the user adds a Stylitics data item to the cart, using below syntax.
```swift
        let engagementsTrackingInfo = EngagementsTrackingInfo(event: .addToCart,
                                                              engagementInfo: .outfitItem(outfitItem: outfitItem,
                                                                                          position: index))
        StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
            if let error = response.error {
                print("Error in Engagements api response \(error)")
            }
        }
```

#### API Call to Track Purchase event
* #### Purchase event
  This event should be triggered by Integrator/Sample app when user purchases any item provided by Stylitics data. It will help Stylitics server to provide better recommendations for the user. Below is the syntax for triggering it.
```swift
        let itemInfoList: [PurchasedItemInfo] = [PurchasedItemInfo(itemId: 1234, price: 12.34)!]
```
When order id is not available:
```swift
        let purchasedItems = PurchasedItems(currency: Constants.PURCHASE_CURRENCY_USD,
                                            itemInfoList: itemInfoList)
```

When order id is available:
```swift
        let purchasedItems = PurchasedItems(currency: Constants.PURCHASE_CURRENCY_USD, 
                                            orderId: orderId, 
                                            itemInfoList: itemInfoList)
```


```swift
        StyliticsDataApis.purchases(purchasedItems: purchasedItems) { response in
            if let error = response.error {
                print("Error in Purchase api response \(error)")
            }
        }
```
## Stylitics API Documentation

Go through the below link to learn more about the Data and Tracking APIs and their input parameters.

#### Data API 
  
https://widget-api.stylitics.com/index.html#/

#### Tracking API 

http://datastream-staging.stylitics.com/api/api-docs/

## iOS Versioning Support

* Minimum required iOS APP version to access features of SDK is - (iOS 13.0)

## License

copyright © 2023 Stylitics
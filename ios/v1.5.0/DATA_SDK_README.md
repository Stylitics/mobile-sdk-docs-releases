# Stylitics Data SDK

The Stylitics Data SDK enables a host app to use Stylitics services. Currently, the Data SDK provides services to fetch outfits, item replacements, dynamic gallery and send tracking events.

## Dependency

In any file, to access the Data SDK APIs, please import the SDK using below syntax:

```swift
import StyliticsData
```


## SDK Configuration

Integrator App must provide a valid string value for `clientName`. All other configurations have a default value, which can be customized if required by the Integrator app.

- `timeout` - default value is 60 (seconds)
- `enableDebugLogs` - default set to `false`
- `dataApisHost `- default host(Production Environment)
- `trackingApisHost` - default host(Production Environment)
- `clientName` - should always be provided. It is used by Data SDK to fetch Stylitics Experience Configs to identify which features are enabled for the client. Stylitics will provide this value to you.
- `locale` - Global config for locale. Default value is nil. When it is set to a valid locale, Data SDK APIs will retrieve the relevant data from the server and return localized data.
- `customerProfileId` - Global config for customer profile Id. Default value is nil. When it is set to a string, the Data SDK will internally convert it to MD5 hash and send it to all tracking events by adding a new key-value pair, i.e., `"client_user_id"`:`"converted hashed string for profile id"`.

***Notes*** :

- If the integrator app tries to access any of the Data APIs
    - Before configuring the Data SDK with a valid client name, the Data SDK throws a runtime error with the message “*Client name not configured*”.
- This configuration should be provided only once before using SDK APIs. If you try to configure it again, the Data SDK throws a runtime error with the message *“Already configured, cannot be initialised again”*.



### Configuration Examples

Below are a few samples of how Sample Integrator app can use the configuration API.

To continue with default configurations, use the below syntax:

```swift
do {
    try StyliticsDataApis.configure(config: StyliticsConfig(clientName: "ABC"))
} catch {
    print("error \(error)")
}
```

To enable Stylitics SDK logs in console, use the below syntax:

```swift
do {
    try StyliticsDataApis.configure(config: StyliticsConfig(enableDebugLogs: true,
                                                            clientName: "ABC"))
} catch {
    print("error \(error)")
}
```

To change the timeout value for Stylitics Data SDK APIs, use the below syntax:

```swift
do {
    try StyliticsDataApis.configure(config: StyliticsConfig(timeoutInSecs: 70,
                                                            clientName: "ABC"))
} catch {
    print("error \(error)")
}
```

To configure the locale value for Stylitics Data SDK APIs, use the below syntax:

```swift
do {
    try StyliticsDataApis.configure(config: StyliticsConfig(locale: "en-gb",
                                                            clientName: "ABC"))
} catch {
    print("error \(error)")
}
```

To configure the Customer Profile Id value for Stylitics Data SDK APIs, use the below syntax:

```swift
do {
    try StyliticsDataApis.configure(config: StyliticsConfig(clientName: "ABC",
                                                            customerProfileId: "myCustomerProfileId@123"))
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

For **Tracking APIs**, the SDK supports Production, Staging, and Custom hosts. The syntax for setting up a staging and custom host for tracking APIs is as follows: (*Note : Since production is default no need to configure it*.)

**Staging**

```swift
do {
    try StyliticsDataApis.configure(config: StyliticsConfig(trackingApisHost: .staging,
                                                            clientName: "ABC"))
} catch {
    print("error \(error)")
}
```

**Custom**

```swift
do {
    try StyliticsDataApis.configure(config: StyliticsConfig(trackingApisHost: .custom("xyz.stylitics.com"),
                                                            clientName: "ABC"))
} catch {
    print("error \(error)")
}
```

To update multiple or all configuration values for Stylitics Data SDK APIs, use the below syntax:

```swift
do {
    try StyliticsDataApis.configure(config: StyliticsConfig(timeoutInSecs: 90,
                                                            enableDebugLogs: true,
                                                            dataApisHost: .staging,
                                                            trackingApisHost: .staging,
                                                            locale: "en-gb",
                                                            clientName: "ABC",
                                                            customerProfileId: "myCustomerProfileId@123"))
} catch {
    print("error \(error)")
}
```

## Update Global locale

The `updateGlobalLocale(_:)` method is used to update global locale configured during SDK initialisation.

- `locale`: An optional string representing the locale to be set globally.

### Description

This method checks whether the SDK has been configured or not. If the SDK is configured, it updates the global locale value. If the SDK is not configured, it throws an error. After a successful update, all subsequent Stylitics API calls will use the updated global locale value (if there is no locale in outfits Api call). The results of previously fetched API calls will not be affected.

### Throws

This method may throw an error if the SDK is not configured. The error type is `Error` with the message "Data SDK isn't configured yet.".

### Example

```swift
do {
    try StyliticsDataApis.updateGlobalLocale("en-US")
} catch {
    print("error \(error)")
}
```

## Update Global Customer Profile Id

The `updateCustomerProfileId(_:)` method is used to update Customer Profile Id.

- `customerProfileId`: An optional string representing the Customer Profile Id to be set globally.

### Description

This method checks whether the SDK has been configured or not. If the SDK is configured, it updates the `customerProfileId` value. If the SDK is not configured, it throws an error. After a successful update, the Data SDK will internally convert it to MD5 hash and send it to all tracking events.

### Throws

This method may throw an error if the SDK is not configured. The error type is `Error` with the message "Data SDK isn't configured yet".

### Example

```swift
do {
    try StyliticsDataApis.updateCustomerProfileId("myCustomerProfileId@123")
} catch {
    print("error \(error)")
}
```

## Mix and Match

If the client has the Mix and Match Experience enabled and if the Sample Integrator app sends `shouldEnableMixAndMatch` as true in the Outfits API call, Mix and Match feature is enabled.

### Experiences API  /  in-app config flag behavior

| Experience config API, MnM enabled? | shouldEnableMixAndMatch flag | Result |
| --- | --- | --- |
| true | true | true |
| true | false | false |
| false | true | false |
| false | false | false |

*Note : Experience API permissions are fetched by Data SDK using provided `clientName` during SDK configuration.*

### Example to enable Mix and Match from Sample Integrator App

*Note: assumes per the above table that MnM is enabled server-side, and reported as such by the Experiences API*

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

After configuring the Stylitics Data SDK, the Integrator App can invoke the APIs

### Fetch Outfits using item number

*Note*: *More than 1 item number can be passed as a comma separated string.*

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

### Fetch Outfits using tags

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

### Fetch Outfits using item number and locale

Note : If the Integrator App has provided a locale value in the API call, it will have higher precedence than the Global config (if configured).

```swift
let filterInfo = ["username": "xyz", "item_number": "123456", "locale": "en-gb"]

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
The price fields that are accessible for localization in the API response are listed below.
- `priceLocalized` - Actual product price
- `salePriceLocalized` - Price after discount

### Fetch Outfits using item number with price configs

To configure price, Integrator App should send `price_rounding` and `price_hide_double_zero_cents` as URL params.

*_**Price rounding**_*

| Fields | Description |
| --- | --- |
| `none` | default |
| `floor` | round down in every case no matter the decimal count |
| `ceiling` | round up in every case no matter the decimal count |
| `round` | round up when equal or greater than .50, and round down when equal or lesser than .49 |

`price_hide_double_zero_cents` - Hide decimals that are .00

```swift
let filterInfo = ["username": "xyz", "item_number": "123456", "price_rounding": "round", "price_hide_double_zero_cents": "false"]

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

The configured price values are accessible using below fields.
- `priceLocalized` - Actual product price
- `salePriceLocalized` - Price after discount

### Fetch Replacements

*Note : More than 1 id can be passed as a comma separated string.*

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

### Fetch Replacements with price configs

To configure price, Integrator App should send `price_rounding` and `price_hide_double_zero_cents` as URL params.

### Price rounding

| Fields | Description |
| --- | --- |
| `none` | default |
| `floor` | round down in every case no matter the decimal count |
| `ceiling` | round up in every case no matter the decimal count |
| `round` | round up when equal or greater than .50, and round down when equal or lesser than .49 |

- `price_hide_double_zero_cents` - Hide decimals that are .00

```swift
guard let requestId = outfitResponse.data?.list.first?.requestId else {
    return
}

let optionsInfo = ["ids": "123456", "request_id": requestId, "price_rounding": "floor", "price_hide_double_zero_cents": "true"]

StyliticsDataApis.replacements(optionsInfo: optionsInfo) { [weak self] response in
    if let error = response.error {
        print("error \(error)")
    } else if let replacements = response.data {
        print("Success \(replacements)")
    }
} 
```

The configured price values are accessible using below fields.
- `priceLocalized` - Actual product price
- `salePriceLocalized` - Price after discount

*_**Note**_*
- To configure price, Integrator app should send `request_id` value from Outfits response
- If the Replacement API has no price configurations, it will use the price configuration used in Outfits Api (if any)

### Fetch Dynamic Gallery

Below is the list of filter parameters to invoke the Dynamic Gallery Api, along with an example of an Api call.
- `username` - Should always be provided.
- `session_id` - Data SDK generates the UUID if Integrator App doesn't pass a value for this. 
- `page` - A string value that represents a web url of the page. Should always be provided.
- `max` - Integrator App should send `max` value in range 1 to 10. If it is beyond the range or no value is sent, Data SDK sets it to `10`.
- `min` - Integrator App should send `min` value in range 1 to 10. If it is beyond the range or no value is sent, Data SDK sets it to `3`. It should not be greater than `max` value.
- `p.b` - A string value of comma-separated previously browsed item ids.
- `p.p` - A string value of comma-separated previously purchased item ids.

```Swift
let filterInfo = [Constants.USERNAME_KEY: "demo",
                  "session_id": "9fc06715-0a59-47e4-b752-be29ff282ea4",
                  "page": "www.stylitics-demo.com/womens/apparel",
                  "min": "3",
                  "max": "10",
                  "p.b": "BP313_EE3740,BP313_BK0001,BM546_WQ9746,BF456_BR6302",
                  "p.p": "BP508_NA6445,BG654_BL8133,608066_2661"]

do {
    try StyliticsDataApis.dynamicGalleries(filterInfo: filterInfo) { response in
        if let error = response.error {
            print("error \(error)")
        } else if let galleryBundles = response.data {
            print("Success \(galleryBundles)")
        }
    }
} catch {
    print("error \(error)")
}
```

## Tracking API Examples

*Note: The `position` is 0 index-based, where referenced*

### Jumplink clicked event

This event should be triggered when the user clicks on the “See How To Wear It” button, using below syntax.  *Note: This must be triggered by the host app, even if the UI SDK is present, because the “jumplink” is always implemented at the host app level.*

```swift
let engagementTrackingInfo = EngagementsTrackingInfo(event: .jumplink)
StyliticsDataApis.engagement(trackingInfo: engagementTrackingInfo)
```

### Outfit view event
This event is already triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits. If Integrator App uses its own custom view to display Outfits, please refer to the below information on triggering the Outfit view event. This event should be triggered when Outfits come into the viewport and are visible. *This event should be triggered only once per Outfit page session, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*

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

### Outfit click event

This event is triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits. Using below syntax.

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

### OutfitItem view event

This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items. If Integrator App uses its own custom view to display OutfitItem, this event should be triggered when the user clicks on an outfit and is shown a list of items. Send the outfitItem view event when the item comes into the viewport and is visible, using below syntax.

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

### OutfitItem click event

This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items. If Integrator App uses its own custom view to display OutfitItem, this event should be triggered when a user clicks on an item, using below syntax.

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

## Dynamic Gallery Bundle view event

This event is already triggered inside UX SDK when Dynamic Gallery template is used from UX SDK to display GalleryBundles. If Integrator App uses its own custom view to display GalleryBundles, please refer to the below information on triggering the GalleryBundle view event. This event should be triggered when GalleryBundles come into the viewport and are visible. *This event should be triggered only once per GalleryBundle page session, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .galleryBundle(galleryBundle: galleryBundle,
                                                                                     position: index))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

## Dynamic Gallery Bundle click event

This event is already triggered inside UX SDK when an Dynamic Gallery template is used from UX SDK to display GalleryBundles. If Integrator App uses its own custom view to display GalleryBundles, please refer to the below example of triggering the GalleryBundle click event.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .galleryBundle(galleryBundle: galleryBundle,
                                                                                     position: index))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

## Dynamic Gallery Bundle Item view event

This event is already handled inside UX SDK when Gallery Product List view is used from UX SDK to display Gallery Bundle items. If Integrator App uses its own custom view to display GalleryBundleItems, this event should be triggered when the user clicks on an GalleryBundle and is shown a list of items. Send the outfitItem view event when the item comes into the viewport and is visible, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .galleryBundleItem(galleryBundleItem: galleryBundleItem,
                                                                                         position: index))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```
## Dynamic Gallery Bundle Item click event

This event is already handled inside UX SDK when Gallery Product List view is used from UX SDK to display Gallery Bundle items. If Integrator App uses its own custom view to display GalleryBundleItems, this event should be triggered when a user clicks on an item, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .galleryBundleItem(galleryBundleItem: galleryBundleItem,
                                                                                         position: index))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Item add-to-cart event

This event should be triggered when the user adds a Stylitics data item to the cart, using below syntax.

**Outfit Item**

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

**Gallery Bundle Item**

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .addToCart,
                                                      engagementInfo: .galleryBundleItem(galleryBundleItem: galleryBundleItem,
                                                                                         position: index))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Purchase event

This event should be triggered by Sample Integrator app when user purchases any item provided by Stylitics data. It will help Stylitics provide better recommendations for the user.

Setting up the purchased items data:

Price can be sent as an Int or Double, and itemId can be sent as an Int or String as shown below.

```swift
let itemInfoList: [PurchasedItemInfo] = [PurchasedItemInfo(itemId: "1234", price: 12.34)!]
                              OR
let itemInfoList: [PurchasedItemInfo] = [PurchasedItemInfo(itemId: "25634", price: 54)!]
                              OR
let itemInfoList: [PurchasedItemInfo] = [PurchasedItemInfo(itemId: 25634, price: 54.12)!]                     
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

*_**Note**_*
- When the Data and Tracking APIs are invoked from the main thread in the Integrator App, the Data SDK switches it to background queue to avoid freezing of UI.

## Stylitics API Documentation

Go through the below link to learn more about the Data and Tracking APIs and their input parameters.

### Data API 
  
[https://widget-api-staging.stylitics.com/index.html#/](https://widget-api-staging.stylitics.com/index.html#/)

### Tracking API 

[http://datastream-staging.stylitics.com/api/api-docs/](http://datastream-staging.stylitics.com/api/api-docs/)

## iOS Versioning Support

- Minimum required iOS APP version to access features of SDK is - (iOS 13.0)

## Technologies Used

Below technologies are used for implementing SDK

- Xcode as IDE
- Swift as programming language
- SwiftLint to enforce Swift style and conventions

## License

Copyright © 2023 Stylitics

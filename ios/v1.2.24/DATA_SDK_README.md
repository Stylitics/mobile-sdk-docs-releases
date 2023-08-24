# Stylitics Data SDK

The Stylitics Data SDK enables a host app to use Stylitics services. Currently, the Data SDK provides services to fetch outfits, item replacements, and send tracking events. 

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

***Notes*** :

- If the integrator app tries to access the outfits API
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
                                                            clientName: "ABC"))
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

### Item add-to-cart event

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

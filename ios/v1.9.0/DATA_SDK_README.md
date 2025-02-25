# Stylitics Data SDK

The Stylitics Data SDK enables a host app to use Stylitics services. Currently, the Data SDK provides below listed services 
1. Fetch data of :
    - Outfits
    - Item Replacements
    - Dynamic Gallery
    - Shop The Set
    - Styled For You
    - Outfit Landing Page(OLP)
    - Upsells Items
    - Trending Bundles
    - Styled For You Full Page
2. Send tracking events

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

- If the Integrator app tries to access any of the Data APIs
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

* For **Data APIs**, the SDK supports Production, Staging, and Custom hosts. The syntax for setting up a staging and custom host for Data APIs is as follows:<br />
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

This method may throw an error if the SDK is not configured. The error type is `Error` with the message "Data SDK isn't configured yet".

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

## Do Not Track

The `doNotTrack(doNotTrack: Bool)` method is to enable or disable the tracking feature. 

- `doNotTrack`: This parameter accepts a boolean value indicating whether tracking should be enabled or disabled within the Data SDK.

### Description

By default, `doNotTrack` is set to `false` indicating tracking is enabled. Data SDK uses previously saved state when app is re-opened.

When `doNotTrack` is set to `true`, all Engagement and Purchase tracking events are restricted within the Data SDK.

When `doNotTrack` is set to `false`, all Engagement and Purchase tracking events will be invoked.

### Example

```Swift
let enableDoNotTrack: Bool = true
StyliticsDataApis.doNotTrack(enableDoNotTrack)
```

## Outfits Count

The `outfitsCountDisplayedByUxSdk(outfits: Outfits, outfitTemplate: OutfitTemplate)` method determines how many outfits will be displayed by the UX SDK for the provided Outfit Template.

- `outfits`: This parameter takes the outfits data.
- `outfitTemplate`: This parameter takes a template type from the available options of Classic, Hotspot, and Grid.

### Description

According to the template type, this method returns the outfits count.

*Note: Integrators using the UX SDK, can use this method to determine how many Outfits will be displayed for the provided template. If it returns 0, Integrator should ensure to set the height to 0 for the Outfit widget to avoid empty space being displayed.*

### Example

```swift
 var shouldShowWidget: Bool {
    StyliticsDataApis.outfitsCountDisplayedByUxSdk(outfits: outfits, outfitTemplate: .grid) > 0
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

### Fetch Outfits using multiple item numbers

More than 1 item number can be passed as a comma separated string.

```swift
        let filterInfo = ["username": "xyz", "item_number": "123456,789101,112131"]

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

### Fetch Shop The Set

Below is the list of filter parameters to invoke the Shop The Set, along with an example of an Api call.
- `username` - Should always be provided.
- `item_number` - should always be provided.
- `total` - Integrator App should send `total` value in range 1 to 10. If it is beyond the range or no value is sent, Data SDK sets it to `4`.
- `min` - Integrator App should send `min` value in range 1 to 10. If it is beyond the range or no value is sent, Data SDK sets it to `3`. It should not be greater than `total`

```Swift
let filterInfo = ["username": "xyz",
                  "item_number": "123456"]

do {
    try StyliticsDataApis.shopTheSet(filterInfo: filterInfo) { response in
        if let error = response.error {
            print("error \(error)")
        } else if let shopTheSet = response.data {
            print("Success \(shopTheSet)")
        }
    }
} catch {
    print("error \(error)")
}
```
### Fetch Styled For You

Below is the list of filter parameters to invoke the Styled For You Api, along with an example of an Api call.
- `username` - Should always be provided.
- `session_id` - Data SDK generates the UUID if Integrator App doesn't pass a value for this. 
- `max` - Integrator App should send `max` value in range 1 to 10. If it is beyond the range or no value is sent, Data SDK sets it to `10`.
- `min` - Integrator App should send `min` value in range 1 to 10. If it is beyond the range or no value is sent, Data SDK sets it to `3`. It should not be greater than `max` value.
- `p.b` - A string value of comma-separated previously browsed item ids.
- `p.p` - A string value of comma-separated previously purchased item ids.

```Swift
let filterInfo = ["username": "demo123",
                  "session_id": "0ca24165-4739-4108-847d-8f6dda95f4ae",
                  "min": "3",
                  "max": "10",
                  "p.b": "123456",
                  "p.p": "123456"]

do {
    try StyliticsDataApis.styledForYou(filterInfo: filterInfo) { response in
        if let error = response.error {
            print("error \(error)")
        } else if let styledForYouData = response.data {
            print("Success \(styledForYouData)")
        }
    }
} catch {
    print("error \(error)")
}
```
### Fetch Outfit Landing Page

Below is the list of filter parameters to invoke the Outfit Landing Page, along with an example of an Api call.
- `username` - Should always be provided.
- `outfitId` - Should always be provided.

```Swift
let outfitId = 123456
let filterInfo = ["username": "demo123"]

do {
    try StyliticsDataApis.outfitLandingPage(outfitId: outfitId,
                                            filterInfo: filterInfo) { response in
        if let error = response.error {
            print("error \(error)")
        } else if let outfitLandingPage = response.data {
            print("Success \(outfitLandingPage)")
        }
    }
} catch {
    print("error \(error)")
}
```

### Fetch Outfit Landing Page with Locale

*Note : If the Integrator App has provided a locale value in the API call, it will have higher precedence than the Global config (if configured).*

```Swift
let outfitId = 123456
let filterInfo = ["username": "demo123", "locale": "en-gb"]

do {
    try StyliticsDataApis.outfitLandingPage(outfitId: outfitId,
                                            filterInfo: filterInfo) { response in
        if let error = response.error {
            print("error \(error)")
        } else if let outfitLandingPage = response.data {
            print("Success \(outfitLandingPage)")
        }
    }
} catch {
    print("error \(error)")
}
```

### Fetch Trending Bundles

Below is the list of filter parameters to invoke the Trending Bundles Api, along with an example of an Api call.

- `username` - Should always be provided.
- `session_id` - Data SDK generates the UUID if Integrator App doesn't pass a value for this.
- `p.b` - A string value of comma-separated previously browsed item ids.
- `p.p` - A string value of comma-separated previously purchased item ids.

```swift
let filterInfo = [Constants.USERNAME_KEY: "demo123",
                  "session_id": "9fc06715-0a59-47e4-b752-be29ff282ea4",
                  "p.b": "123456,789101",
                  "p.p": "234567,89101"]

do {
    try StyliticsDataApis.trendingBundles(filterInfo: filterInfo) { response in
        if let error = response.error {
            print("error \(error)")
        } else if let trendingBundles = response.data {
            print("Success \(trendingBundles)")
        }
    }
} catch {
    print("error \(error)")
}
```

### Fetch Upsell Items

Below is the list of filter parameters to invoke the Upsell Items Api, along with an example of an Api call.

- `username` - Should always be provided.
- `session_id` - Data SDK generates the UUID if Integrator App doesn't pass a value for this.
- `p.b` - A string value of comma-separated previously browsed item ids.
- `p.p` - A string value of comma-separated previously purchased item ids.

```swift
let filterInfo = [Constants.USERNAME_KEY: "demo123",
                  "session_id": "9fc06715-0a59-47e4-b752-be29ff282ea4",
                  "p.b": "123456,789101",
                  "p.p": "234567,89101"]

do {
    try StyliticsDataApis.upsells(filterInfo: filterInfo) { response in
        if let error = response.error {
            print("error \(error)")
        } else if let upsells = response.data {
            print("Success \(upsells)")
        }
    }
} catch {
    print("error \(error)")
}
```

### Fetch Styled For You Full Page 

Below is the list of filter parameters to invoke the StyledForYouFullPage Api, along with an example of an Api call.

- `username` - Should always be provided.
- `session_id` - Data SDK generates the UUID if Integrator App doesn't pass a value for this.
- `p.b` - A string value of comma-separated previously browsed item ids.
- `p.p` - A string value of comma-separated previously purchased item ids.

```swift
let filterInfo = [Constants.USERNAME_KEY: "demo123",
                  "session_id": "9fc06715-0a59-47e4-b752-be29ff282ea4",
                  "p.b": "123456,789101",
                  "p.p": "234567,89101"]
do {
    try StyliticsDataApis.styledForYouFullPage(filterInfo: filterInfo) { response in
        let styledForYouResponse = response.styledForYouResponse
        let trendingBundlesResponse = response.trendingBundlesResponse
        let upsellItemsResponse = response.upsellsResponse

        //This block of code is to access StyledForYou data from StyledForYouFullPageResponse
        if let error = styledForYouResponse.error {
            print("error \(error)")
        } else if let styledForYou = styledForYouResponse.data {
            print("Success \(styledForYou)")
        }

        //This block of code is to access TrendingBundles from StyledForYouFullPageResponse
        if let error = trendingBundlesResponse.error {
            print("error \(error)")
        } else if let trendingBundles = trendingBundlesResponse.data {
            print("Success \(trendingBundles)")
        }

        //This block of code is to access UpsellItems from StyledForYouFullPageResponse
        if let error = upsellItemsResponse.error {
            print("error \(error)")
        } else if let upsells = upsellItemsResponse.data {
            print("Success \(upsells)")
        }
    }
} catch {
    print("error \(error)")
}
```

## Tracking API Examples

*Note: The `position` is 0 index-based, where referenced*

### Jumplink Clicked Event

The **Jumplink Clicked Event** should be triggered when the user clicks on the "See How To Wear It" button. Use the following syntax to handle this event.

> **Note:** This event must be triggered by the Integrator app, because the "jumplink" is always implemented at the Integrator app level.

#### Creating Engagement Tracking Info

1. **For Outfit Bundle:**
   ```swift
   let engagementTrackingInfo = EngagementsTrackingInfo(
       event: .jumplink,
       engagementInfo: .outfitBundle(outfitBundle: outfit, position: 0)
   )
   ```

2. **For Shop the Set:**
   ```swift
   if let topItem = shopTheSetCellViewModel?.shopTheSet.itemSets?[0].first,
      let bottomItem = shopTheSetCellViewModel?.shopTheSet.itemSets?[1].first {
       let engagementTrackingInfo = EngagementsTrackingInfo(
           event: .jumplink,
           engagementInfo: .shopTheSetItem(
               shopTheSetItemsInfo: ShopTheSetItemsInfo(
                   topItem: topItem,
                   topItemPosition: 0,
                   bottomItem: bottomItem,
                   bottomItemPosition: 0,
                   actionItemPosition: .top
               ),
               shopTheSetEventType: .set
           )
       )
   }
   ```

#### Sending the Jumplink Event

Once the tracking info is set up, you can send the event using the following method:

```swift
StyliticsDataApis.engagement(trackingInfo: engagementTrackingInfo)
```

### OutfitBundle

All **OutfitBundle** tracking events are already triggered inside UX SDK when any of the templates are used from UX SDK to display Outfit Bundle for below list of data. 
1. Outfits
2. Dynamic Gallery 
3. Trending Bundles 
4. Styled For You 
5. Outfit Landing Page(OLP)
6. Styled For You Full Page


If Integrator App uses its own custom view to display OutfitBundle, it is suggested to invoke these tracking events by including the `UI component` parameters in the EngagementsTrackingInfo for all *_**OutfitBundle**_* tracking events, so that Stylitics can provide more accurate and personalized recommendations for products.

*Note: `UIComponent` is declared in the Data SDK and is accessible in the Integrator App (outside of the SDK).*


#### OutfitBundle view event

Please refer to the below information on triggering the OutfitBundle view event. This event should be triggered when Outfits come into the viewport and are visible. ***This event should be triggered only once per OutfitBundle page session.***

```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.collage.rawValue]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .outfitBundle(outfitBundle: outfitBundle,
                                                                                    position: index),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### OutfitBundle click event

Please refer to the below example of triggering the OutfitBundle click event.

```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.collage.rawValue]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .outfitBundle(outfitBundle: outfitBundle,
                                                                                    position: index),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### OutfitBundleItem

All **OutfitBundleItem** tracking events are already triggered inside UX SDK when any of the templates are used from UX SDK to display the below list of data. 
1. OutfitBundle Items
2. Upsells Items
3. Outfit Landing Page(OLP) Items

If Integrator App uses its own custom view to display OutfitBundleItem, it is suggested to invoke these tracking event by including the following parameters in the EngagementsTrackingInfo for all *_**OutfitBundleItem**_* tracking events, so that Stylitics can provide more accurate and personalized recommendations for products.
1. UI Component
2. Outfit Bundle

For above parameters, the *_**TrackingInfoKey**_* enum is declared in Data SDK and is accessible in the Integrator App.

#### OutfitBundleItem view event

This event should be triggered when the item comes into the viewport and is visible, using below syntax. ***this event should be triggered only once per current page session.***

```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.productList.rawValue,
                                "outfit_bundle": outfitBundle]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .outfitBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                        position: index),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### OutfitBundleItem click event

This event should be triggered when a user clicks on an item, using below syntax.

```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.productList.rawValue,
                                "outfit_bundle": outfitBundle]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .outfitBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                        position: index),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

*Note:</br>
1. For Product List items, use the UI_COMPONENT value as `UIComponent.PRODUCT_LIST`.</br>
2. For Upsell items, use the UI_COMPONENT value as `UIComponent.ITEM_LIST`, and the outfit_bundle parameter is not required to pass as part of extraInfo.</br>
3. For Outfit Landing Page (OLP) items, use the UI_COMPONENT value as `UIComponent.ITEM_LIST`.*

### Mix and Match

The below events are already triggered inside UX SDK when Classic, Hotspot and Grid template is used from UX SDK to display Outfits. If Integrator App uses its own custom view to display Outfits, please refer to the below information on triggering the Mix and Match related events.

#### OutfitBundleItem swap event

UX SDK sends this tracking event when a swap happens in Product List screen or OutfitBundle collage. If Integrator App uses its own custom view to display Outfits, please refer to the below example of triggering the OutfitBundle item swap event.

```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.productList.rawValue,
                                "outfit_bundle": outfitBundle]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .swap,
                                                      engagementInfo: EngagementInfo.replacement(outfitBundle: outfitBundle,
                                                                                                 oldOutfitBundleItem: oldOutfitBundleItem,
                                                                                                 newOutfitBundleItem: newOutfitBundleItem),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Collapse See More Options event

This event should be triggered when Mix and Match screen come into the viewport and is visible. If Integrator App uses its own custom view to display Outfits, please refer to the below information on triggering the Expand See More Options event. *When triggered by the Stylitics UX SDK, this is accounted for.*

```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.productList.rawValue,
                                "outfit_bundle": outfitBundle]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .seeMoreCollapse,
                                                      engagementInfo: .outfitBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                        position: index),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Expand See More Options event

This event should be triggered upon the close event of Mix and Match screen. If Integrator App uses its own custom view to display Outfits, please refer to the below information on triggering the Collapse See More Options event. *When triggered by the Stylitics UX SDK, this is accounted for.*

```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.productList.rawValue,
                                "outfit_bundle": outfitBundle]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .seeMoreExpand,
                                                      engagementInfo: .outfitBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                        position: index),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Shop The Set Events

#### Shop the Set

It is suggested to include the following parameters in the EngagementsTrackingInfo for all *_**Shop the Set**_* tracking events, so that Stylitics can provide more accurate and personalized recommendations for products.
1. UI Component

*Note: *_**UIComponent**_* is declared in the Data SDK and is accessible in Integrator App (outside of the SDK).*

Tracking events for Shop the Set widget are already handled inside UX SDK when Shop the Set template is used from UX SDK.

#### Item view event for Item

If Integrator App uses its own custom view to display ShopTheSet, please refer to below information on triggering the view event for `item`.
When user swipes top or bottom item from carousel, this event should be triggered for the new item. 
*This event should be triggered only once per page session for a unique item, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .shopTheSetItem(shopTheSetItemsInfo: shopTheSetItemsInfo,
                                                                                      shopTheSetEventType: .item))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Item view event for set

If Integrator App uses its own custom view to display ShopTheSet, please refer to below information on triggering the view event for `set`.
When user swipes top or bottom item from carousel, this event should be triggered for the new unique item set, 
*This event should be triggered only once per page session for a unique item set, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .shopTheSetItem(shopTheSetItemsInfo: shopTheSetItemsInfo,
                                                                                      shopTheSetEventType: .set))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```
#### Item click event for item

If Integrator App uses its own custom view to display ShopTheSet, please refer to below information on triggering the item click event for `item`.
This event should be triggered, when user clicks on the item from the item list of Shop the Set widget.
```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.itemListCta.rawValue]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .shopTheSetItem(shopTheSetItemsInfo: shopTheSetItemsInfo,
                                                                                      shopTheSetEventType: .item),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Item click event for Set

If Integrator App uses its own custom view to display ShopTheSet, please refer to below information on triggering the Item click event for the `set`.
This event should be triggered for the Set of top and bottom, when user clicks on the item from the item list of Shop the Set widget.

```swift
let extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: UIComponent.itemListCta.rawValue]
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .shopTheSetItem(shopTheSetItemsInfo: shopTheSetItemsInfo,
                                                                                      shopTheSetEventType: .set),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Item add-to-cart event

It is suggested to include *_**Page URL**_* parameter in the EngagementsTrackingInfo, so that Stylitics can provide more accurate and personalized recommendations for products.

**Outfit Bundle Item**

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .addToCart,
                                                      engagementInfo: .outfitBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                        position: index))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Purchase Event

This event should be triggered by the Integrator app when a user purchases any item. It enables Stylitics to offer better recommendations for the user.

#### Setting Up Purchased Items Data

`price`: Required; An `Int` or `Double`.
- `itemId`: Required; An `Int` or `String`.
    -  Typically the SKU value or whichever unique identifier you have asked Stylitics to use; 
-  `remoteId`: Optional; A `String`.
    -  Placeholder for future integrations

#### Extracting `itemId`

- For **OutfitBundleItem**, extract it from `outfitBundleItem.itemId`.
- For **ShopTheSetItem**, extract it from `shopTheSetItem.itemId`.

#### Extracting `remoteId`

- For **OutfitBundleItem**, extract it from `outfitBundleItem.remoteId`.
- For **ShopTheSetItem**, extract it from `shopTheSetItem.remoteId`.

*Note:*
1. It is recommended to send the `remoteId` whenever available, as Stylitics uses it to provide recommendations based on past purchases.
2. If `price` or `itemId` is `nil`, the `PurchasedItemInfo` object will not be created.

Example usage:

```swift
let itemInfoList: [PurchasedItemInfo] = [PurchasedItemInfo(remoteId: "remoteId123", itemId: "1234", price: 12.34)!]
                              OR
let itemInfoList: [PurchasedItemInfo] = [PurchasedItemInfo(remoteId: "remoteId123", itemId: "25634", price: 54)!]
                              OR
let itemInfoList: [PurchasedItemInfo] = [PurchasedItemInfo(remoteId: "remoteId123", itemId: 25634, price: 54.12)!]
                              OR
let itemInfoList: [PurchasedItemInfo] = [PurchasedItemInfo(remoteId: nil, itemId: 1234, price: 54.12)!]                                   
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

Copyright © 2024 Stylitics

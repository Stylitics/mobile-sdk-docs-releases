# Stylitics Data SDK

The Stylitics Data SDK enables a host app to use Stylitics services. Currently, the Data SDK provides below listed services 
1. Fetch data of :
    - Outfits
    - Item Replacements
    - Dynamic Gallery
    - Shop The Set
    - Styled For You
    - Outfit Landing Page(OLP)
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

## Tracking API Examples

*Note: The `position` is 0 index-based, where referenced*

### Jumplink clicked event

This event should be triggered when the user clicks on the “See How To Wear It” button, using below syntax.  *Note: This must be triggered by the host app, even if the UI SDK is present, because the “jumplink” is always implemented at the host app level.*

```swift
let engagementTrackingInfo = EngagementsTrackingInfo(event: .jumplink)
StyliticsDataApis.engagement(trackingInfo: engagementTrackingInfo)
```

### Outfit Events

It is suggested to include the following parameters in the EngagementsTrackingInfo for all *_**Outfit's**_* tracking events, so that Stylitics can provide more accurate and personalized recommendations for products.
1. UI Component
2. Page URL
3. Widget Type

For all above parameters, the *_**TrackingInfoKey**_* enum is declared in Data SDK and is accessible in the Integrator App.

```swift
extension Outfit {
    func extraInfo(uiComponent: UIComponent) -> [String: Any]? {
        var extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: uiComponent.rawValue]
        extraInfo[TrackingInfoKey.widgetType.rawValue] = TrackingWidgetType.classic.rawValue
        extraInfo[TrackingInfoKey.outfit.rawValue] = self
        extraInfo[TrackingInfoKey.pageUrl.rawValue] = self.getAnchorItem()?.affiliateLink
        return extraInfo
    }
}
```

*Note:*
* *For Classic Outfit template, `Widget Type` value will be `TrackingWidgetType.classic.rawValue`.*
* *For Hotspot Outfit template, `Widget Type` value will be `TrackingWidgetType.hotspot.rawValue`.*
* *For Grid Outfit template, `Widget Type` value will be `TrackingWidgetType.grid.rawValue`.*
* *UIComponent is declared in the Data SDK and is accessible in the Integrator App (outside of the SDK).*

#### Outfit view event
This event is already triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits. If Integrator App uses its own custom view to display Outfits, please refer to the below information on triggering the Outfit view event. This event should be triggered when Outfits come into the viewport and are visible. *This event should be triggered only once per Outfit page session, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .outfit(outfit: outfit,
                                                                              position: index),
                                                      extraInfo: outfit.extraInfo(uiComponent: .collage))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Outfit click event

This event is triggered inside UX SDK when an Outfit template is used from UX SDK to display Outfits. Using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .outfit(outfit: outfit,
                                                                              position: index),
                                                      extraInfo: outfit.extraInfo(uiComponent: .collage))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### OutfitItem view event

This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items. If Integrator App uses its own custom view to display OutfitItem, this event should be triggered when the user clicks on an outfit and is shown a list of items. Send the outfitItem view event when the item comes into the viewport and is visible, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .outfitItem(outfitItem: outfitItem,
                                                                                  position: index),
                                                      extraInfo: outfit.extraInfo(uiComponent: .productList)))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### OutfitItem click event

This event is already handled inside UX SDK when product list view is used from UX SDK to display Outfit items. If Integrator App uses its own custom view to display OutfitItem, this event should be triggered when a user clicks on an item, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .outfitItem(outfitItem: outfitItem,
                                                                                  position: index),
                                                      extraInfo: outfit.extraInfo(uiComponent: .productList)))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Outfit Item swap event

UX SDK sends this tracking event when a swap happens in Product List screen or Outfit collage. If Integrator App uses its own custom view to display Outfits, please refer to the below example of triggering the Outfit item swap event.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .swap,
                                                      engagementInfo: EngagementInfo.replacement(outfit: outfit,
                                                                                                 oldOutfitItem: oldOutfitItem,
                                                                                                 newOutfitItem: newOutfitItem),
                                                      extraInfo: outfit.extraInfo(uiComponent: .productList)))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Dynamic Gallery Events

It is suggested to include the following parameters in the EngagementsTrackingInfo for all *_**Dynamic Gallery**_* tracking events, so that Stylitics can provide more accurate and personalized recommendations for products.
1. UI Component
2. Page URL

For all above parameters, the *_**TrackingInfoKey**_* enum is declared in Data SDK and is accessible in the Integrator App.

```swift
extension OutfitBundle {
    func extraInfo(uiComponent: UIComponent) -> [String: Any]? {
        var extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: uiComponent.rawValue]
        extraInfo[TrackingInfoKey.pageUrl.rawValue] = self.getAnchorItem()?.affiliateLink
        return extraInfo
    }
}

```

*Note: *_**UIComponent**_* is declared in the Data SDK and is accessible in Integrator App (outside of the SDK).*

#### Outfit Bundle view event

This event is already triggered inside UX SDK when Dynamic Gallery template is used from UX SDK to display GalleryBundles. If Integrator App uses its own custom view to display GalleryBundles, please refer to the below information on triggering the GalleryBundle view event. This event should be triggered when GalleryBundles come into the viewport and are visible. *This event should be triggered only once per GalleryBundle page session, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .galleryBundle(outfitBundle: outfitBundle,
                                                                                     position: index),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .collage)))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Outfit Bundle click event

This event is already triggered inside UX SDK when an Dynamic Gallery template is used from UX SDK to display GalleryBundles. If Integrator App uses its own custom view to display GalleryBundles, please refer to the below example of triggering the GalleryBundle click event.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .galleryBundle(outfitBundle: outfitBundle,
                                                                                     position: index),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .collage))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Outfit Bundle Item view event

This event is already handled inside UX SDK when OutfitBundle Product List view is used from UX SDK to display OutfitBundle items. If Integrator App uses its own custom view to display OutfitBundleItems, this event should be triggered when the user clicks on an OutfitBundle and is shown a list of items. Send the outfitItem view event when the item comes into the viewport and is visible, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .galleryBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                         position: index),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .productList))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```
#### Outfit Bundle Item click event

This event is already handled inside UX SDK when OutfitBundle Product List view is used from UX SDK to display OutfitBundle items. If Integrator App uses its own custom view to display OutfitBundleItems, this event should be triggered when a user clicks on an item, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .galleryBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                         position: index),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .productList)))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Item add-to-cart event

It is suggested to include *_**Page URL**_* parameter in the EngagementsTrackingInfo, so that Stylitics can provide more accurate and personalized recommendations for products.

**Outfit Item**

```swift
var extraInfo: [String: Any] = [:]
extraInfo[TrackingInfoKey.pageUrl.rawValue] = outfitItem.affiliateLink

let engagementsTrackingInfo = EngagementsTrackingInfo(event: .addToCart,
                                                      engagementInfo: .outfitItem(outfitItem: outfitItem,
                                                                                  position: index),
                                                      extraInfo: extraInfo)

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

**Gallery Outfit Bundle Item**

```swift
var extraInfo: [String: Any] = [:]
extraInfo[TrackingInfoKey.pageUrl.rawValue] = outfitBundleItem.affiliateLink

let engagementsTrackingInfo = EngagementsTrackingInfo(event: .addToCart,
                                                      engagementInfo: .galleryBundleItem(outfitBundleItem: outfitBundleItem,
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

For all above parameters, the *_**TrackingInfoKey**_* enum is declared in Data SDK and is accessible in the Integrator App.

```swift
extension ShopTheSetItemsInfo {
    func extraInfo(uiComponent: UIComponent) -> [String: Any]? {
        var extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: uiComponent.rawValue]
        return extraInfo
    }
}
```

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
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .shopTheSetItem(shopTheSetItemsInfo: shopTheSetItemsInfo,
                                                                                      shopTheSetEventType: .item),
                                                      extraInfo: shopTheSetItemsInfo.extraInfo(uiComponent: .itemListCta))

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
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .shopTheSetItem(shopTheSetItemsInfo: shopTheSetItemsInfo,
                                                                                      shopTheSetEventType: .set),
                                                      extraInfo: shopTheSetItemsInfo.extraInfo(uiComponent: .itemListCta))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Styled For You

It is suggested to include the following parameters in the EngagementsTrackingInfo for all *_**Styled For You**_* tracking events, so that Stylitics can provide more accurate and personalized recommendations for products.
1. UI Component
2. Page URL

For all above parameters, the *_**TrackingInfoKey**_* enum is declared in Data SDK and is accessible in the Integrator App.

```swift
extension OutfitBundle {
    func extraInfo(uiComponent: UIComponent) -> [String: Any]? {
        var extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: uiComponent.rawValue]
        extraInfo[TrackingInfoKey.pageUrl.rawValue] = self.getAnchorItem()?.affiliateLink
        return extraInfo
    }
}
```

*Note: *_**UIComponent**_* is declared in the Data SDK and is accessible in Integrator App (outside of the SDK).*

Below events are already triggered inside UX SDK when Styled For You template is used from UX SDK to display StyledForYouBundles.

#### Bundle view event

If Integrator App uses its own custom view to display StyledForYouBundles, please refer to the below information on triggering the StyledForYouBundle view event. This event should be triggered when Outfit bundle come into the viewport and are visible. *This event should be triggered only once per Outfit bundle page session, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .styledForYouBundle(outfitBundle: outfitBundle,
                                                                                          position: index),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .collage)))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```
#### Bundle click event

 If Integrator App uses its own custom view to display Outfit bundle, this event should be triggered when a user clicks on an item, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .styledForYouBundle(outfitBundle: outfitBundle,
                                                                                          position: index),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .collage)))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Outfit Bundle Item view event

This event is already handled inside UX SDK when OutfitBundle Product List view is used from UX SDK to display OutfitBundle items. If Integrator App uses its own custom view to display OutfitBundleItems, this event should be triggered when the user clicks on an OutfitBundle and is shown a list of items. Send the outfitItem view event when the item comes into the viewport and is visible, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .styledForYouBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                              position: index),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .productList))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```
#### Outfit Bundle Item click event

This event is already handled inside UX SDK when OutfitBundle Product List view is used from UX SDK to display OutfitBundle items. If Integrator App uses its own custom view to display OutfitBundleItems, this event should be triggered when a user clicks on an item, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                      engagementInfo: .styledForYouBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                              position: index),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .productList)))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

### Outfit Landing Page

It is suggested to include the following parameters in the EngagementsTrackingInfo for all tracking events, so that Stylitics can provide more accurate and personalized recommendations for products.
 1. UI Component
 2. Page URL
 3. Outfit Bundle
 
 For all above parameters, the *_**TrackingInfoKey**_* enum is declared in Data SDK and is accessible in the Integrator App.

 ```swift
 extension OutfitBundle {
    func extraInfo(uiComponent: UIComponent) -> [String: Any]? {
        var extraInfo: [String: Any] = [TrackingInfoKey.uiComponent.rawValue: uiComponent.rawValue]
        extraInfo[TrackingInfoKey.pageUrl.rawValue] = self.getAnchorItem()?.affiliateLink
        extraInfo[TrackingInfoKey.outfitBundle.rawValue] = self
        return extraInfo
    }
}
```
*Note: *_**UIComponent**_* is declared in the Data SDK and is accessible in Integrator App (outside of the SDK).*


#### Outfit Bundle view event

This event is already triggered inside UX SDK for *_**OutfitBundle**_* and *_**Additional bundles**_* when an OLP template is used from UX SDK. 
If the Integrator App uses its own custom view to display OutfitBundles, please refer to the below information on triggering the view event for OutfitBundle and additional bundles. This event should be triggered when OutfitBundle/Additional bundles come into the viewport and are visible. *This event should be triggered only once per OLP page session, using the below syntax. When triggered by the Stylitics UX SDK, this is accounted for.*


```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .view,
                                                      engagementInfo: .outfitLandingPageBundle(outfitBundle: outfitBundle,
                                                                                               position: position),
                                                      extraInfo: outfitBundle.extraInfo(uiComponent: .collage))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Outfit Bundle click event

This event is already triggered on click event of `Shop this look` cta inside UX SDK for *_**Additional bundles**_* when an OLP template is used from UX SDK to display OLP data. If the Integrator App uses its own custom view to display OLP data, please refer to the below example of triggering the OutfitBundle click event.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                  engagementInfo: .outfitLandingPageBundle(outfitBundle: outfitBundle,
                                                                                           position: position),
                                                  extraInfo: outfitBundle.extraInfo(uiComponent: .collage))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```

#### Outfit Bundle Item view event

This event is already handled inside UX SDK when OLP template and product list view is used from UX SDK to display OutfitBundle items. If the Integrator App uses its own custom view to display OutfitBundle item, this event should be triggered when the user is shown a list of items. 
Send the OutfitBundle item view event when the item comes into the viewport and is visible, using below syntax.

```swift
let engagementsTrackingInfo =  EngagementsTrackingInfo(event: .view,
                                                  engagementInfo: .outfitLandingPageBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                               position: position),
                                                  extraInfo: outfitBundle.extraInfo(uiComponent: .itemList))

StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```
*Note: For Product list OutfitBundle Item view and click events, UIComponent will be *_**productList**_*.*

#### Outfit Bundle Item click event

This event is already handled inside UX SDK when OLP template and product list view is used from UX SDK to display OutfitBundle items. If the Integrator App uses its own custom view to display OutfitBundle item, this event should be triggered when a user clicks on an item, using below syntax.

```swift
let engagementsTrackingInfo = EngagementsTrackingInfo(event: .click,
                                                  engagementInfo: .outfitLandingPageBundleItem(outfitBundleItem: outfitBundleItem,
                                                                                               position: position),
                                                  extraInfo: outfitBundle.extraInfo(uiComponent: .itemList))
                                                  
StyliticsDataApis.engagement(trackingInfo: engagementsTrackingInfo) { response in
    if let error = response.error {
        print("Error in Engagements api response \(error)")
    }
}
```
*Note: For Product list OutfitBundle Item view and click events, UIComponent will be *_**productList**_*.*

### Purchase Event

This event should be triggered by the Integrator app when a user purchases any item. It enables Stylitics to offer better recommendations for the user.

#### Setting Up Purchased Items Data

- `price`: Optional, can be an `Int` or `Double`.
- `itemId`: Optional, can be an `Int` or `String`.
- `remoteId`: Optional `String`.

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

Copyright © 2023 Stylitics

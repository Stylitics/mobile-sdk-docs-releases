
# Stylitics UX SDK

It provides views to display Stylitics data. It also handles invoking of tracking events based on user interaction with these views.

## Features

SDK provides multiple custom view options to Sample Integrator App so it can easily display Stylitics data.
 
1. Outfits
    * Classic Outfit Widget
2. Outfit Items
    * Product List View
3. Replacements
    * Replacement View (Displayed internally from Product List View)

## Classic Outfit Widget

Below are the features for Classic Outfit Widget.

- Configure all the UI elements for each Outfit.
- Handles Outfit `View` and `Click` tracking events so Sample Integrator App does not have to do it.
- Provides listeners to Sample Integrator App so they can handle the Outfit `View` and `Click` events.
- Configure whether to display Outfit Items directly from SDK or not
    - When Outfit Items configured to display from SDK, Sample Integrator App can provide configs for it

### Classic Outfit Widget Configurations:

![image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/ios/ux-sdk-readme/Classic_Outfit_With_Default_new1.png)

**Widget Background**

| Fields | Description | Default Value |
| --- | --- | --- |
| borderColor | is the widget border color | #E1E1E1|
| borderWeight | is the widget border weight | 1px|
| cornerRadius | is the widget corner radius | 8px|
| backgroundColor | is the widget background color | #FFFFFF|

**Top Label**

| Fields | Description | Default Value |
| --- | --- | --- |
| backgroundColor | is the background color of top label  | #FFFFFF |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue Regular |
| fontSize | is the size in CGFloat  | 14px |
| fontColor | is text color  | #212121 |
| position | is to change top label position to top left or top right | TopLabelPosition.topLeft  |
| paddingVertical | is top and bottom padding of the top label in CGFloat | 6px |
| paddingHorizontal | is left and right padding of the top label in CGFloat  | 16px |

**Bottom Label**

| Fields | Description | Default Value |
| --- | --- | --- |
| title | to set the title of the label | View Detail |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue Regular |
| fontSize | is the font size in CGFloat| 14px |
| fontColor | is text color | #212121 |

**Shop The Model**

| Fields | Description | Default Value |
| --- | --- | --- |
| name | is the of image to be displayed for Shop the model badge | Mandatory |
| position | is to change the badge position to the Top Left, Top Right, Bottom Left and Bottom Right. 16px to the top and the left is the default padding | ShopTheModelPosition.topLeft |
| width | is the width of image view in CGFloat  | 60px |
| height | is the height of image view in CGFloat | 60px |

### Default Configurations:

Below are the examples of Classic Outfit Widget when Sample Integrator App chooses to use default UI configurations.</br>

- The Classic Outfit UI component can be implemented in below different ways.
    1. Product List enabled from SDK
    2. Product List disabled from SDK
    3. Configure Event Listeners
    4. Shop The Model

*_**swift**_*

*_**1. Product List enabled from SDK:**_*

When product list is enabled from UX SDK and Sample Integrator App does not provide configurations, it will take default configurations from SDK.

```swift
    private func classicWidgetWithProductListFromUXSDK(outfits: Outfits) -> UIView {
        let productScreenListConfig = ProductListScreenConfig(productListTemplate: .standard( productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            /// In addition to any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
            print("onOutfitItemClick outfit index = \(outfitInfo.position), outfitItemPosition = \(outfitItemInfo.position)")
        })))
        return StyliticsUIApis.load(outfits: outfits,
                                    outfitsTemplate: .classic(),
                                    productListScreenState: .enable(productListScreenConfig: productScreenListConfig))
    }
```

*_**2. Product List disabled from SDK:**_*

```swift
private func classicWidgetWhenProductListFromIntegrator(outfits: Outfits) -> UIView {
    StyliticsUIApis.load(outfits: outfits,
                         outfitsTemplate: .classic(classicListener: ClassicListener(onClick: { outfitInfo in
        /// To display Product List Screen (from Integrator) when user selects an Outfit
        ScreenDisplayUtility.showDetailsOverlayScreen(outfit: outfitInfo.outfit)
    })),
                         productListScreenState: .disable)
}
```

*_**3. Configure Event Listeners:**_*

```swift
private func classicWidgetWithListenersConfigured(outfits: Outfits) -> UIView {
    StyliticsUIApis.load(outfits: outfits,
                         outfitsTemplate: .classic(classicListener: ClassicListener(onClick: { outfitInfo in
        print("Outfit click event triggered : OutfitInfo : \(String(describing: outfitInfo.outfit.id))")
    }, onView: { outfitInfo in
        print("Outfit view event triggered : OutfitInfo : \(String(describing: outfitInfo.outfit.id))")
    })))
}
```

*_**4. Shop The Model:**_*

If in the Outfits response, `on-model-image` flag is true & Sample Integrator App provides a valid image for Shop The Model it will be displayed for the Outfit.

```swift
private func classicWidgetWithShopTheModel(outfits: Outfits) -> UIView {
    StyliticsUIApis.load(outfits: outfits,
                         outfitsTemplate: .classic(classicConfig: ClassicConfig(shopTheModel: ShopTheModel(name: "shopTheLook"))))
}
```
**Default Classic Outfit Widget Screen**

- Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

![default_classic_outfit](https://user-images.githubusercontent.com/28857/227242348-d7481e30-a993-48bd-a0c4-60d209fb8113.png)


### Custom Configurations:

- Sample Integrator App can customise some or all configurations & implement listeners.
- Below are the examples of Classic Outfit Widget when Sample Integrator App customises configurations.

*_**1. With all custom configurations & Listeners:**_*
```swift
func classicWidgetWithAllCustomConfigurations(outfits: Outfits) -> UIView {
    let classicConfig = ClassicConfig(widget: ClassicConfig.Widget(borderColor: CGColor(red: 0.047,
                                                                                        green: 0.125,
                                                                                        blue: 0.560,
                                                                                        alpha: 0.44)),
                                      topLabel: ClassicConfig.TopLabel(fontFamily: "calibri",
                                                                       fontSize: 14,
                                                                       fontColor: UIColor(red: 0.596,
                                                                                          green: 0.364,
                                                                                          blue: 0.007,
                                                                                          alpha: 1),
                                                                       backgroundColor: UIColor(red: 1,
                                                                                                green: 1,
                                                                                                blue: 1,
                                                                                                alpha: 1),
                                                                       borderColor: CGColor(red: 1,
                                                                                            green: 1,
                                                                                            blue: 1,
                                                                                            alpha: 1),
                                                                       borderWeight: 2,
                                                                       cornerRadius: 0,
                                                                       position: .topRight,
                                                                       paddingVertical: 8,
                                                                       paddingHorizontal: 10),
                                      bottomLabel: ClassicConfig.BottomLabel(title: "View More",
                                                                             fontFamily: "calibri",
                                                                             fontSize: 15,
                                                                             fontColor: UIColor(red: 0,
                                                                                                green: 0,
                                                                                                blue: 0,
                                                                                                alpha: 1),
                                                                             showUnderline: false,
                                                                             borderColor: CGColor(red: 0.88,
                                                                                                  green: 0.88,
                                                                                                  blue: 0.88,
                                                                                                  alpha: 1),
                                                                             borderWeight: 2,
                                                                             paddingVertical: 5,
                                                                             paddingHorizontal: 10),
                                      shopTheModel: ShopTheModel(name: "shopTheLook",
                                                                 position: .bottomLeft,
                                                                 width: 60,
                                                                 height: 60))
    
    let classicListener = ClassicListener(onClick: { outfitInfo in
        print("Outfit click event triggered : \(String(describing: outfitInfo.outfit.id))")
    }, onView: { outfitInfo in
        print("Outfit view event triggered : \(String(describing: outfitInfo.outfit.id))")
    })
    
    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(classicConfig: classicConfig,
                                                          classicListener: classicListener))
}
```
*Note : For Shop the model configuration, if height and width provided by Sample Integrator has different aspect ratio than the Image, it will leave some default space around the image and the image will be at the center.*

- Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

![classic_widget_with_all_custom_configs](https://user-images.githubusercontent.com/28857/227242684-c65a2065-e38f-4f7a-9f15-e0a8f156838b.png)

*_**2. With some custom configurations & Listeners:**_*

If Sample Integrator App provides only few custom configurations, UX SDK will take default configurations for missing fields.

```swift
func classicWidgetWithSomeCustomConfigurations(outfits: Outfits) -> UIView {
    let classicConfig = ClassicConfig(topLabel: ClassicConfig.TopLabel(fontFamily: "calibri",
                                                                       fontSize: 14,
                                                                       fontColor: UIColor(red: 0.596,
                                                                                          green: 0.364,
                                                                                          blue: 0.007,
                                                                                          alpha: 1),
                                                                       backgroundColor: UIColor(red: 0.937,
                                                                                                green: 0.937,
                                                                                                blue: 0.937,
                                                                                                alpha: 1),
                                                                       borderWeight: 2,
                                                                       cornerRadius: 0,
                                                                       position: .topRight),
                                      bottomLabel: ClassicConfig.BottomLabel(title: "View Details",
                                                                             fontFamily: "calibri",
                                                                             fontWeight: UIFont.Weight.medium,
                                                                             fontSize: 15,
                                                                             fontColor: UIColor(red: 0,
                                                                                                green: 0.501,
                                                                                                blue: 0.501,
                                                                                                alpha: 1),
                                                                             borderColor: CGColor(red: 0.88,
                                                                                                  green: 0.88,
                                                                                                  blue: 0.88,
                                                                                                  alpha: 1),
                                                                             borderWeight: 2,
                                                                             paddingVertical: 5,
                                                                             paddingHorizontal: 10),
                                      shopTheModel: ShopTheModel(name: "shopTheLook",
                                                                 position: .bottomLeft))
    
    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(classicConfig: classicConfig,
                                                          classicListener: ClassicListener(onClick: { outfitInfo in
        print("Outfit click event triggered : \(String(describing: outfitInfo.outfit.id))")
    })))
}
```

- Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

![classic_widget_with_some_custom_configs](https://user-images.githubusercontent.com/28857/227242909-1abe2725-9544-4963-bc1c-911dd4d18d8c.png)

## Product List Screen

* This screen is displayed when user clicks on an outfit.
* There are two different ways to show Product List Screen.
    1. Product List Screen From UX SDK
    2. Product List Screen Form Sample Integrator App

### Product List Screen From UX SDK

Below are the features for Product List Screen 
- Configure all the UI elements for Product List Screen.
- Handles Outfit Item `View` and `Click` tracking events so Sample Integrator App does not have to do it.
- Provides listeners to Sample Integrator App so they can handle the Outfit Item `View` and `Click` events.
- If Sample Integrator App does not implement Outfit Item click listener, a Web View is opened when user selects an Outfit Item.

*Note - It is recommended that Sample Integrator App always provides the `onOutfitItemClick` listener implementation.*

### Product List Screen Configurations

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/ios/ux-sdk-readme/PL_Default_Titles.png)

*_**Header**_*

  
| Fields | Description | Default Value |
| --- | --- | --- |
| title | to set the title of the text  | Product List |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 16px |
| fontColor | is text color  | #212121 |

*_**Item Name**_*
  
| Fields | Description | Default Value |
| --- | --- | --- |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 16px |
| fontColor | is text color  | #212121 |

*_**Item Sale Price**_*
    
| Fields | Description | Default Value |
| --- | --- | --- |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 16px |
| fontColor | is text color  | #212121 |
| style | is to show or hide the Strike Through Price | PriceStrikethrough = .show |
| slashFontColor | is strike through price text color  | #757575 |
| decimal | is the number of digits to show after decimal point and it is accepted as a integer  | 0 |

*_**Shop CTA**_*

It can be used as a Text or Button, below are the configurations for both

* *_**Shop Text**_*

| Fields | Description | Default Value |
| --- | --- | --- |
| title | to set the title of the text  | Shop |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 14px |
| fontColor | is text color  | #212121 |
| position | is to change shop text position to bottom left or bottom right side  | ShopViewPosition = .left |

* *_**Shop Button**_*
    
| Fields | Description | Default Value |
| --- | --- | --- |
| title | to set the title of the text  | Shop |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
|buttonBackgroundColor | is the shop button background color  | #F5F5F5 |
| fontSize | is the size in CGFloat  | 14px |
| fontColor | is text color  | #212121 |
| horizontalPadding | is left and right padding of the shop button in CGFloat  | 16px |
| verticalPadding | is top and bottom padding of the shop button in CGFloat  | 8px |

### Product List Screen from UX SDK with Default Configurations

Below is the example of Product List Screen when Sample Integrator App chooses to use default UI configurations.

*_**Swift**_*

Below is the code to access Product List Screen from SDK.

It is recommended that Sample Integrator App provide the `onOutfitItemClick` listener implementation.

```swift
private func classicWidgetWithProductListFromUXSDKAndAllDefaultConfigurations(outfits: Outfits) -> UIView {
    StyliticsUIApis.load(outfits: outfits,
                         outfitsTemplate: .classic(),
                         productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(productListTemplate: .standard(productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        print("Outfit item click event triggered : outfitInfo: \(outfitInfo.position), outfitItemInfo: \(outfitItemInfo.position)")
    })))))
}
```

- When Product List Screen is displayed from UX SDK, Sample Integrator App can choose to close it using below code.

```swift
StyliticsUIApis.closeProductListScreen()
```

- Below is the Product List screenshot when Sample Integrator App uses the default configurations

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/ios/ux-sdk-readme/PL_Default.png)

### Product List Screen from UX SDK with Custom Configurations

Below are the examples of Product List Screen when Sample Integrator App chooses to use custom configurations.

*_**Swift**_*

*_**1. With All Custom Configurations and Listeners**_*

```swift
private func classicWidgetWithProductListFromUXSDKAndAllCustomConfigurations(outfits: Outfits) -> UIView {
    let productListConfig = ProductListConfig(itemName: ProductListConfig.ItemName(fontFamily: "calibri",
                                                                                   fontSize: 19,
                                                                                   fontWeight: UIFont.Weight.medium),
                                              itemSalePrice: ProductListConfig.ItemSalePrice(fontSize: 19,
                                                                                             fontWeight: UIFont.Weight.bold,
                                                                                             style: .hide,
                                                                                             decimal: 0),
                                              shop: .text(ProductListConfig.ShopText(title: "Buy Now",
                                                                                     fontFamily: "calibri",
                                                                                     fontSize: 20,
                                                                                     fontColor: UIColor(red: 0,
                                                                                                        green: 0,
                                                                                                        blue: 0.501,
                                                                                                        alpha: 1))
                                              ))

    let productListListener = ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        // Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
        print("Outfit item click event triggered, outfitInfo : \(String(describing: outfitInfo.outfit.id)), outfitItemInfo : \(String(describing: outfitItemInfo.outfitItem.name))")
    }, onOutfitItemView: { outfitInfo, outfitItemInfo in
        print("Outfit item view event triggered : : \(String(describing: outfitInfo.outfit.id)), outfitItemInfo : \(String(describing: outfitItemInfo.outfitItem.name))")
    })

    let itemListHeader = ProductListScreenConfig.ItemListHeader(title: "Product",
                                                                fontSize: 26,
                                                                fontWeight: UIFont.Weight.bold,
                                                                fontColor: UIColor(red: 0,
                                                                                   green: 0.501,
                                                                                   blue: 0.501,
                                                                                   alpha: 1))

    let productListScreenConfig = ProductListScreenConfig(itemListHeader: itemListHeader,
                                                          productListTemplate: .standard(productListConfig: productListConfig,
                                                                                         productListListener: productListListener))

    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(),
                                productListScreenState: .enable(productListScreenConfig: productListScreenConfig))
}
```
- Below is the Product List screenshot when Sample Integrator App uses the above configurations.

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/ios/ux-sdk-readme/PL_All_Config.png)

*_**2. With some custom configurations and listeners**_*

```swift
private func classicWidgetWithProductListFromUXSDKAndSomeCustomConfigurations(outfits: Outfits) -> UIView {
    let productListConfig = ProductListConfig(itemSalePrice: ProductListConfig.ItemSalePrice(fontSize: 18,
                                                                                             fontWeight: UIFont.Weight.bold,
                                                                                             style: .hide,
                                                                                             decimal: 0),
                                              shop: .button(ProductListConfig.ShopButton(fontSize: 14,
                                                                                         fontWeight: UIFont.Weight.medium,
                                                                                         fontColor: UIColor(red: 0,
                                                                                                            green: 0,
                                                                                                            blue: 1,
                                                                                                            alpha: 1)
                                                                                        )
                                              ))

    let productListListener = ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        // Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
        print("Outfit item click event triggered : outfitInfo : \(String(describing: outfitInfo.outfit.id)), outfitItemInfo : \(outfitItemInfo.position))")
    })

    let productListScreenConfig = ProductListScreenConfig(productListTemplate: .standard(productListConfig: productListConfig,
                                                                                         productListListener: productListListener))

    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(),
                                productListScreenState: .enable(productListScreenConfig: productListScreenConfig))
}
```
- Below is the Product List screenshot when Sample Integrator App uses the above configurations.

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/ios/ux-sdk-readme/PL_Some_Config.png)

### Product List Screen From Sample Integrator App

If Sample Integrator App wants to implement their own Product List Screen, they need to implement Outfit click listener as shown below and create view on their own.

```swift
private func classicWidgetWhenProductListFromIntegrator(outfits: Outfits) -> UIView {
    StyliticsUIApis.load(outfits: outfits,
                         outfitsTemplate: .classic(classicListener: ClassicListener(onClick: { outfitInfo in
        /// To display Product List Screen (from Integrator) when user selects an Outfit
        ScreenDisplayUtility.showDetailsOverlayScreen(outfit: outfitInfo.outfit)
    })),
                         productListScreenState: .disable)
}
```
Sample Integrator can create their own Product List View or access and implement it from UX SDK as given below.

*_**1. Product List View with default configurations**_*

Below is the code to call your own Product List Screen. 

```swift
static func showDetailsOverlayScreen(outfit: Outfit) {
    DispatchQueue.main.async {
        let storyboard = UIStoryboard(name: Constants.CLASSIC_DISPLAY_STORYBOARD_IDENTIFIER,
                                      bundle: nil)
        let detailsOverlayViewController = storyboard.instantiateViewController(withIdentifier: Constants.DETAILS_OVERLAY_SCREEN_IDENTIFIER) as! DetailsOverlayViewController
        detailsOverlayViewController.viewModel.prepareData(outfit)
        UIApplication.shared.activeViewController?.present(detailsOverlayViewController, animated: true)
    }
}
```

```swift
func showProductListFromIntegrator() {
    if let outfit = viewModel.outfit {
        let outfitsView = StyliticsUIApis.load(outfit: outfit,
                                               productListTemplate: .standard(productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Outfit item click event triggered : outfitInfo: \(outfitInfo), outfitItemInfo: \(outfitItemInfo)")
        })))
        containerView.addSubviewConstraints(subview: outfitsView)
    }
}
```

*_**2. Product List View with custom configurations**_*

```swift
func showProductListFromIntegrator() {
    if let outfit = viewModel.outfit {
        let outfitsView = StyliticsUIApis.load(outfit: outfit,
                                               productListTemplate: .standard(productListConfig: ProductListConfig(itemName: ProductListConfig.ItemName(fontFamily: "calibri",
                                                                                                                                                        fontSize: 18,
                                                                                                                                                        fontWeight: UIFont.Weight.medium),
                                                                                                                   itemSalePrice: ProductListConfig.ItemSalePrice(fontSize: 14,
                                                                                                                                                                  fontWeight: UIFont.Weight.bold,
                                                                                                                                                                  slashFontColor: UIColor(red: 0,
                                                                                                                                                                                          green: 128/255,
                                                                                                                                                                                          blue: 128/255,
                                                                                                                                                                                          alpha: 1),
                                                                                                                                                                  style: .show,
                                                                                                                                                                  decimal: 0),
                                                                                                                   shop: .text(ProductListConfig.ShopText(title: "Buy",
                                                                                                                                                          fontFamily: "calibri",
                                                                                                                                                          fontSize: 18,
                                                                                                                                                          fontColor: UIColor(red: 0,
                                                                                                                                                                             green: 0,
                                                                                                                                                                             blue: 128/255,
                                                                                                                                                                             alpha: 1))
                                                                                                                   )),
                                                                              productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Outfit item click event triggered : outfitInfo : \(outfitInfo.position), outfitItemInfo : \(outfitItemInfo.position)")
        },
                                                                                                                       onOutfitItemView: { outfitInfo, outfitItemInfo in
            print("Outfit item view event triggered : outfitInfo : \(outfitInfo.position), outfitItemInfo : \(outfitItemInfo.position)")
        })))
        containerView.addSubviewConstraints(subview: outfitsView)
    }
}
```

## Mix and Match (MnM)

- Mix and Match (MnM) feature can be enabled or disabled from Sample Integrator App.
- [Data SDK](DataSDK_ReadMe.md#mix-and-match) has more details to enable Mix & Match.
- When Mix and Match feature is enabled, user can swap items from -
    1. Classic Outfit Widget
    2. Product List View
    
### Classic Outfit Widget with Mix and Match enabled

- When Mix and Match is enabled
    * Swap action is disabled by default but Sample Integrator can enable it
    * Handles item swap tracking event and exposes its listener to Sample Integrator App, for item swap event
- Below is the example to enable swap action and implement the swap listener
 
```swift
private func classicWidgetWithItemSwapEnabled(outfits: Outfits) -> UIView {
    StyliticsUIApis.load(outfits: outfits,
                         outfitsTemplate: .classic(classicListener: ClassicListener(onItemSwap: { outfitId, oldItemId, newItemId in
        print("Item swap event triggered, outfitId = \(outfitId), oldItemId = \(oldItemId), newItemId = \(newItemId)")
    }),
                                                   isItemSwapEnabled: true))
}
```

- Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/ios/ux-sdk-readme/Outfit_widget_Merged.png)

### Product List Screen with Mix and Match

- When Mix and Match is enabled
    - See more options CTA will be displayed for each Outfit Item having Replacement Items.
    - User can swap item from replacement row in Product List.
    - Handles item swap tracking event and exposes its listener to Sample Integrator App, for item swap event.
    
### See more options
  
| Fields | Description | Default Value |
| --- | --- | --- |
| title | to set the title of the text  | 'See more options' |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue Regular |
| fontSize | is the size in CGFloat  | 12px |
| fontColor | is text color  | #212121 |

*_**MnM with Default Configurations**_*

*_**Swift**_*

```swift
private func classicWidgetWithItemSwapFeatureEnabled(outfits: Outfits) -> UIView {
    // When fetching outfits from the Server make sure Mix And Match is enabled to test below feature
    let productListListener = ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        // Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
        print("Item click event triggered, outfitInfo: \(String(describing: outfitInfo.outfit.id)), outfitItemInfo :\(outfitItemInfo.position)")
    },
                                                  onItemSwap: { outfitId, oldItemId, newItemId in
        // When user swaps any item in Product List Screen, this will be
        print("Item swap event triggered, outfitId = \(outfitId), oldItemId = \(oldItemId), newItemId = \(newItemId)")
    })

    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(isItemSwapEnabled: true),
                                productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(productListTemplate: .standard(productListListener: productListListener))))
}
```
*Note: When replacement row is open the title will change to Close and it is not configurable by Sample Integrator*.

- Below is the Product List screenshot when Sample Integrator App uses the above configurations.

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/ios/ux-sdk-readme/PL_Default_Merged.png)

*_**Classic Outfit Widget with some custom configurations for Product List**_*

By default Shop CTA is displayed on left and See more options CTA displayed on right. Sample Integrator can choose to display Shop CTA on right which automatically moves See more options CTA to left

```swift
private func classicWidgetWithMnMCustomConfigurationsOnProductListScreen(outfits: Outfits) -> UIView {
    // When fetching outfits from the Server make sure Mix And Match is enabled to test below feature. Here is the link to know about Mix and Match - https://github.com/Stylitics/mobile-sdk-ui-ios-app/blob/main/DataSDK_ReadMe.md
    let productListConfig = ProductListConfig(shop: .text(ProductListConfig.ShopText(fontFamily: "calibri",
                                                                                     fontColor: UIColor(red: 1,
                                                                                                        green: 0.827,
                                                                                                        blue: 0,
                                                                                                        alpha: 1),
                                                                                     position: .right)
    ),
                                              seeMoreOptions: ProductListConfig.SeeMoreOptions(title: "More Details",
                                                                                               fontFamily: "calibri",
                                                                                               fontSize: 17,
                                                                                               fontWeight: UIFont.Weight.bold,
                                                                                               fontColor: UIColor(red: 0.596,
                                                                                                                  green: 0.364,
                                                                                                                  blue: 0.007,
                                                                                                                  alpha: 1)))

    let productListListener = ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        // Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
        print("Item click event triggered, outfitInfo: \(String(describing: outfitInfo.outfit.id)), outfitItemInfo :\(outfitItemInfo.position)")
    },
                                                  onItemSwap: { outfitId, oldItemId, newItemId in
        // When user swaps any item in Product List Screen, this will be triggered.
        print("Item swap event triggered, outfitId = \(outfitId), oldItemId = \(oldItemId), newItemId = \(newItemId)")
    })

    let productListScreenConfig = ProductListScreenConfig(productListTemplate: .standard(productListConfig: productListConfig ,
                                                                                         productListListener: productListListener))

    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(),
                                productListScreenState: .enable(productListScreenConfig: productListScreenConfig))
}
```
* Below is the Product List screenshot when Sample Integrator App uses the above configurations.

![68747470733a2f2f73746f726167652e676f6f676c65617069732e636f6d2f7374796c69746963732d6d6973632d7075626c69632f73646b2d696d616765732f696f732f75782d73646b2d726561646d652f504c5f44656661756c745f4d65726765642e706e67](https://user-images.githubusercontent.com/28857/227243493-cd0b7306-b37f-4f0d-81fc-5efd6b423149.png)

## iOS Versioning Support

- Minimum required iOS APP version to access features of SDK is - (iOS 13.0)

## License

Copyright © 2023 Stylitics

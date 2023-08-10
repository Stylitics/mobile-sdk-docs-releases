
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

![image1](Screenshots/Classic_Outfit_With_Default_new.png)

**Widget Background**

| Fields | Description | Default Value |
| --- | --- | --- |
| borderColor | is the widget border color | #E1E1E1|
| borderWeight | is the widget border weight | 1px|
| cornerRadius | is the widget corner radius | 8px|
| backgroundColor | is the widget background color | #FFFFFF|

**Top Label**

UX SDK provides various Label styles for the Top Label. [Click here](LABELS_README.md) to learn more about it.

**Bottom Label**

| Fields | Description | Default Value |
|---|---|---|
| title | to set the title of the label | View Detail |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue Regular |
| fontSize | is the font size in CGFloat | 14px |
| fontColor | is text color | #212121 |
| showUnderLine | to HIDE or SHOW the underline for label | true(Show Underline)   |
| paddingVertical | is top and bottom padding of the button in CGFloat | 0px |
| paddingHorizontal | is left and right padding of the button in CGFloat| 0px |
| backgroundColor | is background color | #FFFFFF |
| borderColor | to set border color and | #E1E1E1 |
| borderWeight | is border width | 0px |
| cornerRadius | is border corner radius | 0px |
| bottomSpacing | is to set space between the bottom of label and bottom of widget | 24px |

**Shop The Model**

| Fields | Description | Default Value |
| --- | --- | --- |
| name | is the name of image to be displayed for Shop the model badge | Mandatory |
| position | is to change the badge position to the Top Left, Top Right, Bottom Left and Bottom Right. 16px to the top and the left is the default padding | ShopTheModelPosition.topLeft |
| width | is the width of image view in CGFloat  | 60px |
| height | is the height of image view in CGFloat | 60px |

**Show ScrollBar**

| Fields | Description | Default Value |
| --- | --- | --- |
| showScrollBar | is Boolean value, to Show or Hide the horizontal ScrollBar of Classic Outfit widget | false |

**Widget Top Spacing**

| Fields | Description | Default Value |
|---|---|---|
| widgetTopSpacing | is Float value to to set space between the top of Classic Outfit widget and Outfit ImageView top | 37.5px |

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
            print("outfitItem otherClientItemId = \(String(describing: outfitItemInfo.outfitItem.otherClientItemIds))")
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

![Image1](Screenshots/default_classic_top_model_and_shop.png)

### Custom Configurations:

- Sample Integrator App can customise some or all configurations & implement listeners.
- Below are the examples of Classic Outfit Widget when Sample Integrator App customises configurations.

*_**1. With all custom configurations & Listeners:**_*
```swift
private func classicWidgetWithAllCustomConfigurations(outfits: Outfits) -> UIView {
    guard let widgetBorderColor = UIColor(named: "classic_widget_border_color"),
          let topLabel6FontColor = UIColor(named: "classic_top_label6_font_color"),
          let topLabel6BackgroundColor = UIColor(named: "classic_top_label6_background_color"),
          let bottomLabelFontColor = UIColor(named: "classic_bottom_label_font_color"),
          let bottomLabelBorderColor = UIColor(named: "classic_bottom_label_border_color") else {
        return UIView()
    }
    let classicConfig = ClassicConfig(widget: ClassicConfig.Widget(borderColor: widgetBorderColor,
                                                                   backgroundColor: .clear,
                                                                   widgetTopSpacing: 40),
                                      topLabel: TopLabel(label6: TopLabel.Label6(fontFamilyAndWeight: "Gill Sans Medium",
                                                                                 fontSize: 14,
                                                                                 fontColor: topLabel6FontColor,
                                                                                 backgroundColorAfterAnimation: topLabel6BackgroundColor,
                                                                                 cornerRadius: 0,
                                                                                 position: .topRight,
                                                                                 paddingVertical: 8,
                                                                                 paddingHorizontal: 10)),
                                      bottomLabel: ClassicConfig.BottomLabel(title: "View More",
                                                                             fontFamilyAndWeight: "Gill Sans Bold",
                                                                             fontSize: 15,
                                                                             fontColor: bottomLabelFontColor,
                                                                             showUnderline: false,
                                                                             backgroundColor: .clear,
                                                                             borderColor: bottomLabelBorderColor,
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
                                                          classicListener: classicListener,
                                                          showScrollBar: true))
}
```
*Note : For Shop the model configuration, if height and width provided by Sample Integrator has different aspect ratio than the Image, it will leave some default space around the image and the image will be at the center.*

- Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](Screenshots/classic_widget_with_all_custom_configs.png)

*_**2. With some custom configurations & Listeners:**_*

If Sample Integrator App provides only few custom configurations, UX SDK will take default configurations for missing fields.

```swift
func classicWidgetWithSomeCustomConfigurations(outfits: Outfits) -> UIView {
    guard let topLabel6FontColor = UIColor(named: "classic_top_label6_font_color"),
          let topLabel6BackgroundColor = UIColor(named: "classic_top_label6_background_color"),
          let bottomLabelFontColor = UIColor(named: "classic_bottom_label_font_color"),
          let bottomLabelBorderColor = UIColor(named: "classic_bottom_label_border_color") else {
        return UIView()
    }
    let classicConfig = ClassicConfig(topLabel: TopLabel(label6: TopLabel.Label6(fontFamilyAndWeight: "Gill Sans Bold",
                                                                                 fontSize: 14,
                                                                                 fontColor: topLabel6FontColor,
                                                                                 backgroundColorAfterAnimation: topLabel6BackgroundColor,
                                                                                 cornerRadius: 0,
                                                                                 position: .topRight)),
                                      bottomLabel: ClassicConfig.BottomLabel(title: "View Details",
                                                                             fontFamilyAndWeight: "Gill Sans Medium",
                                                                             fontSize: 15,
                                                                             fontColor: bottomLabelFontColor,
                                                                             borderColor: bottomLabelBorderColor,
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

</br>![Image1](Screenshots/classic_widget_with_some_custom_configs.png)

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

![Image1](Screenshots/PL_Default_Titles.png)

*_**Header**_*

  
| Fields | Description | Default Value |
| --- | --- | --- |
| title | to set the title of the text  | Product List |
| productListScreenHeaderAlign | to set the product list screen title alignment. It will be center aligned when the value is CENTER | Top |  
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 16px |
| fontColor | is text color  | #212121 |

*_**Item Name**_*
  
| Fields | Description | Default Value |
| --- | --- | --- |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 16px |
| fontColor | is text color  | #212121 |
| titleTextTransform | to change the case of item name text. When the value is upper it will appear in upper case | none |

*_**Item Price**_*
    
| Fields | Description | Default Value |
| --- | --- | --- |
| fontFamilyAndWeight | is the text font style with the font weight  | Helvetica Neue medium |
| fontSize | is the font size in CGFloat  | 16px |
| priceFontColor | to set item price text color  | #212121 |
| salePriceFontColor | to set item sale price text color  | #212121 |
| strikeThroughPriceFontColor | is strike through price text color  | #757575 |
| style | is to show or hide the Strike Through Price | PriceStrikethrough = .show |
| swapPricesPosition | is boolean value, when it is false it shows strike through price first and then sale price. Vice versa when true | false |
| decimal | is the number of digits to show after decimal point and it is accepted as a integer  | 2 |

*_**Shop CTA**_*

It can be used as a Text or Button, below are the configurations for both

* *_**Shop Text**_*

| Fields | Description | Default Value |
| --- | --- | --- |
| title | to set the title of the text  | Shop |
| fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 14px |
| fontColor | is text color  | #212121 |
| position | is to change shop text position to bottom left or bottom right side  | ShopViewPosition = .left |

* *_**Shop Button**_*
    
| Fields | Description | Default Value |
| --- | --- | --- |
| title | to set the title of the text  | Shop |
| fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| buttonBackgroundColor | is the shop button background color  | #F5F5F5 |
| fontSize | is the size in CGFloat  | 14px |
| fontColor | is text color  | #212121 |
| horizontalPadding | is left and right padding of the shop button in CGFloat  | 16px |
| verticalPadding | is top and bottom padding of the shop button in CGFloat  | 8px |

*_**Show ScrollBar**_*

| Fields | Description | Default Value |
| --- | --- | --- |
| showScrollBar | is Boolean value, to Show or Hide the vertical ScrollBar of Product List view | false |

*_**Product List Item Background Color**_*

| Fields | Description | Default Value |
| --- | --- | --- |
| itemBackgroundColor | is to change Product List view background color | #FFFFFF |

*_**Product List Item Divider Color**_*

| Fields | Description | Default Value |
| --- | --- | --- |
| itemDividerColor | is to change item divider color | #EEEEEE |


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
            print("Outfit item click event triggered : outfitInfo: \(String(describing: outfitInfo.outfit.id)), outfitItemInfo: \(outfitItemInfo.position)")
            print("outfitItem otherClientItemId = \(String(describing: outfitItemInfo.outfitItem.otherClientItemIds))")
    })))))
}
```

- When Product List Screen is displayed from UX SDK, Sample Integrator App can choose to close it using below code.

```swift
StyliticsUIApis.closeProductListScreen()
```

- Below is the Product List screenshot when Sample Integrator App uses the default configurations

![Image1](Screenshots/PL_Default.png)

### Product List Screen from UX SDK with Custom Configurations

Below are the examples of Product List Screen when Sample Integrator App chooses to use custom configurations.

*_**Swift**_*

*_**1. With All Custom Configurations and Listeners**_*

```swift
private func classicWidgetWithProductListFromUXSDKAndAllCustomConfigurations(outfits: Outfits) -> UIView {
    guard let itemNameFontColor = UIColor(named: "standard_product_list_item_name_font_color"),
          let strikeThroughColor = UIColor(named: "standard_product_list_item_price_strike_color"),
          let shopTextFontColor = UIColor(named: "standard_product_list_shop_text_font_color"),
          let productListScreenTitleFontColor = UIColor(named: "standard_product_list_screen_title_font_color"),
          let productListItemPriceFontColor = UIColor(named: "standard_product_list_item_price_font_color"),
          let productListItemSalePriceFontColor = UIColor(named: "standard_product_list_item_sale_price_font_color"),
          let productListItemBackgroundColor = UIColor(named: "standard_product_list_item_background_color"),
          let itemDividerBackgroundColor = UIColor(named: "standard_product_list_item_divider_color") else {
        return UIView()
    }
    let productListConfig = ProductListConfig(itemName: ProductListConfig.ItemName(fontFamilyAndWeight: "Gill Sans Medium",
                                                                                   fontSize: 19,
                                                                                   fontColor: itemNameFontColor,
                                                                                   titleTextTransform: .upper),
                                              /**
                                               itemPrice - is to set ItemPrice configurations.
                                               priceFontColor - is the color configuration of actual price.
                                               salePriceFontColor - is the color configuration of sale price.
                                               strikeThroughPriceFontColor - is the color configuration of old price.
                                               style - is Hide or Show the strikeThroughPriceFontColor.
                                               swapPricesPosition - swaps the positions of Sale Price and Strike-Through Price.
                                               */
                                              itemPrice: ProductListConfig.ItemPrice(fontFamilyAndWeight: "Gill Sans Italic",
                                                                                     fontSize: 18,
                                                                                     priceFontColor: productListItemPriceFontColor,
                                                                                     salePriceFontColor: productListItemSalePriceFontColor,
                                                                                     strikeThroughPriceFontColor: strikeThroughColor,
                                                                                     style: .show,
                                                                                     swapPricesPosition: true,
                                                                                     decimal: 2),
                                              shop: .text(ProductListConfig.ShopText(title: "Buy Now",
                                                                                     fontFamilyAndWeight: "Gill Sans Bold",
                                                                                     fontSize: 20,
                                                                                     fontColor: shopTextFontColor)
                                              ),
                                              itemBackgroundColor: productListItemBackgroundColor,
                                              itemDividerColor: itemDividerBackgroundColor
    )

    let productListListener = ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        // Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
        print("Outfit item click event triggered, outfitInfo : \(String(describing: outfitInfo.outfit.id)), outfitItemInfo : \(String(describing: outfitItemInfo.outfitItem.name))")
        print("outfitItem otherClientItemId = \(String(describing: outfitItemInfo.outfitItem.otherClientItemIds))")
    }, onOutfitItemView: { outfitInfo, outfitItemInfo in
        print("Outfit item view event triggered : \(String(describing: outfitInfo.outfit.id)), outfitItemInfo : \(String(describing: outfitItemInfo.outfitItem.name))")
    })

    let itemListHeader = ProductListScreenConfig.ItemListHeader(title: "Products",
                                                                productListScreenHeaderAlign: .centre,
                                                                fontSize: 26,
                                                                fontColor: productListScreenTitleFontColor)

    let productListScreenConfig = ProductListScreenConfig(itemListHeader: itemListHeader,
                                                          productListTemplate: .standard(productListConfig: productListConfig,
                                                                                         productListListener: productListListener,
                                                                                         showScrollBar: true))

    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(),
                                productListScreenState: .enable(productListScreenConfig: productListScreenConfig))
}
```
- Below is the Product List screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](Screenshots/PL_All_Custom_configs.png)

*_**2. With some custom configurations and listeners**_*

```swift
private func classicWidgetWithProductListFromUXSDKAndSomeCustomConfigurations(outfits: Outfits) -> UIView {
    guard let shopButtonFontColor = UIColor(named: "standard_product_list_shop_button_font_color"),
          let productListItemSalePriceFontColor = UIColor(named: "standard_product_list_item_sale_price_font_color") else {
        return UIView()
    }
    let productListConfig = ProductListConfig(itemPrice: ProductListConfig.ItemPrice(fontSize: 18,
                                                                                     fontWeight: UIFont.Weight.bold,
                                                                                     strikeThroughPriceFontColor: productListItemSalePriceFontColor,
                                                                                     style: .hide),
                                              shop: .button(ProductListConfig.ShopButton(fontSize: 14,
                                                                                         fontWeight: UIFont.Weight.medium,
                                                                                         fontColor: shopButtonFontColor
                                                                                        )
                                              ))

    let productListListener = ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        // Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
        print("Outfit item click event triggered : outfitInfo : \(String(describing: outfitInfo.outfit.id)), outfitItemInfo : \(outfitItemInfo.position)")
        print("outfitItem otherClientItemId = \(String(describing: outfitItemInfo.outfitItem.otherClientItemIds))")
    })

    let productListScreenConfig = ProductListScreenConfig(productListTemplate: .standard(productListConfig: productListConfig,
                                                                                         productListListener: productListListener))

    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(),
                                productListScreenState: .enable(productListScreenConfig: productListScreenConfig))
}
```
- Below is the Product List screenshot when Sample Integrator App uses the above configurations.

![Image1](Screenshots/pl_some_custom_config.png)

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
                                               productListTemplate: .standard(productListConfig: ProductListConfig(itemName: ProductListConfig.ItemName(fontFamilyAndWeight: "Gill Sans Medium",
                                                                                                                                                        fontSize: 18),
                                                                                                                   itemPrice: ProductListConfig.ItemPrice(fontSize: 14,
                                                                                                                                                          strikeThroughPriceFontColor: UIColor(red: 0,
                                                                                                                                                                                               green: 128/255,
                                                                                                                                                                                               blue: 128/255,
                                                                                                                                                                                               alpha: 1),
                                                                                                                                                          style: .show,
                                                                                                                                                          decimal: 0),
                                                                                                                   shop: .text(ProductListConfig.ShopText(title: "Buy",
                                                                                                                                                          fontFamilyAndWeight: "Gill Sans Bold",
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
- [DATA SDK](DATA_SDK_README.md#mix-and-match) has more details to enable Mix & Match.
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

![Image1](Screenshots/Outfit_widget_Merged.png)

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
        print("outfitItem otherClientItemId = \(String(describing: outfitItemInfo.outfitItem.otherClientItemIds))")
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

![Image1](Screenshots/PL_Default_Merged.png)

*_**Classic Outfit Widget with some custom configurations for Product List**_*

By default Shop CTA is displayed on left and See more options CTA displayed on right. Sample Integrator can choose to display Shop CTA on right which automatically moves See more options CTA to left

```swift
private func classicWidgetWithMnMCustomConfigurationsOnProductListScreen(outfits: Outfits) -> UIView {
    // When fetching outfits from the Server make sure Mix And Match is enabled to test below feature. Here is the link to know about Mix and Match - https://github.com/Stylitics/mobile-sdk-ui-ios-app/blob/main/DATA_SDK_README.md
    guard let shopTextFontColor = UIColor(named: "standard_product_list_shop_text_font_color"),
          let seeMoreFontColor = UIColor(named: "standard_product_list_see_more_font_color") else {
        return UIView()
    }
    let productListConfig = ProductListConfig(shop: .text(ProductListConfig.ShopText(fontFamilyAndWeight: "Gill Sans Bold",
                                                                                     fontColor: shopTextFontColor,
                                                                                     position: .right)
    ),
                                              seeMoreOptions: ProductListConfig.SeeMoreOptions(title: "More Details",
                                                                                               fontFamilyAndWeight: "Gill Sans Italic",
                                                                                               fontSize: 17,
                                                                                               fontColor: seeMoreFontColor))

    let productListListener = ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        // Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
        print("Item click event triggered, outfitInfo: \(String(describing: outfitInfo.outfit.id)), outfitItemInfo :\(outfitItemInfo.position)")
        print("outfitItem otherClientItemId = \(String(describing: outfitItemInfo.outfitItem.otherClientItemIds))")
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

![Image1](Screenshots/pl_some_configs_and_swap.png)</br>

## Localization

Below is the screenshot with localized data when locale is configured to "en-gb".

</br>![Image1](Screenshots/ProductList_with_localization.png)

*Note : When 'locale' is configured and the product items are displayed with the localized data, the price decimal parameter configuration will be ignored.*

## iOS Versioning Support

- Minimum required iOS APP version to access features of SDK is - (iOS 13.0)

## License

Copyright © 2023 Stylitics

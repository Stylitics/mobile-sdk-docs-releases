# Light and Dark Mode Support

This document provides the information about *_**Light and Dark mode support**_* for both default and custom configurations.

## Default Light and Dark Mode Support

Below are the implementation details for the *_**default Light and Dark mode**_* support for UI components implemented in UX SDK.

### 1. Classic Outfit Widget

**Screenshots :**

![Image1](Screenshots/default_light_and_dark_mode_support_for_classic_outfit_widget.png)

### 2. Hotspot Outfit Widget

No default dark mode support is added to the Hotspot Outfit Widget. In dark mode, the Outfit widget will be displayed with the same configurations as shown below.

![Image1](Screenshots/default_light_and_dark_mode_support_for_hotspot_outfit_widget.png)

### 3. Product List

**Screenshots :**

![Image2](Screenshots/default_light_and_dark_mode_support_for_standard_product_list_screen.png)


## Custom Light and Dark Mode Support

The default Light and Dark mode colors can be customised by adding colors to the *_**Assets file**_*. Use *_**Any**_* to define Light mode colors and *_**Dark**_* to define Dark mode colors. Below is the screenshot for the reference.<br /><br />

![Image3](Screenshots/colors_resource_file_location.png)

**_**Note**_* : <br />1. If you select appearance to be of type *_**None**_*, the same color will be used in light and Dark mode. <br />*

Below are the implementation details to customise the Light and Dark mode support from Sample Integrator App.

### 1. Classic Outfit Widget

**Colors :** Below table shows the Classic Outfit Widget component's *_**custom colors for Light and Dark**_* mode.

| Component | Color in Light Mode | Color in Dark Mode |
|---|---|---|
| Bottom label font color        | `#D85454` | `#FFFFFF` |
| Bottom label border font color | `#00838F` | `#FB048C` |
| Widget border color            | `#0C8F66` | `#0277BD` |
| Top label font color           | `#A15B26` | `#13F6C9` |
| Top label border color         | `#EEEEEE` | `#70C5F8` |
| Top label background color     | `#FFFFFF` | `#04304A` |


**Screenshots :**

![Image4](Screenshots/custom_light_and_dark_mode_support_for_classic_outfit_widget.png)

### 2. Hotspot Outfit Widget

**Colors :** Below table shows the Hotspot Outfit Widget component's *_**custom colors for Light and Dark**_* mode.

| Component | Color in Light Mode | Color in Dark Mode |
|---|---|---|
| Widget border color | `#428E66` | `#0277BC` |
| Info label font color | `#FFFFFF` | `#FFFFFF` |
| Info label background color | `#F97D20` | `#AD1457` |
| Info label Price font color | `#000000` | `#FFFFFF` |
| Shop the Look font color | `#30B3C6` | `#A3EC3C` |

**Screenshots :**

![Image4](Screenshots/custom_light_and_dark_mode_support_for_hotspot_outfit_widget.png)

### 3. Product List

**Colors :** Below table shows the Product List component's *_**custom colors in Light and Dark**_* mode.

| Component | Color in Light Mode | Color in Dark Mode |
|---|---|---|
| Screen title font color | `#FF018786` | `#25F772` |
| Item name font color | `#000000` | `#EFEE37` |
| Item price font color | `#FA0505` | `#3AF4DD` |
| Item sale price font color | `#FF000000` | `#EF7CAE` |
| item strike through price font color | `#FF018786` | `#FA0A73` |
| Shop text font color | `#631D35` | `#F4BCBC` |
| Shop button font color  | `#680D35` | `#09CA13` |
| See more config font color | `#631D35` | `#D59FF7` |
| Product list item background color | `#E3C0F8` | `#6A1B9A` |
| Product list item divider color | `#6A1B9A` | `#E3C0F8` |

**Screenshots :**

![Image5](Screenshots/custom_light_and_dark_mode_support_for_standard_product_list.png)

**Code example to customize the Dark And Light Mode colors**

```swift
func classicWidgetWithProductListFromUXSDKAndAllCustomConfigurations(outfits: Outfits) -> UIView {
    guard let itemNameFontColor = UIColor(named: "standard_product_list_item_name_font_color"),
          let productListBrandNameFontColor = UIColor(named: "standard_product_list_brand_name_font_color"),
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
                                              brandName: ProductListConfig.BrandName(fontFamilyAndWeight: "Gill Sans Italic",
                                                                                     fontSize: 18,
                                                                                     fontColor: productListBrandNameFontColor),
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
                                              itemDividerColor: itemDividerBackgroundColor,
                                              hideAnchorItem: true
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
                                                          productListConfig: productListConfig,
                                                          productListListener: productListListener,
                                                          presentationStyle: .fullScreen,
                                                          showScrollBar: true)
    let productListScreenTemplate = ProductListScreenTemplate.standard(productListScreenConfig: productListScreenConfig)

    return StyliticsUIApis.load(outfits: outfits,
                                outfitsTemplate: .classic(),
                                productListScreenTemplate: productListScreenTemplate)
}
```

## In-App Dark Mode Support 

### Settings Screen

In this screen, user can enable/disable the dark mode manually from application.

Below is the reference code to programatically change light/dark mode

```swift
@IBAction func darkModeSwitchAction(_ sender: Any) {
    if #available(iOS 13.0, *) {
        let appDelegate = UIApplication.shared.windows.first
        if darkModeSwitch.isOn {
            appDelegate?.overrideUserInterfaceStyle = .dark
            viewModel.updateDarkModeState(true)
            return
        }
        appDelegate?.overrideUserInterfaceStyle = .light
        viewModel.updateDarkModeState(false)
        return
    }
}
```

![Image6](Screenshots/settings_screen.png)

### Disable Dark Mode

Use the below line of code to disable Dark mode in Integrator app and SDK.

```swift
let appDelegate = UIApplication.shared.windows.first
appDelegate?.overrideUserInterfaceStyle = .light
```

## iOS Versioning Support

- Minimum required iOS APP version to access features of SDK is - (iOS 13.0)

## License

Copyright © 2023 Stylitics

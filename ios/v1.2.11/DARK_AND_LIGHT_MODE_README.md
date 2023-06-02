# Light and Dark Mode Support

This document provides the information about *_**Light and Dark mode support**_* for both default and custom configurations.

## Default Light and Dark Mode Support

Below are the implementation details for the *_**default Light and Dark mode**_* support for UI components implemented in UX SDK.

### 1. Classic Outfit Widget

**Colors :** Below table shows the Classic Outfit Widget component's default colors in Light and Dark mode.

| Component                     | Light Mode     | Dark Mode |
|-------------------------------|----------------|-----------|
| Bottom label font color       | #212121        | #212121   |
| Bottom label background color | #FFFFFF        | #FFFFFF   |
| Widget background color       | #FFFFFF        | #FFFFFF   |
| Widget border color           | #E1E1E1        | #E1E1E1   |

**Screenshots :**

![Image1](https://storage.googleapis.com/stylitics-misc-public/ios/dm1.png)

### 2. Product List

**Colors :** Below table shows the Product List component's default colors in Light and Dark mode.

| Component                            | Light Mode   | Dark Mode   |
|--------------------------------------|--------------|-------------|
| Product list screen title font color | #212121      | #F3F6F6     |
| Item name font color                 | #212121      | #F3F6F6     |
| Item price font color                | #212121      | #F3F6F6     |
| Item sale price font color           | #212121      | #F3F6F6     |
| Strike through price font color      | #757575      | #858994     |
| Shop text font color                 | #212121      | #F3F6F6     |
| Shop button text font color          | #212121      | #33363E     |
| Shop button background color         | #F5F5F5      | #D9DFEB     |
| See more config font color           | #212121      | #F3F6F6     |


**Screenshots :**

![Image2](https://storage.googleapis.com/stylitics-misc-public/ios/dm2.png)


## Custom Light and Dark Mode Support

The default Light and Dark mode colors can be customised by adding colors to the *_**Assets file**_*. Use *_**Any**_* to define Light mode colors and *_**Dark**_* to define Dark mode colors. Below is the screenshot for the reference.<br /><br />

![Image3](https://storage.googleapis.com/stylitics-misc-public/ios/dm3.png)

**_**Note**_* : <br />1. If you select appearance to be of type *_**None**_*, the same color will be used in light and Dark mode. <br />*

Below are the implementation details to customise the Light and Dark mode support from Sample Integrator App.

### 1. Classic Outfit Widget

**Colors :** Below table shows the Classic Outfit Widget component's *_**custom colors for Light and Dark**_* mode.

| Component                      | Color in Light Mode | Color in Dark Mode |
|--------------------------------|---------------------|--------------------|
| Bottom label font color        | #D85454             | #FFFFFFFF          |
| Bottom label border font color | #00838F             | #FB048C            |
| Widget border color            | #0C8F66             | #0277BD            |
| Top label font color           | #A15B26             | #13F6C9            |
| Top label border color         | #EEEEEE             | #70C5F8            |
| Top label background color     | ##FFFFFFFF          | #04304A            |


**Screenshots :**

![Image4](https://storage.googleapis.com/stylitics-misc-public/ios/dm4.png)

### 2. Product List

**Colors :** Below table shows the Product List component's *_**custom colors in Light and Dark**_* mode.

| Component                            | Color in Light Mode | Color in Dark Mode |
|--------------------------------------|---------------------|--------------------|
| Screen title font color              | #FF018786           | #25F772            |
| Item name font color                 | #000000             | #EFEE37            |
| Item price font color                | #FA0505             | #3AF4DD            |
| Item sale price font color           | #FF000000           | #EF7CAE            |
| item strike through price font color | #FF018786           | #FA0A73            |
| Shop text font color                 | #631D35             | #F4BCBC            |
| Shop button font color               | #680D35             | #09CA13            |
| See more config font color           | #631D35             | #D59FF7            |


**Screenshots :**

![Image5](https://storage.googleapis.com/stylitics-misc-public/ios/dm5.png)

**Code example to customize the Dark And Light Mode colors**

```swift

func classicWidgetWithProductListFromUXSDKAndAllCustomConfigurations(outfits: Outfits) -> UIView {
    guard let itemNameFontColor = UIColor(named: "standard_product_list_item_name_font_color"),
          let strikeThroughColor = UIColor(named: "standard_product_list_item_price_strike_color"),
          let shopTextFontColor = UIColor(named: "standard_product_list_shop_text_font_color"),
          let productListScreenTitleFontColor = UIColor(named: "standard_product_list_screen_title_font_color"),
          let productListItemPriceFontColor = UIColor(named: "standard_product_list_item_price_font_color"),
          let productListItemSalePriceFontColor = UIColor(named: "standard_product_list_item_sale_price_font_color") else {
        return UIView()
    }
    let productListConfig = ProductListConfig(itemName: ProductListConfig.ItemName(fontFamily: "calibri",
                                                                                   fontSize: 19,
                                                                                   fontWeight: UIFont.Weight.medium,
                                                                                   fontColor: itemNameFontColor),
                                              /**
                                               itemPrice - is to set ItemPrice configurations.
                                               priceFontColor - is the color configuration of actual price.
                                               salePriceFontColor - is the color configuration of sale price.
                                               strikeThroughPriceFontColor - is the color configuration of old price.
                                               style - is Hide or Show the strikeThroughPriceFontColor.
                                               swapPricesPosition - swaps the positions of Sale Price and Strike-Through Price.
                                               */
                                              itemPrice: ProductListConfig.ItemPrice(fontFamily: "calibri",
                                                                                     fontSize: 18,
                                                                                     fontWeight: UIFont.Weight.bold,
                                                                                     priceFontColor: productListItemPriceFontColor,
                                                                                     salePriceFontColor: productListItemSalePriceFontColor,
                                                                                     strikeThroughPriceFontColor: strikeThroughColor,
                                                                                     style: .show,
                                                                                     swapPricesPosition: true,
                                                                                     decimal: 2),
                                              shop: .text(ProductListConfig.ShopText(title: "Buy Now",
                                                                                     fontFamily: "calibri",
                                                                                     fontSize: 20,
                                                                                     fontColor: shopTextFontColor)
                                              ))
    
    let productListListener = ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
        // Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
        print("Outfit item click event triggered, outfitInfo : \(String(describing: outfitInfo.outfit.id)), outfitItemInfo : \(String(describing: outfitItemInfo.outfitItem.name))")
        print("outfitItem otherClientItemId = \(String(describing: outfitItemInfo.outfitItem.otherClientItemIds))")
    }, onOutfitItemView: { outfitInfo, outfitItemInfo in
        print("Outfit item view event triggered : \(String(describing: outfitInfo.outfit.id)), outfitItemInfo : \(String(describing: outfitItemInfo.outfitItem.name))")
    })
    
    let itemListHeader = ProductListScreenConfig.ItemListHeader(title: "Product",
                                                                fontSize: 26,
                                                                fontWeight: UIFont.Weight.bold,
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
```

![Image6](https://storage.googleapis.com/stylitics-misc-public/ios/dm6.png)


## iOS Versioning Support

- Minimum required iOS APP version to access features of SDK is - (iOS 13.0)

## License

Copyright © 2023 Stylitics

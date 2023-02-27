
## Stylitics UX SDK

It provides views to display Stylitics data. It also handles invoking of tracking events based on user interaction with these views.

### Features

SDK provides multiple custom view options to Integrator App so it can easily display Stylitics data.
 
1. Outfits
    * Classic Display Widget
2. Outfit Items
    * Product List View
3. Replacements
    * Replacement View (Displayed internally from Product List View)

### Classic Outfit Widget
Below are the features for Classic Outfit Widget.</br>

* Configure all the UI elements for each Outfit.
* Handles Outfit 'View' and 'Click' tracking events so integrator app does not have to do it.
* Provides listeners to Integrator App so they can handle the Outfit View and Click events.
* Configure whether to display Outfit Items directly from SDK or not
    * When Outfit Items configured to display from SDK, integrator app can provide configs for it

</br>*_**Classic Outfit Widget Configurations:**_*</br>
</br>
![image1](Screenshots/Classic_Outfit_With_Default_new1.png)
#### Widget 
| Fields | Description | Default Value |
| ---- | ---- | ---- |
| borderColor | is the widget border color | #E1E1E1|
| borderWeight | is the widget border weight | 1px|
| cornerRadius | is the widget corner radius | 8px|
| backgroundColor | is the widget background color | #FFFFFF|

#### Top Label
| Fields | Description | Default Value |
| ---- | ---- | ---- |
| backgroundColor | is the background color of top label  | #FFFFFF |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue Regular |
| fontSize | is the size in CGFloat  | 14px |
| fontColor | is text color  | #212121 |
| position | is to change top label position to top left or top right | TopLabelPosition.topLeft  |
| paddingVertical | is top and bottom padding of the top label in CGFloat | 6px |
| paddingHorizontal | is left and right padding of the top label in CGFloat  | 16px |

#### Bottom Label
| Fields | Description | Default Value |
| ---- | ---- | ---- |
| title | to set the title of the label | View Detail |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue Regular |
| fontSize | is the font size in CGFloat| 14px |
| fontColor | is text color | #212121 |

#### Shop The Model
| Fields | Description | Default Value |
| ---- | ---- | ---- |
| name | is the of image to be displayed for Shop the model badge | Mandatory |
| position | is to change the badge position to the Top Left, Top Right, Bottom Left and Bottom Right. 16px to the top and the left is the default padding | ShopTheModelPosition.topLeft |
| width | is the width of image view in CGFloat  | 60px |
| height | is the height of image view in CGFloat | 60px |

### Default Configurations:

* Below are the examples of Classic Outfit Widget when Integrator App chooses to use default UI configurations.</br>

* The Classic Outfit UI component can be implemented in below different ways.
    1. Product List enabled from SDK
    2. Product List disabled from SDK
    3. Configure Event Listeners
    4. Shop The Model

*_**swift**_*

**1. Product List enabled from SDK:**

When product list is enabled from UX SDK and Integrator App does not provide configurations, it will take default configurations from SDK.

```swift
   private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(),
                             productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(productListTemplate: .standard(productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Outfit Item click event triggered, outfitInfo : \(outfitInfo.position), OutfitItemInfo : \(outfitItemInfo.position)")
        })))))
    }
```

**2. Product List disabled from SDK:**

```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(),
                             productListScreenState: disable)
    }
```

**3. Configure Event Listeners:**

```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(classicListener: ClassicListener(onClick: { outfitinfo in
            print("Outfit click event triggered : OutfitInfo : \(outfitInfo.position)")
        }, onView: { outfitinfo in
            print("Outfit view event triggered : OutfitInfo : \(outfitInfo.position)")
        }, onItemSwap: {outfitId, oldItemId, newItemId in
            print("Outfit swap event OutfitId: \(outfitId), OldItemId: \(oldItemId), NewItemId: \(newItemId)")
        })),
                             productListScreenState: .disable)
    }

```

**4. Shop The Model:**

If in the Outfits response, *_**on-model-image**_* flag is true & Integrator App provides a valid image for Shop The Model it will be displayed for the Outfit.

```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(classicConfig: ClassicConfig(shopTheModel: ShopTheModel(name: "shopTheLook"))),
                             productListScreenState: .disable)
    }
```
**Default Classic Outfit Widget Screen**

* Below is the Classic Outfit Widget screenshot when Integrator App uses the above configurations.

</br>)</br>

### Custom Configurations:

* Integrator App can customise some or all configurations & implement listeners.
* Below are the examples of Classic Outfit Widget when Integrator App customises configurations.

*_**1. With all custom configurations & Listeners:**_*
```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(classicConfig: ClassicConfig(widget: ClassicConfig.Widget(borderColor: CGColor(red: 12/255,
                                                                                                                                      green: 32/255,
                                                                                                                                      blue: 143/255,
                                                                                                                                      alpha: 0.44)),
                                                                                    topLabel: ClassicConfig.TopLabel(fontFamily: "calibri",
                                                                                                                     fontSize: 14,
                                                                                                                     fontColor: UIColor(red: 152/255,
                                                                                                                                        green: 93/255,
                                                                                                                                        blue: 2/255,
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
                                                                                                                           fontSize: 15),
                                                                                    shopTheModel: ShopTheModel(name: "shopTheLook",
                                                                                                               position: .bottomLeft,
                                                                                                               width: 60,
                                                                                                               height: 60)),
                                                       classicListener: ClassicListener(onClick: { outfitinfo in
            print("Outfit click event triggered : \(outfitinfo.position) ")
        }, onView: { outfitinfo in
            print("Outfit view event triggered : \(outfitinfo.position)")
        }, onItemSwap: {outfitId, oldItemId, newItemId in
            print("Outfit swap event OutfitId: \(outfitId), OldItemId: \(oldItemId), NewItemId: \(newItemId)")
        }),
                                                       isItemSwapEnabled: true),
                             productListScreenState: .disable)
    }

```
*_**Note**_* :- For Shop the model configuration, if height and width provided by Integrator has different aspect ratio than the Image, it will leave some default space around the image and the image will be at the center.

* Below is the Classic Outfit Widget screenshot when Integrator App uses the above configurations.

  </br>)

*_**2. With some custom configurations & Listeners:**_*

If Integrator App provides only few custom configurations, UX SDK will take default configurations for missing fields.

```swift
      private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
            StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(classicConfig: ClassicConfig(topLabel: ClassicConfig.TopLabel(fontFamily: "calibri",
                                                                                                                     fontSize: 14,
                                                                                                                     fontColor: UIColor(red: 152/255,
                                                                                                                                        green: 93/255,
                                                                                                                                        blue: 2/255,
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
                                                                                                                           fontSize: 15),
                                                                                    shopTheModel: ShopTheModel(name: "shopTheLook",
                                                                                                               position: .bottomLeft)),
                                                       classicListener: ClassicListener(onClick: { outfitinfo in
            print("Outfit click event triggered : \(outfitinfo.position)")
        }, onItemSwap: {outfitId, oldItemId, newItemId in
            print("Outfit swap event OutfitId: \(outfitId), OldItemId: \(oldItemId), NewItemId: \(newItemId)")
        }),
                                                       isItemSwapEnabled: true),
                             productListScreenState: .disable)
    }
```

* Below is the Classic Outfit Widget screenshot when Integrator App uses the above configurations.

</br>)

## Product List Screen

* This screen is displayed when user clicks on an outfit.
* There are two different ways to show Product List Screen.
    1. Product List Screen From UX SDK
    2. Product List Screen Form Integrator App

### 1. Product List Screen From UX SDK

Below are the features for Product List Screen 
* Configure all the UI elements for Product List Screen.
* Handles Outfit Item 'View' and 'Click' tracking events so Integrator App does not have to do it.
* Provides listeners to Integrator App so they can handle the Outfit Item View and Click events.
* If Integrator App does not implement Outfit Item click listener, a Web View is opened when user selects an Outfit Item.

* Note - It is recommended that Integrator App always provides the **onOutfitItemClick** listener implementation.

### **Product List Screen Configurations**

</br>)

### Header
  
| Fields | Description | Default Value |
| ---- | ---- | ---- |
| title | to set the title of the text  | Product List |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 16px |
| fontColor | is text color  | #212121 |

### Item Name
  
| Fields | Description | Default Value |
| ---- | ---- | ---- |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 16px |
| fontColor | is text color  | #212121 |

### ItemSalePrice
    
| Fields | Description | Default Value |
| ---- | ---- | ---- |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 16px |
| fontColor | is text color  | #212121 |
| style | is to show or hide the Strike Through Price | PriceStrikethrough = .show |
| slashFontColor | is strike through price text color  | #757575 |
| decimal | is the number of digits to show after decimal point and it is accepted as a integer  | 0 |

### Shop CTA

It can be used as a Text or Button, below are the configurations for both

 * *_**Shop Text**_*

| Fields | Description | Default Value |
| ---- | ---- | ---- |
| title | to set the title of the text  | Shop |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
| fontSize | is the size in CGFloat  | 14px |
| fontColor | is text color  | #212121 |
| position | is to change shop text position to bottom left or bottom right side  | ShopViewPosition = .left |

 * *_**Shop Button**_*
    
| Fields | Description | Default Value |
| ---- | ---- | ---- |
| title | to set the title of the text  | Shop |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue medium |
|buttonBackgroundColor | is the shop button background color  | #F5F5F5 |
| fontSize | is the size in CGFloat  | 14px |
| fontColor | is text color  | #212121 |
| horizontalPadding | is left and right padding of the shop button in CGFloat  | 16px |
| verticalPadding | is top and bottom padding of the shop button in CGFloat  | 8px |

### **Product List Screen from UX SDK with Default Configurations**

Below is the example of Product List Screen when Integrator App chooses to use default UI configurations.

*_**Swift**_*

Below is the code to access Product List Screen from SDK.

It is recommended that Integrator App provide the **onOutfitItemClick** listener implementation.

```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(),
                             productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(productListTemplate: .standard(productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Outfit item click event triggered : outfitInfo: \(outfitInfo.position), outfitItemInfo: \(outfitItemInfo.position)")
        }))))
    }
```

* When Product List Screen is displayed from UX SDK, Integrator App can choose to close it using below code.

```swift
     StyliticsUIApis.closeProductListScreen()
```

* Below is the Product List screenshot when Integrator App uses the default configurations

</br>)

### **Product List Screen from UX SDK with Custom Configurations**

Below are the examples of Product List Screen when Integrator App chooses to use Custom configurations.

*_**Swift**_*

### **1. With All Custom Configurations and Listeners**

```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(),
                             productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(itemListHeader: ProductListScreenConfig.ItemListHeader(title: "Product",
                                                                                                                                                                                           fontSize: 26,
                                                                                                                                                                                           fontWeight: UIFont.Weight.bold,
                                                                                                                                                                                           fontColor: UIColor(red: 0,
                                                                                                                                                                                                              green: 128/255,
                                                                                                                                                                                                              blue: 128/255,
                                                                                                                                                                                                              alpha: 1)),
                                                                                                                                    productListTemplate: .standard(productListConfig: ProductListConfig(itemName: ProductListConfig.ItemName(fontFamily: "calibri",
                                                                                                                                                                                                                                                                fontSize: 18,
                                                                                                                                                                                                                                                                fontWeight: UIFont.Weight.medium),
                                                                                                                                                                                                                           itemSalePrice: ProductListConfig.ItemSalePrice(fontSize: 14,
                                                                                                                                                                                                                                                                          fontWeight: UIFont.Weight.bold,
                                                                                                                                                                                                                                                                          slashFontColor: UIColor(red: 0,
                                                                                                                                                                                                                                                                                                  green: 128/255,
                                                                                                                                                                                                                                                                                                  blue: 128/255,
                                                                                                                                                                                                                                                                                                  alpha: 1),
                                                                                                                                                                                                                                                                          style:
                                                                                                                                                                                                                                .show,
                                                                                                                                                                                                                                                                          decimal: 0),
                                                                                                                                                                                                                           shop: .text(ProductListConfig.ShopText(title: "Buy Now",
                                                                                                                                                                                                                                                                                                fontFamily: "calibri",
                                                                                                                                                                                                                                                                                                fontSize: 18,
                                                                                                                                                                                                                                                                                                fontColor: UIColor(red: 0,
                                                                                                                                                                                                                                                                                                                   green: 0,
                                                                                                                                                                                                                                                                                                                   blue: 128/255,
                                                                                                                                                                                                                                                                                                                   alpha: 1))
                                                                                                                                                                                                                           )),
                                                                                                                                                                                      productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Outfit item click event triggered, outfitInfo : \(outfitInfo.position), outfitItemInfo : \(outfitItemInfo.position)")
        }, onOutfitItemView: { outfitInfo, outfitItemInfo in
            print("Outfit item view event triggered : : \(outfitInfo.position), outfitItemInfo : \(outfitItemInfo.position)")
        }, onItemSwap: { outfitId, oldItemId, newItemId in
            print("Outfit item swap event triggered, OutfitId: \(outfitId), OldItemId: \(oldItemId), NewItemId: \(newItemId)")
        })))))
    }
```
* Below is the Product List screenshot when Integrator App uses the above configurations.

</br>)

### **2. With some custom configurations and listeners**

```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(),
                             productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(productListTemplate: .standard(productListConfig: ProductListConfig(itemName: ProductListConfig.ItemName(fontFamily: "calibri",
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
                                                                                                                                                                                                                           shop: .button(ProductListConfig.ShopButton(fontSize: 14,
                                                                                                                                                                                                                                                                      fontWeight: UIFont.Weight.medium,
                                                                                                                                                                                                                                                                      fontColor: UIColor(red: 0,
                                                                                                                                                                                                                                                                                         green: 0,
                                                                                                                                                                                                                                                                                         blue: 1,
                                                                                                                                                                                                                                                                                         alpha: 1)
                                                                                                                                                                                                                                                                     )
                                                                                                                                                                                                                           )),
                                                                                                                                                                                      productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Outfit item click event triggered : outfitInfo : \(outfitInfo.position), outfitItemInfo : \(outfitItemInfo.position)")
        })))))
    }
```
* Below is the Product List screenshot when Integrator App uses the above configurations.

</br>)

### **2. Product List Screen From Integrator App**

If Integrator App wants to implement their own Product List Screen, they need to implement Outfit click listener as shown below and create Activity/Fragment by their own.

```swift
    private func getClassicDisplayWithoutProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(classicListener: ClassicListener(onClick: { outfitInfo in
                //Invoke your own product list here
                            ScreenDisplayUtility.showDetailsOverlayScreen(outfit: outfitInfo.outfit)
        })),
                             productListScreenState: .disable)
    }
```
Integrator can create their own Product List View or access and implement it from UX SDK as given below.

#### **1. Product List View with default configurations**

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

#### **2. Product List View with custom configurations**

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

* Mix and Match (MnM) feature can be enabled or disabled from Integrator App.
*[Data SDK](DataSDK_ReadMe.md#mix-and-match) has more details to enable Mix and Match.
* When Mix and Match feature is enabled, user can swap items from -
    1. Classic Outfit Widget
    2. Product List View
    
### Classic Outfit Widget with Mix and Match enabled

 * When Mix and Match is enabled
    * Swap action is disabled by default but Integrator can enable it
    * Handles item swap tracking event and exposes its listener to Integrator App, for item swap event
 * Below is the example to enable swap action and implement the swap listener
 
```swift
   private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(classicListener: ClassicListener(onItemSwap: { outfitId, oldItemId, newItemId in
            print("Item swap event triggered, outfitId = \(outfitId), oldItemId = \(oldItemId), newItemId = \(newItemId)")
        }),
                                                       isItemSwapEnabled: true),
                             productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(productListTemplate: .standard(productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Item click event triggered, outfitInfo: \(outfitInfo.position) outfitItemInfo :\(outfitItemInfo.position)")
        })))))
    }
```

* Below is the Classic Outfit Widget screenshot when Integrator App uses the above configurations.

</br>)</br>

### Product List Screen with Mix and Match

* When Mix and Match is enabled
    * See more options CTA will be displayed for each Outfit Item having Replacement Items.
    * User can swap item from replacement row in Product List.
    * Handles item swap tracking event and exposes its listener to Integrator App, for item swap event.
    
### See more options
  
| Fields | Description | Default Value |
| ---- | ---- | ---- |
| title | to set the title of the text  | 'See more options' |
|fontFamilyAndWeight | is the font style with the font weight  | Helvetica Neue Regular |
| fontSize | is the size in CGFloat  | 12px |
| fontColor | is text color  | #212121 |

**MnM with Default Configurations**

**Swift**

```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
          StyliticsUIApis.load(outfits: outfits,
                               outfitsTemplate: .classic(),
                               productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(productListTemplate: .standard(productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Item click event triggered, outfitInfo: \(outfitInfo.position) outfitItemInfo :\(outfitItemInfo.position)")
        },
                                                                                      onItemSwap: { outfitId, oldItemId, newItemId in
            print("Item swap event triggered, outfitId = \(outfitId), oldItemId = \(oldItemId), newItemId = \(newItemId)")
        })))))
    }
```
**Note:** When replacement row is open the title will change to Close and it is not configurable by Integrator.

* Below is the Product List screenshot when Integrator App uses the above configurations.

</br>)</br>

**Classic Outfit Widget with some custom configurations for Product List**

By default Shop CTA is displayed on left and See more options CTA displayed on right. Integrator can choose to display Shop CTA on right which automatically moves See more options CTA to left

```swift
    private func getClassicDisplayWithProductListScreen(outfits: Outfits) -> UIView {
        StyliticsUIApis.load(outfits: outfits,
                             outfitsTemplate: .classic(),
                             productListScreenState: .enable(productListScreenConfig: ProductListScreenConfig(productListTemplate: .standard(productListConfig: ProductListConfig(shop: .text(ProductListConfig.ShopText(position: .right)),
                                                                                                                                                                                  seeMoreOptions: ProductListConfig.SeeMoreOptions(title: "View More",
                                                                                                                                                                                                                                   fontFamily: "calibri",
                                                                                                                                                                                                                                   fontSize: 14,
                                                                                                                                                                                                                                   fontWeight: UIFont.Weight.bold,
                                                                                                                                                                                                                                   fontColor: UIColor(red: 0,
                                                                                                                                                                                                                                                      green: 128/255,
                                                                                                                                                                                                                                                      blue: 128/255,
                                                                                                                                                                                                                                                      alpha: 1))),
                                                                                                                                             productListListener: ProductListListener(onOutfitItemClick: { outfitInfo, outfitItemInfo in
            print("Item click event triggered, outfitInfo: \(outfitInfo.position) outfitItemInfo :\(outfitItemInfo.position)")
        })))))
    }
```
* Below is the Product List screenshot when Integrator App uses the above configurations.

</br>)</br>

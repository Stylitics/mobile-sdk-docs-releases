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

Below are the features for Classic Outfit Widget.</br>

* Configure all the UI elements for each Outfit
* Handles Outfit `View` and `Click` tracking events so Sample Integrator App does not have to do it
* Provides listeners to Sample Integrator App so they can handle the Outfit View and Click events
* Configure whether to display Outfit Items directly from SDK or not
    * When Outfit Items configured to display from SDK, Sample Integrator App can provide configs for it

### Classic Outfit Widget Configurations:

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/ux-sdk-readme/classic_with_top_label_stl.png)

**Widget Background**

| Fields          | Description                                                                                    | Default Value |
|-----------------|------------------------------------------------------------------------------------------------|---------------|
| borderColor     | is border color and is accessed from *_**stroke color**_* in drawable resource file             | #E1E1E1       |
| borderWeight    | is border width and is accessed from *_**stroke width**_* in drawable resource file            | 1px           |
| cornerRadius    | is border corner radius and is accessed from *_**corner radius**_* in drawable resource file   | 8px           |
| backgroundColor | is widget background color and is accessed from *_**solid color**_* in drawable resource file | #FFFFFF       |

In Android, Outfit Widget background is set using below XML code of drawable resource file, which contains configurations for the above parameters. 

Drawable Resource File name : classic_widget_background
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="@color/white"/>
    <stroke
        android:width="@dimen/size_2"
        android:color="#6F0C208F" />

    <corners android:radius="@dimen/size_10"/>
</shape>
```

*_**widgetBackground**_* is the configurable parameter to set borderColor, borderWeight, cornerRadius and backgroundColor as shown below.

```kotlin
 widgetBackground = R.drawable.classic_widget_background
```

**Top Label**

UX SDK provides various Label styles for the Top Label. [Click here](LABELS_README.md) to learn more about it.

**Bottom Label**

| Fields              | Description                                                                                    | Default Value          |
|---------------------|------------------------------------------------------------------------------------------------|------------------------|
| title               | to set the title of the label                                                                  | View Detail            |
| fontFamilyAndWeight | is the font style with the font weight and is accessed from the font resource folder           | Helvetica Neue Regular |
| fontSize            | is the font size in float and internally it is converted into sp                               | 14f                    |
| fontColor           | is text color and is accessed from color.xml resource file                                     | #212121                |
| showUnderLine       | to HIDE or SHOW the underline for label                                                        | true(Show Underline)   |
| paddingVertical     | is top and bottom padding of the button in float and internally it is converted to dp          | 0f                     |
| paddingHorizontal   | is left and right padding of the button in float and internally it is converted to dp          | 0f                     |
| bottomSpacing       | is to set space between the bottom of label and widget of bottom                               | 24f                    |
| backgroundColor     | is background color and is accessed from *_**solid color**_* in drawable resource file         | #FFFFFF                |
| borderColor         | to set border color and is accessed from *_**stroke color**_* in drawable resource file        | #E1E1E1                |
| borderWeight        | is border width and is accessed from *_**stroke width**_* in drawable resource file            | 0f                     |
| cornerRadius        | is border corner radius and is accessed from *_**corner radius**_* in drawable resource file   | 0f                     |
| bottomSpacing       | is to set space between the bottom of label and widget of bottom                               | 24f                    |

**Shop The Model**

| Fields   | Description                                                                                                                                   | Default Value                 |
|----------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|
| name     | is the name of image to be displayed for Shop the model badge and is accessed from drawable resource folder                                   | Mandatory                     |
| position | is to change the badge position to the Top Left, Top Right, Bottom Left and Bottom Right. 16dp to the top and the left is the default padding | ShopTheModelPosition.TOP_LEFT |
| width    | is the width of image view in float and internally it is converted to dp                                                                      | 60f                           |
| height   | is the height of image view in float and internally it is converted to dp                                                                     | 60f                           |

**Show ScrollBar**

| Fields        | Description                                                                         | Default Value |
|---------------|-------------------------------------------------------------------------------------|---------------|
| showScrollBar | is Boolean value, to Show or Hide the horizontal ScrollBar of Classic Outfit widget | false         |

### Default Configurations:

* Below are the examples of Classic Outfit Widget when Sample Integrator App chooses to use default UI configurations.</br>

* The Classic Outfit UI component can be implemented in below different ways.
    1. Product List enabled from SDK
    2. Product List disabled from SDK
    3. Configure Event Listeners
    4. Shop The Model

*_**XML**_*

```xml
<com.stylitics.ui.StyliticsUIApi 
        android:id="@+id/outfitsRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="@dimen/size_400" />
```

*_**Kotlin**_*

*_**1. Product List enabled from SDK:**_*

When product list is enabled from UX SDK and Sample Integrator App does not provide configurations, it will take default configurations from SDK.

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithProductListFromUXSDK(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(),
        ProductListScreenState.Enable(
            productListScreenConfig = ProductListScreenConfig(
                productListTemplate = ProductListTemplate.Standard(
                    productListListener = ProductListListener(
                        onOutfitItemClick = { outfitInfo, outfitItemInfo ->
                            //Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
                            Toast.makeText(this, getString(R.string.outfit_item_clicked).plus(" ${outfitItemInfo.position}"), Toast.LENGTH_LONG).show()
                        }
                    )
                )
            )
        )
    )
}
```

*_**2. Product List disabled from SDK:**_*

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWhenProductListFromIntegrator(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(
            classicListener = ClassicListener(
                onClick = { outfitInfo ->
                    //Display Product List Screen from Integrator here
                    showProductList(outfitInfo.outfit)
                }
            )),
        ProductListScreenState.Disable
    )
}
```

*_**3. Configure Event Listeners:**_*

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithListenersConfigured(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(
            classicListener = ClassicListener(
                onClick = { outfitInfo ->
                    Log.i("OutfitEvent", " Outfit click event triggered. ${outfitInfo.outfit.id}")
                },
                onView = { outfitInfo ->
                    Log.i("OutfitEvent", " Outfit view event triggered. ${outfitInfo.outfit.id}")
                }
            )
        )
    )
}
```

*_**4. Shop The Model:**_*

If in the Outfits response, `on-model-image` flag is true & Sample Integrator App provides a valid image for Shop The Model it will be displayed for the Outfit.

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithShopTheModel(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(
            classicConfig = ClassicConfig(
                shopTheModel = ShopTheModel(name = R.drawable.shop_the_look)
            )
        )
    )
}
```

**Default Classic Outfit Widget Screen**

* Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/ux-sdk-readme/default_classic_outfit.png)</br>

### Custom Configurations:

* Sample Integrator App can customise some or all configurations & implement listeners.
* Below are the examples of Classic Outfit Widget when Sample Integrator App customises configurations.

*_**1. With all configurations & Listeners:**_*

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithAllCustomConfigurations(outfits: Outfits) {
    val topLabelConfig = TopLabel(
        label3 = TopLabel.Label3(
            position = TopLabelPosition.TOP_RIGHT,
            fontFamilyAndWeight = R.font.amaranth,
            fontSize = 14f,
            fontColor = R.color.dark_brown,
            paddingVertical = 8f,
            paddingHorizontal = 10f,
            background = R.drawable.top_label_border
        )
    )
    outfitsRecyclerView.load(
        outfits, OutfitsTemplate.Classic(
            classicConfig = ClassicConfig(
                widgetBackground = R.drawable.classic_outfit_border,
                topLabel = topLabelConfig,
                bottomLabel = ClassicConfig.BottomLabel(
                    title = "View More",
                    fontFamilyAndWeight = R.font.amaranth,
                    fontColor = R.color.black,
                    fontSize = 15f,
                    background = R.drawable.view_detail_background,
                    paddingHorizontal = 10f,
                    paddingVertical = 10f,
                    showUnderLine = false
                ),
                shopTheModel = ShopTheModel(
                    name = R.drawable.shop_the_look,
                    position = ShopTheModelPosition.BOTTOM_LEFT,
                    width = 80f,
                    height = 80f
                ),
                showScrollBar = true
            ),
            classicListener = ClassicListener(
                onClick = { outfitInfo ->
                    Log.i("OutfitEvent", " Outfit click event triggered. ${outfitInfo.outfit.id}")
                },
                onView = { outfitInfo ->
                    Log.i("OutfitEvent", " Outfit view event triggered. ${outfitInfo.outfit.id}")
                }
            )
        )
    )
}         
```

*Note : For Shop the model configuration, if height and width provided by Sample Integrator has different aspect ratio than the Image, it will leave some default space around the image and the image will be at the center*.

* Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

  </br>![Image1](https://storage.googleapis.com/stylitics-misc-public/android/u3.png)

</br>*_**2. With some custom configurations & Listeners:**_*

If Sample Integrator App provides only few configurations, UX SDK will take default configurations for missing fields.

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithSomeCustomConfigurations(outfits: Outfits) {
    //Passing custom configurations for multiple Label styles
    val topLabelConfig = TopLabel(
        label2 = TopLabel.Label2(
            background = R.drawable.top_label_border,
        ),
        label3 = TopLabel.Label3(
            position = TopLabelPosition.TOP_RIGHT,
            fontFamilyAndWeight = R.font.amaranth,
            fontSize = 14f,
            fontColor = R.color.dark_brown,
            paddingVertical = 8f,
            paddingHorizontal = 10f,
            background = R.drawable.top_label_border
        ),
        label7 = TopLabel.Label7(
            fontColor = R.color.black
        )
    )
    outfitsRecyclerView.load(
        outfits, OutfitsTemplate.Classic(
            classicConfig = ClassicConfig(
                topLabel = topLabelConfig,
                bottomLabel = ClassicConfig.BottomLabel(
                    title = "View Detail",
                    fontFamilyAndWeight = R.font.amaranth,
                    fontSize = 15f,
                    fontColor = R.color.teal_700,
                    background = R.drawable.view_detail_background,
                    paddingHorizontal = 10f,
                    paddingVertical = 10f
                ),
                shopTheModel = ShopTheModel(
                    name = R.drawable.shop_the_look,
                    position = ShopTheModelPosition.BOTTOM_LEFT
                )
            ),
            classicListener = ClassicListener(
                onClick = { outfitInfo ->
                    Log.i("OutfitEvent", " Outfit click event triggered. ${outfitInfo.outfit.id}")
                }
            )
        )
    )
}         
```

* Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/android/u4.png)

</br>*_**3. Bottom label configuration to display border without overlap with Outfit image:**_*

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithBottomLabelBorder(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(
            classicConfig = ClassicConfig(
                bottomLabel = ClassicConfig.BottomLabel(
                    title = "SHOP THE LOOK",
                    fontSize = 13f,
                    fontColor = R.color.black,
                    showUnderLine = false,
                    paddingVertical = 7f,
                    paddingHorizontal = 10f,
                    fontFamilyAndWeight = R.font.calibri,
                    background = R.drawable.bottom_label_border
                )
            )
        ),
        ProductListScreenState.Enable()
    )
}
```

* Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/android/u5.png)


## Product List Screen

* This screen is displayed when user clicks on an Outfit.
* There are two different ways to show Product List Screen.
    1. Product List Screen From UX SDK
    2. Product List Screen From Sample Integrator App

### Product List Screen From UX SDK

Below are the features for Product List Screen 
* Configure all the UI elements for Product List Screen
* Handles Outfit Item `View` and `Click` tracking events so Sample Integrator App does not have to do it
* Provides listeners to Sample Integrator App so they can handle the Outfit Item `View` and `Click` events
* If Sample Integrator App does not implement Outfit Item click listener, a Web View is opened when user selects an Outfit Item

*Note: It is recommended that Sample Integrator App always provides the **onOutfitItemClick** listener implementation*.

### Product List Screen Configurations

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/ux-sdk-readme/default_product_list_with_arrow.png)

*_**Header**_*

| Fields                        | Description                                                                                                          | Default Value                |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|------------------------------|
| title                         | to set the header of the screen                                                                                      | Product List                 |
| productListScreenHeaderAlign  | to set the product list screen title alignment with close button. It will be center aligned when the value is CENTER | Top                          |
| fontFamilyAndWeight           | is the font style with the font weight and is accessed from the font resource folder                                 | R.font.helvetica_neue_medium |
| fontSize                      | is the font size in float and internally it is converted into SP                                                     | 16f                          |
| fontColor                     | is text color and is accessed from color.xml resource file                                                           | #212121                      |

*_**Item Name**_*

| Fields              | Description                                                                           | Default Value                |
|---------------------|---------------------------------------------------------------------------------------|------------------------------|
| fontFamilyAndWeight | is text font style with the font weight and is accessed from the font resource folder | R.font.helvetica_neue_medium |
| fontSize            | is font size in float and internally it is converted into SP                          | 16f                          |
| fontColor           | is text color which is accessed from color.xml resource file                          | #212121                      |

*_**Item Price**_*

| Fields                      | Description                                                                                                    | Default Value                |
|---|-------------------------------------------------------------------------------------------------------------------|---|
| fontFamilyAndWeight         | is the text font style with the font weight and is accessed from the font resource folder                         | R.font.helvetica_neue_medium |
| fontSize                    | is font size in float and internally it is converted into sp                                                      | 16f                          |
| priceFontColor              | to set item price text color which is accessed from color.xml resource file                                       | #212121                      |
| salePriceFontColor          | to set item sale price text color which is accessed from color.xml resource file                                  | #212121      |
| strikeThroughPriceFontColor | is strike through price text color which is accessed from color.xml resource file                                 | #757575                      |
| style                       | to show or hide the Strike Through Price                                                                          | PriceStrikeThrough.SHOW      |
| decimal                     | is the number of digits to show after decimal point and it is accepted as a integer                               | 2                            |
| swapPricesPosition          | is boolean value, when it is false it shows strike through price first and then sale price. Vice versa when true. | false                        |

*Note : 1. *_**swapPricesPosition**_* in Item Price is to reverse the positions of Price and Strike Through Price.<br />
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. *_**priceFontColor**_* is to change the color of item price.<br />
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. *_**salePriceFontColor**_* is to change the color of item sale price.<br />
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;3. *_**strikeThroughPriceFontColor**_* is to change the color of old price of item.*


*_**Shop CTA**_*

It can be used as a Text or Button, below are the configurations for both.

* *_**Shop Text**_*

| Fields              | Description                                                                               | Default Value                |
|---------------------|-------------------------------------------------------------------------------------------|------------------------------|
| title               | to set the title of the text                                                              | SHOP                         |
| fontFamilyAndWeight | is the text font style with the font weight and is accessed from the font resource folder | R.font.helvetica_neue_medium |
| fontSize            | is font size in float and internally it is converted into sp                              | 14f                          |
| fontColor           | is text color which is accessed from color.xml resource file                              | #212121                      |
| position            | is to change shop text position to bottom left or bottom right                            | ShopViewPosition.LEFT        |

* *_**Shop Button**_*

| Fields                | Description                                                                               | Default Value                     |
|-----------------------|-------------------------------------------------------------------------------------------|-----------------------------------|
| title                 | to set the title of the text                                                              | Shop                              |
| fontFamilyAndWeight   | is the text font style with the font weight and is accessed from the font resource folder | R.font.helvetica_neue_medium      |
| buttonBackgroundColor | is the shop button background drawable                                                    | R.drawable.shop_button_background |
| fontSize              | is font size in float and internally it is converted into sp                              | 14f                               |
| fontColor             | is text color which is accessed from color.xml resource file                              | #212121                           |
| paddingVertical       | is top and bottom padding of the button in float and internally it is converted to dp     | 8f                                |
| paddingHorizontal     | is left and right padding of the button in float and internally it is converted to dp     | 16f                               |

*_**Show ScrollBar**_*

| Fields        | Description                                                                   | Default Value |
|---------------|-------------------------------------------------------------------------------|---------------|
| showScrollBar | is Boolean value, to Show or Hide the vertical ScrollBar of Product List view | false         |

### Product List Screen from UX SDK with Default Configurations

Below is the example of Product List Screen when Sample Integrator App chooses to use default UI configurations.

*_**XML**_*

```xml
<com.stylitics.ui.StyliticsUIApi 
        android:id="@+id/outfitsRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="@dimen/size_400" />
```

*_**Kotlin**_*

Below is the code to access Product List Screen from UX SDK.

It is recommended that Sample Integrator App provide the **onOutfitItemClick** listener implementation.

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithProductListFromUXSDKAndAllDefaultConfigurations(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(),
        ProductListScreenState.Enable(
            productListScreenConfig = ProductListScreenConfig(
                productListTemplate = ProductListTemplate.Standard(
                    productListListener = ProductListListener(
                        onOutfitItemClick = { outfitInfo, outfitItemInfo ->
                            //Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
                            Toast.makeText(this, getString(R.string.outfit_item_clicked).plus(" ${outfitItemInfo.position}"), Toast.LENGTH_LONG).show()
                        }
                    )
                )
            )
        )
    )
}
```
* When Product List Screen is displayed from UX SDK, Sample Integrator App can choose to close it using below code.

```Kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)
outfitsRecyclerView.closeProductListScreen()
```
* Below is the Product List screenshot when Sample Integrator App uses the default configurations

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/ux-sdk-readme/default_product_list.png)

### Product List Screen from UX SDK with Custom Configurations

Below are the examples of Product List Screen when Sample Integrator App chooses to use custom configurations.

*_**XML**_*

```xml
<com.stylitics.ui.StyliticsUIApi 
        android:id="@+id/outfitsRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="@dimen/size_400" />
```

*_**Kotlin**_*

*_**1. With All Custom Configurations and Listeners**_*

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithProductListFromUXSDKAndAllCustomConfigurations(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(),
        ProductListScreenState.Enable(
            productListScreenConfig = ProductListScreenConfig(
                itemListHeader = ProductListScreenConfig.ItemListHeader(
                    title = "Products",
                    productListScreenHeaderAlign = ProductListScreenHeaderAlign.CENTER,
                    fontFamilyAndWeight = R.font.amaranth,
                    fontColor = R.color.teal_700,
                    fontSize = 26f
                ),
                productListTemplate = ProductListTemplate.Standard(
                    productListConfig = ProductListConfig(
                        itemName = ProductListConfig.ItemName(
                            fontFamilyAndWeight = R.font.amaranth,
                            fontSize = 19f,
                            fontColor = R.color.black
                        ),
                        /*
                            itemPrice - is to set ItemPrice configurations.
                            priceFontColor - is the color configuration of actual price.
                            salePriceFontColor - is the color configuration of sale price.
                            strikeThroughPriceFontColor - is the color configuration of old price.
                            style - is Hide or Show the strikeThroughPriceFontColor.
                            swapPricesPosition - swaps the positions of Sale Price and Strike-Through Price.
                        */
                        itemPrice = ProductListConfig.ItemPrice(
                            fontFamilyAndWeight = R.font.amaranth,
                            fontSize = 19f,
                            priceFontColor = R.color.black,
                            salePriceFontColor = R.color.red,
                            strikeThroughPriceFontColor = R.color.teal_700,
                            style = PriceStrikeThrough.SHOW,
                            decimal = 2,
                            swapPricesPosition = true
                        ),
                        shop = ShopViewType.Text(
                            title = "Buy Now",
                            fontFamilyAndWeight = R.font.amaranth,
                            fontSize = 20f,
                            fontColor = R.color.brown
                        ),
                        showScrollBar = true
                    ),
                    productListListener = ProductListListener(
                        onOutfitItemClick = { outfitInfo, outfitItemInfo ->
                            //Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
                            Toast.makeText(this, getString(R.string.outfit_item_clicked).plus(" ${outfitItemInfo.position}"), Toast.LENGTH_LONG).show()
                        },
                        onOutfitItemView = { outfitInfo, outfitItemInfo ->
                            Log.i("OutfitItemEvent", " OutfitItem view event triggered. ${outfitInfo.outfit.id}, ${outfitItemInfo.outfitItem.name}")
                        }
                    )
                )
            )
        )
    )
}
```

* Below is the Product List screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/android/u7.png)

*_**2. With some custom configurations and listeners**_*

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithProductListFromUXSDKAndSomeCustomConfigurations(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(),
        ProductListScreenState.Enable(
            productListScreenConfig = ProductListScreenConfig(
                productListTemplate = ProductListTemplate.Standard(
                    productListConfig = ProductListConfig(
                        itemPrice = ProductListConfig.ItemPrice(
                            fontFamilyAndWeight = R.font.amaranth,
                            strikeThroughPriceFontColor = R.color.teal_700,
                            fontSize = 18f,
                            style = PriceStrikeThrough.HIDE
                        ),
                        shop = ShopViewType.Button(
                            fontFamilyAndWeight = R.font.amaranth,
                            fontSize = 18f,
                            fontColor = R.color.purple
                        )
                    ),
                    productListListener = ProductListListener(
                        onOutfitItemClick = { outfitInfo, outfitItemInfo ->
                            //Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
                            Toast.makeText(this, getString(R.string.outfit_item_clicked).plus(" ${outfitItemInfo.position}"), Toast.LENGTH_LONG).show()
                        }
                    )
                )
            )
        )
    )
}
```

* Below is the Product List screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/android/u8.png)

### Product List Screen From Sample Integrator App

If Sample Integrator App wants to implement their own Product List Screen, they need to implement Outfit click listener as shown below and create Activity/Fragment by their own.

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWhenProductListFromIntegrator(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(
            classicListener = ClassicListener(
                onClick = { outfitInfo ->
                    //Display Product List Screen from Integrator here
                    showProductList(outfitInfo.outfit)
                }
            )),
        ProductListScreenState.Disable
    )
}
```

Sample Integrator can create their own Product List View or access and implement it from UX SDK as given below.

*_**1. Product List View with default configurations**_*

Below is the code to call your own Product List Screen. 

```Kotlin
private fun showProductList(outfit: Outfit) {
    val outfitItemsFragment = OutfitItemsFragment.newInstance(outfit)
    outfitItemsFragment.show(supportFragmentManager, outfitItemsFragment.tag)
}
```

*_**OutfitItemsFragment**_* is the fragment class to show Product List Screen

Add below xml code to your Fragments xml file

*_**XML**_*

```xml
<com.stylitics.ui.StyliticsUIApi 
        android:id="@+id/outfitItemsRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="@dimen/size_400" />
```

*_**Kotlin**_*

To load the Product List invoke below method from Fragments `onCreateView`. 

```Kotlin
val itemView: View = inflater.inflate(R.layout.outfit_items_fragment, container, false)
val outfitItemRecyclerView = itemView.findViewById<StyliticsUIApi>(R.id.outfitsItemRecyclerView)

private fun displayOutfitItemsWithDefaultConfigurations(containerView: View) {
    lifecycleScope.launch {
        itemFragmentViewModel.outfit.collect { outfit ->
            outfitItemRecyclerView.load(
                outfit,
                ProductListTemplate.Standard(
                    productListListener = ProductListListener(
                        onOutfitItemClick = { outfitInfo, outfitItemInfo ->
                            //Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
                            Toast.makeText(context, getString(R.string.outfit_item_clicked).plus(" ${outfitItemInfo.position}"), Toast.LENGTH_LONG).show()
                        }
                    )
                )
            )
        }
    }
}
```
*_**2. Product List View with custom configurations**_*

```Kotlin
val itemView: View = inflater.inflate(R.layout.outfit_items_fragment, container, false)
val outfitItemRecyclerView = itemView.findViewById<StyliticsUIApi>(R.id.outfitsItemRecyclerView)

private fun displayOutfitItemsWithCustomConfigurations(containerView: View) {
    lifecycleScope.launch {
        itemFragmentViewModel.outfit.collect { outfit ->
            outfitItemRecyclerView.load(
                outfit,
                ProductListTemplate.Standard(
                    productListConfig = ProductListConfig(
                        itemName = ProductListConfig.ItemName(
                            fontFamilyAndWeight = R.font.amaranth,
                            fontSize = 19f,
                            fontColor = R.color.black
                        ),
                        itemPrice = ProductListConfig.ItemPrice(
                            fontFamilyAndWeight = R.font.amaranth,
                            fontSize = 18f,
                            priceFontColor = R.color.black,
                            strikeThroughPriceFontColor = R.color.teal_700,
                            style = PriceStrikeThrough.SHOW,
                            decimal = 2
                        ),
                        shop = ShopViewType.Text(
                            title = "Buy",
                            fontFamilyAndWeight = R.font.amaranth,
                            fontSize = 18f,
                            fontColor = R.color.gray_800
                        )
                    ),
                    productListListener = ProductListListener(
                        onOutfitItemClick = { outfitInfo, outfitItemInfo ->
                            //Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
                            Toast.makeText(context, getString(R.string.outfit_item_clicked).plus(" ${outfitItemInfo.position}"), Toast.LENGTH_LONG).show()
                        },
                        onOutfitItemView = { outfitInfo, outfitItemInfo ->
                            Log.i("OutfitItemEvent", " OutfitItem view event triggered. ${outfitInfo.outfit.id}, ${outfitItemInfo.outfitItem.name}")
                        }
                    )
                )
            )
        }
    }
}
```

## Mix and Match (MnM)

* Mix and Match (MnM) feature can be enabled or disabled from Sample Integrator App
* [Data SDK](DATA_SDK_README.md#mix-and-match) has more details to enable Mix & Match
* When Mix and Match feature is enabled, user can swap items from -
    1. Classic Outfit Widget
    2. Product List View

### Classic Outfit Widget with Mix and Match enabled

 * When Mix and Match is enabled
    * Swap action is disabled by default but Sample Integrator can enable it
    * Handles item swap tracking event and exposes its listener to Sample Integrator App, for item swap event
 * Below is the example to enable swap action and implement the swap listener
 
*_**XML**_*

```xml
<com.stylitics.ui.StyliticsUIApi 
        android:id="@+id/outfitsRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="@dimen/size_400" />
```

*_**Kotlin**_*
 ```Kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithItemSwapEnabled(outfits: Outfits) {
    // When fetching outfits from the Server make sure Mix And Match is enabled to test below feature.
    outfitsRecyclerView.load(
        outfits, OutfitsTemplate.Classic(
            classicListener = ClassicListener(
                onItemSwap = { outfitId, oldItemId, newItemId ->
                    //When user swaps any item in Product List Screen, this will be triggered.
                    Log.i("ItemSwapEvent", "Event: Swap, OutfitId: $outfitId, OldItemId: $oldItemId, NewItemId: $newItemId")
                }),
            isItemSwapEnabled = true
        )
    )
}
```

* Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/android/u9.png)

### Product List Screen with Mix and Match

* When Mix and Match is enabled
    * See more options CTA will be displayed for each Outfit Item having Replacement Items
    * User can swap item from replacement row in Product List
    * Handles item swap tracking event and exposes its listener to Sample Integrator App, for item swap event

### See More Options

| Fields              | Description                                                                               | Default Value                 |
|---------------------|-------------------------------------------------------------------------------------------|-------------------------------|
| title               | is the title of text                                                                      | 'See more options'            |
| fontFamilyAndWeight | is the text font style with the font weight and is accessed from the font resource folder | R.font.helvetica_neue_regular |
| fontSize            | is font size in float and internally it is converted into sp                              | 12f                           |
| fontColor           | is text color which is accessed from color.xml resource file                              | #212121                       |

*_**MnM with Default Configurations**_*

*_**Kotlin**_*

 ```Kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithItemSwapFeatureEnabled(outfits: Outfits) {
    // When fetching outfits from the Server make sure Mix And Match is enabled to test below feature
    outfitsRecyclerView.load(
        outfits, OutfitsTemplate.Classic(),
        ProductListScreenState.Enable(
            productListScreenConfig = ProductListScreenConfig(
                productListTemplate = ProductListTemplate.Standard(
                    productListListener = ProductListListener(
                        onOutfitItemClick = { outfitInfo, outfitItemInfo ->
                            //Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
                            Toast.makeText(this, getString(R.string.outfit_item_clicked).plus(" ${outfitItemInfo.position}"), Toast.LENGTH_LONG).show()
                        },
                        onItemSwap = { outfitId, oldItemId, newItemId ->
                            //When user swaps any item in Product List Screen, this will be triggered.
                            Log.i("ItemSwapEvent", "Event: Swap, OutfitId: $outfitId, OldItemId: $oldItemId, NewItemId: $newItemId")
                        }
                    )
                )
            )
        )
    )
}
```
*Note: When replacement row is open the title will change to Close and it is not configurable by Sample Integrator*.

* Below is the Product List screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/ux-sdk-readme/pl_with_mnm.png)

*_**Classic Outfit Widget with some custom configurations for Product List**_*

By default Shop CTA is displayed on left and See more options CTA displayed on right. Sample Integrator can choose to display Shop CTA on right which automatically moves See more options CTA to left

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun classicWidgetWithMnMCustomConfigurationsOnProductListScreen(outfits: Outfits) {
    // When fetching outfits from the Server make sure Mix And Match is enabled to test below feature. Here is the link to know about Mix and Match - https://github.com/Stylitics/mobile-sdk-ui-android-app/blob/main/DATA_SDK_README.md
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(),
        ProductListScreenState.Enable(
            productListScreenConfig = ProductListScreenConfig(
                productListTemplate = ProductListTemplate.Standard(
                    productListConfig = ProductListConfig(
                        shop = ShopViewType.Text(
                            fontColor = R.color.light_yellow,
                            position = ShopViewPosition.RIGHT,
                            fontFamilyAndWeight = R.font.amaranth
                        ),
                        seeMoreConfig = ProductListConfig.SeeMoreOptionsConfig(
                            title = "More Details",
                            fontSize = 17f,
                            fontColor = R.color.brown,
                            fontFamilyAndWeight = R.font.amaranth
                        )
                    ),
                    productListListener = ProductListListener(
                        onOutfitItemClick = { outfitInfo, outfitItemInfo ->
                            //Here, in addition to handling any integrator analytics, natively navigate the user to the selected item's PDP (or launch a quick shop experience).
                            Toast.makeText(this, getString(R.string.outfit_item_clicked).plus(" ${outfitItemInfo.position}"), Toast.LENGTH_LONG).show()
                        },
                        onItemSwap = { outfitId, oldItemId, newItemId ->
                            //When user swaps any item in Product List Screen, this will be triggered.
                            Log.i("ItemSwapEvent", "Event: Swap, OutfitId: $outfitId, OldItemId: $oldItemId, NewItemId: $newItemId")
                        }
                    )
                )
            )
        )
    )
}
```

* Below is the Product List screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/ux-sdk-readme/pl_with_shop_switch_mnm.png)

## Android Versioning Support

- Minimum required android API level to access features of UX SDK is - 21(Android 5.0)
- Target android API level is 32(Android 13/ Beta)

## License

Copyright © 2023 Stylitics

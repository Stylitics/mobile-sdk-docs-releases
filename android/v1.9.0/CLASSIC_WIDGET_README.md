# CLASSIC WIDGET

It provides view to display Stylitics data. It also handles invoking of Widget tracking events based on user interaction with these views.

Below are the features for Classic Outfit Widget.</br>

* Configure all the UI elements for each Outfit
* Handles Outfit `View` and `Click` tracking events so Integrator App does not have to do it
* Provides listeners to Integrator App so they can handle the Outfit View and Click events
* Configure whether to display Outfit Items directly from SDK or not
    * When Outfit Items are configured to display from SDK, Integrator App can provide configs for it along with Classic configs

## Configurations:

![Image1](Screenshots/default_classic_outfit_widget_with_arrows.jpeg)

### Widget

| Fields            | Description                                                                                                     | Default Value  |
|-------------------|-----------------------------------------------------------------------------------------------------------------|----------------|
| `cornerRadius`    | is the border corner radius and is accessed as float and internally it is converted to dp                       | `14f`          |
| `backgroundColor` | is the widget background color and is accessed from color.xml resource file                                     | `#FFFFFF`      |
| `cardGutter`      | is the space between two OutfitBundle cards and is accessed as float and internally it is converted to dp       | `12f`          |
| `cardPeek`        | is the previous and next OutfitBundle card peek and is accessed as float and internally it is converted to dp   | `16f`          |  

### Top Label

UX SDK provides various Label styles for the Top Label. [Click here](LABELS_README.md) to learn more about it.

### Bottom Label

| Fields                 | Description                                                                                                              | Default Value                           | 
|------------------------|--------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| `ctaTitle`             | to set the title of the label                                                                                            | `Shop this look`                        |            
| `fontFamilyAndWeight`  | is the label font style with the font weight and is accessed from the font resource folder                               | `R.font.helvetica_neue_regular`         |            
| `fontSize`             | is the label font size in float and internally it is converted into sp                                                   | `15f`                                   |
| `fontColor`            | is label text color and is accessed from color.xml resource file                                                         | `#FFFFFF`                               | 
| `backgroundColor`      | is widget footer background color and is accessed from color.xml resource file                                           | `#F7F7F7`                               | 
| `ctaBackgroundColor`   | is label background color and is accessed from *_**solid color**_* in drawable resource file                             | `R.drawable.shop_this_look_background`  | 
| `paddingVertical`      | is top and bottom spacing for the content inside widget footer, accepts float value and internally it is converted to dp | `16f`                                   |            
| `paddingHorizontal`    | is left and right spacing for the content inside widget footer, accepts float value and internally it is converted to dp | `20f`                                   |            
| `ctaPaddingVertical`   | is top and bottom spacing for the label's content, accepts float value and internally it is converted to dp              | `7f`                                    |            
| `ctaPaddingHorizontal` | is left and right spacing for the label's content, accepts float value and internally it is converted to dp              | `14f`                                   |

In Android, Bottom label background is set using below XML code of drawable resource file, which contains configurations for the above parameters.

Drawable Resource File name : shop_this_look_background
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">

  <corners android:radius="@dimen/size_100"/>
  <solid android:color="@android:color/black"/>
</shape>
```

*_**ctaBackgroundColor**_* is the configurable parameter to set cornerRadius and ctaBackgroundColor as shown below.

```kotlin
 ctaBackgroundColor = R.drawable.shop_this_look_background
```


### Shop The Model

| Fields      | Description                                                                                                 | Default Value              |
|-------------|-------------------------------------------------------------------------------------------------------------|----------------------------|
| `name`      | is the name of image to be displayed for Shop the model badge and is accessed from drawable resource folder | Mandatory                  |
| `position`  | is to change the badge position to the Top and Bottom. 16dp is the default padding to this view.            | `ShopTheModelPosition.TOP` |
| `width`     | is the width of image view in float and internally it is converted to dp                                    | `60f`                      |
| `height`    | is the height of image view in float and internally it is converted to dp                                   | `60f`                      |

*Note:</br>1. If the "Shop The Model" position is set to ShopTheModelPosition.TOP, the "Shop The Model" view will be placed either to the Right or Left of the screen, depending on the position of the Top Label, in order to avoid any overlapping of views.</br>2. If the "Shop The Model" position is set to ShopTheModelPosition.BOTTOM, it will be positioned to the Left to prevent overlapping with the Bottom Label.*

### Bullet

| Fields               | Description                                                                                                    | Default Value | 
|----------------------|----------------------------------------------------------------------------------------------------------------|---------------|
| `unselectedColor`    | is color of unselected bullet and is accessed from color.xml resource file                                     | `#D3D3D3`     | 
| `selectedColor`      | is color of selected bullet and is accessed from color.xml resource file                                       | `#212121`     | 
| `paddingVertical`    | is top and bottom spacing of the page indicator view in float and internally it is converted to dp             | `10f`         |             
| `paddingHorizontal`  | is spacing between two adjacent bullets of page indicator view in float and internally it is converted to dp   | `8f`          |             

### Top Label Position

| Fields              | Description                                                  | Default Value |
|---------------------|--------------------------------------------------------------|---------------|
| `topLabelPosition`  | is to change the top label position to top left or top right | `TOP.LEFT`    |

[Click here](CODE_REFERENCE_README.md#Classic-Widget-Configuration-Samples) to find code references for different configuration examples.

## Implement Exposed Listeners
Below are the list of Classic Outfit widget listeners exposed to the Integrator app. If integrator wishes to implement their own product list screen they will have to provide the definition for widget `onClick` listener.

1. `onClick` - On click event of widget, this listener will be triggered.
2. `onView` - On view event of Outfit, this listener will be triggered.

## Default Configurations:

* Below are the examples of Classic Outfit Widget when Integrator App chooses to use default UI configurations.</br>

* The Classic Outfit UI component can be implemented in below different ways.
    1. Product List enabled from SDK
    2. Product List disabled from SDK
    3. Configure Event Listeners
    4. Shop The Model

* Classic Outfit Widget supports `WRAP_CONTENT` as a height.


*_**XML**_*

```xml
<com.stylitics.ui.StyliticsUIApi 
        android:id="@+id/outfitsRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
```

*_**Kotlin**_*

### 1. Product List enabled from SDK:

When product list is enabled from UX SDK and Integrator App does not provide configurations, it will take default configurations from SDK.

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun widgetWithProductListFromUXSDK(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic()
    )
}

```

### 2. Product List disabled from SDK:

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun widgetWhenProductListFromIntegrator(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(
            classicListener = ClassicListener(
                onClick = { outfitBundleInfo ->
                    //Display Product List Screen from Integrator here
                    showProductList(outfitBundleInfo.outfitBundle)
                }
            )),
        displayProductListFromSDK = false
    )
}
```

### 3. Configure Event Listeners:

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun widgetWithListenersConfigured(outfits: Outfits) {
    outfitsRecyclerView.load(
        outfits,
        OutfitsTemplate.Classic(
            classicListener = ClassicListener(
                onClick = { outfitBundleInfo ->
                    Log.i("OutfitEvent", " Outfit click event triggered. ${outfitBundleInfo.outfitBundle.id}")
                },
                onView = { outfitBundleInfo ->
                    Log.i("OutfitEvent", " Outfit view event triggered. ${outfitBundleInfo.outfitBundle.id}")
                }
            )
        )
    )
}
```

### 4. Shop The Model:

If in the Outfits response, `on-model-image` flag is true & Integrator App provides a valid image for Shop The Model it will be displayed for the Outfit.

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

private fun widgetWithShopTheModel(outfits: Outfits) {
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

### Default Classic Outfit Widget Screen

* Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](Screenshots/default_classic_outfit.png)</br>

## Custom Configurations:

* Integrator App can customise some or all configurations & implement listeners.
* Below are the examples of Classic Outfit Widget when Sample Integrator App customises configurations.

### 1. With all configurations & Listeners:

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

fun widgetWithAllCustomConfigurations(outfits: Outfits) {
  val topLabelConfig = TopLabel(
    label5 = TopLabel.Label5(
      fontFamilyAndWeight = R.font.amaranth,
      fontSize = 14f,
      fontColor = R.color.top_label_font_color,
      paddingVertical = 8f,
      paddingHorizontal = 10f,
      background = R.drawable.top_label_border
    )
  )

  outfitsRecyclerView?.load(
    outfits, OutfitsTemplate.Classic(
      classicConfig = ClassicConfig(
        bullet = ClassicConfig.Bullet(
          unselectedColor = R.color.classic_unselected_bullet_color,
          selectedColor = R.color.classic_selected_bullet_color,
          paddingVertical = 10f
        ),
        widget = ClassicConfig.Widget(
          cardGutter = ConfigManager.cardGutter,
          cornerRadius = 16f,
          cardPeek = ConfigManager.cardPeek,
          backgroundColor = R.color.classic_widget_background_color
        ),
        topLabel = topLabelConfig,
        topLabelPosition = TopLabelPosition.TOP_RIGHT,
        bottomLabel = ClassicConfig.BottomLabel(
          ctaTitle = "Get this look",
          fontFamilyAndWeight = R.font.calibri,
          fontColor = R.color.classic_bottom_label_font_color,
          fontSize = 16f,
          paddingVertical = 17f,
          paddingHorizontal = 22f,
          ctaPaddingVertical = 6f,
          ctaPaddingHorizontal = 15f,
          backgroundColor = R.color.classic_bottom_label_background_color,
          ctaBackgroundColor = R.drawable.custom_shop_this_look_background_for_classic
        ),
        shopTheModel = ShopTheModel(
          name = R.drawable.shop_the_look,
          position = ShopTheModelPosition.BOTTOM,
          width = 80f,
          height = 80f
        )
      ),
      classicListener = ClassicListener(
        onClick = { outfitBundleInfo ->
          Log.i("OutfitEvent", " Outfit click event triggered. ${outfitBundleInfo.outfitBundle.id}")
        },
        onView = { outfitBundleInfo ->
          Log.i("OutfitEvent", " Outfit view event triggered. ${outfitBundleInfo.outfitBundle.id}")
        }
      )
    )
  )
}
```

*Note : For Shop the model configuration, if height and width provided by Sample Integrator has different aspect ratio than the Image, it will leave some default space around the image and the image will be at the center*.

* Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](Screenshots/classic_widget_with_all_custom_config.png)

### 2. With some custom configurations & Listeners:

If Sample Integrator App provides only few configurations, UX SDK will take default configurations for missing fields.

```kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)

fun widgetWithSomeCustomConfigurations(outfits: Outfits) {
  //Passing custom configurations for multiple Label styles
  val topLabelConfig = TopLabel(
    label2 = TopLabel.Label2(
      background = R.drawable.top_label_border,
    ),
    label1 = TopLabel.Label1(
      fontFamilyAndWeight = R.font.amaranth,
      fontSize = 14f,
      fontColor = R.color.top_label_font_color,
      paddingVertical = 8f,
      paddingHorizontal = 10f,
      background = R.drawable.top_label_border
    ),
    label7 = TopLabel.Label7(
      fontColor = R.color.top_label_font_color
    )
  )
  outfitsRecyclerView?.load(
    outfits, OutfitsTemplate.Classic(
      classicConfig = ClassicConfig(
        topLabel = topLabelConfig,
        widget = ClassicConfig.Widget(
          cornerRadius = 40f,
          backgroundColor = R.color.white,
        ),
        bottomLabel = ClassicConfig.BottomLabel(
          ctaTitle = "Purchase the look",
          fontColor = R.color.white,
          backgroundColor = R.color.gallery_header_context_background_color,
          ctaPaddingHorizontal = 25f,
        ),
        shopTheModel = ShopTheModel(
          name = R.drawable.shop_the_look,
          position = ShopTheModelPosition.BOTTOM
        ),
        topLabelPosition = TopLabelPosition.TOP_RIGHT
      ),
      classicListener = ClassicListener(
        onClick = { outfitBundleInfo ->
          Log.i("OutfitEvent", " Outfit click event triggered. ${outfitBundleInfo.outfitBundle.id}")
        }
      )
    )
  )
}         
```

* Below is the Classic Outfit Widget screenshot when Sample Integrator App uses the above configurations.

</br>![Image1](Screenshots/classic_outfit_widget_with_custom_bottom_label.png)

## Refresh Classic Outfit Widget

**Overview**

The `refreshTemplate` method can be used to update the Classic Outfit widget data or its configurations or both.

**Example**

```Kotlin
fun refreshTemplate(outfits: Outfits? = null, widgetConfig: IWidgetConfig? = null)
```

**Parameters**

- `outfits`: Optional parameter to provide updated Outfits data.
- `widgetConfig`: Optional parameter to provide updated configurations for Classic Outfits template.

**Usage**

Call the method on the view with optional data/config.

- Get the Classic Outfit Widget Template id
```Kotlin
val outfitsRecyclerView = findViewById<StyliticsUIApi>(R.id.outfitsRecyclerView)
//Load Classic Outfit Widget Template
outfitsRecyclerView.load(outfits, OutfitsTemplate.Classic())
```

- To refresh the Classic Outfit Widget Template with new Outfit data
```Kotlin
outfitsRecyclerView.refreshTemplate(outfits = newOutfits)
```
- To refresh the Classic Outfit Widget Template with new config
```Kotlin
outfitsRecyclerView.refreshTemplate(widgetConfig = newConfig)
```
- To refresh the Classic Outfit Widget Template with both new Outfit data and config
```Kotlin
outfitsRecyclerView.refreshTemplate(newOutfits, newConfig)
```

## License

Copyright © 2023 Stylitics
# Stylitics Sample Integrator App Code Reference 

This document provides references for implementation details by Sample Integrator App screen.

## Configure Username and Environment

Below are the implementation details for Username and Environment configurations screen

* **UserAccountsActivity.kt** This activity has all the implementations to display the available usernames and based on the user selection it proceeds to the user, client and environment configuration.
* **UserAccountsViewModel.kt** This class handles business logic behind first screen using below methods.
     1. **configureSDK()** - This performs the required data configurations


## StyliticsUI Screen

* **ItemsActivity.kt** This class has an implementation for the Tab Layout that holds the two tabs. These tabs are responsible for holding the Grid screen, 
Product detail screen and custom Product List(If default Product List from UX SDK is disabled.) 

### Show Product Items(Grid Screen)

Below are the implementation details for Show Product Items(Grid screen) screen

* **ItemsFragment.kt** This class is responsible for displaying the Sample Product items.

### PDP Screen

Below are the implementation details for Product details page.

* **OutfitsViewModel.kt** This class fetches Stylitics Data from the server using Data SDK. It has below methods implemented.
     1. **`outfits(itemNumber: String = "1234567890")`** This makes the Outfits API call using item number.
     2. **`success(outfits: Outfits)`** This saves the success response for the Outfits API call.
     3. **`dynamicGalleryData()`** This makes the Dynamic Gallery API call using a map of parameters.
     4. **`success(bundles: GalleryBundles)`** This saves the success response for the Dynamic Gallery API call.

* **ProductDetailPageFragment.kt** Based on template selection in settings screen This class is responsible for displaying the product details and its related Outfits/Dynamic Gallery using sample methods defined in the template based classes.
     1. **`displayOutfits()`** This makes call to the sample code to display required Views.
 
* **ProductDetailPageViewModel.kt** This class handles business logic for Product Detail screen.
     1. **`trackJumplinkClickedEvent()`** Invokes the `jumplinkclicked` tracking event.

* **fragment_product_detail_page.xml** This is the XML file which holds the UI components to be displayed on the screen.

### Show Product Items for Outfit Items

* **OutfitItemsFragment** This is the fragment to display Product List and implements the click listener. This implementation is invoked when Product List screen from UX SDK is disabled.
  1. **`displayOutfitItems()`** This method loads the Product List view implemented in the UX SDK.

* **ItemFragmentViewModel** This class handles business logic for Product List screen.

### Show Product Items for Dynamic Gallery Bundle Items

* **DynamicGalleryProductListFragment.kt** This is the fragment to display Dynamic Gallery Product List and implements the click listener. This implementation is invoked when Dynamic Gallery Product List screen from UX SDK is disabled.
  1. **`displayGalleryBundleItems(containerView: View)`** This method loads the Dynamic Gallery Product List view implemented in the UX SDK.

* **DynamicProductListFragmentViewModel.kt** This class handles business logic for Dynamic Gallery Product List screen

## Cart Screen

Below are the implementation details for Cart page.

* **CartViewModel.kt** This class Handles the business logic for Cart screen.
  1. **`fun cartItems()`** Retrieves the list of cart items from the local storage.
  2. **`fun trackPurchases()`** Invokes the 'purchase' tracking event.

* **CartActivity.kt** This class is responsible for displaying the item added by user to the Cart.
  1. **`fun updateViews()`** This has implementation to update the views and based on user input it initiates the purchase operations.
  2. **`fun updateScreenViews()`** This manages the Empty Cart view's visibility.


## Classic Widget Configuration Samples

* **ClassicWidgetConfigSamples.kt** class has code samples for various custom configurations for Classic template. 

## Hotspot Widget Configuration Samples

* **HotspotWidgetConfigSamples.kt** class has code samples for various custom configurations for Hotspot template.

## Standard Product List View Configuration Samples

* **ProductListViewConfigSamples.kt** class has code samples for Product list view when called from Integrator App.

## Dynamic Gallery Widget Configuration Samples

* **DynamicGalleryWidgetConfigSamples.kt** class has code samples for various custom configurations for Dynamic Gallery Widget.

## Dynamic Gallery Product List View Configuration Samples

* **GalleryProductListViewConfigSamples.kt** class has code samples for various custom configurations for Dynamic Gallery Product List view.

## Classic - Shop The Model

Integrator app should add image for Shop The Model view in *_**Android resource drawable folder**_* as shown in below screenshot.

</br>![Image1](Screenshots/shop_the_model_image_location.png)

Shop The Model configuration for Classic widget can be done as below,

```kotlin
  shopTheModel = ShopTheModel(
    name = R.drawable.shop_the_look,
    position = ShopTheModelPosition.BOTTOM_LEFT,
    width = 80f,
    height = 80f
  )
```

**ClassicWidgetConfigSamples.kt** class has an example for Shop The Model configuration.

## Settings Screen

* **SettingsActivity.kt**  This class is responsible for showing application settings.

## Gallery Screen

Below are the implementation details for Gallery screen.

* **GalleryActivity.kt** This class is responsible for displaying the gallery items by fetching required data from the *_**GalleryViewModel.kt**_*. 

* **GalleryViewModel.kt** This class handles business logic for Gallery screen.

* **ConfigStyles** This class has sample style configurations for Outfit widget and Product list Templates.
    1. **`Style`**  - is the enum for set of styles
    2. **`fun getStyle(style: Style = Style.DEFAULT_STYLE): OutfitsTemplate`** This method returns the OutfitTemplate configs for the provided style.
    3. **`fun getStyle(style: Style = Style.DEFAULT_STYLE): ProductListTemplate`** This method returns the ProductListTemplate configs for the provided style. 

* **activity_gallery** This is the XML file which holds the UI components to be displayed on the Gallery screen.

## License

Copyright © 2023 Stylitics

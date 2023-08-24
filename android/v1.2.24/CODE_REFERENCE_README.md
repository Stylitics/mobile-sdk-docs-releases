# Stylitics Sample Integrator App Code Reference 

This document provides references for implementation details by Sample Integrator App screen.

## Configure Username and Environment

Below are the implementation details for Username and Environment configurations screen

* *_**UserAccountsActivity.kt**_* This activity has all the implementations to display the available usernames and based on the user selection it proceeds to the user, client and environment configuration.
* *_**UserAccountsViewModel.kt**_* This class handles business logic behind first screen using below methods.
     1. *_**configureSDK()**_* - This performs the required data configurations



## Show Product Items(Grid Screen)

Below are the implementation details for Show Product Items(Grid screen) screen

* *_**ItemsActivity.kt**_* This class is responsible for displaying the Sample Product items.

## PDP Screen

Below are the implementation details for Product details page.

* *_**OutfitsViewModel.kt**_* This class fetches Stylitics Outfits data from the server using Data SDK. It has below methods implemented.
     1. *_**outfits(itemNumber: String = "1234567890")**_* This makes the Outfits API call using item number.
     2. *_**success(outfits: Outfits)**_* This saves the success response for the Outfits API call.

* *_**ProductDetailPageActivity.kt**_* This class is responsible for displaying the product details and its related Outfits using below method.
     1. *_**outfitsWithDefaultProductList(outfits:Outfits)**_* This method load the Classic Outfits Widget and enable the Product List UI implemented in the UX SDK.
     2. *_**outfitsWithProductListView(outfits:Outfits)**_* This is to load the  Classic Outfits Widget. Here Integrator app has to implement their own Product list screen.
     3. *_**showOutfitItems(outfit: Outfit)**_* This invokes the product list screen implemented by the Sample Integrator App.

* *_**ProductDetailPageViewModel.kt**_* This class handles business logic for Product Detail screen.
     1. *_**trackJumplinkClickedEvent()**_* Invokes the `jumplinkclicked` tracking event.

* *_**activity_product_detail_page.xml**_* This is the XML file which holds the UI components to be displayed on the screen.

## Show Product Items

* *_**OutfitItemsFragment**_* This is the bottomSheet fragment to display product list and implements the click listener. This implementation is invoked when product list screen from UX SDK is disabled.
     1. *_**updateViews()**_* This method loads the product list view implemented in the UX SDK. 

* *_**ItemFragmentViewModel**_* This class handles business logic for Product Detail screen

## Shop The Model

Integrator app should add image for Shop The Model view in *_**Android resource drawable folder**_* as shown in below screenshot.

</br>![Image1](Screenshots/shop_the_model_image_location.png)

Shop The Model configuration can be done as below,

```kotlin
  shopTheModel = ShopTheModel(
    name = R.drawable.shop_the_look,
    position = ShopTheModelPosition.BOTTOM_LEFT,
    width = 80f,
    height = 80f
  )
```

*_**ProductDetailsPageActivity.kt**_* class has an example for Shop The Model configuration.


### Classic Widget Configuration Samples

*_**ProductDetailsPageActivity.kt**_* class has UI configuration examples for some and all custom configurations of Classic views.


### Settings Screen

* *_**SettingsActivity**_*  This class is responsible for showing application settings.

## License

Copyright © 2023 Stylitics

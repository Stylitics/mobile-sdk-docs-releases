# Stylitics Sample App Code Reference 


### Configure Username and Environment

- *_**BuildViewController.swift**_* - This controller is used to display Configure Username and Environment screen.
- *_**BuildViewModel.swift**_* - This class handles the business logic.
  1. *_**configureApisHostEnvironment()**_* - This method is used to Configure the Data SDK.


### Show Product Items(Grid Screen)

- *_**ItemsViewController.swift**_* - This controller is used to display the Sample Product items.


### PDP Screen

- *_**ProductDetailsViewModel.swift**_* - This class fetches Stylitics Outfits data from the server using Data SDK. 
     1. *_**fetchOutfits(itemNumber: String, isSuccess: @escaping (Bool)()**_* - This makes the Outfits API call using item number.
     2. *_**trackJumplinkClickedEvent()**_* - Invokes the `jumplinkclicked` tracking event when user taps the "See How To Wear It" button.
     
- *_**OutfitsTableViewCell.swift**_* - This table view cell is used to display the Outfits fetched using Data SDK.
     1. *_**getClassicDisplayWithProductListScreen(outfits: Outfits)**_* - This method loads the Classic Outfits Widget with the option to display Product List Screen from UX SDK. 
     2. *_**getClassicDisplayWithoutProductListScreen(outfits: Outfits)**_* - This is to load the Classic Outfits Widget. Here Integrator app has to implement their own Product list screen.

## Cart Screen

Below are the implementation details for Cart page.

* **CartViewModel.swift** This class Handles the business logic for Cart screen.

* **CartItemTableViewCell.swift** This class is responsible for displaying the item added by user to the Cart.
  
### Product List Screen

- *_**DetailsOverlayViewController.swift**_* - This controller is used to display Outfit Items for the selected Outfit. It is used only when the option to display Product List Screen from UX SDK is disabled.

## Shop The Model

Integrator app should add image for Shop The Model view in *_**Assets**_* as shown in below screenshot.

</br>![Image1](Screenshots/shop_the_model_image_location.png)

Shop The Model configuration can be done as below,

```swift
  ShopTheModel(name: "shopTheLook",
               position: .bottomLeft,
               width: 60,
               height: 60)
```

### Classic Widget Configuration Samples

*_**ClassicWidgetConfigurationSamples.swift**_* class has UI configuration examples for some and all custom configurations of Classic views.

### Hotspot Widget Configuration Samples

*_**HotspotWidgetConfigurationSamples.swift**_* class has UI configuration examples for some and all custom configurations of Hotspot views.

### Standard Product List Configuration Samples

*_**StandardProductListConfigurationSamples.swift**_* class has UI configuration examples for some and all custom configurations of Standard Product List views.


## License

Copyright © 2023 Stylitics

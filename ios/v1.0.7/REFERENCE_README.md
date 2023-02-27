# Stylitics Sample App Code Reference 

### 1. Configure Username and Environment

* *_**BuildViewController.swift**_* This controller is used to display Configure Username and Environment screen.
* *_**BuildViewModel.swift**_* This class handles the business logic.
  1. *_**configureApisHostEnvironment()**_* - This method is used to Configure the Data SDK.

![Image1](Screenshots/user_name_environment_configuration.png)


### 2. Show Product Items(Grid Screen)

* *_**ItemsViewController.swift**_* This controller is used to display the Sample Product items.

![Image1](Screenshots/product_list_screen.png)

### 3. PDP Screen

* *_**ProductDetailsViewModel.swift**_* This class fetches Stylitics Outfits data from the server using Data SDK. 
     1. *_**fetchOutfits(itemNumber: String, isSuccess: @escaping (Bool)()**_* This makes the Outfits API call using item number.
     2. *_**trackJumplinkClickedEvent()**_* Invokes the 'jumplinkclicked' tracking event when user taps the "See How To Wear It" button.
     
* *_**OutfitsTableViewCell.swift**_* This table view cell is used to display the Outfits fetched using Data SDK.
     1. *_**getClassicDisplayWithProductListScreen(outfits: Outfits)**_* This method loads the Classic Outfits Widget with the option to display Product List Screen from UX SDK. 
     2. *_**getClassicDisplayWithoutProductListScreen(outfits: Outfits)**_* This is to load the Classic Outfits Widget. Here Integrator app has to implement their own Product list screen.

![Image1](Screenshots/see_how_to_wear_it.png)

### 4. Product List Screen

* *_**DetailsOverlayViewController.swift**_* This controller is used to display Outfit Items for the selected Outfit. It is used only when the option to display Product List Screen from UX SDK is disabled.

![Image1](Screenshots/product_items_list_screen.png)

## License

copyright © 2023 Stylitics
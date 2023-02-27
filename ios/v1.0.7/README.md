# Stylitics Sample/Integrator App

This is a Sample/Integrator application to showcase the integration of Stylitics Data & UX SDK.
* Data SDK is used by Sample/Integrator app to use Stylitics services. Currently the Data SDK provides services to fetch Outfits, replacements and send tracking events.
* UX SDK provides views to display Stylitics data. For example, Classic Outfit widget can be used to display Outfits Data. These views automatically sends the required tracking events to Stylitics server which reduces efforts for the Integrator App.

## Features

Sample app supports below screens to showcase features of Stylitics Data and UX SDK.

* *_**Configure Username and Environment**_* - This screen lets the user select and configure the username and Environment. When user hits the *_**Build Store**_* button, Data SDK Config Api is invoked to configure some required and optional configurations for the Data SDK</br>.
  

* *_**Show Product Items(Grid Screen)**_* - This shows the grid of some sample data from Stylitics server that we use to showcase Data and UX SDK features.
  When user selects one of the sample product, they are navigated to the PDP screen.</br>
  

* *_**PDP Screen**_* - This is Product details page which showcases some of the features like "See how to wear it" and related Outfits.
   * When user enters this screen, it fetches Outfits from the Data SDK.
   * It sends the Outfit response to the UX SDK to display Outfits data in Outfits views.</br>
  

* *_**See How to Wear It**_* - * When user clicks on the "See how to wear it" button in the Product details page, 
   * It automatically scrolls down to the Outfits view created using UX SDK.
   * Integrator/Sample App should invoke 'jumplinkClicked' event using engagements API in Data SDK.
   * When Outfits comes into the view port, UX SDK sends the Outfit 'view' event for the visible Outfits using engagement API in Data SDK.
   * When user selects one of the Outfit, they are navigated to the Product List screen</br>
   *_**Note**_* : If there are no outfits for an item, See How to wear it button will be hidden.
  

* *_**Show Product Items**_* - This screen shows the list of all Outfit items belonging to the selected Outfit from Product details page.</br>
  * When user enters this screen, it sends item 'view' tracking events for the visible items using Data SDK's engagements API.
  * When user taps on any item in the list- 
    1. It sends 'click tracking event' using Data SDK's engagements API.
    2. If item click listener exposed by UX SDK is implemented in Integrator/Sample App, it will perform implemented action.
    3. If item click listener exposed by UX SDK is not implemented in Integrator/Sample App, it redirects to the web view.</br>


* *_**Tracking Events**_* - This is to track the user interaction with the App by calling the engagements API implemented in Data SDK.
    1. Invoked from Data SDK
        1. Outfit load event - When Data SDK receives the Outfit API response from the Stylitics server, Data SDK invokes this event.
    2. Invoked from UX SDK
        1. Outfit view event - When user enters the PDP page, UX SDK invokes this tracking event for all Outfits viewed by the user. This tracking event is sent once per Outfit page session.
        2. Outfit item view event - UX SDK sends this event for all items in the Product List which are viewed by the user.
        3. Outfit click event - When user clicks on any Outfit from Classic Outfit widget, UX SDK invokes this event.
        4. Outfit item click event - When user clicks on any Outfit Item from Product List view, UX SDK invokes this event.
        5. Outfit item swap event - UX SDK sends this tracking event when swap happens in Product List screen or Outfit Collage.
    3. Invoked from Sample/Integrator app
        1. Jumplink Clicked event - This event is sent when user clicks on "See How To Wear It" button in the PDP page. 

*_**Note**_* : [Click here](REFERENCE_README.md) to refer files for the Data and UX SDK.

## Technologies Used

Below technologies are used for implementing the Sample/Integrator App.

* Xcode as IDE(**v** 13.3.1)
* Swift as programming language(**v** 5)
* Storyboards for UI implementation
* SwiftLint as quality assurance tool(**v** 0.46.4)

## SDK version details

* Data SDK version used in the Sample App : *_**V1.0.5**_*
* Data SDK version used in the UX SDK : *_**V1.0.5**_*
* UX SDK version used in the Sample app : *_**V1.0.5**_*

## Steps to Implement Integrator/Sample App using UX SDK
Below are the steps to integrate and use Stylitics UX SDK in Integrator/Sample app

### 1. Import Data SDK to access its features
Below are the steps that needs to be followed to import the Stylitics Data SDK in the Integrator App.

    Step 1: Download Stylitics Data SDK.

    Step 2: Import .xcframework file to the Integrator App
         i. In Integrator project, select project file.
         ii. Select project file from in target section and select build phase.
         iii. Select Embed framework and add framework file.
         iv. Build your project. After successful build you will be able to access SDK implementations.

</br>[Click here to read about Data SDK features](DATASDK_README.md)

### 2. Import UX SDK to access its features
Below are the steps that needs to be followed to import the Stylitics UX SDK in the Integrator App.

    Step 1: Download Stylitics UX SDK.

    Step 2: Import .xcframework file to the Integrator App
         i. In Integrator project, select project file.
         ii. Select project file from in target section and select build phase.
         iii. Select Embed framework and add framework file.
         iv. Build your project. After successful build you will be able to access SDK implementations.

</br>[Click here to read about UX SDK features](UXSDK_ReadMe.md#features)


### 3. Configure Data SDK
There are some configurations which are required to access Data SDK features. [Click here](DataSDK_ReadMe.md#sdk-configuration) to know about the SDK configuration details.

### 4. Fetch Stylitics Data
Fetch Outfits data using Outfits API provided by the Data SDK. [Here](DataSDK_ReadMe.md#Example-to-fetch-outfits-using-item-number) are the code examples for fetching the Outfits.

### 5. Display Stylitics Data
Use UX SDK views to display the Stylitics data provided by the Data SDK. [Click here](UXSDK_README.md) to know more about the UX SDK views and their configurations.

### 6. Implement Exposed Listeners
Outfit and ProductList screen exposes below list of listeners to the Sample/Integrator app.

* For Outfit :
  1. onClick - On click event of Outfit, this listener will be triggered.
  2. onView - On view event of Outfit, this listener will be triggered.
  3. onItemSwap - On swap event of item in Outfit collage, this listener will be triggered.

* For Product List :
  1. onOutfitItemClick - On click event of Outfit Item, this listener will be triggered. It is highly recommended that Integrator app should implement this listener.
  2. onOutfitItemView - On view event of Outfit Item, this listener will be triggered.
  3. onItemSwap - On swap event of item in Product List screen, this listener will be triggered.

### 7. Purchase Events
This event should be triggered by Integrator/Sample app when user purchases any item provided by Stylitics data. It will help Stylitics server to provide better recommendations for the user. [Click here to read about Purchase Event implementation](DataSDK_ReadMe.md#api-call-to-track-purchase-event).

### 8 Mix And Match
This feature enables user to view replacements for any outfit item.

* Enable Mix and Match: 
    [Click here](DataSDK_ReadMe.md#mix-and-match) to enable disable mix and match feature.

* Configure Mix and Match:
    [Click here](UXSDK_ReadMe.md#mix-and-match-mnm) to configure Mix and Match UI feature.

* *_**See more options**_* - This option is available when Mix and Match feature is enabled by the Integrator app.  
   * When user clicks on this button, UX SDK shows a list of all replacement items received from the Data SDK belonging to the selected item.</br>
  </br>

* *_**Swap item in Outfit collage**_* - When user clicks on any item in Outfit collage, it will be replaced with the next item from its available replacements. For swapping it iterates through the replacement item list. When it reaches to the last item, it starts from the first item(default item) in the list</br>.
  

* *_**Swap item in Product List**_* - When user clicks on any item in the replacements row, the currently displayed item will be replaced with the selected item.</br>
  </br>

## iOS Versioning Support

- Minimum required iOS APP version to access features of SDK is - (iOS 13.0)

## License

copyright © 2023 Stylitics

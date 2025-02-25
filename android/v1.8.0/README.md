# Stylitics Sample Integrator App

This is a Sample Integrator application to showcase the integration of Stylitics Data & UX SDK.
- The Data SDK is used by Sample Integrator app to use Stylitics services. Currently the Data SDK provides services to fetch Stylitics Data and send tracking events.
- The UX SDK provides views to display Stylitics data. For example, Classic Outfit widget can be used to display Outfits Data. These views automatically sends the required tracking events to Stylitics server which reduces efforts for the Integrator App.
- This file is an overview of the Sample Integrator app, with a brief description of its screens and how it uses Stylitics SDKs; more detail can be found in the [Data SDK README](DATA_SDK_README.md) and the [UX SDK README](UX_SDK_README.md).

## Dependency Manager

### Steps To install Dependencies:
1. Add it in your root build.gradle at the end of repositories
```gradle
  repositories {
	maven { url 'https://jitpack.io' }
  }
```

2. Add below mentioned dependencies in build.gradle

```gradle
     api('com.github.Stylitics:android-data-sdk-artifacts:1.8.0@aar') { transitive = true }
     api('com.github.Stylitics:android-ux-sdk-artifacts:1.8.0@aar') { transitive = true }
```

3. After adding dependency sync your project. Once project is sync successfully you are ready to use the framework.

### Steps to update any particular version of framework
* Update version in build.gradle file like below:

```gradle
     api('com.github.Stylitics:android-data-sdk-artifacts:1.8.0@aar') { transitive = true }
     api('com.github.Stylitics:android-ux-sdk-artifacts:1.8.0@aar') { transitive = true }
```
* After updating dependency version sync your project.

## Screens and Features
The Sample Integrator app supports the below screens to showcase features of Stylitics Data and UX SDKs.  

### Configure Username and Environment
This initial screen lets the user select and configure a Stylitics account username and Environment. When the user taps the *Build Store* button, the Data SDK's Config API is invoked to configure some required and optional configurations for the Data SDK.

### Sample Integrator Product Grid
When the "store" is ready, the user is shown two tabs. Both tabs shows a grid of products that have outfit coverage (per the API server of the selected environment). Tapping on one brings the user to a sample PDP screen.

Tapping on -
* The gallery icon, the user will navigate to the Gallery screen
* The settings icon, the user will navigate to the Settings screen.
* The cart icon, the user will navigate to the Cart screen.
* The tab icon, the user can switch to another tab.

</br>![Image1](Screenshots/item_grid_screen.png)

### Gallery

In this screen, we show multiple carousels for the outfits fetched from Stylitics server using different filters.

</br>![Image1](Screenshots/gallery_screen.png)

### PDP Screen 
On this screen, basic product details are shown, in addition to a recommended client-implemented feature (jumplink / "See How to Wear It"), and the app uses the Data SDK to fetch Stylitics Data. It sends the Api response data to the UX SDK to display in the Widget.

![Image1](Screenshots/product_details_screen.png)

### Jumplink - See How to Wear It
When user clicks on the "See How to Wear it" button in the Product details page, 
   * It automatically scrolls down to the Outfits Widget created using UX SDK.
   * Sample Integrator App should invoke 'jumplinkClicked' event using engagements API in Data SDK.
   * When Outfits comes into the view port, UX SDK sends the Outfit 'view' event for the visible Outfits using the tracking API in the Data SDK.


*Note : If there are no outfits for an item, the **See how to wear it** button will be hidden.*

*Note: this is a feature that Stylitics recommend the integrator app put in place; it is not a component provided by the Stylitics UX SDK, currently.*

### Add To Cart
When user taps on Add to Cart CTA in the PDP page, 
* The Item will be added to the Cart.
* Sample Integrator app sends the [`add-to-cart`](DATA_SDK_README.md#Item-add-to-cart-event) tracking event using Data SDK's engagements API.

### Cart Screen
This screen shows the list of product items added by user and they can purchase all the product items from the Cart.
When user taps on `PURCHASE` button, Sample Integrator app sends the [`Purchase`](DATA_SDK_README.md#Purchase-event) tracking event using Data SDK's engagements API.

![Image1](Screenshots/cart_screen.png)

### Show Product Items
This screen shows the list of all Outfit items belonging to the selected Outfit from Product details page. When the user enters this screen, the UX SDK sends item 'view' tracking events for the visible items using the Data SDK's engagements API.

When user taps on any item in the list:

- The UX SDK sends the `outfitItemClicked` tracking event using Data SDK's engagements API.
- The UX SDK calls a corresponding listener function, which can be provided / attached by the integrator. All key UX events fire a similar callback/listener function. Many are useful for integrator-specific analytics attachments, but this one is particularly important because Stylitics strongly recommends that the client/integrator implement and attach this callback in order to -- in addition to any integrator analytics -- natively navigate the user to the selected item's PDP (or launch a quick shop experience).
- If the item click listener is not implemented by Sample Integrator App, it redirects to the web view when an item is clicked.
  
  ![Image1](Screenshots/product_item_list_view.png)

### Settings Screen

1. Enable/Disable Dark Mode

   Users can switch application's appearance using this option, by default it reflects the device's appearance setting

2. Enable/Disable Custom Configuration

   With this option, users can switch to the custom configuration for UX SDK UI components. By default, it is disabled.

3. Do Not Track

   With this option, users can enable or disable the tracking feature. By default, tracking is enabled. 

4. Change Locale
   
   This shows the current locale selection in the app and also provides an option to change the locale in global configuration.  

5. Widget Height

   With this option, users can change the Outfit widget view height in Sample/Integrator App.

6. Profile Id
    
   It provides an option to change Customer Profile Id in global configuration. It also shows the current value for Customer Profile Id. 

7. Widget Card Peek

   It provides an option to change Card peek configuration for Classic Outfit Widget. It also shows the current value for Card Peek.

8. Widget Card Gutter

   It provides an option to change Card gutter configuration for Classic Outfit Widget. It also shows the current value for Card Gutter.

10. Api Request Params
  
    It provides an option to change Api Request Params. It also shows the current values for these params.  

11. Select Widget

    With this option, users can change the template for UX SDK UI components used to display Stylitics Data. By default, it is set to Classic.

12. Session-ID

    This is a UUID generated in DataSDK, user can view and copy this Id to clipboard.

    ![Image1](Screenshots/settings_screen.png)

## Tracking Events
This is to track the user's interaction with the App by calling the engagement API implemented in Data SDK.

### Invoked from UX SDK

Widget related `view` and `click` tracking events are invoked and handled by the UX SDK. [Click here](DATA_SDK_README.md#Tracking-API-Examples) to read more about all tracking events.

### Invoked from Sample Integrator app
- Jumplink Clicked event - This event is invoked when user clicks on "See How To Wear It" button on the PDP page. 
- Purchase events (See Data SDK README)

---

[Click here](CODE_REFERENCE_README.md) for reference to files in the Sample Integrator Application that uses Data and UX SDK features.

---

## Steps to Implement Sample Integrator App using UX SDK
Below are the steps to integrate and use Stylitics UX SDK in Sample Integrator App.

### Import Data SDK 

Below are the steps that need to be followed to import the Stylitics Data SDK into the Sample Integrator App.

1. Download Stylitics Data SDK provided by Stylitics.
2. In Sample Integrator App project, create new directory in project level named libs.
3. Copy and paste downloaded .AAR file here to the libs folder.
4. In build.gradle file of module, add `implementation files('libs/styliticsdatarelease.aar')`
5. Sync your project. After successful sync you will be able to access SDK implementations.

[Click here to read about Data SDK features](DATA_SDK_README.md)

###  Import UX SDK 

Steps to import the Stylitics UX SDK into the Sample Integrator App:

 1. Download Stylitics UX SDK provided by Stylitics.
 2. In Sample Integrator App project, copy and paste downloaded .AAR file to the libs folder.
 3. In build.gradle file of module, add: `implementation files('libs/styliticsuirelease.aar')`
 4. Sync your project. After successful sync you will be able to access UX SDK implementations.

[Click here to read about UX SDK features](UX_SDK_README.md)



### Set Required Permissions
This App needs Internet access, so below given permission is added to the *_**AndroidManifest.xml**_*.

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```



### Application Context

*_**Application.kt**_* is the file from where the globally accessible application level context is created, so that it can be referred to inside of the data SDK.

This Application class needs to be registered in *_**AndroidManifest.xml**_*, to initiate this class before any other class when the process for your application/package is created. This registration should be done in the manifest file as below.

*_**XML**_*

 ```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" 
package="com.stylitics.styliticssampleapp">

        <uses-permission android:name="android.permission.INTERNET" />

        <application android:name=".MyApplication">
        </application>
</manifest>
```

### Configure Data SDK
There are some configurations which are required to access Data SDK features. [Click here](DATA_SDK_README.md#SDK-Configuration) to know about the SDK configuration details.

### Fetch Stylitics Data
Fetch Stylitics data using Data APIs provided by the Data SDK. [Here](DATA_SDK_README.md#Stylitics-Data-API-Call-Examples) are the code examples for fetching the Stylitics Data.

### Display Stylitics Data
Use UX SDK views to display the Stylitics data provided by the Data SDK. [Click here](UX_SDK_README.md) to learn more about the UX SDK views and their configurations.

### Implement Exposed Listeners
Click on the widget template below to learn more about the list of listeners exposed to the Sample Integrator app.

1. [Classic Outfit widget](CLASSIC_WIDGET_README.md#Implement-Exposed-Listeners)
2. [Hotspot Outfit widget](HOTSPOT_WIDGET_README.md#Implement-Exposed-Listeners)
3. [Grid_Outfit_Widget](GRID_WIDGET_README.md#Implement-Exposed-Listeners)
4. [Standard Product list view](STANDARD_PRODUCTLIST_README.md#Implement-Exposed-Listeners)
5. [Dynamic Gallery widget](DYNAMIC_GALLERY_WIDGET_README.md#Implement-Exposed-Listeners)
6. [Outfit Bundle Product list view](OUTFIT_BUNDLE_PRODUCTLIST_README.md#Implement-Exposed-Listeners)
7. [Shop the Set widget](SHOP_THE_SET_WIDGET_README.md#Implement-Exposed-Listeners)
8. [Styled For You widget](STYLED_FOR_YOU_WIDGET_README.md#Implement-Exposed-Listeners)
9. [Outtfit Landing Page](OUTFIT_LANDING_PAGE_WIDGET_README.md#Implement-Exposed-Listeners)
10. [Styled For You Full Page](STYLED_FOR_YOU_FULL_PAGE_WIDGET_README.md#STYLED-FOR-YOU-FULL-PAGE-WIDGET)
11. [Trending Bundles](TRENDING_BUNDLES_WIDGET_README.md#Implement-Exposed-Listeners)
12. [Upsell Items](UPSELL_ITEMS_WIDGET_README.md#Implement-Exposed-Listeners)
13. [Variant Widget](VARIANT_WIDGET_README.md#Implement-Exposed-Listeners)

###  Purchase Events
This event should be triggered by Sample Integrator app when user purchases any item provided by Stylitics data. It will help the Stylitics server provide better recommendations for the user. [Click here](DATA_SDK_README.md#Purchase-event) to read about Purchase Event implementation.

## Mix And Match
When this feature is enabled, it enables the user to view replacements for the Outfit items.

* *_**Enable Mix and Match**_* -
  [Click here](DATA_SDK_README.md#Mix-and-Match) to learn about enabling/disabling the mix and match feature.

* *_**Configure Mix and Match**_* - 
  [Click here](MIX_AND_MATCH_SCREEN_README.md#Configurations) to configure Mix and Match UI feature.

* *_**Swap CTA**_* - When user clicks on this button in Product List screen, UX SDK Navigates to Mix and Match screen and shows a list of all replacement items received from the Data SDK belonging to the selected item.

* *_**Mix and Match**_* - When user clicks on any item in the Mix and Match screen, the currently displayed item Product list screen will be replaced with the clicked item in Mix and Match screen.
  [Here](MIX_AND_MATCH_SCREEN_README.md#MIX-AND-MATCH-SCREEN) are more details and a code example to enable the item swapping feature in Product list screen.

## Memory Management

It is advised to clear data for a view whenever it's getting removed from screen.
* To clear data manually call below method:

```kotlin
  StyliticsUIApi.clearData(requestId)
```

* To clear data when Activity/Fragment is destroyed, override the onCleared() method of view model and add code below:

```kotlin
override fun onCleared() {
  super.onCleared()
  outfitsData?.list?.firstOrNull()?.let {
    StyliticsUIApi.clearData(it.requestId)
  }
}
```
* *_**Note**_*
  1. `requestId` is present in outfit and outfitItem model.
  2. If memory is released when Outfit template view(ex. Classic Widget) is in use it could lead to crash.

## Light And Dark Mode support

The UX SDK provides the default Light and Dark mode support and it is customizable from the Sample Integrator App. [Click here ](DARK_AND_LIGHT_MODE_README.md) to learn more about it.

## Technologies Used

Below technologies are used for implementing the Sample Integrator App.

- Android Studio as IDE(**v** Chipmunk | 2021.2.1 Patch 1)
- Kotlin as programming language(**v** 1.6.21)
- XML for UI implementation(**v** 1.0)

## Android Versioning Support

*_**build.gradle(:app)**_* is the file where App version and device versioning support is defined.

- Minimum required Android API level to access features of this App is - 21(Android 5.0).

- Target Android API level is 32(Android 13/ Beta).

## License

Copyright © 2023 Stylitics

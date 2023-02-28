# Stylitics Sample Integrator App

This is a Sample/Integrator application to showcase the integration of Stylitics Data & UX SDK.
- The Data SDK is used by Sample/Integrator app to use Stylitics services. Currently the Data SDK provides services to fetch Outfits, replacements and send tracking events.
- The UX SDK provides views to display Stylitics data. For example, Classic Outfit widget can be used to display Outfits Data. These views automatically sends the required tracking events to Stylitics server which reduces efforts for the Integrator App.
- This file is an overview of the sample app, with a brief description of its screens and how it uses Stylitics SDKs; more detail can be found in the Data SDK README and the UX SDK README.

## Screens and Features

Sample app supports the below screens to showcase features of Stylitics Data and UX SDKs.  

### Configure Username and Environment
This screen lets the user select and configure the username and Environment. When user taps the *Build Store* button, the Data SDK's Config API is invoked to configure some required and optional configurations for the Data SDK.

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/env-setup.png)

### Sample Product Grid
When the "store" is ready, the user is shown a grid of products which have outfit coverage.  Tapping on one brings the user to a sample PDP screen.

### PDP Screen 
On this screen, basic product details are shown, in addition to a recommended client-implemented feature (jumplink / "See How to Wear It"), and the app uses the Data SDK to fetch Outfits. It sends the Outfit response data to the UX SDK to display in the Outfits Widget.

![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/pdp.png)

### See How to Wear It
When user clicks on the "See Wow to Wear it" button in the Product details page, 
   * It automatically scrolls down to the Outfits Widget created using UX SDK.
   * Integrator/Sample App should invoke 'jumplinkClicked' event using engagements API in Data SDK.
   * When Outfits comes into the view port, UX SDK sends the Outfit 'view' event for the visible Outfits using engagement API in Data SDK.
   * When user selects one of the Outfit, they are navigated to the Product List screen. 

*Note : If there are no outfits for an item, the **See how to wear it** button will be hidden.*

*Note: this is a feature that Stylitics recommend the integrator app put in place; it is not a component provided by the Stylitics UX SDK, currently.*

### Show Product Items
This screen shows the list of all Outfit items belonging to the selected Outfit from Product details page.When user enters this screen, the UX SDK sends item 'view' tracking event for the visible item using Data SDK's engagements API.

When user taps on any item in the list:

- The UX SDK sends the `outfitItemClicked` tracking event using Data SDK's engagements API.
- The UX SDK calls a corresponding listener function, which can be provided / attached by the integrator. All key UX events fire a similar callback/listener function. Many are useful for integrator-specific analytics attachments, but this one is particularly important because Stylitics strongly recommends that the client/integrator implement and attach this callback in order to -- in addition to any integrator analytics -- natively navigate the user to the selected item's PDP (or launch a quick shop experience).
- If the item click listener is not implemented by Integrator/Sample App, it redirects to the web view when an item is clicked.
  
  ![Image1](https://storage.googleapis.com/stylitics-misc-public/sdk-images/android/sdk-product-list.png)

## Tracking Events
This is to track the user's interaction with the App by calling the engagement API implemented in Data SDK.

### Invoked from Data SDK

- **Outfit load event** - When Data SDK receives the Outfit API response from the Stylitics server, it invokes this event.

### Invoked from UX SDK
- **Outfit view event** - When user enters the PDP page, UX SDK invokes this tracking event for all Outfits viewed by the user. This tracking event is sent once per Outfit page session.
- **Outfit item view event** - UX SDK sends this event for all items in the Product list that are viewed by the user.
- **Outfit click event** - When user clicks on any Outfit in the Classic Outfit Widget, UX SDK invokes this event.
- **Outfit item click event** - When user clicks on any Outfit item in the Product list view, UX SDK invokes this event.
- **Outfit item swap event** - UX SDK sends this tracking event when a swap happens in Product List screen or Outfit collage.


### Invoked from Sample/Integrator app
- Jumplink Clicked event - This event is invoked when user clicks on "See How To Wear It" button on the PDP page. 
- Purchase events (See Data SDK README)

---

[Click here](Code_Reference.md) for reference to files in the Data and UX SDK.

---

## Steps to Implement Integrator/Sample App using UX SDK
Below are the steps to integrate and use Stylitics UX SDK in Integrator/Sample App.

### Import Data SDK 

Below are the steps that need to be followed to import the Stylitics Data SDK into the Integrator/Sample App.

1. Download Stylitics Data SDK provided by Stylitics.
2. Import .AAR file to the Integrator App
3. In Integrator/Sample App project, create new directory in project level named libs.
4. Copy and paste downloaded .AAR file here to the libs folder.
5. In build.gradle file of module, add `implementation files('libs/styliticsdatarelease.aar')`
6. Sync your project. After successful sync you will be able to access SDK implementations.

[Click here to read about Data SDK features](DATA_SDK_README.md)

###  Import UX SDK 

Steps to import the Stylitics UX SDK into the Integrator/Sample App:

 1. Download Stylitics UX SDK provided by Stylitics.
 2. Import .AAR file to the Integrator App
 3. In Integrator/Sample App project, copy and paste downloaded .AAR file to the libs folder.
 4. In build.gradle file of module, add: `implementation files('libs/styliticsuirelease.aar')`
 5. Sync your project. After successful sync you will be able to access UX SDK implementations.

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

### Fetch Outfits
Fetch Outfits data using Outfits API provided by the Data SDK. [Here](DATA_SDK_README.md#Example-to-Fetch-Outfits-using-item-number) are the code examples for fetching the Outfits.

### Display Outfits
Use UX SDK views to display the Stylitics data provided by the Data SDK. [Click here](UX_SDK_README.md) to know more about the UX SDK views and their configurations.

###  Implement Exposed Listeners
Outfit and ProductList screen exposes below list of listeners to the Sample/Integrator app.

**For Outfit**:
  1. `onClick` - On click event of Outfit, this listener will be triggered.
  2. `onView` - On view event of Outfit, this listener will be `triggered`.
  3. `onItemSwap` - On swap event of item in Outfit collage, this listener will be triggered.

**For Product List**:
  1. `onOutfitItemClick` - On click event of Outfit Item, this listener will be triggered. It is highly recommended that Integrator app should implement this listener.
  2. `onOutfitItemView` - On view event of Outfit Item, this listener will be triggered.
  3. `onItemSwap` - On swap event of item in Product List screen, this listener will be triggered.

###  Purchase Events
This event should be triggered by Integrator/Sample app when user purchases any item provided by Stylitics data. It will help the Stylitics server provide better recommendations for the user. [Click here](DATA_SDK_README.md#API-Call-to-Track-Purchase-event) to read about Purchase Event implementation.

## Mix And Match
When this feature is enabled, it enables the user to view replacements for the Outfit items.

* *_**Enable Mix and Match**_* -
  [Click here](DATA_SDK_README.md#Mix-and-Match) to learn about enabling/disabling the mix and match feature.

* *_**Configure Mix and Match**_* - 
  [Click here](UX_SDK_README.md#Mix-and-Match-(MnM)) to configure Mix and Match UI feature.

* *_**See more options**_* -  When user clicks on this button, UX SDK shows a list of all replacement items received from the Data SDK belonging to the selected item.

* *_**Swap item in Outfit collage**_* - When user clicks on any item in Outfit collage, it will be replaced with the next item from its available replacements. For swapping it iterates through the replacement item list. When it reaches to the last item, it starts from the first item(default item) in the list. [Here](UX_SDK_README.md#1-Classic-Outfit-Widget-with-Mix-and-Match-enabled) are more details and a code example to enable item swapping feature with direct tap on an item in the Outfit collage.

* *_**Swap item in Product List**_* - When user clicks on any item in the replacements row, the currently displayed item will be replaced with the clicked item.
  [Here](UX_SDK_README.md#2-Product-List-Screen-with-Mix-and-Match) are more details and a code example to enable the item swapping feature in Product list screen.

## Technologies Used

Below technologies are used for implementing the Sample/Integrator App.

- Android Studio as IDE(**v** Chipmunk | 2021.2.1 Patch 1)
- Kotlin as programming language(**v** 1.6.21)
- XML for UI implementation(**v** 1.0)

## SDK version details

- Data SDK version used in the Sample App : *_**V1.0.11**_*
- Data SDK version used in the UX SDK : *_**V1.0.11**_*
- UX SDK version used in the Sample App : *_**V1.0.11**_*

## Android Versioning Support

*_**build.gradle(:app)**_* is the file where App version and device versioning support is defined.

- Minimum required Android API level to access features of this App is - 21(Android 5.0).

- Target Android API level is 32(Android 13/ Beta).

## License

copyright © 2023 Stylitics

# Stylitics Sample/Integrator App

This is a Sample/Integrator application to showcase the integration of Stylitics Data & UX SDK.
* Data SDK is used by Sample/Integrator app to use Stylitics services. Currently the Data SDK provides services to fetch Outfits, replacements and send tracking events.
* UX SDK provides views to display Stylitics data. For example, Classic Outfit widget can be used to display Outfits Data. These views automatically sends the required tracking events to Stylitics server which reduces efforts for the Integrator App.

## Features

Sample app supports below screens to showcase features of Stylitics Data and UX SDK.

* *_**Configure Username and Environment**_* - This screen lets the user select and configure the username and Environment. When user hits the *_**Build Store**_* button, Data SDK Config Api is invoked to configure some required and optional configurations for the Data SDK</br>.
  </br>

* *_**Show Product Items(Grid Screen)**_* - This shows the grid of some sample data from Stylitics server that we use to showcase Data and UX SDK features.
  When user selects one of the sample product, they are navigated to the PDP screen.</br>
  </br>

* *_**PDP Screen**_* - This is Product details page which showcases some of the features like "See how to wear it" and related Outfits.
   * When user enters this screen, it fetches Outfits from the Data SDK.
   * It sends the Outfit response to the UX SDK to display Outfits data in Outfits Widget.</br>
  </br>

* *_**See How to Wear It**_* - When user clicks on the "See how to wear it" button in the Product details page, 
   * It automatically scrolls down to the Outfits Widget created using UX SDK.
   * Integrator/Sample App should invoke 'jumplinkClicked' event using engagements API in Data SDK.
   * When Outfits comes into the view port, UX SDK sends the Outfit 'view' event for the visible Outfits using engagement API in Data SDK.
   * When user selects one of the Outfit, they are navigated to the Product List screen. </br>
*_**Note**_* : If there are no outfits for an item, *_**See how to wear it**_* button will be hidden.</br>
  </br>

* *_**Show Product Items**_* - This screen shows the list of all Outfit items belonging to the selected Outfit from Product details page.</br>
  * When user enters this screen, it sends item 'view' tracking event for the visible item using Data SDK's engagements API.
  * When user taps on any item in the list- 
    1. It sends 'click tracking event' using Data SDK's engagements API.
    2. It is recommended that Integrator App handles the item click action by implementing the item click listener exposed by UX SDK.
    3. If the item click listener is not implemented by Integrator/Sample App, it redirects to the web view when an item is clicked.</br>
  </br>

* *_**Tracking Events**_* - This is to track the user's interaction with the App by calling the engagement API implemented in Data SDK.
    1. Invoked from Data SDK
        1. Outfit load event - When Data SDK receives the Outfit API response from the Stylitics server, it invokes this event.
    2. Invoked from UX SDK
        1. Outfit view event - When user enters the PDP page, UX SDK invokes this tracking event for all Outfits viewed by the user. This tracking event is sent once per Outfit page session.
        2. Outfit item view event - UX SDK sends this event for all items in the Product list that are viewed by the user.
        3. Outfit click event - When user clicks on any Outfit in the Classic Outfit Widget, UX SDK invokes this event.
        4. Outfit item click event - When user clicks on any Outfit item in the Product list view, UX SDK invokes this event.
        5. Outfit item swap event - UX SDK sends this tracking event when a swap happens in Product List screen or Outfit collage.
    3. Invoked from Sample/Integrator app
        1. Jumplink Clicked event - This event is invoked when user clicks on "See How To Wear It" button on the PDP page. 

*_**Note**_* : [Click here](Code_Reference.md) to refer files for the Data and UX SDK.

## Technologies Used

Below technologies are used for implementing the Sample/Integrator App.

* Android Studio as IDE(**v** Chipmunk | 2021.2.1 Patch 1)
* Kotlin as programming language(**v** 1.6.21)
* XML for UI implementation(**v** 1.0)

## SDK version details

* Data SDK version used in the Sample App : *_**V1.0.10**_*
* Data SDK version used in the UX SDK : *_**V1.0.10**_*
* UX SDK version used in the Sample App : *_**V1.0.10**_*

## Steps to Implement Integrator/Sample App using UX SDK
Below are the steps to integrate and use Stylitics UX SDK in Integrator/Sample App.

### 1. Import Data SDK to access its features
Below are the steps that need to be followed to import the Stylitics Data SDK into the Integrator/Sample App.

     Step 1: Download Stylitics Data SDK from below link
          Link will be provided here.

     Step 2: Import .AAR file to the Integrator App
          a. In Integrator/Sample App project, create new directory in project level named libs.
          b. Copy and paste downloaded .AAR file here to the libs folder.
          c. Now in build.gradle file of module, add below implementation.
               implementation files('libs/styliticsdatarelease.aar')
          d. Sync your project. After successful sync you will be able to access SDK implementations.

[Click here to read about Data SDK features](DATA_SDK_README.md)

### 2. Import UX SDK to access its features

Below are the steps that need to be followed to import the Stylitics UX SDK into the Integrator/Sample App.

     Step 1: Download Stylitics UX SDK from below link
          Link will be provided here.

     Step 2: Import .AAR file to the Integrator App
          a. In Integrator/Sample App project, copy and paste downloaded .AAR file to the libs folder.
          b. Now in build.gradle file of module, add below implementation.
              implementation files('libs/styliticsuirelease.aar')
          c. Sync your project. After successful sync you will be able to access UX SDK implementations.

[Click here to read about UX SDK features](UX_SDK_README.md)

### 3. Add Required Dependencies 

Below dependencies are used to develop the Sample/Integrator App. These dependencies are been imported to the **_*build.gradle(:app)*_** file.

* Retrofit : Retrofit is used for network call implementations. With retrofit library it is easy to add custom headers and request types, file uploads, mocking responses.

      1. implementation "com.squareup.retrofit2:retrofit:2.9.0"
      
      2. implementation "com.squareup.retrofit2:converter-gson:2.9.0"
      
      3. implementation 'com.squareup.okhttp3:logging-interceptor:4.9.3'

* Activity and Fragment : These are to access composable APIs built on top of Activity and Fragment.

      1. implementation "androidx.activity:activity-ktx:1.4.0"
    
      2. implementation "androidx.fragment:fragment-ktx:1.4.1"

* Glide : This is the image loader library to load the images from API response to Android views.

      1. implementation 'com.github.bumptech.glide:glide:4.12.0'

* SDKs : This implementation is to import Stylitics data & UX SDK stored in libs folder

      1. implementation files('libs/styliticsdatarelease.aar')

      2. implementation files('libs/styliticsuirelease.aar')




### 4. Set Required Permissions
This App needs Internet access, so below given permission is added to the *_**AndroidManifest.xml**_*.

```xml
    <uses-permission android:name="android.permission.INTERNET"/>
```



### 5. Application Context

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

### 6. Configure Data SDK
There are some configurations which are required to access Data SDK features. [Click here](DATA_SDK_README.md#SDK-Configuration) to know about the SDK configuration details.

### 7. Fetch Outfits
Fetch Outfits data using Outfits API provided by the Data SDK. [Here](DATA_SDK_README.md#Example-to-Fetch-Outfits-using-item-number) are the code examples for fetching the Outfits.

### 8. Display Outfits
Use UX SDK views to display the Stylitics data provided by the Data SDK. [Click here](UX_SDK_README.md) to know more about the UX SDK views and their configurations.

### 10. Implement Exposed Listeners
Outfit and ProductList screen exposes below list of listeners to the Sample/Integrator app.

* For Outfit :
  1. onClick - On click event of Outfit, this listener will be triggered.
  2. onView - On view event of Outfit, this listener will be triggered.
  3. onItemSwap - On swap event of item in Outfit collage, this listener will be triggered.

* For Product List :
  1. onOutfitItemClick - On click event of Outfit Item, this listener will be triggered. It is highly recommended that Integrator app should implement this listener.
  2. onOutfitItemView - On view event of Outfit Item, this listener will be triggered.
  3. onItemSwap - On swap event of item in Product List screen, this listener will be triggered.

### 11. Purchase Events
This event should be triggered by Integrator/Sample app when user purchases any item provided by Stylitics data. It will help the Stylitics server provide better recommendations for the user. [Click here](DATA_SDK_README.md#API-Call-to-Track-Purchase-event) to read about Purchase Event implementation.

### 12. Mix And Match
When this feature is enabled, it enables the user to view replacements for the Outfit items.

* *_**Enable Mix and Match**_* -
  [Click here](DATA_SDK_README.md#Mix-and-Match) to enable disable mix and match feature.

* *_**Configure Mix and Match**_* - 
  [Click here](UX_SDK_README.md#Mix-and-Match-(MnM)) to configure Mix and Match UI feature.

* *_**See more options**_* -  When user clicks on this button, UX SDK shows a list of all replacement items received from the Data SDK belonging to the selected item.</br>
    </br>

* *_**Swap item in Outfit collage**_* - When user clicks on any item in Outfit collage, it will be replaced with the next item from its available replacements. For swapping it iterates through the replacement item list. When it reaches to the last item, it starts from the first item(default item) in the list. </br>
  [Here](UX_SDK_README.md#1-Classic-Outfit-Widget-with-Mix-and-Match-enabled) are more details and a code example to enable item swapping feature in Outfit collage.</br>
  </br>

* *_**Swap item in Product List**_* - When user clicks on any item in the replacements row, the currently displayed item will be replaced with the clicked item.</br>
  [Here](UX_SDK_README.md#2-Product-List-Screen-with-Mix-and-Match) are more details and a code example to enable the item swapping feature in Product list screen.</br>
  </br>

## Android Versioning Support

*_**build.gradle(:app)**_* is the file where App version and device versioning support is defined.

- Minimum required Android API level to access features of this App is - 21(Android 5.0).

- Target Android API level is 32(Android 13/ Beta).

## License

copyright © 2023 Stylitics

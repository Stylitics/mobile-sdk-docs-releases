# Stylitics UX SDK

It provides views to display Stylitics data. It also handles invoking of tracking events based on user interaction with these views.

## Features

SDK provides multiple custom view options to Sample Integrator App so it can easily display Stylitics data.

### Outfits

1. Outfits
    * [Classic Outfit Widget](CLASSIC_WIDGET_README.md)
    * [Hotspot Outfit Widget](HOTSPOT_WIDGET_README.md)
    * [Grid Outfit Widget](GRID_WIDGET_README.md)

2. Outfit Items
    * [Standard Product List View](STANDARD_PRODUCTLIST_README.md)

3. Replacements
    * Replacement View (Displayed internally from Product List View)
    
### Dynamic GalleryBundles

1. GalleryBundles
   * [Dynamic Gallery Widget](DYNAMIC_GALLERY_WIDGET_README.md)

2. OutfitBundleItems
   * [Outfit Bundle Product List View](OUTFIT_BUNDLE_PRODUCTLIST_README.md)
   
### Styled For You

1. StyledForYouBundles
   * [Styled For You Widget](STYLED_FOR_YOU_WIDGET_README.md)
   
### Outfit Landing Page(OLP)
1. OutfitLandingPage
   * [Outfit Landing Page Widget](OUTFIT_LANDING_PAGE_WIDGET_README.md)

2. OutfitBundleItems
   * [Outfit Bundle Product List View](OUTFIT_BUNDLE_PRODUCTLIST_README.md)

### Shop The Set
   * [Shop The Set Widget](SHOP_THE_SET_WIDGET_README.md) 

## Localization

Below is the screenshot with localized data when locale is configured to "en-GB". Locale parameter in Outfits data Api is case-insensitive, so it accepts both "en-GB" and "en-gb".

</br>![Image1](Screenshots/ProductList_with_localization.png)

*Note : When 'locale' is configured and the product items are displayed with the localized data, the price decimal parameter configuration will be ignored.*

## iOS Versioning Support

- Minimum required iOS APP version to access features of SDK is - (iOS 13.0)

## License

Copyright © 2023 Stylitics

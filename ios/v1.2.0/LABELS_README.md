# Labels

UX SDK provides various Label styles for the Top Label view displayed in Classic Outfit Widget.

## Label Styles Configuration

Data SDK fetches from the Stylitics server which Label style is supposed to be used for displaying Top Label in the Classic Outfit Widget. The Top label is displayed only when below conditions are satisfied-
1. Data SDK receives from Stylitics server which label style is expected to be used. 
2. When Outfits received from Stylitics server are configured to display the Label.

There are seven Label styles available in the UX SDK. If Integrator app does not provide configurations for the top label styles, UX SDK uses the default configurations. If Integrator app does not want to continue with the default configs, they can provide their own custom configurations.

Below are the label styles with their *_**default configurations, syntax to pass custom configurations and their screenshots**_*.

### Label 1 

*_**1. Default configurations**_*

| Config Parameters | Description| Default Value |
|---|---|---|
| title | to set the title of the label | Personalised Text |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue Regular |
| fontSize | is the font size in CGFloat  | 14 |
| fontColor | is text color | #212121 |
| backgroundColor | is background color | #FFFFFF |
| borderColor | is border color | #8E39E3 |
| borderWeight | is border weight in CGFloat | 2 |
| cornerRadius | is corner radius in CGFloat | 15 |
| paddingVertical | is top and bottom padding of the top label in CGFloat | 6 |
| paddingHorizontal | is left and right padding of the top label in CGFloat | 16 |
| position | is to change top label position to top left or top right | TopLabelPosition.topLeft |

*_**2. Syntax to pass custom Configurations**_*

Below is the code to provide the custom configurations for the Label1.

```swift
let topLabelConfig = TopLabel(label1: TopLabel.Label1(fontFamily: "calibri",
                                                      fontSize: 15,
                                                      fontColor: .black,
                                                      fontWeight: UIFont.Weight.bold,
                                                      backgroundColor: UIColor(red: 0.388,
                                                                               green: 0.113,
                                                                               blue: 0.207,
                                                                               alpha: 0.70),
                                                      borderColor: CGColor(red: 0.003,
                                                                           green: 0.529,
                                                                           blue: 0.525,
                                                                           alpha: 1),
                                                      borderWeight: 2,
                                                      cornerRadius: 20,
                                                      position: .topRight,
                                                      paddingVertical: 10,
                                                      paddingHorizontal: 15))

StyliticsUIApis.load(outfits: outfits,
                     outfitsTemplate: .classic(classicConfig: ClassicConfig(topLabel: topLabelConfig)))
```

</br>![Image1](Screenshots/label_style_1.png)


### Label 2

*_**1. Default configurations**_*

| Config Parameters | Description | Default Value |
|---|---|---|
| title | to set the title of the label | Personalised Text |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue bold |
| fontSize | is the font size in CGFloat | 14 |
| fontColor | is text color | #FFFFFF |
| backgroundColor | is background color | #000000 |
| textAndIconSpacing | is space between the icon and the title text | 8 |
| paddingVertical | is top and bottom padding of the top label in CGFloat | 6 |
| paddingHorizontal | is left and right padding of the top label in CGFloat | 16 |
| position | is to change top label position to top left or top right |TopLabelPosition.topLeft|

*_**2. Syntax to pass custom Configurations**_*

Below is the code to provide the custom configurations for the Label2.

```swift
let topLabelConfig = TopLabel(label2: TopLabel.Label2(fontFamily: "calibri",
                                                      fontSize: 15,
                                                      fontColor: .white,
                                                      fontWeight: UIFont.Weight.bold,
                                                      backgroundColor: UIColor(red: 0.003,
                                                                               green: 0.529,
                                                                               blue: 0.525,
                                                                               alpha: 1),
                                                      cornerRadius: 20,
                                                      position: .topRight,
                                                      paddingVertical: 10,
                                                      paddingHorizontal: 15,
                                                      iconAndTitleSpacing: 10))
```

</br>![Image1](Screenshots/label_style_2.png)

### Label 3

*_**1. Default configurations**_*

| Config Parameters | Description | Default Value |
|---|---|---|
| title | to set the title of the label | Personalised Text |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue Light |
| fontSize | is the font size in CGFloat | 16 |
| fontColor | is text color | #000000 |
| backgroundColor | is background color | #FFFFFF |
| shadowColor | is the shadow color | #000000 |
| shadowRadius | is the shadow radius in CGFloat | 4 |
| cornerRadius | is corner radius in CGFloat | 8 |
| position | is to change top label position to top left or top right | TopLabelPosition.topLeft |
| paddingVertical | is top and bottom padding of the top label in CGFloat | 8 |
| paddingHorizontal | is left and right padding of the top label in CGFloat | 12 |

*_**2. Syntax to pass custom Configurations**_*

Below is the code to provide the custom configurations for the Label3.

```swift
let topLabelConfig = TopLabel(label3: TopLabel.Label3(fontFamily: "calibri",
                                                      fontSize: 15,
                                                      fontColor: .white,
                                                      fontWeight: UIFont.Weight.bold,
                                                      backgroundColor: UIColor(red: 0.003,
                                                                               green: 0.529,
                                                                               blue: 0.525,
                                                                               alpha: 1),
                                                      cornerRadius: 20,
                                                      position: .topRight,
                                                      paddingVertical: 9,
                                                      paddingHorizontal: 20))
```

</br>![Image1](Screenshots/label_style_3.png)

### Label 4

*_**1. Default configurations**_*

| Config Parameters  | Description | Default Value |
|---|---|---|
| title | to set the title of the label | Personalised Text |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue Regular |
| fontSize | is the font size in CGFloat | 12 |
| fontColor | is text color | #FFFFFF |
| backgroundColor | is background color | #2B4B75 |
| shadowColor | is the shadow color | #000000 |
| shadowRadius | is the shadow radius in CGFloat | 4 |
| cornerRadius | is corner radius in CGFloat | 6 |
| position | is to change top label position to top left or top right | TopLabelPosition.topLeft |
| paddingVertical | is top and bottom padding of the top label in CGFloat | 12 |
| paddingHorizontal | is left and right padding of the top label in CGFloat| 12 |


*_**2. Syntax to pass custom Configurations**_*

Below is the code to provide the custom configurations for the Label4.

```swift
let topLabelConfig = TopLabel(label4: TopLabel.Label4(fontFamily: "calibri",
                                                      fontSize: 15,
                                                      fontColor: .black,
                                                      fontWeight: UIFont.Weight.bold,
                                                      backgroundColorAfterAnimation: UIColor(red: 0.607,
                                                                                             green: 0.607,
                                                                                             blue: 0.611,
                                                                                             alpha: 1),
                                                      cornerRadius: 10,
                                                      position: .topRight,
                                                      paddingVertical: 10,
                                                      paddingHorizontal: 20))
```

</br>![Image1](Screenshots/label_style_4.png)

### Label 5

*_**1. Default configurations**_*

| Config Parameters | Description | Default Value |
|---|---|---|
| title | to set the title of the label | Personalised Text |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue medium  |
| fontSize | is the font size in CGFloat | 14 |
| fontColor | is text color | #4700AB |
| backgroundColor | is background color | #EBECFE |
| cornerRadius | is corner radius in CGFloat | 15 |
| position | is to change top label position to top left or top right | TopLabelPosition.topLeft |
| paddingVertical | is top and bottom padding of the top label in CGFloat | 8 |
| paddingHorizontal | is left and right padding of the top label in CGFloat | 16 |

*_**2. Syntax to pass custom Configurations**_*

Below is the code to provide the custom configurations for the Label5.

```swift
let topLabelConfig = TopLabel(label5: TopLabel.Label5(fontFamily: "calibri",
                                                      fontSize: 15,
                                                      fontColor: UIColor(red: 0.003,
                                                                         green: 0.529,
                                                                         blue: 0.525,
                                                                         alpha: 1),
                                                      fontWeight: UIFont.Weight.bold,
                                                      backgroundColor: UIColor(red: 0.733,
                                                                               green: 0.956,
                                                                               blue: 0.741,
                                                                               alpha: 1),
                                                      cornerRadius: 20,
                                                      position: .topRight,
                                                      paddingVertical: 10,
                                                      paddingHorizontal: 15))
```

</br>![Image1](Screenshots/label_style_5.png)

### Label 6

*_**1. Default configurations**_*

| Config Parameters | Description | Default Value |
|---|---|---|
| title | to set the title of the label | Personalised Text |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue Bold |
| fontSize  | is the font size in CGFloat | 16 |
| fontColor | is text color | #1E1E1E |
| backgroundColor | is background color  | #FFFFFF |
| shadowColor | is the shadow color | #000000 |
| shadowRadius | is the shadow radius in CGFloat | 10 |
| shadowOffset | is the shadow offset | height = 4 |
| borderColor | is border color | #8E39E3 |
| borderWeight | is border weight in CGFloat | 2 |
| cornerRadius | is corner radius in CGFloat  | 32 |
| position | is to change top label position to top left or top right | TopLabelPosition.topLeft |
| paddingVertical | is top and bottom padding of the top label in CGFloat | 12 |
| paddingHorizontal | is left and right padding of the top label in CGFloat | 22 |

*_**2. Syntax to pass custom Configurations**_*

Below is the code to provide the custom configurations for the Label6.

```swift
let topLabelConfig = TopLabel(label6: TopLabel.Label6(fontFamily: "calibri",
                                                      fontSize: 15,
                                                      fontColor: UIColor(red: 0.003,
                                                                         green: 0.529,
                                                                         blue: 0.525,
                                                                         alpha: 1),
                                                      fontWeight: UIFont.Weight.bold,
                                                      backgroundColorAfterAnimation: .white,
                                                      cornerRadius: 20,
                                                      shadowColor: UIColor(red: 0,
                                                                           green: 0,
                                                                           blue: 0,
                                                                           alpha: 0.40),
                                                      shadowRadius: 6,
                                                      shadowOffset: CGSize(width: 0, height: 5),
                                                      position: .topRight,
                                                      paddingVertical: 8,
                                                      paddingHorizontal: 20))
```

</br>![Image1](Screenshots/label_style_6.png)

### Label 7

*_**1. Default configurations**_*

| Config Parameters | Description | Default Value |
|---|---|---|
| title | to set the title of the label | Personalised Text |
| icon | is background color of icon | #000000 |
| fontFamilyAndWeight | is the font style with the font weight | Helvetica Neue medium  |
| fontSize | is the font size in CGFloat | 14 |
| fontColor | is text color | #1E1E1E |
| textAndIconSpacing | is space between the icon and the title text | 8 |
| position | is to change top label position to top left or top right | TopLabelPosition.topLeft |

*_**2. Syntax to pass custom Configurations**_*

Below is the code to provide the custom configurations for the Label7.

```swift
let topLabelConfig = TopLabel(label7: TopLabel.Label7(fontFamily: "calibri",
                                                      fontSize: 15,
                                                      fontColor: UIColor(red: 0.003,
                                                                         green: 0.529,
                                                                         blue: 0.525,
                                                                         alpha: 1),
                                                      fontWeight: UIFont.Weight.bold,
                                                      position: .topRight,
                                                      iconAndTitleSpacing: 10,
                                                      iconColor: UIColor(red: 0.003,
                                                                         green: 0.529,
                                                                         blue: 0.525,
                                                                         alpha: 1)))
```

</br>![Image1](Screenshots/label_style_7.png)

### Configuring All Label Styles

Integrator app can pass custom configurations for multiple Label styles.

*_**Syntax to pass multiple Label custom configuration**_*

```swift
let topLabelConfig = TopLabel(label1: TopLabel.Label1(fontFamily: "calibri",
                                                      fontSize: 15),
                              label2: TopLabel.Label2(fontSize: 15,
                                                      iconAndTitleSpacing: 10),
                              label3: TopLabel.Label3(cornerRadius: 20,
                                                      position: .topRight,
                                                      paddingVertical: 9,
                                                      paddingHorizontal: 20),
                              label4: TopLabel.Label4(fontFamily: "calibri",
                                                      paddingVertical: 10,
                                                      paddingHorizontal: 20),
                              label5: TopLabel.Label5(position: .topRight),
                              label6: TopLabel.Label6(fontSize: 15,
                                                      fontColor: UIColor(red: 0.003,
                                                                         green: 0.529,
                                                                         blue: 0.525,
                                                                         alpha: 1),
                                                      shadowOffset: CGSize(width: 0, height: 5),
                                                      position: .topRight,
                                                      paddingVertical: 8,
                                                      paddingHorizontal: 20),
                              label7: TopLabel.Label7(fontSize: 15,
                                                      fontColor: UIColor(red: 0.003,
                                                                         green: 0.529,
                                                                         blue: 0.525,
                                                                         alpha: 1)))

StyliticsUIApis.load(outfits: outfits,
                     outfitsTemplate: .classic(classicConfig: ClassicConfig(topLabel: topLabelConfig)))
```

### Configuring Some Label Styles

*_**Syntax to pass some Label custom configuration**_*

```swift
let topLabelConfig = TopLabel(label2: TopLabel.Label2(fontSize: 15,
                                                      iconAndTitleSpacing: 10),
                              label3: TopLabel.Label3(cornerRadius: 20,
                                                      position: .topRight,
                                                      paddingVertical: 9,
                                                      paddingHorizontal: 20),
                              label7: TopLabel.Label7(fontSize: 15,
                                                      fontColor: UIColor(red: 0.003,
                                                                         green: 0.529,
                                                                         blue: 0.525,
                                                                         alpha: 1),
                                                      position: .topRight,
                                                      iconAndTitleSpacing: 10))

StyliticsUIApis.load(outfits: outfits,
                     outfitsTemplate: .classic(classicConfig: ClassicConfig(topLabel: topLabelConfig)))
```

## License

Copyright © 2023 Stylitics

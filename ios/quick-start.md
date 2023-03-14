# Quick Start

This section contains necessary steps to display a demo UserZoom survey within your application.

> **Note:** UserZoom SDK for iOS requires iOS 13.0 or greater.

## Installation

### Swift Package Manager

To integrate the UserZoom SDK into your Xcode project using Swift Package Manager, you need to add the repository URL to your `package.swift` file or in Xcode, select `File ▸ Swift Packages ▸ Add Package Dependency...`

**package.swift**
```
https://github.com/userzoom/UserZoomSDK-iOS
```

### CocoaPods

To integrate UserZoom SDK into your Xcode project using CocoaPods, specify it in your Podfile:

**Podfile**
```
pod 'UserzoomSDK', :git => 'git@github.com:userzoom/UserZoomSDK-iOS.git'
```

> **Note:** You might want to look at this FAQs: [How do I create a Podfile?](ios/sdk-ios-faq#how-do-i-create-a-podfile) 
 or [How do I run a project with CocoaPods?](ios/sdk-ios-faq#how-do-i-run-a-project-with-cocoapods)

### Carthage

To integrate UserZoom SDK into your Xcode project using Carthage, specify it in your Cartfile:

**Cartfile**
```
github "userzoom/UserZoomSDK-iOS"
```

## Launch the demo survey

Execute the following code anywhere in your app to launch the demo survey. It may for example be placed in the `viewDidLoad` method of the controller related to the view where you wish to show the survey.

``` swift
UserzoomSDK.clearExpirationData(); // Use this method only when testing!
UserzoomSDK.show("QzUzMUExMjdEMSAg");
```

## Result

Few seconds after the `show` method is called, an intercepting pop-up should appear on the screen. You can find an example below:

<img 
    alt="UserZoom intercept example"
    src="img/intercept-example.png"
    style="width: 50%; display:block; margin: auto;"
/>


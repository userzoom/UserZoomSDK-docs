# Initial Setup

>*Note: This document provides the necessary steps to integrate the UserZoom SDK into an iOS project, presuming Xcode is being used for the iOS development.* 

>**Note: This SDK requires iOS 13.0 or greater.**

## Adding the SDK via Swift Package Manager

To integrate the UserzoomSDK into your Xcode project using Swift Package Manager, you need to add the repository url UserzoomSDK in your package.swift file or in Xcode, select File ▸ Swift Packages ▸ Add Package Dependency... :

```
https://github.com/userzoom/UserZoomSDK-iOS
```

## Adding the SDK via CocoaPods

To integrate UserzoomSDK into your Xcode project using CocoaPods, specify it in your Podfile:

```
pod 'UserzoomSDK', :git => 'git@github.com:userzoom/UserZoomSDK-iOS.git'
```

> ***Note: might want to look at this FAQs: [How do I create a podfile?](ios/sdk-ios-faq#how-do-i-create-a-podfile) or [How do I run a project with cocoapods?](ios/sdk-ios-faq#how-do-i-run-a-project-with-cocoapods)***

## Adding the SDK via Carthage

To integrate UserzoomSDK into your Xcode project using Carthage, specify it in your Cartfile:

```
github "userzoom/UserZoomSDK-iOS"
```

----
## Integrating the SDK into your app

### Configuration keys

In order to prepare the application to be able to **request camera and microphone permissions**, configuration keys need to be added. To do so, select the project within the Project Navigator, go to the `Info` section and scroll to the bottom of the menu. Then, expand the `Custom iOS Target Properties`.

![properties]

If you hover over the list, press the + button to add new properties. If they do not exist already, you need to create the following: `Privacy - Camera Usage Description` and `Privacy - Microphone Usage Description`. 
> ***Note: Make sure their type is set as `String` and then type the value you want to show to users when the system requests video and audio permissions.***

---
### Initialize the SDK
Add this code to the `didFinishLaunchingWithOptions` method of the application delegate

```swift
    UserzoomSDK.initWithTag("{YourAppTag}", options: launchOptions);
```
>***Note: Replace `{YourAppTag}` with the correct tag, which can be provided by the study administrator.***

>**Example**
>```swift
>func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
>    // ...
>    UserzoomSDK.initWithTag("{YourAppTag}", options: launchOptions);
>    // ...
>}
>```

---
### Prepare for invitation links

#### Configure the AppDelegate
 On the Project Navigator, open the source file of the application delegate (AppDelegate)  

 Add the `UNUserNotificationCenterDelegate` protocol to the class and register the delegate on the `UNUserNotificationCenter` following the below code: 

>**AppDelegate**
>```swift
>import UserzoomSDK
>
>@UIApplicationMain
>class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {
>
>    #pragma mark - UIApplicationDelegate
>
>    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
>        // - Register delegate for the new User Notification API in iOS 10.0+
>        UNUserNotificationCenter.current().delegate = self
>        ... 
>   
>        return true
>    }
>```


 Implement the method `userNotificationCenter: didReceiveNotificationResponse` of the `UNUserNotificationCenterDelegate` protocol and invoke the UserzoomSDK library method called `didReceiveNotificationResponse`

>**AppDelegate**
>```swift
>    #pragma mark - UNUserNotificationCenterDelegate
>
>    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
>        UserzoomSDK.didReceive(response, withCompletionHandler: completionHandler)
>    }   
>}
>```

 Implement the `openURL` method in the application delegate to call the UserZoomSDK library `openURL` method:

> **AppDelegate**
>```swift
>func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
>    return UserzoomSDK.open(url);
>}
>```

#### Configure the URL Scheme

In order to prepare the application to be able to open invitation links, the URL Scheme needs to be added. To do so, select the project within the Project Navigator, go to the `Info` section and scroll to the bottom of the menu. Then, expand the `URL Types` and click on the `+` button to add a new URL Scheme.

![][info]

Finally, fill the `Identifier` and `URL schemes` fields by replacing `{YourURLScheme}` with the correct values, which can be provided by the study administrator.

![][schemes]

[info]: ./images/info.png
[schemes]: ./images/schemes.png
[properties]: ./images/properties.png
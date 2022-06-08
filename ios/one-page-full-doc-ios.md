# Initial Setup

>***Note: This document provides the necessary steps to integrate the UserZoom SDK into an iOS project, presuming Xcode is being used for the iOS development. The UserZoom SDK requires iOS 11.0 or higher.*** 

## Adding UserZoom SDK to a project using CocoaPods

To integrate UserzoomSDK into your Xcode project using CocoaPods, specify it in your Podfile:

```
pod 'UserzoomSDK', :git => 'git@github.com:UserZoomDev/UserzoomSDK-iOS.git'
```
----
## Integrate UserZoom SDK into your app

### Configuration keys

In order to prepare the application to be able to **request camera and microphone permissions**, configuration keys need to be added. To do so, select the project within the Project Navigator, go to the `Info` section and scroll to the bottom of the menu. Then, expand the `Custom iOS Target Properties`.

![you need to create the following: `Privacy - Camera Usage Description` and `Privacy - Microphone Usage Description`. 
> ***Note: Make sure their type is set as `String` and then type the value you want to show to users when the system requests video and audio permissions.***

---
### Initialize the UserzoomSDK
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
### Invitation links

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

# Custom tags

## Tag events

Just like other Analytic tools, in order to tag events correctly, the `sendEvent` method needs to be used, being able to send up to three tags at the same time.

```swift
UserzoomSDK.sendEvent("{TAG1}", tag2: "{TAG2}" tag3: "{TAG3}");
```

To do so, replace `{TAG1}`, `{TAG2}` and `{TAG3}` with the content of the tags to be sent.

> ***⚠️ WARNING: Please do not use the tag `UZ` for `{TAG1}` since it is a reserved TAG for UserZoom.***

## Tag search keywords

If the application has search fields, add tags to collect the keywords participants entered by using the `sendKeywords` method:

```swift
UserzoomSDK.sendKeywords("{SearchBarContent}");
```

> ***Note: replace `{SearchBarContent}` with the content of the search field.***

## Security

In order to interrupt and resume UserZoom from visually collecting views on certain areas of the App, use the `blockRecord` method. 

***To interrupt:***
```swift
UserzoomSDK.blockRecord(true);
```

***To resume:***
```swift
UserzoomSDK.blockRecord(false);
```

The most common use is to block a concrete ViewController, implementing the `viewWillAppear` and `viewDidDisappear` methods:

>**Example**
>```swift
>override func viewWillAppear(_ animated: Bool) {
>   super.viewWillAppear(animated);
>   UserzoomSDK.blockRecord(true);
>}
>
>override func viewDidDisappear(_ animated: Bool) {
>    super.viewDidDisappear(animated);
>    UserzoomSDK.blockRecord(false);
>}
>```

---
## Advanced settings (Optional)

### Force finalize study

Finish a study at any time by calling the `finalizeStudy` method:

```swift
UserzoomSDK.finalizeStudy();
```

---
### Use multiple tags

Use more than one App Tag inside the same application, not only the one specified in the initialization, by calling the `show` method anywhere in the application:

```swift
UserzoomSDK.show("{YourAppTag}");
```

Replace `{YourAppTag}` with the correct tag, which can be provided by the study administrator.

> ***Note: This method does not execute if a study is running at that time.***

---
### Logging levels

Increase or decrease the amount of information displayed in logs by calling the `setDebugLevel` method with one of the following parameters just before the `initWithTag` call:

```swift
UserzoomSDK.setDebugLevel(UZLogSilent);  // (Default) disable all log
UserzoomSDK.setDebugLevel(UZLogError);   // shows errors as well
UserzoomSDK.setDebugLevel(UZLogWarning); // shows warnings as well
UserzoomSDK.setDebugLevel(UZLogInfo);    // shows information
UserzoomSDK.setDebugLevel(UZLogVerbose); // enable all logs
```

---
### Clear Expiration Data

To clear the information about previously completed studies, invoke the `clearExpirationData` method.

```swift
UserzoomSDK.clearExpirationData();
```

> ***⚠️ WARNING: This method is intended for testing purposes only. Calling it on a released application can produce unexpected behaviours.***

---
### Deactivate App after study

Activate the Invitation Link exclusive mode, automatically closing the app unless the SDK is started from an Invitation Link. To do so, add the following code to the `didFinishLaunchingWithOptions` method of the application delegate:

```swift
UserzoomSDK.deactivateApp(afterStudy: launchOptions);
```

If the app is not opened through an Invitation Link, an alert message will be shown to users with the following information: _"Please go to the study email you received and tap on 'Start study' to begin. If you have already completed this study, you may uninstall the app."_. The app is closed automatically afterwards.

In order to customize this message, use the following method instead:

```swift
UserzoomSDK.deactivateAppAfterStudy(withMessage: "Your custom message", andButtonText: "Ok", andOptions: launchOptions);
```

---
### Custom Intercept

UserZoom SDK automatically displays an _Intercept_ view when the study is initialized and ready, but also offers the option of building your own user interface in order to intercept participants.

To do so, set an **UZShowCustomInterceptDelegate** implementation to **UserzoomSDK** and show the custom view once `showCustomIntercept` is invoked.

>**Example**
>```swift
>override func viewDidLoad() {
>    super.viewDidLoad();
>    UserzoomSDK.setShowCustomInterceptDelegate(self);
>    // ...
>}
>
>func showCustomIntercept(_ delegate: UZInterceptProtocol!) {
>
>    let alert: UIAlertController = UIAlertController(title: "Hello!!", message: "Do you want to take a study?", preferredStyle: UIAlertControllerStyle.alert);
>
>    let defaultAction: UIAlertAction = UIAlertAction(title: "Ok", style: UIAlertActionStyle.default) { (action:UIAlertAction) in
>        delegate.allowIntercept();
>    }
>
>    let cancelAction: UIAlertAction = UIAlertAction(title: "NO", style: UIAlertActionStyle.default) { (action:UIAlertAction) in
>        delegate.closeIntercept();
>    }
>
>    alert.addAction(defaultAction);
>    alert.addAction(cancelAction);
>
>    self.present(alert, animated: true, completion: nil);
>}
>```

The `showCustomIntercept` method receives a `UZInterceptProtocol` delegate that **must** be called when the user decides whether or not take the study:

```objc
@protocol UZInterceptProtocol <NSObject>
-(void) allowIntercept;
-(void) closeIntercept;
@end
```

---
### Custom Vars

In order to be able to input additional information in run time, you need to use custom vars. To do so, add the following line to your code before starting the study:

```swift
UserzoomSDK.addCustomVar(withKey: "{MyKey}", andValue: "{MyValue}");
```

>***Note: Replace `{MyKey}` and `{MyValue}` with the correct values.***

Or use the Dictionary implementation:

```swift
UserzoomSDK.addCustomVars(["{MyKey1}" : "{MyValue1}", "{MyKey2}" : "{MyValue2}"]);
```
 
>***⚠️ WARNING: You need to cal that method*** *before* ***the study starts, otherwise the information will not be sent to the server.***

If you want to delete all information you have already input, add the following line to your code before starting the study:
```swift
UserzoomSDK.clearCustomVars();
```

>***⚠️ WARNING: You need to call this method*** *before* ***starting the study. If the study has already started, there is no way to clear that information.***

---

## Test the integration

To test the integration, clear the expiration data with `clearExpirationData` and set the log level to `UZLogVerbose`, right before calling the `initWithTag` method. Then, add the following code to the `didFinishLaunchingWithOptions` method of the application delegate

>**Example**
>```swift
>func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
>       // ...
>       UNUserNotificationCenter.current().delegate = self
>       UserzoomSDK.setDebugLevel(UZLogVerbose);
>       UserzoomSDK.clearExpirationData();
>       UserzoomSDK.initWithTag("{YourAppTag}");
>       UserzoomSDK.show();
>       // ... 
>  
>       return true
>   }
>```

Replace `{YourAppTag}` with the correct tag, which can be provided by the study administrator.

Build and run the application to see if an intercept message displays on startup. The console should print some information logs at this stage.

Finally, let's test the integration with an Invitation Link. Clear the test integration code you just added, return to the Info tab of the project and change the URL Scheme added in Step 4 to `userzoomC4A1`. Once the application is running, open this URL using Safari browser:

> [https://m.userzoom.com/m/MyBDNFMxOTcx](https://m.userzoom.com/m/MyBDNFMxOTcx)

Press 'Start Study' and verify the app opens as well as the used is asked to participate to the test study.

>***⚠️ WARNING: Once this testing is done, make sure you revert the URL Scheme back to `{YourURLScheme}`.***

---
## Troubleshoot

A. This library requires internet connection and full access to the userzoom.com domain to work properly.

B. If the intercept message is not displaying after testing the integration, use this test App Tag instead in order to perform a basic test:

```swift
UserzoomSDK.setDebugLevel(UZLogVerbose);
UserzoomSDK.clearExpirationData();
UserzoomSDK.initWithTag("QzRBMUQx");
UserzoomSDK.show();
```

If this works, please contact the study Administrator to correctly launch the study in the UserZoom tool. If, once launched, the issue persists, contact UserZoom Support Team at [support@userzoom.com][tickets] providing the following information:

* Confirmation that the test App Tag has been tested and it does not work.
* Confirmation that the logs and developer mode were enabled and the issue is still persistent.
* If possible, provide the logs, including everything that has been done, and a test App where the problem can be replicable.

---
## FAQ

[Frequently asked questions][faq]

---
## License

Developer's use of the SDK is governed by the license in the applicable UserZoom [Terms of Service][terms].

A commercial UserZoom SDK license allows using the UserZoom SDK within your own. Any results obtained by using a commercial UserZoom SDK license may be used for commercial purposes. A distribution of own networks or modules is permitted. However, redistribution of the UserZoom SDK, either completely or in parts, or using the UserZoom software or parts thereof for commercial products or services is not permitted. Thus, it is not permitted to combine a commercial distribution of own networks or modules with the UserZoom runtime environment.

The Customer must only use the Service as intended by UserZoom. The Customer must not copy or share the Service or the code on which the Service is based (or in any other way violate UserZoom intellectual property rights).

The dealings with the participants (or any other possible user) interacting with the software application, in which a UserZoom SDK is integrated, or with a UserZoom Application are between the Customer and the participants. The Customer is, in relation to UserZoom, thus solely responsible for the participants and their legal rights and obligations related to the Service.

The UserZoom website, UserZoom SDKs and UserZoom Applications are the property of UserZoom and violation of any intellectual property right connected thereto is deemed as copyright infringement. The Service is not sold to the Customer by approving these Terms of Use, through the integration with any software application or by using a UserZoom Application, and UserZoom retains ownership of the Service and the code on which the Service is based.

The Customer must not change, modify, adapt or alter the Service or change, modify or alter another website or service so as to falsely imply that it is associated with the Service or UserZoom.
The integration of UserZoom’s SDK does not grant the Customer any right to use UserZoom’s domain names, service marks, logos or any other features attributable to UserZoom.

[here]: http://www.userzoom.com
[cocoapods]: http://cocoapods.org
[manager]: https://manager.userzoom.com
[downloadLink]: https://github.com/userzoom/userzoom-sdk-ios
[info]: ./images/ios-target-info.png
[schemes]: ./images/ios-target-url-scheme.png
[properties]: ./images/ios-target-properties.png
[tickets]: support@userzoom.com
[faq]: ios/FAQ.md
[terms]: https://s.userzoom.com/documents/UserZoom_Terms_of_Service.pdf
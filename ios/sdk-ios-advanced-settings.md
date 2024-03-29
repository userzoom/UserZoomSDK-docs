# Advanced settings (Optional)

## Logging levels

Increase or decrease the amount of information displayed in logs by calling the `setDebugLevel` method with one of the following parameters just before the `initWithTag` or `show` call:

```swift
UserzoomSDK.setDebugLevel(UZLogSilent);  // (Default) disable all log
UserzoomSDK.setDebugLevel(UZLogError);   // shows errors as well
UserzoomSDK.setDebugLevel(UZLogWarning); // shows warnings as well
UserzoomSDK.setDebugLevel(UZLogInfo);    // shows information
UserzoomSDK.setDebugLevel(UZLogVerbose); // enable all logs
```

## Clear Expiration Data

To clear the information about previously completed studies, invoke the `clearExpirationData` method.

```swift
UserzoomSDK.clearExpirationData();
```

> ***⚠️ WARNING: This method is intended for testing purposes only. Calling it on a released application can produce unexpected behaviors.***

## Invitation Link exclusive mode

Activate the Invitation Link exclusive mode, automatically closing the app unless the SDK is started from an Invitation Link. To do so, add the following code to the `didFinishLaunchingWithOptions` method of the application delegate:

```swift
UserzoomSDK.deactivateApp(afterStudy: launchOptions);
```

If the app is not opened through an Invitation Link, an alert message will be shown to users with the following information: _"Please go to the study email you received and tap on 'Start study' to begin. If you have already completed this study, you may uninstall the app."_. The app is closed automatically afterwards.

In order to customize this message, use the following method instead:

```swift
UserzoomSDK.deactivateAppAfterStudy(withMessage: "Your custom message", andButtonText: "Ok", andOptions: launchOptions);
```

## Pause screen recording

In order to interrupt and resume UserZoom from visually collecting views on certain areas of the App, use the `blockRecord` method. 

**To interrupt:**
```swift
UserzoomSDK.blockRecord(true);
```

**To resume:**
```swift
UserzoomSDK.blockRecord(false);
```

The most common use is to block a concrete ViewController, implementing the `viewWillAppear` and `viewDidDisappear` methods:

**Example**
```swift
override func viewWillAppear(_ animated: Bool) {
   super.viewWillAppear(animated);
   UserzoomSDK.blockRecord(true);
}

override func viewDidDisappear(_ animated: Bool) {
    super.viewDidDisappear(animated);
    UserzoomSDK.blockRecord(false);
}
```

## Force study finalization

Finish a study at any time by calling the `finalizeStudy` method:

```swift
UserzoomSDK.finalizeStudy();
```

## Custom Intercept

UserZoom SDK automatically displays an _Intercept_ view when the study is initialized and ready, but also offers the option of building your own user interface in order to intercept participants.

To do so, set an **UZShowCustomInterceptDelegate** implementation to **UserzoomSDK** and show the custom view once `showCustomIntercept` is invoked.

**Example**
```swift
override func viewDidLoad() {
    super.viewDidLoad();
    UserzoomSDK.setShowCustomInterceptDelegate(self);
    // ...
}

func showCustomIntercept(_ delegate: UZInterceptProtocol!) {

    let alert: UIAlertController = UIAlertController(title: "Hello!!", message: "Do you want to take a study?", preferredStyle: UIAlertControllerStyle.alert);

    let defaultAction: UIAlertAction = UIAlertAction(title: "Ok", style: UIAlertActionStyle.default) { (action:UIAlertAction) in
        delegate.allowIntercept();
    }

    let cancelAction: UIAlertAction = UIAlertAction(title: "NO", style: UIAlertActionStyle.default) { (action:UIAlertAction) in
        delegate.closeIntercept();
    }

    alert.addAction(defaultAction);
    alert.addAction(cancelAction);

    self.present(alert, animated: true, completion: nil);
}
```

The `showCustomIntercept` method receives a `UZInterceptProtocol` delegate that **must** be called when the user decides whether or not take the study:

```objc
@protocol UZInterceptProtocol <NSObject>
-(void) allowIntercept;
-(void) closeIntercept;
@end
```

## Custom variables

In order to be able to input additional information in run time, you need to use custom variables. To do so, add the following line to your code before starting the study:

```swift
UserzoomSDK.addCustomVar(withKey: "{MyKey}", andValue: "{MyValue}");
```

>***Note: Replace `{MyKey}` and `{MyValue}` with the correct values.***

Or use the Dictionary implementation:

```swift
UserzoomSDK.addCustomVars(["{MyKey1}" : "{MyValue1}", "{MyKey2}" : "{MyValue2}"]);
```
 
>***⚠️ WARNING: You need to call that method*** *before* ***the study starts, otherwise the information will not be sent to the server.***

If you want to delete all information you have already input, add the following line to your code before starting the study:
```swift
UserzoomSDK.clearCustomVars();
```

>***⚠️ WARNING: You need to call this method*** *before* ***starting the study. If the study has already started, there is no way to clear that information.***

## Custom events

UserZoom SDK lets you register custom events by using the `sendEvent` method. You should provide up to three descriptive event strings.

```swift
UserzoomSDK.sendEvent("{STRING1}", tag2: "{STRING2}" tag3: "{STRING3}");
```

>***⚠️ WARNING: Please do not use the tag `UZ` for `{STRING1}` since it is a reserved by the UserZoom SDK.***

Additionally, if the application has search fields, you can register the keywords that the participants entered by passing them to the `sendKeywords` method:

```swift
UserzoomSDK.sendKeywords("{SearchBarContent}");
```

> ***Note: replace `{SearchBarContent}` with the content of the search field.***
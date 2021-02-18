# Test the integration

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

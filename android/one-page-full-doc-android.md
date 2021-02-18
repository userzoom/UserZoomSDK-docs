# Initial Setup

> *Note: This document provides the necessary steps to integrate the UserZoom SDK into an Android project, presuming Android Studio is being used for the Android development.*

# Adding UserZoom SDK to the project

> ***Note:*** *Currently the GitHub Packages requires us to Authenticate to download an Android Library (Public or Private) hosted on the GitHub Package. This might change for future releases.*

## Step 1: Generate a Personal Access Token for GitHub
Inside you GitHub account: Settings -> Developer Settings -> Personal Access Tokens -> Generate new token 

Make sure you select the following scope **“read:packages”** and Generate a token.

> ***⚠️ WARNING:  After generating the token make sure to copy your new personal access token. You won't be able to see it again, the only option is to generate a new key***.

## Step 2: Store your GitHub — Personal Access Token details
- Create a **github.properties** file within your root Android project.  
> ***Note:*** *In case of a public repository make sure you add this file to .gitignore to keep the token private.*   
- Add properties **gpr.usr** and **gpr.key** to the file  
>**github.properties**
>```
>gpr.usr=GITHUB_USERID
>gpr.key=PERSONAL_ACCESS_TOKEN
>```
> ***Note:*** *Replace GITHUB_USERID with personal / organisation Github User ID and PERSONAL_ACCESS_TOKEN with the token generated in Step 1.*  
- (Optional) Alternatively you can add the GPR_USER and GPR_API_KEY values to your environment variables on you local machine or build server to avoid creating a github properties file.

## Step 3 : Update build.gradle inside the application module
Add the following code to build.gradle inside the app module that will be using the library published on GitHub Packages

>**build.gradle**  
>```gradle
>allprojects {
>
>    def githubProperties = new Properties()
>    githubProperties.load(new FileInputStream(rootProject.file("github.properties")))
>
>    repositories {
>        google()
>        jcenter()
>        maven {
>            name = "GitHubPackages"
>            url = uri("https://maven.pkg.github.com/UserZoomDev/UserzoomSDK-Android")
>            credentials {
>                username = githubProperties['gpr.usr'] ?: System.getenv("GPR_USER")
>                password = githubProperties['gpr.key'] ?: System.getenv("GPR_API_KEY")
>            }
>        }
>    }
>}
>```

---
>**app/build.gradle**  
>```gradle
>dependencies { implementation 'com.userzoom:sdk:+' }
>```


----
# Integrate Userzoom SDK into your app

## Add the UserZoom's Android components and permissions in AndroidManifest.xml

Add the following Android components in the App's `AndroidManifest.xml` file, within the `<application>` tag.

>**AndroidManifest.xml**
>```xml
><activity android:name="com.userzoom.sdk.presentation.UserzoomActivity"
>    android:configChanges="orientation|keyboard|keyboardHidden|screenSize"
>    android:theme="@style/Theme.AppCompat.Translucent" >
></activity>
>```

We encourage to use our default `android:theme` for that, define on your resources file a new `style` coping the code below and applying the new style `Theme.AppCompat.Translucent` to the `UserzoomActivty` on already added `AndroidManifest.xml`.

>**AndroidManifest.xml**
>```xml
><resources>
>    <style name="Theme.AppCompat.Translucent" parent="Theme.AppCompat.Light.NoActionBar">
>        <item name="android:windowNoTitle">true</item>
>        <item name="android:windowBackground">@android:color/transparent</item>
>        <item name="android:colorBackgroundCacheHint">@null</item>
>        <item name="android:windowIsTranslucent">true</item>
>        <item name="android:windowAnimationStyle">@android:style/Animation</item>
>    </style>
></resources>
>```

Also, ensure the following Android permissions are declared within the `manifest` tag:

>**AndroidManifest.xml**
>```xml
><uses-permission android:name="android.permission.INTERNET" />
><uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
><uses-permission android:name="android.permission.VIBRATE" />
><uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
><uses-permission android:name="android.permission.CAMERA" />
><uses-permission android:name="android.permission.RECORD_AUDIO" />
>
><uses-feature android:name="android.hardware.camera" android:required="false" />
><uses-feature android:name="android.hardware.camera.front" android:required="false"/>
><uses-feature android:name="android.hardware.camera.autofocus"  android:required="false"/>
>```

## Initialize the Userzoom SDK

### Default initialization

In order to initialize the library, add this code to the `onCreate` method of the **main Activity**:

```Java
UserzoomSDK.init(this);
```
> *Note: The `this` parameter represents the Activity object.*

> **Example**
>```Java
>@Override
>protected void onCreate(Bundle savedInstanceState) {
>    super.onCreate(savedInstanceState);
>    // ...
>    UserzoomSDK.init(this);
>    // ...
>}
>```

### App tag initialization

If the initialization needs to use a specific App tag, initialize the library as follows:

```Java
UserzoomSDK.init(this, "{YourAppTag}");
```
> *Note: Replace {YourAppTag} with the correct tag, which can be provided by the study administrator.*

> **Example**
>```Java
>@Override
>protected void onCreate(Bundle savedInstanceState) {
>    super.onCreate(savedInstanceState);
>    // ...
>    UserzoomSDK.init(this, "{YourAppTag}");
>    // ...
>}
>```

----

## Invitation links

In order to prepare the application to be able to open invitation links, the **URL Scheme needs to be added**. Add the `intent-filter` on already added `UserzoomActivity` in the App’s `AndroidManifest.xml` file.

>```xml
><activity android:name="com.userzoom.sdk.presentation.UserzoomActivity"
>    android:configChanges="orientation|keyboard|keyboardHidden|screenSize"
>    android:theme="@style/Theme.AppCompat.Translucent" >
>    <intent-filter>
>        <action android:name="android.intent.action.VIEW" />
>        <category android:name="android.intent.category.DEFAULT" />
>        <category android:name="android.intent.category.BROWSABLE" />
>        <data android:scheme="{YourURLScheme}" />
>    </intent-filter>
></activity>
>```

Fill the `android:scheme` field by replacing `{YourURLScheme}` with the correct value, which will be provided by the study administrator.

---

## Tag events

Just like other Analytic tools, in order to tag events correctly, the `sendEvent` method needs to be used, being able to send up to three tags at the same time.

```Java
    UserzoomSDK.sendEvent("{TAG1}", "{TAG2}", "{TAG3}");
```

To do so, replace `{TAG1}`, `{TAG2}` and `{TAG3}` with the content of the tags to be sent.

>***⚠️ WARNING: Please do not use the tag `UZ` for `{TAG1}` since it is a reserved TAG for UserZoom.***

---

## Security

In order to interrupt and resume UserZoom from visually collecting views on certain areas of the App, use the `blockRecord` method.  

***To interrupt:***
```Java
UserzoomSDK.blockRecord(true);
```

***To resume:***

```Java
UserzoomSDK.blockRecord(false);
```

The most common use is to block a concrete Activity, invoking the method in the `onCreate` and `onStop` methods.

> **Example**
>```Java
>@Override
>protected void onCreate(Bundle savedInstanceState) {
>    super.onCreate(savedInstanceState);
>    // ...
>    UserzoomSDK.blockRecord(true);
>    // ...
>}
>
>@Override
>protected void onStop() {
>    super.onStop();
>    // ...
>    UserzoomSDK.blockRecord(false);
>    // ...
>}
>```


---

## Advanced settings (Optional)

### Force finalize study

Finish a study at any time by calling the `finalizeStudy` method:

```Java
UserzoomSDK.finalizeStudy();
```

### Use multiple tags

Use more than one App Tag inside the same application, not only the one specified in the initialization, by calling the `show` method anywhere in the application:

```Java
UserzoomSDK.show(this, "{YourAppTag}");
```

Replace `{YourAppTag}` with the correct tag, which can be provided by the study administrator. Also, consider invoking this method only within an Activity, since the first parameter should be the current Activity.

>***Note: This method does not execute if a study is running at that time.***

---
### Logging levels

Increase or decrease the amount of information displayed in logs by calling the `setDebugLevel` method with one of the following parameters just before the `init` call:

```Java
UserzoomSDK.setDebugLevel(LOG_LEVEL.SILENT);     // (Default) disable all logs
UserzoomSDK.setDebugLevel(LOG_LEVEL.ERROR);       // shows errors as well
UserzoomSDK.setDebugLevel(LOG_LEVEL.WARNING);   // shows warnings as well
UserzoomSDK.setDebugLevel(LOG_LEVEL.INFO);      // shows information
UserzoomSDK.setDebugLevel(LOG_LEVEL.VERBOSE);   // enable all logs
```

---
### Notification icon

The UserZoom SDK can use Android's notifications. To modify the notification icon to be used, invoke the `setIconResourceNotification` method. By default, the icon displayed will be the App's icon.

```Java
UserzoomSDK.setIconResourceNotification(R.drawable.notification_icon);
```

---
### Clear Expiration Data

To clear the information about previously completed studies, invoke the `clearExpirationData` method.

```Java
UserzoomSDK.clearExpirationData();
```

>***⚠️ WARNING: This method is intended for testing purposes only. Calling it on a released application can produce unexpected behaviours.***

---
### Deactivate App after study

Activate the Invitation Link exclusive mode, automatically closing the app unless the SDK is started from an Invitation Link. To do so, add the following code to the `onCreate` method of the main Activity:

```Java
UserzoomSDK.deactivateAppAfterStudy(this);
```

If the app is not opened through an Invitation Link, an alert message will be shown to users with the following information: _"Please go to the study email you received and tap on 'Start study' to begin. If you have already completed this study, you may uninstall the app."_. The app is closed automatically afterwards.

In order to customize this message, use the following method instead:

```Java
UserzoomSDK.deactivateAppAfterStudy(this, "Your custom message", "Ok");
```

---
### UserZoom Callback

It is possible to receive some notifications related to the UserZoom SDK, for which you need to define a `Callback` object and provide the library with it:

``` Java
UserzoomSDK.setCallback(new UserzoomSDKCallback() {
    /**
     * Invoked when the valid test fails.
     *
     * @param reason The reason of the failure.
     */
    void onDeviceNotValid(String reason) {
        
    }

    /**
     * Invoked when the study has started.
     */
    void onStudyStarted() {
        
    }

    /**
     * Invoked when the study has finished.
     *
     * @param reason Has the study finished by user action.
     */
    void onStudyFinalized(Boolean isQuitByUser) {
        
    }
    
    /**
     * Invoked when a Navigation Task has started. Only applies to Advanced/Basic UX Research for Apps.
     */
    void onNavigationTaskStart() {
        
    }

    /**
     * Invoked when a Navigation Task has ended. Only applies to Advanced/Basic UX Research for Apps.
     */
    void onNavigationTaskEnd() {
        
    }
})
```

---
### Custom Intercept

UserZoom SDK automatically displays an _Intercept_ view when the study is initialized and ready, but also offers the option of building your own user interface in order to intercept participants.

To do so, set an **IShowCustomInterceptCallback** implementation to **UserzoomSDK** and show the custom view once `showCustomIntercept` is invoked.

>**Example**
>```Java
>@Override
>protected void onCreate(Bundle savedInscanteState) {
>   UserzoomSDK.setShowCustomInterceptCallback(this);
>}
>
>@Override
>public void showCustomIntercept(final IInterceptCallback interceptCallback) {
>    /* 
>        Show your custom intercept view here. If you want to launch a new Activity,
>        we encourage to do so with 'startActivityForResult' and invoke the
>        'interceptCallback' when activity results.
>    */
>    new AlertDialog.Builder(context)
>    .setTitle("Hello!")
>    .setMessage("Do you want to take a study?")
>    .setPositiveButton(android.R.string.yes, new DialogInterface.OnClickListener() {
>        public void onClick(DialogInterface dialog, int which) { 
>            interceptCallback.accept();
>        }
>     })
>    .setNegativeButton(android.R.string.no, new DialogInterface.OnClickListener() {
>        public void onClick(DialogInterface dialog, int which) { 
>            interceptCallback.cancel();
>        }
>     })
>    .setIcon(android.R.drawable.ic_dialog_alert)
>   .show();
>}
>```

The `showCustomIntercept` method receives an `IInterceptCallback` interface that **must** be called when the user decides whether or not take the study:

```Java
public interface IInterceptCallback {

    /**
     * Participant clicks the cancel at the welcome intercept
     */
    void cancel();

    /**
     * Participant clicks the accept at the welcome intercept
     */
    void accept();

    /**
     * Participant clicks the close button at the welcome intercept
     */
    void close();
}
```

---
### Custom Vars

In order to be able to input additional information in run time, you need to use custom vars. To do so, add the following line to your code before starting the study:

```Java
UserzoomSDK.addCustomVar("{MyKey}", "{MyValue}");
```

> ***Note: Replace `{MyKey}` and `{MyValue}` with the correct values.***

Or use the JSON implementation:

```Java
UserzoomSDK.addCustomVar(new JSONObject("{\"{MyKey1}\":\"{MyValue1}\",\"{MyKey2}\":\"{MyValue2}\"}"));
```

>***⚠️ WARNING:  You need to call this method*** *before* ***the study starts, otherwise the information will not be sent to the server.***

If you want to delete all information you have already input, add the following line to your code before starting the study:

```Java
UserzoomSDK.clearCustomVars();
```

>***⚠️ WARNING:  You need to call this method*** *before* ***starting the study. If the study has already started, there is no way to clear that information.***


---
## Test the integration

To test the integration, clear the expiration data with `clearExpirationData` and set the log level to `LOG_LEVEL.VERBOSE`, right before calling the `init` method. Include this code in the application's main activity, within the `onCreate` method.

>**Example**
>```Java
>@Override
>protected void onCreate(Bundle savedInstanceState) {
>    super.onCreate(savedInstanceState);
>    // ...
>    UserzoomSDK.setDebugLevel(LOG_LEVEL.VERBOSE);
>    UserzoomSDK.clearExpirationData();
>    UserzoomSDK.init(this, "{YourAppTag}");
>    UserzoomSDK.show(this);
>    // ...
>}
>```

Replace `{YourAppTag}` with the correct tag, which can be provided by the study administrator.

Build and run the application to see if an intercept message displays on startup. The console should print some information logs at this stage.


Finally, let's test the integration with an Invitation Link. Clear the test integration code you just added, return to the `AndroidManifest.xml` of the project and change the URL Scheme added in Step 7 to `userzoomc4a79`. Once the application is running, **open this URL using the device browser**:

>[https://m.userzoom.com/m/NCBDNFMxOTcx](https://m.userzoom.com/m/NCBDNFMxOTcx)

Press 'Start Study' and verify the app opens as well as the used is asked to participate to the test study.

>***⚠️WARNING: Once this testing is done, make sure you revert the URL Scheme back to `{YourURLScheme}`.***

---
## ProGuard

Static analysis tools like ProGuard may consider that some of the classes used by UserZoomSDK are unused. In order to compile correctly the app with UserZoomSDK integrated, please add the following line to your `proguard-rules.pro` file:

```Java
-dontwarn sun.misc.Unsafe
```

---
## Troubleshoot

A. This library requires internet connection and full access to the userzoom.com domain to work properly.

B. If the intercept message is not displaying after testing the integration, use this test App Tag instead in order to perform a basic test:

```Java
UserzoomSDK.setDebugLevel(LOG_LEVEL.VERBOSE);
UserzoomSDK.clearExpirationData();
UserzoomSDK.init(this, "QzRBMzREMSAg"); 
UserzoomSDK.show(this);
```

If this works, please contact the study Administrator to correctly launch the study in the UserZoom tool. If, once launched, the issue persists, contact UserZoom Support Team at [support@userzoom.com][tickets] providing the following information:

* Confirmation that the test App Tag has been tested and it does not work.
* Confirmation that the logs were enabled, the expiration data was cleared and the issue is still persistent.
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

[tickets]: support@userzoom.com
[faq]: ./FAQ.md
[terms]: https://s.userzoom.com/documents/UserZoom_Terms_of_Service.pdf
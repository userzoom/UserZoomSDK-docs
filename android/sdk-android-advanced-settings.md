# Advanced settings

## Logging levels

Increase or decrease the amount of information displayed in logs by calling the `setDebugLevel` method with one of the following parameters just before the `init` or `show` call:

```Java
UserzoomSDK.setDebugLevel(LOG_LEVEL.SILENT);     // (Default) disable all logs
UserzoomSDK.setDebugLevel(LOG_LEVEL.ERROR);       // shows errors as well
UserzoomSDK.setDebugLevel(LOG_LEVEL.WARNING);   // shows warnings as well
UserzoomSDK.setDebugLevel(LOG_LEVEL.INFO);      // shows information
UserzoomSDK.setDebugLevel(LOG_LEVEL.VERBOSE);   // enable all logs
```

## Clear expiration data

To clear the information about previously completed studies, invoke the `clearExpirationData` method.

```Java
UserzoomSDK.clearExpirationData();
```

>***⚠️ WARNING: This method is intended for testing purposes only. Calling it on a released application can produce unexpected behaviors.***


## Invitation Link exclusive mode

Activate the Invitation Link exclusive mode, automatically closing the app unless the SDK is started from an Invitation Link. To do so, add the following code to the `onCreate` method of the main Activity:

```Java
UserzoomSDK.deactivateAppAfterStudy(this);
```

If the app is not opened through an Invitation Link, an alert message will be shown to users with the following information: _"Please go to the study email you received and tap on 'Start study' to begin. If you have already completed this study, you may uninstall the app."_. The app is closed automatically afterwards.

In order to customize this message, use the following method instead:

```Java
UserzoomSDK.deactivateAppAfterStudy(this, "Your custom message", "Ok");
```

## Pause screen recording

In order to interrupt and resume UserZoom from visually collecting views on certain areas of the App, use the `blockRecord` method.  

**To interrupt:**
```Java
UserzoomSDK.blockRecord(true);
```

**To resume:**

```Java
UserzoomSDK.blockRecord(false);
```

The most common use is to block a concrete Activity, invoking the method in the `onCreate` and `onStop` methods.

**Example**
```Java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    // ...
    UserzoomSDK.blockRecord(true);
    // ...
}

@Override
protected void onStop() {
    super.onStop();
    // ...
    UserzoomSDK.blockRecord(false);
    // ...
}
```

## Force study finalization

Finish a study at any time by calling the `finalizeStudy` method:

```Java
UserzoomSDK.finalizeStudy();
```

## Custom callback

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

## Custom intercept

UserZoom SDK automatically displays an _Intercept_ view when the study is initialized and ready, but also offers the option of building your own user interface in order to intercept participants.

To do so, set an **IShowCustomInterceptCallback** implementation to **UserzoomSDK** and show the custom view once `showCustomIntercept` is invoked.

**Example**
```Java
@Override
protected void onCreate(Bundle savedInscanteState) {
   UserzoomSDK.setShowCustomInterceptCallback(this);
}

@Override
public void showCustomIntercept(final IInterceptCallback interceptCallback) {
    /* 
        Show your custom intercept view here. If you want to launch a new Activity,
        we encourage to do so with 'startActivityForResult' and invoke the
        'interceptCallback' when activity results.
    */
    new AlertDialog.Builder(context)
    .setTitle("Hello!")
    .setMessage("Do you want to take a study?")
    .setPositiveButton(android.R.string.yes, new DialogInterface.OnClickListener() {
        public void onClick(DialogInterface dialog, int which) { 
            interceptCallback.accept();
        }
     })
    .setNegativeButton(android.R.string.no, new DialogInterface.OnClickListener() {
        public void onClick(DialogInterface dialog, int which) { 
            interceptCallback.cancel();
        }
     })
    .setIcon(android.R.drawable.ic_dialog_alert)
   .show();
}
```

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

## Custom notification icon

The UserZoom SDK can use Android's notifications. To modify the notification icon to be used, invoke the `setIconResourceNotification` method. By default, the icon displayed will be the App's icon.

```Java
UserzoomSDK.setIconResourceNotification(R.drawable.notification_icon);
```

## Custom variables

In order to be able to input additional information in run time, you need to use custom variables. To do so, add the following line to your code before starting the study:

```Java
UserzoomSDK.addCustomVar("{MyKey}", "{MyValue}");
```

> ***Note: Replace `{MyKey}` and `{MyValue}` with the correct values.***

Or use the JSON implementation:

```Java
UserzoomSDK.addCustomVar(new JSONObject("{\"{MyKey1}\":\"{MyValue1}\",\"{MyKey2}\":\"{MyValue2}\"}"));
```

>***⚠️ WARNING: You need to call this method*** *before* ***the study starts, otherwise the information will not be sent to the server.***

If you want to delete all information you have already input, add the following line to your code before starting the study:

```Java
UserzoomSDK.clearCustomVars();
```

>***⚠️ WARNING: You need to call this method*** *before* ***starting the study. If the study has already started, there is no way to clear that information.***

## Custom events

UserZoom SDK lets you register custom events by using the `sendEvent` method. You should provide up to three descriptive event strings.

```Java
    UserzoomSDK.sendEvent("{STRING1}", "{STRING2}", "{STRING3}");
```

>***⚠️ WARNING: Please do not use the tag `UZ` for `{STRING1}` since it is a reserved by the UserZoom SDK.***

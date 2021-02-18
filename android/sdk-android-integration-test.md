# Test the integration

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

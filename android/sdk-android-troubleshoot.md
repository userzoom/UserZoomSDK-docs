# Troubleshoot

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

[tickets]: support@userzoom.com
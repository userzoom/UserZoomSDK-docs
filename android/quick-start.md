# Quick Start

This section contains necessary steps to display a demo UserZoom survey within your application.

> **Note:** UserZoom SDK for Android requires API level 26 or greater.

## Installation

Add the following code to your **build.gradle** configuration.

**build.gradle**  
``` gradle
allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://raw.githubusercontent.com/userzoom/UserZoomSDK-Android/master" }
    }
}
```

---

**build.gradle (:app)**  
``` gradle
dependencies { 
    implementation 'com.userzoom:sdk:+' 
}
```

## Declare the UserZoom activity

**AndroidManifest.xml**
``` xml
<activity android:name="com.userzoom.sdk.presentation.UserzoomActivity"
          android:configChanges="orientation|keyboard|keyboardHidden|screenSize" />
```

## Launch the demo survey

Execute the following code anywhere in your app to launch the demo survey. It may for example be placed at the end of the `onCreate` method of the main activity.

``` java
UserzoomSDK.clearExpirationData(); // Use this method only when testing!
UserzoomSDK.show(this, "QzUzMUE5NEQx");
```

> **Note:** The first argument of the `show` method (`this` keyword in the example above) should be an instance of `android.app.Activity` class.

## Result

After the `show` method is called, you should see an intercepting pop-up like the one below:

<img 
    alt="UserZoom intercept example"
    src="/img/android-sdk-intercept.png"
    style="width: 50%; display:block; margin: auto;"
/>
# Quick Start

This document provides the necessary steps to integrate the UserZoom SDK into an Android project, presuming Android Studio is being used for the Android development.

> **Note: UserZoom SDK for Android requires API level 26 or greater.**

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

``` java
UserzoomSDK.clearExpirationData(); // Use this line only when testing
UserzoomSDK.show(this, "QzUzMUE5NEQx");
```

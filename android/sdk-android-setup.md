# Initial Setup

> *Note: This document provides the necessary steps to integrate the UserZoom SDK into an Android project, presuming Android Studio is being used for the Android development.*

# Adding UserZoom SDK to the project

## Update build.gradle inside the application module
Add the following code to build.gradle inside the app module that will be using the library published on GitHub Packages

>**build.gradle**  
>```gradle
> allprojects {
>    repositories {
>        google()
>        jcenter()
>        maven { url "https://maven.pkg.github.com/userzoom/UserZoomSDK-Android" }
>    }
> }
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


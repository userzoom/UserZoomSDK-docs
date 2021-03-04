# Old UserZoom SDK Android migration guide <!-- {docsify-ignore-all} -->

## Step 1. Remove the old UserzoomSDK library

Open your application project and delete the `UserzoomSDK.jar` on the app/libs folder.

## Step 2. Add the new UserZoom SDK

## Update build.gradle inside the application module
Add the following code to build.gradle inside the app module that will be using the library published on GitHub Packages

>**build.gradle**  
>```gradle
> allprojects {
>    repositories {
>        google()
>        jcenter()
>        maven { url "https://raw.githubusercontent.com/userzoom/UserZoomSDK-Android" }
>    }
> }
>```

---
>**app/build.gradle**  
>```gradle
>dependencies { implementation 'com.userzoom:sdk:+' }
>```


----

## Step 3. Run gradle sync
 Sync your project with the changes on the gradle files by pressing the **"Sync Project with Gradle Files"** button on the top right menu bar of Android Studio

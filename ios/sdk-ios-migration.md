# Old UserZoom SDK iOS migration guide <!-- {docsify-ignore-all} -->

## Step 1. Remove the old UserzoomSDK library

Open your application project and delete the `UserzoomSDK.framework` on the Frameworks folder.

## Step 2. Add the new UserZoom SDK

To integrate UserzoomSDK into your Xcode project using CocoaPods, specify it in your Podfile:

>```
>	pod 'UserzoomSDK', :git => 'git@github.com:userzoom/UserZoomSDK-iOS.git'
>```

## Step 3. Run pod install
 - Close your Xcode project
 - Open a terminal window
 - Go to your app directory
 - Run the command `pod install`

## Step 4. Open your app project

 - Open your application project via the .xcworkspace file
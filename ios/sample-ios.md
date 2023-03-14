# UserZoom SDK iOS sample application <!-- {docsify-ignore-all} -->

Here you will find a sample application with the UserZoom SDK integrated and a guide on how to set it up.

## Step 1. Download the sample app  

- You will find the app source code on this [link][sample-app-url]

## Step 2. Run pod install
- Open a terminal window
- Go to the sample app directory
- Run the command `pod install`

## Step 3. Open the sample project

- Open the sample application project via the `Wikipedia.xcworkspace` file

## Step 4. Add the study tags

- On Wikipedia/Code/AppDelegate.m substitute `"UZ_TAG_INIT"` with your study initial tag provided by your study administrator.

**AppDelegate.m**
```objective-c
 [UserzoomSDK initWithTag:@"UZ_TAG_INIT" options: launchOptions];
```

- (Optional) Add your study specific section tag on Wikipedia/Code/ArticleViewController.swift by substituting `"UZ_TAG_SPECIFIC"` for your specific section tag provided by your study administrator.

**ArticleViewController.swift**
```swift
 UserzoomSDK.show("UZ_TAG_SPECIFIC")
```

## Step 5. Add the study scheme

- Substitute `"UZ_SCHEME"` with your study url scheme provided by your study administrator.

![Setting up the URL scheme](../img/ios-demo-scheme.png)


## Step 6. Change the deployment target

- Change the deployment target to `UZWikipediaDemo`

![Where to find target location in XCode](../img/ios-demo-target-location.png)
![Target selection in XCode](../img/ios-demo-target-selection.png)

## Step 7. Run the app


[sample-app-url]: https://github.com/userzoom/UZWikipediaDemo-iOS

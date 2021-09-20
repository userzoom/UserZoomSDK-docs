# Frequently Asked Questions <!-- {docsify-ignore-all} -->

This is a list of common questions about Userzoom SDK. If you cannot find an answer for your question here, please contact our Support Team at <support@userzoom.com>.

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [What local notifications are you actually using and how does that work for the SDK?](#what-local-notifications-are-you-actually-using-and-how-does-that-work-for-the-sdk)
- [How are you able to track the position of the screen where the user did some action?](#how-are-you-able-to-track-the-position-of-the-screen-where-the-user-did-some-action)
- [Is it obligatory to register the camera and audio permissions in the `AppDelegate` file?](#is-it-obligatory-to-register-the-camera-and-audio-permissions-in-the-appdelegate-file)
- [How do you block specific sections of the app that contains sensible data? (e.g. sign in page)](#how-do-you-block-specific-sections-of-the-app-that-contains-sensible-data-eg-sign-in-page)
- [What's the purpose of the `finalizeStudy` method?](#whats-the-purpose-of-the-finalizestudy-method)
- [Do I need to call `FinalizeStudy()` before I can call `Show()` with a different tag?](#do-i-need-to-call-finalizestudy-before-i-can-call-show-with-a-different-tag)
- [Will `{YourAppTag}` change over time or is this a lifetime tag?](#will-yourapptag-change-over-time-or-is-this-a-lifetime-tag)
- [How does Userzoom capture data so there are no conflicts with other SDKS already integrated with our app?](#how-does-userzoom-capture-data-so-there-are-no-conflicts-with-other-sdks-already-integrated-with-our-app)
- [Is it possible to have more than one `{YourAppTag}` in the app?](#is-it-possible-to-have-more-than-one-yourapptag-in-the-app)
- [Will there be additional url schemes / deeplinking in the future?](#will-there-be-additional-url-schemes-deeplinking-in-the-future)
- [How do we validate that the backend is receiving data from the participant?](#how-do-we-validate-that-the-backend-is-receiving-data-from-the-participant)
- [When and where to send events with tags?](#when-and-where-to-send-events-with-tags)
- [What are custom vars used for?](#what-are-custom-vars-used-for)
- [When should we call `deactiveAppAfterStudy`?](#when-should-we-call-deactiveappafterstudy)
- [Should we always call `clearExpirationData`? If so when and where?](#should-we-always-call-clearexpirationdata-if-so-when-and-where)
- [What about logging levels in production?](#what-about-logging-levels-in-production)
- [What devices & OS versions are recommended for testing coverage?](#what-devices-os-versions-are-recommended-for-testing-coverage)
- [Is there any way that Userzoom could capture and pass data on which participant has completed the intercept survey back to the app?](#is-there-any-way-that-userzoom-could-capture-and-pass-data-on-which-participant-has-completed-the-intercept-survey-back-to-the-app)
- [Should the string passed to `initWithTag()` and `Show()` always be the same?](#should-the-string-passed-to-initwithtag-and-show-always-be-the-same)
- [How do I create a podfile?](#how-do-i-create-a-podfile)
- [How do I run a project with cocoapods?](#how-do-i-run-a-project-with-cocoapods)
- [How fix cocoapods could not find compatible versions for pod?](#how-fix-cocoapods-could-not-find-compatible-versions-for-pod)
<!-- /TOC -->

## What local notifications are you actually using and how does that work for the SDK?
Local notifications are only used in `Live Intercept` studies and they are only launched in case we configure that option for the study in Userzoom Manager. The message will only be shown when you have set up a Final Questionnaire and participants exit the app, to remind them to finish the study or cancel it.

We use the framework UserNotifications (https://developer.apple.com/documentation/usernotifications) and the permissions of notifications are only released when the condition of a study requires it.

## How are you able to track the position of the screen where the user did some action?
Basically, we get the touches and the gestures that the user makes on the device from the current window interface. So the coordinates are stored temporarily and sent in packets to our server, for displaying them later in the participant's video.

To capture these events we use the Swizzling method (http://nshipster.com/method-swizzling/) on the window by changing the implementation of an existing selector (in this case: `sendEvent`) at runtime with our own implementation.

## Is it obligatory to register the camera and audio permissions in the `AppDelegate` file?
It is not obligatory as long as you make sure that in all tasks from the study the video and audio are disabled. Otherwise, if these permissions are not added the app might crash. Our recommendation is to add it as part of the integration process.

## How do you block specific sections of the app that contains sensible data? (e.g. sign in page)
In order to interrupt and resume Userzoom from visually collecting views on certain areas of your app, we have the `blockRecord` method.  ([Section 7 - Readme file][section7]).

## What's the purpose of the `finalizeStudy` method?
It is optional and is used when you want to suddenly end the study (for example if the user reaches some specific Activity or some event happens during the study). In case it's not used, the study will be automatically close once it's finished. Take into account that if you use this function the participant that was interrupted will be marked as `Incomplete` in the study results.

## Do I need to call `FinalizeStudy()` before I can call `Show()` with a different tag?
No, if you use the `Show()` function in a different view where no study was called before, you can call it again with a different tag.

## Will `{YourAppTag}` change over time or is this a lifetime tag?
It will not change as long as you don't remove your app from the Mobile Apps Library in Userzoom Manager.

## How does Userzoom capture data so there are no conflicts with other SDKS already integrated with our app?
We swizzle the following methods:
* `sendEvent` from the `UIWindow` class. [More info](https://developer.apple.com/documentation/uikit/uiwindow/1621614-sendevent).
* `viewWillAppear` from the `UIViewController` class. [More info](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621510-viewwillappear)
* `viewDidAppear` from the `UIViewController` class. [More info](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621423-viewdidappear)
* `viewWillDisappear` from the `UIViewController` class. [More info](https://developer.apple.com/documentation/uikit/uiviewcontroller/1621485-viewwilldisappear)
* `show` from the `UIAlertView` class. [More info](https://developer.apple.com/documentation/uikit/uialertview/1620751-show)


## Is it possible to have more than one `{YourAppTag}` in the app?
Yes, you can but you have to make sure that both studies are completely isolated (there cannot be 2 studies at the same time).

## Will there be additional url schemes / deeplinking in the future?
Each app you want to integrate has a unique URL scheme that can be obtained from the Mobile apps library. So when you integrate our SDK with your app you only have to set up one URL Scheme, and this code will not be changed as long as you keep your app registered in the Mobile apps library.

## How do we validate that the backend is receiving data from the participant?
When you activate the logs in our SDK you can see if the data was sent or if there was any error. Also, when the participant completes the study the info will appear in the study results.

## When and where to send events with tags?
With the `sendEvent` method you can send additional events that are not automatically triggered (for example, when clicking on a specific button or doing some specific action, you can embed the sendEvent function there and send the event to Userzoom informing what button was clicked by adding it as a tag to the sendEvent method). This is very similar to using tags in any analytics tool like Google Analytics.

## What are custom vars used for?
They are used when you want to input additional information in run time (for example, some input field or data from the app). This data will then be available in the Results section in Userzoom Manager once the participant completes the study.

## When should we call `deactiveAppAfterStudy`?
This one is used in case you want to automatically close your app when the participant finishes the study (it’s optional).

## Should we always call `clearExpirationData`? If so when and where?
You should use this function when testing your integration. Once a participant completes a study, you can set up in the study settings from Userzoom Manager a expiration time (in days) so that user will not be asked again to take the study. If you use `clearExpirationData`, the app will not remember that you took the study and you will be prompted to take the same study again. It is similar to using cookies to store user's data. Usually, this function is only used when testing the integration to take the study as many times as you need.

## What about logging levels in production?
Our recommendation is to only use the logs within your testing environment to see how your app behaves with our SDK but not when releasing your app.

## What devices & OS versions are recommended for testing coverage?
Any device that runs at least the minimum supported version of iOS can run our SDK. To know which is the minimum supported version, check out our [help center article][requirements].

## Is there any way that Userzoom could capture and pass data on which participant has completed the intercept survey back to the app?
All results are only stored in Userzoom Manager and are not accessible from outside. The transfer of data between your app and Userzoom is only in one way (collect the participant's data), so it's not possible to receive the study results directly into the app.

## Should the string passed to `initWithTag()` and `Show()` always be the same?
No. The tag code that you have to add in the `initWithTag` function is the one that you get from the 'Manage SDK Integration' section in the Mobile Apps Library. This tag code is always linked with the study whose starting method is `Start App`. In case you don't want to start a study at the beginning but at some specific section, you can simply unlink the study or segment from this tag in the Mobile Apps Library and then after the init function declare the Show function with the corresponding tag.

## How do I create a podfile?
1. Install cocoapods if you don't have it already following [this guide](https://guides.cocoapods.org/using/getting-started.html#installation)
2. Open a terminal window
3. Go to your app directory
4. Run the command `pod init`

To add any cocoapod dependecies, open the new podfile created on your app directory with any text editor.

For more detailed information about how setup cocoapods and the podfile please take a look to their official docs (https://guides.cocoapods.org/using/using-cocoapods.html)


## How do I run a project with cocoapods?
To run the project follow this steps:
1. Open a terminal window
2. Go to your app directory
3. Run the command `pod install`
4. Open the application project via the new YourAppName.xcworkspace file (the white one, not the blue one that you usually use)

For more detailed information about how to use cocoapods please take a look to their official docs (https://guides.cocoapods.org/using/using-cocoapods.html)


## How fix CocoaPods could not find compatible versions for pod?

If when you run the command `pod install`, you are getting this fail: CocoaPods could not find compatible versions for pod "DatadogSDKObjC

Run the command `pod repo update` to fix it, and run the command `pod install` again.

For more detailed information about how to use cocoapods please take a look to their official docs (https://guides.cocoapods.org/using/using-cocoapods.html)


[requirements]: https://help.userzoom.com/hc/en-us/articles/360000660898
[section7]: https://userzoom.github.io/UserZoomSDK-docs/#/ios/sdk-ios-security

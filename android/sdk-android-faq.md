# Frequently Asked Questions <!-- {docsify-ignore-all} -->

This is a list of common questions about UserZoom SDK. If you cannot find an answer for your question here, please contact our Support Team at <support@userzoom.com>.

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [What local notifications are you actually using and how does that work for the SDK?](#what-local-notifications-are-you-actually-using-and-how-does-that-work-for-the-sdk)
- [How are you able to track the position of the screen where the user did some action?](#how-are-you-able-to-track-the-position-of-the-screen-where-the-user-did-some-action)
- [Is it obligatory to register the camera and audio permissions in the `AndroidManifest.xml` file?](#is-it-obligatory-to-register-the-camera-and-audio-permissions-in-the-androidmanifestxml-file)
- [How do you block specific sections of the app that contains sensible data? (e.g. sign in page)](#how-do-you-block-specific-sections-of-the-app-that-contains-sensible-data-eg-sign-in-page)
- [What's the purpose of the `finalizeStudy` method?](#whats-the-purpose-of-the-finalizestudy-method)
- [Do I need to call `FinalizeStudy()` before I can call `Show()` with a different tag?](#do-i-need-to-call-finalizestudy-before-i-can-call-show-with-a-different-tag)
- [Will `{YourAppTag}` change over time or is this a lifetime tag?](#will-yourapptag-change-over-time-or-is-this-a-lifetime-tag)
- [Is it possible to have more than one `{YourAppTag}` in the app?](#is-it-possible-to-have-more-than-one-yourapptag-in-the-app)
- [Will there be additional url schemes / deep linking in the future?](#will-there-be-additional-url-schemes-deeplinking-in-the-future)
- [How do we validate that the backend is receiving data from the participant?](#how-do-we-validate-that-the-backend-is-receiving-data-from-the-participant)
- [When and where to send events with tags?](#when-and-where-to-send-events-with-tags)
- [What are custom vars used for?](#what-are-custom-vars-used-for)
- [When should we call `deactiveAppAfterStudy`?](#when-should-we-call-deactiveappafterstudy)
- [Should we always call `clearExpirationData`? If so when and where?](#should-we-always-call-clearexpirationdata-if-so-when-and-where)
- [What about logging levels in production?](#what-about-logging-levels-in-production)
- [What devices & OS versions are recommended for testing coverage?](#what-devices-os-versions-are-recommended-for-testing-coverage)
- [Is there any way that UserZoom could capture and pass data on which participant has completed the intercept survey back to the app?](#is-there-any-way-that-userzoom-could-capture-and-pass-data-on-which-participant-has-completed-the-intercept-survey-back-to-the-app)
- [Should the string passed to `initWithTag()` and `Show()` always be the same?](#should-the-string-passed-to-initwithtag-and-show-always-be-the-same)
- [Is there an API reference for the SDK integration code?](#is-there-an-api-reference-for-the-sdk-integration-code)
- [I can't compile my project due to some ProGuard warnings](#i-can39t-compile-my-project-due-to-some-proguard-warnings)
<!-- /TOC -->

## What local notifications are you actually using and how does that work for the SDK?
Local notifications are only used in `Live Intercept` studies and they are only launched in case we configure that option for the study in UserZoom Manager. The message will only be shown when you have set up a Final Questionnaire and participants exit the app, to remind them to finish the study or cancel it.

## How are you able to track the position of the screen where the user did some action?
Basically, we get the touches and the gestures that the user makes on the device from the current window interface. So the coordinates are stored temporarily and sent in packets to our server, for displaying them later in the participant's video.

## Is it obligatory to register the camera and audio permissions in the `AndroidManifest.xml` file?
It is not obligatory as long as you make sure that in all tasks from the study the video and audio are disabled. Otherwise, if these permissions are not added the app might crash. Our recommendation is to add it as part of the integration process.

## How do you block specific sections of the app that contains sensible data? (e.g. sign in page)
In order to interrupt and resume UserZoom from visually collecting views on certain areas of your app, we have the `blockRecord` method.  ([documentation: pause screen record][blockRecord]).

## What's the purpose of the `finalizeStudy` method?
It is optional and is used when you want to suddenly end the study (for example if the user reaches some specific Activity or some event happens during the study). In case it's not used, the study will be automatically close once it's finished. Take into account that if you use this function the participant that was interrupted will be marked as `Incomplete` in the study results.

## Do I need to call `finalizeStudy()` before I can call `show()` with a different tag?
No, if you use the `Show()` function in a different view where no study was called before, you can call it again with a different tag.

## Will `{YourAppTag}` change over time or is this a lifetime tag?
It will not change as long as you don't remove your app from the Mobile Apps Library in UserZoom Manager.

## Is it possible to have more than one `{YourAppTag}` in the app?
Yes, you can but you have to make sure that both studies are completely isolated (there cannot be 2 studies at the same time).

## Will there be additional URL schemes / deep linking in the future?
Each app you want to integrate has a unique URL scheme that can be obtained from the Mobile apps library. So when you integrate our SDK with your app you only have to set up one URL Scheme, and this code will not be changed as long as you keep your app registered in the Mobile apps library.

## How do we validate that the backend is receiving data from the participant?
When you activate the logs in our SDK you can see if the data was sent or if there was any error. Also, when the participant completes the study the info will appear in the study results.

## When and where to send custom events?
With the `sendEvent` method you can send additional events that are not automatically triggered (for example, when clicking on a specific button or doing some specific action, you can embed the sendEvent function there and send the event to UserZoom informing what button was clicked by adding it as a tag to the sendEvent method). This is very similar to using tags in any analytics tool like Google Analytics.

## What are custom variables used for?
They are used when you want to input additional information in run time (for example, some input field or data from the app). This data will then be available in the Results section in UserZoom Manager once the participant completes the study.

## When should we call `deactiveAppAfterStudy`?
The `deactiveAppAfterStudy` method enables the Invitation Link  exclusive mode. It prevents the users from using the application unless it's been open through an Invitation Link. This method is meant to be used for example when you wish to distribute a non-live version of your app and you want to ensure that it'll be used only during testing with UserZoom.

## Should we always call `clearExpirationData`? If so when and where?
You should use this function when testing your integration. Once a participant completes a study, you can set up in the study settings from UserZoom Manager a expiration time (in days) so that user will not be asked again to take the study. If you use `clearExpirationData`, the app will not remember that you took the study and you will be prompted to take the same study again. It is similar to using cookies to store user's data. Usually, this function is only used when testing the integration to take the study as many times as you need.

## What about logging levels in production?
Our recommendation is to only use the logs within your testing environment to see how your app behaves with our SDK but not when releasing your app.

## What devices & OS versions are recommended for testing coverage?
Any device that runs at least the minimum supported version of Android can run our SDK. To know which is the minimum supported version, check out our [help center article][requirements].

## Is there any way that UserZoom could capture and pass data on which participant has completed the intercept survey back to the app?
All results are only stored in UseZoom Manager and are not accessible from outside. The transfer of data between your app and UserZoom is only in one way (collect the participant's data), so it's not possible to receive the study results directly into the app.

## Should the string passed to `initWithTag()` and `show()` always be the same?
No. The tag code that you have to add in the `initWithTag` function is the one that you get from the 'Manage SDK Integration' section in the Mobile Apps Library. This tag code is always linked with the study whose starting method is `Start App`. In case you don't want to start a study at the beginning but at some specific section, you can simply unlink the study or segment from this tag in the Mobile Apps Library and then after the init function declare the Show function with the corresponding tag.

## I can't compile my project due to some ProGuard warnings

Static analysis tools like ProGuard may consider that some of the classes used by UserZoomSDK are unused. In order to compile correctly the app with UserZoomSDK integrated, please add the following line to your `proguard-rules.pro` file:

```Java
-dontwarn sun.misc.Unsafe
```



[requirements]: https://help.userzoom.com/hc/en-us/articles/360000660898
[blockRecord]: https://userzoom.github.io/UserZoomSDK-docs/#/android/sdk-android-advanced-settings?id=pause-screen-recording

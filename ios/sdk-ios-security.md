# Security

In order to interrupt and resume UserZoom from visually collecting views on certain areas of the App, use the `blockRecord` method. 

***To interrupt:***
```swift
UserzoomSDK.blockRecord(true);
```

***To resume:***
```swift
UserzoomSDK.blockRecord(false);
```

The most common use is to block a concrete ViewController, implementing the `viewWillAppear` and `viewDidDisappear` methods:

>**Example**
>```swift
>override func viewWillAppear(_ animated: Bool) {
>   super.viewWillAppear(animated);
>   UserzoomSDK.blockRecord(true);
>}
>
>override func viewDidDisappear(_ animated: Bool) {
>    super.viewDidDisappear(animated);
>    UserzoomSDK.blockRecord(false);
>}
>```

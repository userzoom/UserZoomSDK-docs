# Security

In order to interrupt and resume UserZoom from visually collecting views on certain areas of the App, use the `blockRecord` method.  

***To interrupt:***
```Java
UserzoomSDK.blockRecord(true);
```

***To resume:***

```Java
UserzoomSDK.blockRecord(false);
```

The most common use is to block a concrete Activity, invoking the method in the `onCreate` and `onStop` methods.

> **Example**
>```Java
>@Override
>protected void onCreate(Bundle savedInstanceState) {
>    super.onCreate(savedInstanceState);
>    // ...
>    UserzoomSDK.blockRecord(true);
>    // ...
>}
>
>@Override
>protected void onStop() {
>    super.onStop();
>    // ...
>    UserzoomSDK.blockRecord(false);
>    // ...
>}
>```


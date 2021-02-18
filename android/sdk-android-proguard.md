# ProGuard

Static analysis tools like ProGuard may consider that some of the classes used by UserZoomSDK are unused. In order to compile correctly the app with UserZoomSDK integrated, please add the following line to your `proguard-rules.pro` file:

```Java
-dontwarn sun.misc.Unsafe
```

# Tag events

Just like other Analytic tools, in order to tag events correctly, the `sendEvent` method needs to be used, being able to send up to three tags at the same time.

```Java
    UserzoomSDK.sendEvent("{TAG1}", "{TAG2}", "{TAG3}");
```

To do so, replace `{TAG1}`, `{TAG2}` and `{TAG3}` with the content of the tags to be sent.

>***⚠️ WARNING: Please do not use the tag `UZ` for `{TAG1}` since it is a reserved TAG for UserZoom.***

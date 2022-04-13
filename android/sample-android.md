# UserZoom SDK android sample application <!-- {docsify-ignore-all} -->

Here you will find a sample application with the UserZoom SDK integrated and a guide on how to set it up.

> #### Step 1. Download the sample app  
> You will find the app source code on this [link][sample-app-url]
>
> #### Step 2. Setup your GitHub personal access token
> - Follow [this guide][git-token-creation] to setup your GitHub personal access token
> - Create a GitHub properties file [(here's how)][git-token-file]
>
> #### Step 3. Open the sample project
> - Open the sample application project
>
> #### Step 4. Add the study tags
>
> - On app/src/main/java/org/wikipedia/main/MainActivity.java substitute `"UZ_TAG_INIT"` with your study initial tag provided by your study administrator.
>
>>**MainActivity.java**
>>```java
>> UserzoomSDK.init(this,"UZ_TAG_INIT");
>>```
>
> - (Optional) Add your study specific section tag on app/src/main/java/org/wikipedia/page/PageActivity.java by substituting `"UZ_TAG_SPECIFIC"` for your specific section tag provided by your study administrator.
>
>>**PageActivity.java**
>>```java
>> UserzoomSDK.show(this,"UZ_TAG_SPECIFIC");
>>```
>
> #### Step 5. Add the study scheme
>
> - On app/src/main/AndroidManifest.xml substitute `"UZ_SCHEME"` with your study url scheme provided by your study administrator.
>
>> **AndroidManifest.xml**
>> ```xml
>> <data android:scheme="UZ_SCHEME" />
>>```
>
> #### Step 6. Run the app
>

[sample-app-url]: https://github.com/userzoom/UZWikipediaDemo-Android
[git-token-creation]: android/sdk-android-setup?id=step-1-generate-a-personal-access-token-for-github
[git-token-file]:android/sdk-android-setup?id=step-2-store-your-github-personal-access-token-details

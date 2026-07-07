Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy/strategies "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy/strategies "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/privacy/strategies "View this page for the Unity platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy/strategies "View this page for the Android (Legacy) platform docs.")

<br />

For key considerations when preparing your app for Google Play and Android
privacy changes, see

[Privacy strategies for Android](https://support.google.com/admob/answer/11402075).


## Publisher first-party ID

The
GMA Next-Gen SDK
introduced
[Publisher first-party ID](https://support.google.com/admob/answer/11402075#publisher-first-party-id),
to help you deliver more relevant and personalized ads by using data collected
from your apps.

Publisher first-party ID is enabled by default, but you can disable it using the
following method.

### Kotlin

    // Disables Publisher first-party ID.
    MobileAds.putPublisherFirstPartyIdEnabled(false)

### Java

    // Disables Publisher first-party ID.
    MobileAds.putPublisherFirstPartyIdEnabled(false);

<br />
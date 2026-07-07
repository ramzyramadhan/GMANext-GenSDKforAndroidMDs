Select platform: [Android](https://developers.google.com/admob/android/next-gen/ad-inspector/launch-ad-inspector "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/ad-inspector/launch-ad-inspector "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/ad-inspector/launch-ad-inspector "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/ad-inspector/launch-ad-inspector "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/ad-inspector/launch-ad-inspector "View this page for the Android (Legacy) platform docs.")

<br />

[Video](https://www.youtube.com/watch?v=seWbUbptcUI)

Before you test your ad integration, you must launch ad inspector in your app.
This page covers how to launch ad inspector
using gestures
and how to launch
programmatically.

## Prerequisites

Before you continue, do the following:

- Complete all items in the initial [Prerequisites](https://developers.google.com/admob/android/next-gen/ad-inspector#prerequisites) to create an AdMob account, set your test device, initialize GMA Next-Gen SDK, and install the latest version.

## Choose a launch option

You can launch ad inspector in the following ways:

- Use the gesture you selected in the AdMob UI after registering a test device. For details, see [Set up a test device](https://support.google.com/admob/answer/9691433).
- Programmatically through the GMA Next-Gen SDK.

### Launch using gestures

To launch ad inspector with a gesture, perform the gesture, such as a double
flick or shake, that you configured in AdMob UI for your test
device. For more details, see
[Test your app with ad inspector](https://support.google.com/admob/answer/10159602).

After you set a gesture in the AdMob UI, allow time to propagate. Make an ad
request through the GMA Next-Gen SDK to register your
gesture setting with your test device. If performing your gesture fails to
open in ad inspector, try to load an ad, restart your app, and test the gesture
again.

### Launch programmatically

Launch ad inspector by running the following:

### Java

    MobileAds.openAdInspector(context, new OnAdInspectorClosedListener() {
      public void onAdInspectorClosed(@Nullable AdInspectorError error) {
        // Error will be non-null if ad inspector closed due to an error.
      }
    });

### Kotlin

    MobileAds.openAdInspector(context) { error ->
      // Error will be non-null if ad inspector closed due to an error.
    }

This method works for test devices registered
programmatically or in the AdMob UI.
For more details, see
[Enable test devices](https://developers.google.com/admob/android/next-gen/test-ads#enable_test_devices).
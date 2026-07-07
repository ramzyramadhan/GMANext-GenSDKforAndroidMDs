Select platform: [Android](https://developers.google.com/admob/android/next-gen/ad-inspector "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/ad-inspector "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/ad-inspector "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/ad-inspector "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/ad-inspector "View this page for the Android (Legacy) platform docs.")

<br />

Ad inspector is an in-app overlay that lets you perform real-time analysis of
test ad requests directly within a mobile app. For a list of ad inspector
features, see
[Test your app with ad inspector](https://support.google.com/admob/answer/10159602).

> [!WARNING]
> **Warning:** Enabling ad inspector increases memory usage of the GMA Next-Gen SDK for test devices.

This guide covers how to perform the following:

- [Launch ad inspector](https://developers.google.com/admob/android/next-gen/ad-inspector/launch-ad-inspector)
- [Verify adapter integrations](https://developers.google.com/admob/android/next-gen/ad-inspector/verify-adapter-integrations)
- [Test ad units](https://developers.google.com/admob/android/next-gen/ad-inspector/test-ad-units)
- [Troubleshoot ad units](https://developers.google.com/admob/android/next-gen/ad-inspector/troubleshoot-ad-units)
- [Troubleshoot privacy settings](https://developers.google.com/admob/android/next-gen/ad-inspector/troubleshoot-privacy-settings)

### Prerequisites

Before you can use ad inspector, you must complete the following tasks:

- Create an AdMob account. For more details, see [Sign up for AdMob](https://support.google.com/admob/answer/7356219).
- [Set up an app in AdMob](https://support.google.com/admob/answer/9989980).
- [Set up GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start).
- Install GMA Next-Gen SDK version 0.9.0-alpha01 or higher. To view the latest version, see [Release notes](https://developers.google.com/admob/android/next-gen/rel-notes).
- Add your device as a test device. To add a test device, see
  [Enable test devices](https://developers.google.com/admob/android/next-gen/test-ads#enable_test_devices)

  > [!IMPORTANT]
  > **Important:** Ad inspector launches only on *test devices*.
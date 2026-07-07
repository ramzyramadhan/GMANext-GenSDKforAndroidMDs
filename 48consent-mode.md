Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy/consent-mode "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy/consent-mode "View this page for the iOS platform docs.") [Flutter](https://developers.google.com/admob/flutter/privacy/consent-mode "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy/consent-mode "View this page for the Android (Legacy) platform docs.")

<br />

The UMP SDK can interpret GDPR consent choices for Google

[Consent Mode](https://support.google.com/admob/answer/10114014#consentmode).

The UMP SDK interprets the following purposes:

- ad storage
- ad personalization
- ad user data
- analytics storage

Interpreting consent mode values is useful for apps that get consent for the use
of your products.

## Before you begin

- Make sure you are using the [Google Analytics for Firebase SDK](https://firebase.google.com/docs/analytics) in your app.
- Choose [Basic versus advanced consent mode](https://developers.google.com/tag-platform/security/concepts/consent-mode#basic-vs-advanced). Google supports advanced mode by default, while basic mode has additional steps.

## Get started

1. Turn on consent mode in the AdMob UI. For more details, see

   [Manage consent mode settings](https://support.google.com/admob/answer/16053245).

2. Use UMP SDK version 3.2.0 or higher.

3. Initialize Firebase before starting the UMP SDK. For more details, see

   [Add the Analytics SDK to your app](https://firebase.google.com/docs/analytics/get-started?platform=android#add-sdk).

## Set default consent state

The UMP SDK updates the consent state when European Union (EU) regulations
apply to the user.

[Set the default consent state](https://developers.google.com/tag-platform/security/guides/app-consent?consentmode=basic&platform=android#default-consent)

when EU regulations don't apply to the user.

## Use basic mode

To use basic mode, see [Consent mode basic mode support](https://developers.google.com/funding-choices/fc-sdks#consent_mode_basic_mode_support).
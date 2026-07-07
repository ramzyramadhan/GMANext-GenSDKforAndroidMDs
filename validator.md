Select platform: [Android](https://developers.google.com/admob/android/next-gen/native/validator "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/native/validator "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/native/validator "View this page for the Android (Legacy) platform docs.")

<br />

Native ads lets you design an ad placement that matches the style of your app. While this provides a lot of flexibility, it's important to ensure your placements remain compliant with AdMob policies.

<br />

Native Validator is a new feature to help you catch policy violations before
your app ships. It automatically identifies certain policy violations in your
app and notifies you through the app's UI.

> [!IMPORTANT]
> **Key Point:** Native Validator will appear only on test ads. Live ad requests will never show Native Validator.

Native Validator is enabled by default for test ads, but can be disabled as
shown below. Keep in mind however that once the validator is disabled, test ads
will no longer show information about potential issues with your ad layouts.

## Prerequisites

Before you continue, do the following:

- Install GMA Next-Gen SDK version `1.0.0` or later.

<!-- -->

- Ensure your device is configured as a [test
  device](https://developers.google.com/admob/android/next-gen/test-ads#enable_test_devices).

## Using Native Validator

Native Validator automatically alerts you of certain policy violations in your
UI through an overlay popup next to the ad.

![](https://developers.google.com/static/admob/images/validator/admob-overlay.png)

Clicking on **See Issues** takes you to a fullscreen list of the relevant policy
violations.

![](https://developers.google.com/static/admob/images/validator/implementation-issues.png)

## Disabling the validator

To disable Native Validator, call `setNativeValidatorDisabled()` when
initializing GMA Next-Gen SDK:

### Kotlin

    MobileAds.initialize(
      this@MainActivity,
      // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
      InitializationConfig.Builder("SAMPLE_APP_ID")
        .setNativeValidatorDisabled()
        .build()
      ) {
        // Adapter initialization is complete.
    }

### Java

    MobileAds.initialize(
        this,
        // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
        new InitializationConfig.Builder("SAMPLE_APP_ID")
            .setNativeValidatorDisabled()
            .build(),
        initializationStatus -> {
            // Adapter initialization is complete.
        });
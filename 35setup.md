Select platform: [Android](https://developers.google.com/admob/android/next-gen/custom-events/setup "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/custom-events/setup "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/custom-events "View this page for the Unity platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/custom-events/setup "View this page for the Android (Legacy) platform docs.")

<br />

Custom events let you add waterfall mediation for an ad network that isn't a
[supported ad network](https://developers.google.com/admob/android/next-gen/choose-networks). You do this by implementing
a custom event adapter for the ad network you want to integrate.

You can find a full sample custom event project in our [GitHub
repo](https://github.com/googleads/googleads-mobile-android-mediation).

## Prerequisites

Before you can create custom events, you must first integrate one of the
following ad format into your app:

- [Banner](https://developers.google.com/admob/android/next-gen/banner)
- [Interstitial](https://developers.google.com/admob/android/next-gen/interstitial)
- [Native](https://developers.google.com/admob/android/next-gen/native)
- [Rewarded](https://developers.google.com/admob/android/next-gen/rewarded)

## Create a custom event in the UI

A custom event must first be created in the AdMob
UI. See the instructions in

[Add a custom event](https://support.google.com/admob/answer/3083407).


You need to supply the following:

Class Name

:   The fully-qualified name of the class that implements the custom event
    adapter---for example,

    `com.google.ads.mediation.sample.customevent.SampleCustomEvent`. As a best
    practice, we recommend using a single adapter class for all custom event ad
    formats.

Label

:   A unique name defining the ad source.

Parameter

:   An optional string argument passed to your custom event adapter.

## Initialize the adapter

When GMA Next-Gen SDK initializes,

`initialize()`

is invoked on all supported third-party adapters and custom events configured
for the app within the AdMob UI. Use this method to
perform any necessary setup or initialization on the required third-party SDK
for your custom event.

### Java

    package com.google.ads.mediation.sample.customevent;

    import com.google.android.gms.ads.mediation.Adapter;
    import com.google.android.gms.ads.mediation.InitializationCompleteCallback;
    import com.google.android.gms.ads.mediation.MediationConfiguration;

    public class SampleAdNetworkCustomEvent extends Adapter {
      private static final String SAMPLE_AD_UNIT_KEY = "parameter";

      @Override
      public void initialize(Context context,
          InitializationCompleteCallback initializationCompleteCallback,
          List<MediationConfiguration> mediationConfigurations) {
        // This is where you will initialize the SDK that this custom
        // event is built for. Upon finishing the SDK initialization,
        // call the completion handler with success.
        initializationCompleteCallback.onInitializationSucceeded();
      }
    }

### Kotlin

    package com.google.ads.mediation.sample.customevent

    import com.google.android.gms.ads.mediation.Adapter
    import com.google.android.gms.ads.mediation.InitializationCompleteCallback
    import com.google.android.gms.ads.mediation.MediationConfiguration

    class SampleCustomEvent : Adapter() {
      private val SAMPLE_AD_UNIT_KEY = "parameter"

      override fun initialize(
        context: Context,
        initializationCompleteCallback: InitializationCompleteCallback,
        mediationConfigurations: List<MediationConfiguration>
      ) {
        // This is where you will initialize the SDK that this custom
        // event is built for. Upon finishing the SDK initialization,
        // call the completion handler with success.
        initializationCompleteCallback.onInitializationSucceeded()
      }
    }

## Report version numbers

All custom events must report to GMA Next-Gen SDK both the version of
the custom event adapter itself and the version of the third-party SDK the
custom event interfaces with. Versions are reported as

[`VersionInfo`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/VersionInfo)

objects:

### Java

    package com.google.ads.mediation.sample.customevent;

    public class SampleCustomEvent extends Adapter {

      @Override
      public VersionInfo getVersionInfo() {
        String versionString = new VersionInfo(1, 2, 3);
        String[] splits = versionString.split("\\.");

        if (splits.length >= 4) {
          int major = Integer.parseInt(splits[0]);
          int minor = Integer.parseInt(splits[1]);
          int micro = Integer.parseInt(splits[2]) * 100 + Integer.parseInt(splits[3]);
          return new VersionInfo(major, minor, micro);
        }

        return new VersionInfo(0, 0, 0);
      }

      @Override
      public VersionInfo getSDKVersionInfo() {
        String versionString = SampleAdRequest.getSDKVersion();
        String[] splits = versionString.split("\\.");

        if (splits.length >= 3) {
          int major = Integer.parseInt(splits[0]);
          int minor = Integer.parseInt(splits[1]);
          int micro = Integer.parseInt(splits[2]);
          return new VersionInfo(major, minor, micro);
        }

        return new VersionInfo(0, 0, 0);
      }
    }

### Kotlin

    package com.google.ads.mediation.sample.customevent

    class SampleCustomEvent : Adapter() {
      override fun getVersionInfo(): VersionInfo {
        val versionString = VersionInfo(1,2,3).toString()
        val splits: List<String> = versionString.split("\\.")

        if (splits.count() >= 4) {
          val major = splits[0].toInt()
          val minor = splits[1].toInt()
          val micro = (splits[2].toInt() * 100) + splits[3].toInt()
          return VersionInfo(major, minor, micro)
        }

        return VersionInfo(0, 0, 0)
      }

      override fun getSDKVersionInfo(): VersionInfo {
        val versionString = VersionInfo(1,2,3).toString()
        val splits: List<String> = versionString.split("\\.")

        if (splits.count() >= 3) {
          val major = splits[0].toInt()
          val minor = splits[1].toInt()
          val micro = splits[2].toInt()
          return VersionInfo(major, minor, micro)
        }

        return VersionInfo(0, 0, 0)
      }
    }

## Request ad

To request an ad, refer to the instructions specific to the ad format:

- [Banner](https://developers.google.com/admob/android/next-gen/custom-events/banner)
- [Interstitial](https://developers.google.com/admob/android/next-gen/custom-events/interstitial)
- [Native](https://developers.google.com/admob/android/next-gen/custom-events/native)
- [Rewarded](https://developers.google.com/admob/android/next-gen/custom-events/rewarded)
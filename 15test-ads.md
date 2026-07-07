Select platform: [Android](https://developers.google.com/admob/android/next-gen/test-ads "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/test-ads "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/test-ads "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/test-ads "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/test-ads "View this page for the Android (Legacy) platform docs.")

<br />


This guide explains how to enable test ads in your ads integration. It's
important to enable test ads during development so that you can click them
without charging Google advertisers. If you click too many ads without being
in test mode, you risk your account being flagged for invalid activity.

There are two ways to get test ads:

1. Use one of Google's [demo ad units](https://developers.google.com/admob/android/next-gen/test-ads#demo_ad_units).
2. Use your own ad unit and [enable test devices](https://developers.google.com/admob/android/next-gen/test-ads#enable_test_devices).

## Prerequisite

Before you continue, [set up GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start).

## Demo ad units

The quickest way to enable testing is to use Google-provided demo ad units.
These ad units are not associated with your AdMob
account, so there's no risk of your account generating invalid traffic when
using these ad units.

> [!WARNING]
> **Warning:** Before publishing your app, make sure that you replace these IDs with your ad unit ID. If you've published your app, [enable test devices](https://developers.google.com/admob/android/next-gen/test-ads#enable_test_devices).

Here are demo ad units that point to specific test creatives for each format:

| Ad format | Demo ad unit ID |
|---|---|
| [App Open](https://developers.google.com/admob/android/next-gen/app-open) | `ca-app-pub-3940256099942544/9257395921` |
| [Achored Adaptive Banner](https://developers.google.com/admob/android/next-gen/banner) | `ca-app-pub-3940256099942544/9214589741` |
| [Inline Adaptive Banner](https://developers.google.com/admob/android/next-gen/banner/inline-adaptive) | `ca-app-pub-3940256099942544/9214589741` |
| [Fixed Size Banner](https://developers.google.com/admob/android/next-gen/banner/fixed-size) | `ca-app-pub-3940256099942544/6300978111` |
| [Interstitial](https://developers.google.com/admob/android/next-gen/interstitial) | `ca-app-pub-3940256099942544/1033173712` |
| [Rewarded Ads](https://developers.google.com/admob/android/next-gen/rewarded) | `ca-app-pub-3940256099942544/5224354917` |
| [Rewarded Interstitial](https://developers.google.com/admob/android/next-gen/rewarded-interstitial) | `ca-app-pub-3940256099942544/5354046379` |
| [Native](https://developers.google.com/admob/android/next-gen/native) | `ca-app-pub-3940256099942544/2247696110` |
| [Native Video](https://developers.google.com/admob/android/next-gen/native/video-ads) | `ca-app-pub-3940256099942544/1044960115` |

## Enable test devices

If you want to do more rigorous testing with production-looking ads, you can
now configure your device as a test device and use your own ad unit IDs that
you've created in the AdMob UI.

Test devices can either be added in the AdMob UI or programmatically using
GMA Next-Gen SDK.


Follow the steps below to add your device as a test device.

> [!IMPORTANT]
> **Key Point:** Android emulators are automatically configured as test devices.

### Add your test device in the AdMob UI

For a non-programmatic way to add a test device and test new or existing
app builds, use the AdMob UI. [Learn
how](https://support.google.com/admob/answer/9691433).

> [!IMPORTANT]
> **Key Point:** New test devices typically start serving test ads in your app within 15 minutes, but it can also take up to 24 hours.

### Add your test device programmatically

To register your test device, complete the following steps:

1. Load your ads-integrated app and make an ad request.
2. Check the logcat output for a message similar to the following, which shows you your device ID and how to add it as a test device:

   ```
   I/Ads: Use RequestConfiguration.Builder.setTestDeviceIds(Arrays.asList("33BE2250B43518CCDA7DE426D04EE231"))
   to get test ads on this device."
   ```
   Copy your test device ID to your clipboard.
3. Modify your code to call

   [`RequestConfiguration.Builder.setTestDeviceIds()`](https://developers.google.com/admob/android/early-access/nextgen/reference/kotlin/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration.Builder#setTestDeviceIds(kotlin.collections.List))

   and pass in a list of your test device IDs.

   ### Java

       List<String> testDeviceIds = Arrays.asList("33BE2250B43518CCDA7DE426D04EE231");
       RequestConfiguration configuration =
       new RequestConfiguration.Builder().setTestDeviceIds(testDeviceIds).build();
       MobileAds.setRequestConfiguration(configuration);

   ### Kotlin

       val testDeviceIds = Arrays.asList("33BE2250B43518CCDA7DE426D04EE231")
       val configuration = RequestConfiguration.Builder().setTestDeviceIds(testDeviceIds).build()
       MobileAds.setRequestConfiguration(configuration)

   You can optionally check

   [`isTestDevice()`](https://developers.google.com/admob/android/early-access/nextgen/reference/kotlin/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration#isTestDevice(android.content.Context))

   to confirm that your device was properly added as a test device.

   > [!CAUTION]
   > **Caution:** Be sure to remove the code that sets these test device IDs before you release your app.

4. Re-run your app. If the ad is a Google ad, you'll see a **Test Ad** label
   centered at the top of the ad (banner, interstitial, or rewarded video):

   ![](https://developers.google.com/static/admob/images/android-testad-0-admob.png)

   For native advanced ads, the headline asset is prepended with the string **Test Ad**.

   ![](https://developers.google.com/static/admob/images/native-testad-android.png)

Ads with this **Test Ad** label are safe to click. Requests, impressions, and
clicks on test ads won't show up in your account's reports.

> [!NOTE]
> **Note:** Mediated ads do *NOT* render a **Test Ad** label. See the following section for details.

## Testing with mediation

Google's sample ad units only show Google Ads. To test your AdMob Mediation
configuration, you must use the [enable test devices](https://developers.google.com/admob/android/next-gen/test-ads#enable_test_devices)
approach.

Mediated ads do *NOT* render a Test Ad label. You are responsible for
ensuring that test ads are enabled for each of your mediation networks so these
networks don't flag your account for invalid activity. See each network's
respective [mediation guide](https://developers.google.com/admob/android/next-gen/mediation)
for more information.

If you aren't sure whether a mediation ad network adapter supports test ads, it
is safest to avoid clicking on ads from that network during development. You
can use the

[`getMediationAdapterClassName()`](https://developers.google.com/admob/android/early-access/nextgen/reference/kotlin/com/google/android/libraries/ads/mobile/sdk/common/ResponseInfo#getMediationAdapterClassName())

method on any of the ad formats to figure out which ad network served the
current ad.
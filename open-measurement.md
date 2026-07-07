Select platform: [Android](https://developers.google.com/admob/android/next-gen/open-measurement "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/open-measurement "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/open-measurement "View this page for the Android (Legacy) platform docs.")

<br />

Open Measurement is an IAB standard that allows publishers to use third-party
viewability providers to verify impressions and click measurements.
GMA Next-Gen SDK

integrates with the
[Open Measurement (OM) SDK](https://iabtechlab.com/standards/open-measurement-sdk/)
to enable third-party viewability measurement.

GMA Next-Gen SDK supports OM SDK version 1.4.

## Prerequisites

- [Set up GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start).

## Implement Open Measurement

GMA Next-Gen SDK automatically implements Open Measurement using
**"Google"** as the Open Measurement partner name for all ads served using the
GMA Next-Gen SDK.

To use a third-party viewability provider, configure it in the
AdMob UI, and configure your line items to use that viewability
provider. For more details, see [Configure a mobile app viewability provider](https://support.google.com/admanager/answer/9025968).

## Ensure that transparent overlays are non-obstructing

For an ad to not be considered blocked, the view that is obscuring the ad must
have one of these settings:

- `alpha = 0`, or,

<!-- -->

- `visibility = View.GONE` or `visibility = View.INVISIBLE`

It doesn't matter if the obscuring view has a transparent background, the
view's alpha and visibility values are what determine whether the view is
blocking your ad.

If the Open Measurement SDK detects an obstruction over the ad, it could impact
whether a viewability provider considers the impression viewable. To fix this,
set your view's alpha to `0` or

set the visibility to `View.GONE` or `View.INVISIBLE`.


## Troubleshooting

Be aware of the following when implementing Open Measurement:

- No support for [AdMob campaigns](https://support.google.com/admob/answer/6162747).

<!-- -->

- You must check with the mediation partner to learn if they support Open
  Measurement for ads they render.

- Ads that are obscured by overlaying views might not register viewability
  measurements. For more information, refer to [Ensure that transparent overlays
  are non-obstructing](https://developers.google.com/admob/android/next-gen/open-measurement#obstructing).

- On [test devices](https://developers.google.com/admob/android/next-gen/test-ads#enable_test_devices), including the
  Android simulator, the **Test Ad** label is detected as non-obstructing
  to the ad view.
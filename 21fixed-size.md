Select platform: [Android](https://developers.google.com/admob/android/next-gen/banner/fixed-size "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/banner/fixed-size "View this page for the iOS platform docs.") [Flutter](https://developers.google.com/admob/flutter/banner/fixed-size "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/banner/fixed-size "View this page for the Android (Legacy) platform docs.")

<br />

The GMA Next-Gen SDK supports fixed ad sizes for situations where adaptive
banners ads don't meet your needs.

The following table lists the standard banner sizes.

| Size in dp (WxH) | Description | Availability | AdSize constant |
|---|---|---|---|
| 320x50 | Banner | Phones and tablets | `https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/banner/AdSize#BANNER()` |
| 320x100 | Large banner | Phones and tablets | `https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/banner/AdSize#LARGE_BANNER()` |
| 300x250 | IAB medium rectangle | Phones and tablets | `https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/banner/AdSize#MEDIUM_RECTANGLE()` |
| 468x60 | IAB full-size banner | Tablets | `https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/banner/AdSize#FULL_BANNER()` |
| 728x90 | IAB leaderboard | Tablets | `https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/banner/AdSize#LEADERBOARD()` |

The size of the container in which you place your ad must be at least as big as
the banner. Any padding effectively decreases the size of your container. If the
container cannot fit the banner ad, the ad isn't shown and the following
warning is logged:

    W/Ads: Not enough space to show ad. Needs 320x50 dp, but only has 288x495 dp.

<br />

<br />
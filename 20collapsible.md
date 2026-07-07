Select platform: [Android](https://developers.google.com/admob/android/next-gen/banner/collapsible "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/banner/collapsible "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/banner/collapsible "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/banner/collapsible "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/banner/collapsible "View this page for the Android (Legacy) platform docs.")

<br />

Collapsible banner ads are banner ads that are initially presented as a larger
overlay, with a button to collapse them to the originally requested banner size.
Collapsible banner ads are intended to improve performance of anchored ads that
are otherwise a smaller size. This guide shows how to turn on collapsible banner
ads for existing banner placements.

![](https://developers.google.com/static/admob/images/collapsible-banner.png)

## Prerequisites

- Complete the [banner ads get started guide](https://developers.google.com/admob/android/next-gen/banner).

## Implementation

Make sure your banner view is defined with the size you would like users to see
in the regular (collapsed) banner state. Include an extras parameter in the ad
request with `collapsible` as the key and the placement of the ad as the value.

The collapsible placement defines how the expanded region anchors to the banner
ad.

| `Placement` value | Behavior | Intended use case |
|---|---|---|
| `top` | The top of the expanded ad aligns to the top of the collapsed ad. | The ad is placed at the top of the screen. |
| `bottom` | The bottom of the expanded ad aligns to the bottom of the collapsed ad. | The ad is placed at the bottom of the screen. |

If the loaded ad is a collapsible banner, the banner shows the collapsible
overlay immediately once it's placed in the view hierarchy.

### Kotlin

    private fun loadBannerAd() {
      // ...

      // Create an extra parameter that aligns the bottom of the expanded ad to
    // the bottom of the bannerView.
    val extras = Bundle()
    extras.putString("collapsible", "bottom")

      val bannerAdRequest = BannerAdRequest.Builder("AD_UNIT_ID", adSize)
        .setGoogleExtrasBundle(extras)
        .build()

      BannerAd.load(
        bannerAdRequest,
        object : AdLoadCallback<BannerAd> {
          override fun onAdLoaded(ad: BannerAd) {
            // ...
          }

          override fun onAdFailedToLoad(loadAdError: LoadAdError) {
            // ...
          }
        },
      )
    }

### Java

    private void loadBannerAd() {
      // ...

      Bundle extras = new Bundle();
    extras.putString("collapsible", "bottom");

      BannerAdRequest bannerAdRequest = new BannerAdRequest.Builder("AD_UNIT_ID", adSize)
          .setGoogleExtrasBundle(extras)
          .build();

      BannerAd.load(
          bannerAdRequest,
          new AdLoadCallback<BannerAd>() {
            @Override
            public void onAdLoaded(@NonNull BannerAd ad) {
              // ...
            }

            @Override
            public void onAdFailedToLoad(@NonNull LoadAdError adError) {
              // ...
            }
          });
    }

<br />

## Ads refreshing behavior

For apps that configure auto-refresh for banner ads in the
AdMob web interface, when a collapsible banner ad
is requested for a banner slot, subsequent ad refreshes won't request
collapsible banner ads. This is because showing a collapsible banner on every
refresh could have a negative impact on user experience.

If you want to load another collapsible banner ad later in the session, you can
load an ad manually with a request containing the collapsible parameter.

## Check if a loaded ad is collapsible

Non-collapsible banner ads are eligible to return for collapsible banner
requests to maximize performance. Call `isCollapsible` to check if the last
banner loaded is collapsible. If the request fails to load and the previous
banner is collapsible, the API returns the value `true`.

### Kotlin

    override fun onAdLoaded(ad: BannerAd) {
      // ...
      Log.i(
        TAG,
        "The last loaded banner is ${if (ad.isCollapsible()) "" else "not "}collapsible."
      )
    }

### Java

    @Override
    public void onAdLoaded(@NonNull BannerAd ad) {
      // ...
      Log.i(TAG, String.format("The last loaded banner is %scollapsible.",
          ad.isCollapsible() ? "" : "not "));
    }

<br />

## Mediation

Collapsible banner ads are only available for Google demand. Ads served through
mediation show as normal, non-collapsible banner ads.
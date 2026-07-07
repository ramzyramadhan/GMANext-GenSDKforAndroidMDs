Select platform: [Android](https://developers.google.com/admob/android/next-gen/test-creative-types "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/test-creative-types "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/test-creative-types "View this page for the Android (Legacy) platform docs.")

<br />

GMA Next-Gen SDK provides an API that lets you specify a creative type
for test queries. When the parameter is set, only creatives of the specified
type are retrieved and rendered.

## Usage

To specify a creative type, include the `ft_ctype` parameter in an extras object
and pass it to the ad request. This may restrict which ads are available and
result in no fill.

> [!NOTE]
> **Note:** The `ft_ctype` parameter only works in [Test Mode](https://developers.google.com/admob/android/next-gen/test-ads#enable_test_devices).

### Kotlin

    val extras = Bundle()
    extras.putString("ft_ctype", "video_app_install")

    val request = AdRequest
      .Builder(AD_UNIT_ID)
      .setGoogleExtrasBundle(extras)
      .build()

### Java

    Bundle extras = new Bundle();
    extras.putString("ft_ctype", "video_app_install");

    AdRequest request = new AdRequest
      .Builder(AD_UNIT_ID)
      .setGoogleExtrasBundle(extras)
      .build();

The following table lists the valid values for `ft_ctype`:

| Creative Type | ft_ctype | Format |
|---|---|---|
| HTML5 | html5 | Banner, Interstitial, Rewarded |
| App install image | image_app_install | Banner, Native, Interstitial, Rewarded |
| Display image | image_display | Banner, Interstitial |
| Display partial slot | partial_slot | Banner, Native, Interstitial |
| App install text | text_app_install | Banner, Native, Interstitial |
| Display text | text_display | Banner, Native, Interstitial |
| Trueview | trueview | Interstitial, Rewarded |
| App install video | video_app_install | Banner, Native, Interstitial, Rewarded |

This feature impacts Google ads only. If your ad unit enables
[mediation](https://developers.google.com/admob/android/next-gen/mediation), ads returned from third-party ad sources don't
respect the `ft_ctype` parameter. We recommend testing with an ad unit that
doesn't have mediation enabled.
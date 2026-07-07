Select platform: [Android](https://developers.google.com/admob/android/next-gen/mediation "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/mediation "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/mediation "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/mediation "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/mediation "View this page for the Android (Legacy) platform docs.")

<br />

AdMob Mediation is a feature that lets you serve ads to your apps from
multiple sources, including the AdMob Network and third-party ad sources, in
one place. AdMob Mediation helps maximize your fill rate and increase your
monetization by sending ad requests to multiple networks to verify you find the
best available network to serve ads.
[Case study](https://admob.google.com/home/resources/cookapps-grows-ad-revenue-86-times-with-admob-rewarded-ads-and-mediation/).


## Prerequisites

> [!IMPORTANT]
> **Important:** Verify that you have the necessary account permissions to complete the mediation configuration. These permissions include access to inventory management, app access, and privacy and messaging features. See [Manage user access to your
> account](https://support.google.com/admob/answer/2784628) for details.

Before you can integrate mediation for an ad format, you need to integrate that
ad format into your app:

- [Banner Ads](https://developers.google.com/admob/android/next-gen/banner)
- [Interstitial Ads](https://developers.google.com/admob/android/next-gen/interstitial)
- [Native Ads](https://developers.google.com/admob/android/next-gen/native)
- [Rewarded Ads](https://developers.google.com/admob/android/next-gen/rewarded)
- [Rewarded Interstitial
  Ads](https://developers.google.com/admob/android/next-gen/rewarded-interstitial)

New to mediation? Read

[Overview of AdMob Mediation](https://support.google.com/admob/answer/3063564).

## Initialize GMA Next-Gen SDK

The quick start guide shows you how to [initialize the GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start#initialize_the_mobile_ads_sdk).
During that initialization call, mediation adapters also
get initialized. It is important to wait for initialization to complete before
you load ads in order to verify full participation from every ad network on the
first ad request.

> [!IMPORTANT]
> **Important:** Bidding adapters require you to explicitly initialize GMA Next-Gen SDK.

The following sample code shows how you can check each adapter's initialization
status prior to making an ad request.

### Kotlin

    import com.google.android.libraries.ads.mobile.sdk.MobileAds
    import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig
    import kotlinx.coroutines.CoroutineScope
    import kotlinx.coroutines.Dispatchers
    import kotlinx.coroutines.launch

    class MainActivity : AppCompatActivity() {
      override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val backgroundScope = CoroutineScope(Dispatchers.IO)
    backgroundScope.launch {
    // Initialize GMA Next-Gen SDK on a background thread.
    MobileAds.initialize(this@MainActivity, InitializationConfig.Builder("SAMPLE_APP_ID").build()) {
    initializationStatus -\>
    for ((adapterName, adapterStatus) in initializationStatus.adapterStatusMap) {
    Log.d(
    "MyApp",
    String.format(
    "Adapter name: %s, Status code: %s, Status string: %s, Latency: %d",
    adapterName,
    adapterStatus.initializationState,
    adapterStatus.description,
    adapterStatus.latency,
    ),
    )
    }
    // Adapter initialization is complete.
    }
    // Other methods on MobileAds can now be called.
    }
      }
    }

### Java

    import com.google.android.libraries.ads.mobile.sdk.MobileAds;
    import com.google.android.libraries.ads.mobile.sdk.initialization.AdapterStatus;
    import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig;

    public class MainActivity extends AppCompatActivity {
      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        new Thread(
    () -\> {
    // Initialize GMA Next-Gen SDK on a background thread.
    MobileAds.initialize(
    this,
    new InitializationConfig.Builder("SAMPLE_APP_ID")
    .build(),
    initializationStatus -\> {
    Map\<String, AdapterStatus\> adapterStatusMap =
    initializationStatus.getAdapterStatusMap();
    for (String adapterClass : adapterStatusMap.keySet()) {
    AdapterStatus adapterStatus = adapterStatusMap.get(adapterClass);
    Log.d(
    "MyApp",
    String.format(
    "Adapter name: %s, Status code: %s, Status description: %s,"
    + " Latency: %d",
    adapterClass,
    adapterStatus.getInitializationState(),
    adapterStatus.getDescription(),
    adapterStatus.getLatency()));
    }
    // Adapter initialization is complete.
    });
    // Other methods on MobileAds can now be called.
    })
    .start();
      }
    }

## Exclude `com.google.android.gms` modules in mediation integrations

Mediation adapters continue to depend on Google Mobile Ads SDK (Legacy). However
,GMA Next-Gen SDK includes all classes required by mediation adapters.
To avoid compile errors related to duplicate symbols, you need to exclude
Google Mobile Ads SDK (Legacy) from being pulled in as a dependency by mediation
adapters.

In your app-level `build.gradle` file, exclude both `play-services-ads` and
`play-services-ads-lite` modules globally from all dependencies:

### Kotlin

```kotlin
configurations.configureEach {
    exclude(group = "com.google.android.gms", module = "play-services-ads")
    exclude(group = "com.google.android.gms", module = "play-services-ads-lite")
}
```

### Groovy

```groovy
configurations.configureEach {
    exclude group: "com.google.android.gms", module: "play-services-ads"
    exclude group: "com.google.android.gms", module: "play-services-ads-lite"
}
```

## Check which ad network adapter class loaded the ad

Here is some sample code that logs the ad network class name for a banner ad:

### Kotlin

    BannerAd.load(
      BannerAdRequest.Builder("AD_UNIT_ID", AdSize.BANNER).build(),
      object : AdLoadCallback<BannerAd> {
        override fun onAdLoaded(ad: BannerAd) {
          Log.d(
            "MyApp", "Adapter class name: " +
              ad.getResponseInfo().mediationAdapterClassName
          )
        }
      }
    )

### Java

    BannerAd.load(
      new BannerAdRequest.Builder("AD_UNIT_ID", AdSize.BANNER).build(),
      new AdLoadCallback<BannerAd>() {
        @Override
        public void onAdLoaded(@NonNull BannerAd ad) {
          Log.d("MyApp",
              "Adapter class name: " + ad.getResponseInfo().getMediationAdapterClassName());
        }
      }
    );

<br />

<br />

## Use banner ads with AdMob Mediation

Make sure to disable refresh in all third-party ad source UIs for banner ad
units used in AdMob Mediation. This prevents a
double refresh since AdMob also triggers a refresh
based on your banner ad unit's refresh rate.

<br />

## US states privacy laws and GDPR

If you need to comply with the [U.S. states privacy
laws](https://support.google.com/admob/answer/9561022) or [General Data Protection
Regulation (GDPR)](https://support.google.com/admob/answer/7666366), follow the
steps in [US state regulations
settings](https://support.google.com/admob/answer/10860309) or [GDPR
settings](https://support.google.com/admob/answer/10113004#adding_ad_partners_to_published_gdpr_messages) to add your
mediation partners in AdMob Privacy \& messaging's
US states or GDPR ad partners list. Failure to do so can lead to partners
failing to serve ads on your app.

Learn more about enabling [restricted data processing
(RDP)](https://developers.google.com/admob/android/next-gen/privacy/us-states) and obtaining GDPR consent with the
[Google User Messaging Platform (UMP) SDK](https://developers.google.com/admob/android/privacy).
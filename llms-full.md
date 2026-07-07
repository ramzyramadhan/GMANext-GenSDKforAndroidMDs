# GMA Next-Gen SDK for Android — Full LLM Documentation

This document is a chronological, comprehensive reference for the Google Mobile Ads (GMA) Next-Gen SDK for Android. It follows the 72 official guide files in order (1 through 72), each section containing the file's description, source link, and curated key code snippets in Kotlin and Java where applicable.

**SDK artifact:** `com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk`
**Minimum SDK:** 24+ | **Compile SDK:** 35+ | **Minimum Kotlin:** 1.9

---

## Table of Contents

1. [Quick Start](#1-quick-start) — SDK integration, Gradle setup, initialization
2. [Deprecation](#2-deprecation) — Version support, deprecation, and sunset schedule
3. [Benefits](#3-benefits) — Advantages of upgrading to Next-Gen SDK
4. [Migration](#4-migration) — Configure build and initialize during migration
5. [Handle Callbacks](#5-handle-callbacks) — Dispatch ad callbacks to the UI thread
6. [Migrate Ad Requests](#6-migrate-ad-requests) — Ad unit IDs and extras in ad requests
7. [Migrate App Open](#7-migrate-app-open) — App open ad load/show differences
8. [Migrate Banner](#8-migrate-banner) — Banner ad load/show differences
9. [Migrate Interstitial](#9-migrate-interstitial) — Interstitial ad load/show differences
10. [Migrate Native](#10-migrate-native) — Native ad implementation differences
11. [Migrate Rewarded](#11-migrate-rewarded) — Rewarded ad load/show differences
12. [Migrate Rewarded Interstitial](#12-migrate-rewarded-interstitial) — Rewarded interstitial load/show differences
13. [Migrate with AI Tools](#13-migrate-with-ai-tools) — Install migration agent skill
14. [Integrate CMP](#14-integrate-cmp) — Consent management platform changes
15. [Test Ads](#15-test-ads) — Enable test ads and test devices
16. [Agent Skills](#16-agent-skills) — Install/update AI agent skills
17. [App Open](#17-app-open) — Full app open ad integration guide
18. [Banner](#18-banner) — Full banner ad integration guide
19. [Inline Adaptive](#19-inline-adaptive) — Inline adaptive banner implementation
20. [Collapsible](#20-collapsible) — Collapsible banner ad implementation
21. [Fixed Size](#21-fixed-size) — Fixed banner ad sizes reference
22. [Interstitial](#22-interstitial) — Full interstitial ad integration guide
23. [Native](#23-native) — Full native ad integration guide
24. [Advanced](#24-advanced) — Native advanced: NativeAdView, display, clicks
25. [Full Screen](#25-full-screen) — Best practices for full-screen native ads
26. [Options](#26-options) — Native ad advanced options and controls
27. [Validator](#27-validator) — Native Validator for policy violations
28. [Video Ads](#28-video-ads) — MediaContent and video lifecycle callbacks
29. [Rewarded](#29-rewarded) — Full rewarded ad integration guide
30. [Rewarded Interstitial](#30-rewarded-interstitial) — Full rewarded interstitial guide
31. [Mediation](#31-mediation) — AdMob Mediation overview and setup
32. [Choose Networks](#32-choose-networks) — Supported ad sources and networks
33. [Meta](#33-meta) — Meta Audience Network mediation guide
34. [Troubleshoot Bidding](#34-troubleshoot-bidding) — Bidding integration checklist
35. [Setup](#35-setup) — Custom events setup in AdMob UI
36. [Custom Events Banner](#36-custom-events-banner) — Banner custom event adapter
37. [Custom Events Interstitial](#37-custom-events-interstitial) — Interstitial custom event adapter
38. [Custom Events Native](#38-custom-events-native) — Native custom event adapter
39. [Custom Events Rewarded](#39-custom-events-rewarded) — Rewarded custom event adapter
40. [Strategies](#40-strategies) — Publisher first-party ID privacy strategy
41. [Ad Serving Modes](#41-ad-serving-modes) — Personalized, non-personalized, limited ads
42. [Play Data Disclosure](#42-play-data-disclosure) — Google Play Data safety info
43. [Precise Location](#43-precise-location) — Consent for precise location data
44. [US States](#44-us-states) — US states privacy laws (RDP/GPP)
45. [Privacy](#45-privacy) — UMP SDK get-started guide
46. [GDPR](#46-gdpr) — GDPR IAB TCF v2 message support
47. [US IAB Support](#47-us-iab-support) — US states regulations message support
48. [Consent Mode](#48-consent-mode) — Google Consent Mode integration
49. [Sync Consent](#49-sync-consent) — Cross-app GDPR consent sync
50. [Release Notes](#50-release-notes) — UMP SDK release notes
51. [Ad Inspector](#51-ad-inspector) — Ad inspector overview
52. [Launch Ad Inspector](#52-launch-ad-inspector) — Launch via gesture or programmatically
53. [Verify Adapter Integrations](#53-verify-adapter-integrations) — View/verify adapters in ad inspector
54. [Test Ad Units](#54-test-ad-units) — Test ad units in ad inspector
55. [Troubleshoot Ad Units](#55-troubleshoot-ad-units) — Debug ad units in ad inspector
56. [Troubleshoot Privacy Settings](#56-troubleshoot-privacy-settings) — Debug privacy/CMP in ad inspector
57. [Copy Troubleshooting Output](#57-copy-troubleshooting-output) — Export JSON troubleshooting data
58. [Test Creative Types](#58-test-creative-types) — Specify creative type for test queries
59. [Ad Load Errors](#59-ad-load-errors) — Handle LoadAdError failures
60. [Response Info](#60-response-info) — ResponseInfo for debugging ads
61. [Charles](#61-charles) — Inspect ad traffic with Charles proxy
62. [App Ads](#62-app-ads) — Set up app-ads.txt authorized sellers
63. [Global Settings](#63-global-settings) — Volume control and cookie consent
64. [Impression-Level Ad Revenue](#64-impression-level-ad-revenue) — onAdPaid revenue reporting
65. [SSV](#65-ssv) — Rewarded server-side verification
66. [Targeting](#66-targeting) — RequestConfiguration and targeting settings
67. [Open Measurement](#67-open-measurement) — IAB viewability integration
68. [Browser](#68-browser) — In-app browser overview for ads
69. [Custom Tabs](#69-custom-tabs) — Web view APIs with Chrome Custom Tabs
70. [WebView](#70-webview) — Configure WebView for ad monetization
71. [API for Ads](#71-api-for-ads) — Web view APIs for ads with WebView
72. [Click Behavior](#72-click-behavior) — Optimize WebView click behavior

---

## 1. Quick Start

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/quick-start.md

Integrating GMA Next-Gen SDK into an Android app: prerequisites, Gradle setup, SDK initialization, and choosing an ad format.

### Configure Gradle repositories

Add Google's Maven repository and Maven central repository to your Gradle settings file.

```kotlin
pluginManagement {
  repositories {
    google()
    mavenCentral()
    gradlePluginPortal()
  }
}

dependencyResolutionManagement {
  repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
  repositories {
    google()
    mavenCentral()
  }
}

rootProject.name = "My Application"
include(":app")
```

### Configure Gradle repositories (Groovy)

Add Google's Maven repository and Maven central repository to your Gradle settings file.

```groovy
pluginManagement {
  repositories {
    google()
    mavenCentral()
    gradlePluginPortal()
  }
}

dependencyResolutionManagement {
  repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
  repositories {
    google()
    mavenCentral()
  }
}

rootProject.name = "My Application"
include ':app'
```

### Add the GMA Next-Gen SDK dependency

Add the `ads-mobile-sdk` artifact to your app-level build file.

```kotlin
dependencies {
  implementation("com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk:1.2.1")
}
```

### Add the GMA Next-Gen SDK dependency (Groovy)

Add the `ads-mobile-sdk` artifact to your app-level build file.

```groovy
dependencies {
  implementation 'com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk:1.2.1'
}
```

### Initialize the GMA Next-Gen SDK

Initialize the SDK on a background thread before loading ads, passing the AdMob app ID via `InitializationConfig`.

```kotlin
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
      MobileAds.initialize(
        this@MainActivity,
        // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
        InitializationConfig.Builder("SAMPLE_APP_ID").build()
      ) {
        // Adapter initialization is complete.
      }
      // SDK initialization is complete. If you don't want to wait for bidding adapters to finish
      // initializing, start loading ads now.
    }
  }
}
```

### Initialize the GMA Next-Gen SDK (Java)

Initialize the SDK on a background thread before loading ads, passing the AdMob app ID via `InitializationConfig`.

```java
import com.google.android.libraries.ads.mobile.sdk.MobileAds;
import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig;

public class MainActivity extends AppCompatActivity {
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    new Thread(
            () -> {
              // Initialize GMA Next-Gen SDK on a background thread.
              MobileAds.initialize(
                  this,
                  // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
                  new InitializationConfig.Builder("SAMPLE_APP_ID")
                      .build(),
                  initializationStatus -> {
                    // Adapter initialization is complete.
                  });
              // SDK initialization is complete. If you don't want to wait for bidding adapters to
              // finish initializing, start loading ads now.
            })
        .start();
  }
}
```

---

## 2. Deprecation

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/deprecation.md

Describes the deprecation and sunset schedule for prior major versions of the GMA Next-Gen SDK, including the timetable and the differences between supported, deprecated, and sunset states.

*No code samples in this guide.*

---

## 3. Benefits

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/benefits.md

Summarizes the benefits of upgrading to the GMA Next-Gen SDK for Android: faster ad request latency, smaller size, background startup, removed RPCs for stability, Kotlin/Java APIs, and support for all ad formats.

*No code samples in this guide.*

---

## 4. Migration

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migration.md

Instructions to migrate from the Google Mobile Ads SDK (Legacy) to GMA Next-Gen SDK: Gradle dependency changes, excluding legacy `com.google.android.gms` modules for mediation, API level requirements, and SDK initialization.

### Include the GMA Next-Gen SDK dependency

Remove the legacy `play-services-ads` artifact and add the new `ads-mobile-sdk` artifact in your app-level build file.

```kotlin
dependencies {
  // ...
  // Comment out/remove play-services-ads.
  // implementation("com.google.android.gms:play-services-ads:25.4.0")
  implementation("com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk:1.2.1")
}
```

### Include the GMA Next-Gen SDK dependency (Groovy)

Remove the legacy `play-services-ads` artifact and add the new `ads-mobile-sdk` artifact in your app-level build file.

```groovy
dependencies {
  // ...
  // Comment out/remove play-services-ads.
  // implementation 'com.google.android.gms:play-services-ads:25.4.0'
  implementation 'com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk:1.2.1'
}
```

### Exclude com.google.android.gms modules in mediation

To avoid duplicate symbols when using mediation adapters, globally exclude `play-services-ads` and `play-services-ads-lite` from all configurations.

```kotlin
configurations.configureEach {
    exclude(group = "com.google.android.gms", module = "play-services-ads")
    exclude(group = "com.google.android.gms", module = "play-services-ads-lite")
}
```

### Exclude com.google.android.gms modules in mediation (Groovy)

To avoid duplicate symbols when using mediation adapters, globally exclude `play-services-ads` and `play-services-ads-lite` from all configurations.

```groovy
configurations.configureEach {
    exclude group: "com.google.android.gms", module: "play-services-ads"
    exclude group: "com.google.android.gms", module: "play-services-ads-lite"
}
```

### Set the AdMob app ID and initialize

GMA Next-Gen SDK no longer uses the AndroidManifest `<meta-data>` tag for the app ID; pass it programmatically via `InitializationConfig`.

```kotlin
// Initialize the Google Mobile Ads SDK.
val initConfig = InitializationConfig.Builder("SAMPLE_APP_ID").build()
MobileAds.initialize(this@MainActivity, initConfig) {}
```

### Set the AdMob app ID and initialize (Java)

GMA Next-Gen SDK no longer uses the AndroidManifest `<meta-data>` tag for the app ID; pass it programmatically via `InitializationConfig`.

```java
// Initialize GMA Next-Gen SDK.
InitializationConfig initConfig = new InitializationConfig.Builder("SAMPLE_APP_ID").build();
MobileAds.initialize(this, initConfig, initializationStatus -> {});
```

---
## 5. Handle Callbacks

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/handle-callbacks.md

GMA Next-Gen SDK runs ad load and event callbacks on a background thread, so UI work inside callbacks must be dispatched to the UI thread.

### Dispatch callback UI work to the UI thread

Show a toast on the UI thread from the `onAdLoaded` callback of a banner ad load.

```kotlin
adView.loadAd(
  adRequest,
  object : AdLoadCallback<BannerAd> {
    override fun onAdLoaded(ad: BannerAd) {
      // Show a toast on the UI thread.
      runOnUiThread {
        Toast.makeText(activity, "Ad loaded.", Toast.LENGTH_SHORT).show()
      }
    }
  },
)
```

### Dispatch callback UI work to the UI thread (Java)

Show a toast on the UI thread from the `onAdLoaded` callback of a banner ad load.

```java
adView.loadAd(
    adRequest,
    new AdLoadCallback<BannerAd>() {
        @Override
        public void onAdLoaded(@NonNull BannerAd ad) {
            // Show a toast on the UI thread.
            runOnUiThread(() ->
                Toast.makeText(activity, "Ad loaded.", Toast.LENGTH_SHORT).show()
            );
        }
    });
```

---

## 6. Migrate Ad Requests

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-ad-requests.md

GMA Next-Gen SDK passes the AdMob ad unit ID directly to the `AdRequest` object instead of the load method, and introduces new methods for passing extras to AdMob and ad source adapters.

### Create an AdRequest with the ad unit ID

Pass the ad unit ID to `AdRequest.Builder` and use a generic `AdLoadCallback` when loading.

```kotlin
val adRequest = AdRequest.Builder("AD_UNIT_ID").build()
InterstitialAd.load(adRequest, object : AdLoadCallback<InterstitialAd> {})
```

### Create an AdRequest with the ad unit ID (Java)

Pass the ad unit ID to `AdRequest.Builder` and use a generic `AdLoadCallback` when loading.

```java
AdRequest adRequest = new AdRequest.Builder("AD_UNIT_ID").build();
InterstitialAd.load(adRequest, new AdLoadCallback<InterstitialAd>() {});
```

### Pass extra parameters to AdMob

Request non-personalized ads by attaching an extras bundle via `setGoogleExtrasBundle`.

```kotlin
val extras = Bundle()
extras.putInt("npa", 1)
val request = AdRequest.Builder("AD_UNIT_ID")
  .setGoogleExtrasBundle(extras)
  .build()
```

### Pass extra parameters to AdMob (Java)

Request non-personalized ads by attaching an extras bundle via `setGoogleExtrasBundle`.

```java
Bundle extras = new Bundle();
extras.putInt("npa", 1);
AdRequest request = new AdRequest.Builder("AD_UNIT_ID")
  .setGoogleExtrasBundle(extras)
  .build();
```

### Pass extra parameters to an ad source adapter

Attach adapter-specific extras via `putAdSourceExtrasBundle` on the request builder.

```kotlin
val extras = Bundle()
extras.putString("exampleKey", "exampleValue")
val request = AdRequest.Builder("AD_UNIT_ID")
  .putAdSourceExtrasBundle(SampleAdapter::class.java, extras)
  .build()
```

### Pass extra parameters to an ad source adapter (Java)

Attach adapter-specific extras via `putAdSourceExtrasBundle` on the request builder.

```java
Bundle extras = new Bundle();
extras.putString("exampleKey", "exampleValue");
AdRequest request = new AdRequest.Builder("AD_UNIT_ID")
  .putAdSourceExtrasBundle(SampleAdapter.class, extras)
  .build();
```

---

## 7. Migrate App Open

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-app-open.md

Differences in loading and showing an app open ad between Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK, including the new `AdLoadCallback` and `AppOpenAdEventCallback` APIs.

### Load an app open ad

Load an app open ad with the ad unit ID on the request and set the `AppOpenAdEventCallback` on the loaded ad.

```kotlin
AppOpenAd.load(
  AdRequest.Builder("AD_UNIT_ID").build(),
  object : AdLoadCallback<AppOpenAd> {
    override fun onAdLoaded(ad: AppOpenAd) {
      // Called when an ad has loaded.
      ad.adEventCallback = object : AppOpenAdEventCallback {
      }
      appOpenAd = ad
    }

    override fun onAdFailedToLoad(loadAdError: LoadAdError) {
      // Called when ad fails to load.
    }
  }
)
```

### Load an app open ad (Java)

Load an app open ad with the ad unit ID on the request and set the `AppOpenAdEventCallback` on the loaded ad.

```java
AppOpenAd.load(
  new AdRequest.Builder("AD_UNIT_ID").build(),
  new AdLoadCallback<AppOpenAd>() {
    @Override
    public void onAdLoaded(@NonNull AppOpenAd ad) {
      // Called when an ad has loaded.
      ad.setAdEventCallback(new AppOpenAdEventCallback() {});
      appOpenAd = ad;
    }

    @Override
    public void onAdFailedToLoad(@NonNull LoadAdError adError) {
      // Called when ad fails to load.
    }
  });
```

### Show an app open ad

Show a loaded app open ad by passing the activity.

```kotlin
appOpenAd?.show(this@AppOpenActivity)
```

### Show an app open ad (Java)

Show a loaded app open ad by passing the activity.

```java
appOpenAd.show(this);
```

---
## 8. Migrate Banner

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-banner.md

Differences in loading and showing a banner ad between Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK, including the new `BannerAdRequest`, `AdLoadCallback<BannerAd>`, `BannerAdEventCallback`, and the new banner refresh callback.

### Load a banner ad

Create an `AdView`, build a `BannerAdRequest` with ad unit ID and size, then load with an `AdLoadCallback` and set the `BannerAdEventCallback` on the loaded ad.

```kotlin
import android.util.Log
import com.google.android.libraries.ads.mobile.sdk.banner.AdSize
import com.google.android.libraries.ads.mobile.sdk.banner.AdView
import com.google.android.libraries.ads.mobile.sdk.banner.BannerAd
import com.google.android.libraries.ads.mobile.sdk.banner.BannerAdEventCallback
import com.google.android.libraries.ads.mobile.sdk.banner.BannerAdRequest
import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback
import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError

class MainActivity : AppCompatActivity() {
  private val TAG = "MainActivity"
  private lateinit var adView: AdView
  private lateinit var binding: ActivityMainBinding

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    // Step 1 - Create an AdView object.
    adView = binding.adView

    // Step 2 - Load the ad.
    val adSize = AdSize.getLargeAnchoredAdaptiveBannerAdSize(this, 360)
    val adRequest = BannerAdRequest.Builder("AD_UNIT_ID", adSize).build()
    adView.loadAd(
      adRequest,
      object : AdLoadCallback<BannerAd> {
        override fun onAdLoaded(ad: BannerAd) {
          ad.adEventCallback = object : BannerAdEventCallback {
            override fun onAdImpression() {
              Log.d(TAG, "Banner ad recorded an impression.")
            }

            override fun onAdClicked() {
              Log.d(TAG, "Banner ad clicked.")
            }
          }
        }

        override fun onAdFailedToLoad(adError: LoadAdError) {
          Log.e(TAG, "Banner ad failed to load: $adError")
        }
      },
    )
  }
}
```

### Load a banner ad (Java)

Create an `AdView`, build a `BannerAdRequest` with ad unit ID and size, then load with an `AdLoadCallback` and set the `BannerAdEventCallback` on the loaded ad.

```java
import android.util.Log;
import com.google.android.libraries.ads.mobile.sdk.banner.AdSize;
import com.google.android.libraries.ads.mobile.sdk.banner.AdView;
import com.google.android.libraries.ads.mobile.sdk.banner.BannerAd;
import com.google.android.libraries.ads.mobile.sdk.banner.BannerAdEventCallback;
import com.google.android.libraries.ads.mobile.sdk.banner.BannerAdRequest;
import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback;
import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError;

public class MainActivity extends AppCompatActivity {
  private static final String TAG = "MainActivity";
  private AdView adView;
  private ActivityMainBinding binding;

  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    binding = ActivityMainBinding.inflate(getLayoutInflater());
    setContentView(binding.getRoot());

    // Step 1 - Create an AdView object.
    adView = binding.adView;

    // Step 2 - Load the ad.
    AdSize adSize = AdSize.getLargeAnchoredAdaptiveBannerAdSize(this, 360);
    BannerAdRequest adRequest = new BannerAdRequest.Builder("AD_UNIT_ID", adSize).build();
    adView.loadAd(
        adRequest,
        new AdLoadCallback<BannerAd>() {
          @Override
          public void onAdLoaded(@NonNull BannerAd ad) {
            ad.setAdEventCallback(
                new BannerAdEventCallback() {
                  @Override
                  public void onAdImpression() {
                    Log.d(TAG, "Banner ad recorded an impression.");
                  }

                  @Override
                  public void onAdClicked() {
                    Log.d(TAG, "Banner ad clicked.");
                  }
                });
          }

          @Override
          public void onAdFailedToLoad(@NonNull LoadAdError adError) {
            Log.e(TAG, "Banner ad failed to load: " + adError);
          }
        });
  }
}
```

### Set the banner ad refresh callback

Set `BannerAdRefreshCallback` on the loaded banner ad to listen for automatic ad refreshes.

```kotlin
adView.loadAd(
  BannerAdRequest.Builder("AD_UNIT_ID", adSize).build(),
  object : AdLoadCallback<BannerAd> {
    override fun onAdLoaded(ad: BannerAd) {
      // Called when an ad has loaded.
      ad.adEventCallback = object : BannerAdEventCallback {}
      ad.bannerAdRefreshCallback = object : BannerAdRefreshCallback {
        // Set the ad refresh callbacks.
        override fun onAdRefreshed() {
          // Called when the ad refreshes.
        }

        override fun onAdFailedToRefresh(adError: LoadAdError) {
          // Called when the ad fails to refresh.
        }
      }
    }

    override fun onAdFailedToLoad(adError: LoadAdError) {
      // Called when ad fails to load.
    }
  }
)
```

### Set the banner ad refresh callback (Java)

Set `BannerAdRefreshCallback` on the loaded banner ad to listen for automatic ad refreshes.

```java
adView.loadAd(
  new BannerAdRequest.Builder("AD_UNIT_ID", adSize).build(),
  new AdLoadCallback<BannerAd>() {
    @Override
    public void onAdLoaded(@NonNull BannerAd ad) {
      // Called when an ad has loaded.
      ad.setAdEventCallback(new BannerAdEventCallback() {});
      ad.setBannerAdRefreshCallback(
          // Set the ad refresh callbacks.
          new BannerAdRefreshCallback() {
            @Override
            public void onAdRefreshed() {
              // Called when the ad refreshes.
            }

            @Override
            public void onAdFailedToRefresh(@NonNull LoadAdError adError) {
              // Called when the ad fails to refresh.
            }
          });
    }

    @Override
    public void onAdFailedToLoad(@NonNull LoadAdError adError) {
      // Called when ad fails to load.
    }
  });
```

---
## 9. Migrate Interstitial

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-interstitial.md

Differences in loading and showing an interstitial ad between Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK, including the new `AdLoadCallback<InterstitialAd>` and `InterstitialAdEventCallback` APIs.

### Load an interstitial ad

Load an interstitial ad with the ad unit ID on the request and set the `InterstitialAdEventCallback` on the loaded ad.

```kotlin
InterstitialAd.load(
  AdRequest.Builder("AD_UNIT_ID").build(),
  object : AdLoadCallback<InterstitialAd> {
    override fun onAdLoaded(ad: InterstitialAd) {
      // Called when an ad has loaded.
      ad.adEventCallback = object : InterstitialAdEventCallback {
      }
      interstitialAd = ad
    }

    override fun onAdFailedToLoad(loadAdError: LoadAdError) {
      // Called when ad fails to load.
    }
  }
)
```

### Load an interstitial ad (Java)

Load an interstitial ad with the ad unit ID on the request and set the `InterstitialAdEventCallback` on the loaded ad.

```java
InterstitialAd.load(
  new AdRequest.Builder("AD_UNIT_ID").build(),
  new AdLoadCallback<InterstitialAd>() {
    @Override
    public void onAdLoaded(@NonNull InterstitialAd ad) {
      // Called when an ad has loaded.
      ad.setAdEventCallback(new InterstitialAdEventCallback() {});
      interstitialAd = ad;
    }

    @Override
    public void onAdFailedToLoad(@NonNull LoadAdError adError) {
      // Called when ad fails to load.
    }
  });
```

### Show an interstitial ad

Show a loaded interstitial ad by passing the activity.

```kotlin
interstitialAd?.show(this@InterstitialActivity)
```

### Show an interstitial ad (Java)

Show a loaded interstitial ad by passing the activity.

```java
interstitialAd.show(this);
```

---

## 10. Migrate Native

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-native.md

Comparison of native ad implementations between Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK: ad types are specified in the request, a single `NativeAdLoaderCallback` handles load success/failure, and event callbacks are registered after the ad loads.

### Load a native ad

Load a native ad by specifying the ad type in `NativeAdRequest` and using `NativeAdLoaderCallback` for both success and failure.

```kotlin
NativeAdLoader.load(
  NativeAdRequest.Builder(AD_UNIT_ID, listOf(NativeAd.NativeAdType.NATIVE)).build(),
  object : NativeAdLoaderCallback {
    override fun onNativeAdLoaded(nativeAd: NativeAd) {
      // Native ad loaded.
    }

    override fun onAdFailedToLoad(adError: LoadAdError) {
      // Native ad failed to load.
    }
  }
)
```

### Load a native ad (Java)

Load a native ad by specifying the ad type in `NativeAdRequest` and using `NativeAdLoaderCallback` for both success and failure.

```java
NativeAdLoader.load(
  new NativeAdRequest.Builder(AD_UNIT_ID, List.of(NativeAd.NativeAdType.NATIVE)).build(),
  new NativeAdLoaderCallback() {
    @Override
    public void onNativeAdLoaded(NativeAd nativeAd) {
      // Native ad loaded.
    }

    @Override
    public void onAdFailedToLoad(LoadAdError adError) {
      // Native ad failed to load.
    }
  }
);
```

### Set native ad event callbacks

In GMA Next-Gen SDK, register ad event callbacks on the loaded native ad via `NativeAdEventCallback`.

```kotlin
NativeAdLoader.load(
  NativeAdRequest
    .Builder(AD_UNIT_ID, listOf(NativeAd.NativeAdType.NATIVE))
    .build(),
  object : NativeAdLoaderCallback {
    override fun onNativeAdLoaded(nativeAd: NativeAd) {
      // Native ad loaded.
      nativeAd.adEventCallback = object : NativeAdEventCallback {
        override fun onAdShowedFullScreenContent() {
          // Native ad showed full screen content.
          // Google Mobile Ads SDK (Legacy) equivalent: onAdOpened()
        }

        override fun onAdDismissedFullScreenContent() {
          // Native ad dismissed full screen content.
          // Google Mobile Ads SDK (Legacy) equivalent: onAdClosed()
        }

        override fun onAdFailedToShowFullScreenContent(
          fullScreenContentError: FullScreenContentError
        ) {
          // Native ad failed to show full screen content.
          // Google Mobile Ads SDK (Legacy) equivalent: N/A
        }

        override fun onAdImpression() {
          // Native ad recorded an impression.
        }

        override fun onAdClicked() {
          // Native ad recorded a click.
        }
      }
    }
  }
)
```

### Set native ad event callbacks (Java)

In GMA Next-Gen SDK, register ad event callbacks on the loaded native ad via `NativeAdEventCallback`.

```java
NativeAdLoader.load(
  new NativeAdRequest.Builder(AD_UNIT_ID, List.of(NativeAd.NativeAdType.NATIVE))
      .build(),
  new NativeAdLoaderCallback() {
    @Override
    public void onNativeAdLoaded(NativeAd nativeAd) {
      // Native ad loaded.
      nativeAd.setAdEventCallback(new NativeAdEventCallback() {
        @Override
        public void onAdShowedFullScreenContent() {
          // Native ad showed full screen content.
          // Google Mobile Ads SDK (Legacy) equivalent: onAdOpened()
        }

        @Override
        public void onAdDismissedFullScreenContent() {
          // Native ad dismissed full screen content.
          // Google Mobile Ads SDK (Legacy) equivalent: onAdClosed()
        }

        @Override
        public void onAdFailedToShowFullScreenContent(FullScreenContentError fullScreenContentError) {
          // Native ad failed to show full screen content.
          // Google Mobile Ads SDK (Legacy) equivalent: N/A
        }

        @Override
        public void onAdImpression() {
          // Native ad recorded an impression.
        }

        @Override
        public void onAdClicked() {
          // Native ad recorded a click.
        }
      });
    }
  }
);
```

### Register the media content asset with NativeAdView

GMA Next-Gen SDK enforces registering the media view at the same time as the native ad via `registerNativeAd`.

```kotlin
private fun displayNativeAd(nativeAd: NativeAd) {
  // Inflate the NativeAdView layout.
  val nativeAdBinding = NativeAdBinding.inflate(layoutInflater)
  // Add the NativeAdView to the view hierarchy.
  binding.nativeViewContainer.addView(nativeAdBinding.root)
  val nativeAdView = nativeAdBinding.root
  // Populate and register the asset views.
  // ...
  // Register the native ad and media content asset with the NativeAdView.
  val mediaView = nativeAdBinding.adMedia
  nativeAdView.registerNativeAd(nativeAd, mediaView)
}
```

### Register the media content asset with NativeAdView (Java)

GMA Next-Gen SDK enforces registering the media view at the same time as the native ad via `registerNativeAd`.

```java
private void displayNativeAd(NativeAd nativeAd) {
  // Inflate the NativeAdView layout.
  NativeAdBinding nativeAdBinding = NativeAdBinding.inflate(getLayoutInflater());
  // Add the NativeAdView to the view hierarchy.
  binding.nativeViewContainer.addView(nativeAdBinding.getRoot());
  NativeAdView nativeAdView = nativeAdBinding.getRoot();
  // Populate and register the asset views.
  // ...
  // Register the native ad and media content asset with the NativeAdView.
  MediaView mediaView = nativeAdBinding.adMedia;
  nativeAdView.registerNativeAd(nativeAd, mediaView);
}
```

---

## 11. Migrate Rewarded

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-rewarded.md

Differences in loading and showing a rewarded ad between Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK, including the new `AdLoadCallback<RewardedAd>` and `RewardedAdEventCallback` APIs.

### Load a rewarded ad

Load a rewarded ad with the ad unit ID on the request and set the `RewardedAdEventCallback` on the loaded ad.

```kotlin
RewardedAd.load(
  AdRequest.Builder("AD_UNIT_ID").build(),
  object : AdLoadCallback<RewardedAd> {
    override fun onAdLoaded(ad: RewardedAd) {
      // Called when an ad has loaded.
      ad.adEventCallback = object : RewardedAdEventCallback {
      }
      rewardedAd = ad
    }

    override fun onAdFailedToLoad(loadAdError: LoadAdError) {
      // Called when ad fails to load.
    }
  }
)
```

### Load a rewarded ad (Java)

Load a rewarded ad with the ad unit ID on the request and set the `RewardedAdEventCallback` on the loaded ad.

```java
RewardedAd.load(
  new AdRequest.Builder("AD_UNIT_ID").build(),
  new AdLoadCallback<RewardedAd>() {
    @Override
    public void onAdLoaded(@NonNull RewardedAd ad) {
      // Called when an ad has loaded.
      ad.setAdEventCallback(new RewardedAdEventCallback() {});
      rewardedAd = ad;
    }

    @Override
    public void onAdFailedToLoad(@NonNull LoadAdError adError) {
      // Called when ad fails to load.
    }
  });
```

### Show a rewarded ad

Show a loaded rewarded ad and handle the user-earned-reward callback.

```kotlin
rewardedAd?.show(
  this@RewardedActivity,
  object : OnUserEarnedRewardListener {
    override fun onUserEarnedReward(rewardItem: RewardItem) {
      // User earned the reward.
      val rewardAmount = rewardItem.amount
      val rewardType = rewardItem.type
    }
  }
)
```

### Show a rewarded ad (Java)

Show a loaded rewarded ad and handle the user-earned-reward callback.

```java
rewardedAd.show(
  this,
  new OnUserEarnedRewardListener() {
    @Override
    public void onUserEarnedReward(@NonNull RewardItem rewardItem) {
      // User earned the reward.
      int rewardAmount = rewardItem.getAmount();
      String rewardType = rewardItem.getType();
    }
  });
```

---

## 12. Migrate Rewarded Interstitial

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-rewarded-interstitial.md

Differences in loading and showing a rewarded interstitial ad between Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK, including the new `AdLoadCallback<RewardedInterstitialAd>` and `RewardedInterstitialAdEventCallback` APIs.

### Load a rewarded interstitial ad

Load a rewarded interstitial ad with the ad unit ID on the request and set the `RewardedInterstitialAdEventCallback` on the loaded ad.

```kotlin
RewardedInterstitialAd.load(
  AdRequest.Builder("AD_UNIT_ID").build(),
  object : AdLoadCallback<RewardedInterstitialAd> {
    override fun onAdLoaded(ad: RewardedInterstitialAd) {
      // Called when an ad has loaded.
      ad.adEventCallback = object : RewardedInterstitialAdEventCallback {
      }
      rewardedInterstitialAd = ad
    }

    override fun onAdFailedToLoad(loadAdError: LoadAdError) {
      // Called when ad fails to load.
    }
  }
)
```

### Load a rewarded interstitial ad (Java)

Load a rewarded interstitial ad with the ad unit ID on the request and set the `RewardedInterstitialAdEventCallback` on the loaded ad.

```java
RewardedInterstitialAd.load(
  new AdRequest.Builder("AD_UNIT_ID").build(),
  new AdLoadCallback<RewardedInterstitialAd>() {
    @Override
    public void onAdLoaded(@NonNull RewardedInterstitialAd ad) {
      // Called when an ad has loaded.
      ad.setAdEventCallback(new RewardedInterstitialAdEventCallback() {});
      rewardedInterstitialAd = ad;
    }

    @Override
    public void onAdFailedToLoad(@NonNull LoadAdError adError) {
      // Called when ad fails to load.
    }
  });
```

### Show a rewarded interstitial ad

Show a loaded rewarded interstitial ad and handle the user-earned-reward callback.

```kotlin
rewardedInterstitialAd?.show(
  this@RewardedInterstitialActivity,
  object : OnUserEarnedRewardListener {
    override fun onUserEarnedReward(rewardItem: RewardItem) {
      // User earned the reward.
      val rewardAmount = rewardItem.amount
      val rewardType = rewardItem.type
    }
  }
)
```

### Show a rewarded interstitial ad (Java)

Show a loaded rewarded interstitial ad and handle the user-earned-reward callback.

```java
rewardedInterstitialAd.show(
  this,
  new OnUserEarnedRewardListener() {
    @Override
    public void onUserEarnedReward(@NonNull RewardItem rewardItem) {
      // User earned the reward.
      int rewardAmount = rewardItem.getAmount();
      String rewardType = rewardItem.getType();
    }
  });
```

---
## 13. Migrate with AI Tools

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-with-ai-tools.md

Covers optimizing your AI model to help migrate from the legacy Google Mobile Ads SDK to GMA Next-Gen SDK by installing a migration agent skill.

### Install the migration skill

Installs the migration skill into the standard `.agents/skills` folder used by AI assistants.

```bash
npx skills add google/skills/skills/ads/google-mobile-ads --skill google-mobile-ads-android-migrate-to-next-gen --agent universal --yes
```

### Update the migration skill

Updates all installed skills to the latest version.

```bash
npx skills update --all
```

### Invoke the migration skill

Example prompt to invoke the skill in your AI assistant's box.

```bash
Migrate the files in my project from Google Mobile Ads SDK (Legacy) to GMA Next-Gen SDK
```

---

## 14. Integrate CMP

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/integrate-cmp.md

Covers the differences in consent management platform (CMP) integration between the legacy GMA SDK and GMA Next-Gen SDK. Ad unit deployment is not supported in the Next-Gen SDK; use the UMP SDK to display European regulations messages instead.

*No code samples in this guide.*

---

## 15. Test Ads

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/test-ads.md

Explains how to enable test ads during development using demo ad units or by programmatically registering test devices with `RequestConfiguration.Builder.setTestDeviceIds()`.

### Set test device IDs

Registers a device as a test device so production ad units serve test ads without charging advertisers.

```kotlin
val testDeviceIds = Arrays.asList("33BE2250B43518CCDA7DE426D04EE231")
val configuration = RequestConfiguration.Builder().setTestDeviceIds(testDeviceIds).build()
MobileAds.setRequestConfiguration(configuration)
```

### Set test device IDs (Java)

Registers a device as a test device so production ad units serve test ads without charging advertisers.

```java
List<String> testDeviceIds = Arrays.asList("33BE2250B43518CCDA7DE426D04EE231");
RequestConfiguration configuration =
new RequestConfiguration.Builder().setTestDeviceIds(testDeviceIds).build();
MobileAds.setRequestConfiguration(configuration);
```

---

## 16. Agent Skills

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/agent-skills.md

Covers installing and updating portable agent skills for GMA Next-Gen SDK that help AI assistants run complex tasks with higher accuracy. Works with Antigravity, Claude Code, Codex, and Cursor.

### Install the agent skills

Installs all agent skills for GMA Next-Gen SDK into the standard `.agents/skills` folder.

```bash
npx skills add google/skills/ads/google-mobile-ads --skill '*' --agent universal --yes
```

### Update the agent skills

Updates all agent skills to the latest version.

```bash
npx skills update --all
```

---

## 17. App Open

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/app-open.md

Guide for integrating app open ads using GMA Next-Gen SDK. App open ads are shown when users bring your app to the foreground and can be closed at any time.

### Extend the Application class

Initializes the Mobile Ads SDK on a background thread within an Application subclass that manages ads tied to the app lifecycle.

```kotlin
/** Application class that initializes, loads and show ads when activities change states. */
class MyApplication : Application() {

  override fun onCreate() {
    super<Application>.onCreate()
    CoroutineScope(Dispatchers.IO).launch {
      // Initialize the Mobile Ads SDK synchronously on a background thread.
      MobileAds.initialize(this@MyApplication, InitializationConfig.Builder(APP_ID).build()) {}
    }
  }

  private companion object {
    // Sample AdMob App ID.
    const val APP_ID = "ca-app-pub-3940256099942544~3347511713"
  }
}
```

### Extend the Application class (Java)

Initializes the Mobile Ads SDK on a background thread within an Application subclass that manages ads tied to the app lifecycle.

```java
/** Application class that initializes, loads and show ads when activities change states. */
public class MyApplication extends Application {

  // Sample AdMob App ID.
  private static final String APP_ID = "ca-app-pub-3940256099942544~3347511713";

  @Override
  public void onCreate() {
    super.onCreate();
    new Thread(
        () -> {
          // Initialize the SDK on a background thread.
          MobileAds.initialize(
              MyApplication.this,
              new InitializationConfig.Builder(APP_ID).build(),
              initializationStatus -> {});
        })
        .start();
  }
}
```

### Load an ad

Loads an app open ad with `AppOpenAd.load()` and handles the `AdLoadCallback` for success and failure.

```kotlin
/**
 * Load an ad.
 *
 * @param context a context used to perform UI-related operations (e.g. display Toast messages).
 *   Loading the app open ad itself does not require a context.
 */
fun loadAd(context: Context) {
  // Do not load ad if there is an unused ad or one is already loading.
  if (isLoadingAd || isAdAvailable()) {
    Log.d(Constant.TAG, "App open ad is either loading or has already loaded.")
    return
  }

  isLoadingAd = true
  AppOpenAd.load(
    AdRequest.Builder(AppOpenFragment.AD_UNIT_ID).build(),
    object : AdLoadCallback<AppOpenAd> {
      /**
       * Called when an app open ad has loaded.
       *
       * @param ad the loaded app open ad.
       */
      override fun onAdLoaded(ad: AppOpenAd) {
        // Called when an ad has loaded.
        appOpenAd = ad
        isLoadingAd = false
        Log.d(Constant.TAG, "App open ad loaded.")
      }

      /**
       * Called when an app open ad has failed to load.
       *
       * @param loadAdError the error.
       */
      override fun onAdFailedToLoad(loadAdError: LoadAdError) {
        isLoadingAd = false
        Log.w(Constant.TAG, "App open ad failed to load: $loadAdError")
      }
    },
  )
}
```

### Load an ad (Java)

Loads an app open ad with `AppOpenAd.load()` and handles the `AdLoadCallback` for success and failure.

```java
/**
 * Load an ad.
 *
 * @param context a context used to perform UI-related operations (e.g. display Toast messages).
 *     Loading the app open ad itself does not require a context.
 */
public void loadAd(@NonNull Context context) {
  // Do not load ad if there is an unused ad or one is already loading.
  if (isLoadingAd || isAdAvailable()) {
    Log.d(Constant.TAG, "App open ad is either loading or has already loaded.");
    return;
  }

  isLoadingAd = true;
  AppOpenAd.load(
      new AdRequest.Builder(AppOpenFragment.AD_UNIT_ID).build(),
      new AdLoadCallback<AppOpenAd>() {
        @Override
        public void onAdLoaded(@NonNull AppOpenAd ad) {
          appOpenAd = ad;
          isLoadingAd = false;
          Log.d(Constant.TAG, "App open ad loaded.");
        }

        @Override
        public void onAdFailedToLoad(@NonNull LoadAdError loadAdError) {
          isLoadingAd = false;
          Log.w(Constant.TAG, "App open ad failed to load: " + loadAdError);
        }
      });
}
```

### Show the ad

Shows the app open ad if available and sets `AppOpenAdEventCallback` to handle show, dismiss, click, and impression events.

```kotlin
/**
 * Show the ad if one isn't already showing.
 *
 * @param activity the activity that shows the app open ad.
 * @param onShowAdCompleteListener the listener to be notified when an app open ad is complete.
 */
fun showAdIfAvailable(activity: Activity, onShowAdCompleteListener: OnShowAdCompleteListener?) {
  // If the app open ad is already showing, do not show the ad again.
  if (isShowingAd) {
    Log.d(Constant.TAG, "App open ad is already showing.")
    onShowAdCompleteListener?.onShowAdComplete()
    return
  }

  // If the app open ad is not available yet, invoke the callback.
  if (!isAdAvailable()) {
    Log.d(Constant.TAG, "App open ad is not ready yet.")
    onShowAdCompleteListener?.onShowAdComplete()
    return
  }

  appOpenAd?.adEventCallback =
    object : AppOpenAdEventCallback {
      override fun onAdShowedFullScreenContent() {
        Log.d(Constant.TAG, "App open ad showed.")
      }

      override fun onAdDismissedFullScreenContent() {
        Log.d(Constant.TAG, "App open ad dismissed.")
        appOpenAd = null
        isShowingAd = false
        onShowAdCompleteListener?.onShowAdComplete()
        loadAd(activity)
      }

      override fun onAdFailedToShowFullScreenContent(
        fullScreenContentError: FullScreenContentError
      ) {
        appOpenAd = null
        isShowingAd = false
        Log.w(Constant.TAG, "App open ad failed to show: $fullScreenContentError")
        onShowAdCompleteListener?.onShowAdComplete()
        loadAd(activity)
      }

      override fun onAdImpression() {
        Log.d(Constant.TAG, "App open ad recorded an impression.")
      }

      override fun onAdClicked() {
        Log.d(Constant.TAG, "App open ad recorded a click.")
      }
    }

  isShowingAd = true
  appOpenAd?.show(activity)
}
```

### Show the ad (Java)

Shows the app open ad if available and sets `AppOpenAdEventCallback` to handle show, dismiss, click, and impression events.

```java
/**
 * Show the ad if one isn't already showing.
 *
 * @param activity the activity that shows the app open ad.
 * @param onShowAdCompleteListener the listener to be notified when an app open ad is complete.
 */
public void showAdIfAvailable(
    @NonNull Activity activity, @Nullable OnShowAdCompleteListener onShowAdCompleteListener) {
  // If the app open ad is already showing, do not show the ad again.
  if (isShowingAd) {
    Log.d(Constant.TAG, "App open ad is already showing.");
    if (onShowAdCompleteListener != null) {
      onShowAdCompleteListener.onShowAdComplete();
    }
    return;
  }

  // If the app open ad is not available yet, invoke the callback.
  if (!isAdAvailable()) {
    Log.d(Constant.TAG, "App open ad is not ready yet.");
    if (onShowAdCompleteListener != null) {
      onShowAdCompleteListener.onShowAdComplete();
    }
    return;
  }

  appOpenAd.setAdEventCallback(
      new AppOpenAdEventCallback() {
        @Override
        public void onAdShowedFullScreenContent() {
          Log.d(Constant.TAG, "App open ad shown.");
        }

        @Override
        public void onAdDismissedFullScreenContent() {
          Log.d(Constant.TAG, "App open ad dismissed.");
          appOpenAd = null;
          isShowingAd = false;
          if (onShowAdCompleteListener != null) {
            onShowAdCompleteListener.onShowAdComplete();
          }
          loadAd(activity);
        }

        @Override
        public void onAdFailedToShowFullScreenContent(
            @NonNull FullScreenContentError fullScreenContentError) {
          appOpenAd = null;
          isShowingAd = false;
          Log.w(Constant.TAG, "App open ad failed to show: " + fullScreenContentError);
          if (onShowAdCompleteListener != null) {
            onShowAdCompleteListener.onShowAdComplete();
          }
          loadAd(activity);
        }

        @Override
        public void onAdImpression() {
          Log.d(Constant.TAG, "App open ad recorded an impression.");
        }

        @Override
        public void onAdClicked() {
          Log.d(Constant.TAG, "App open ad recorded a click.");
        }
      });

  isShowingAd = true;
  appOpenAd.show(activity);
}
```

---

## 18. Banner

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/banner.md

Guide for loading anchored adaptive banner ads into an Android app. Banners are rectangular ads that occupy a portion of an app's layout, anchored at the top or bottom of the screen.

### Create an AdView object

Creates an `AdView` object and adds it to the app's view hierarchy.

```kotlin
private fun createAdView(adViewContainer: FrameLayout, activity: Activity) {
  val adView = AdView(activity)
  adViewContainer.addView(adView)
}
```

### Create an AdView object (Java)

Creates an `AdView` object and adds it to the app's view hierarchy.

```java
private void createAdView(FrameLayout adViewContainer, Activity activity) {
  AdView adView = new AdView(activity);
  adViewContainer.addView(adView);
}
```

### Load an ad

Loads a 360-width large anchored adaptive banner ad into an `AdView` with an `AdLoadCallback`.

```kotlin
private fun loadBannerAd(adView: AdView, activity: Activity) {
  // Get a BannerAdRequest for a 360 wide large anchored adaptive banner ad.
  val adSize = AdSize.getLargeAnchoredAdaptiveBannerAdSize(activity, 360)
  val adRequest = BannerAdRequest.Builder(AD_UNIT_ID, adSize).build()

  adView.loadAd(
    adRequest,
    object : AdLoadCallback<BannerAd> {
      override fun onAdLoaded(ad: BannerAd) {
        Log.d(TAG, "Banner ad loaded.")
      }

      override fun onAdFailedToLoad(adError: LoadAdError) {
        Log.d(TAG, "Banner ad failed to load: $adError")
      }
    },
  )
}
```

### Load an ad (Java)

Loads a 360-width large anchored adaptive banner ad into an `AdView` with an `AdLoadCallback`.

```java
private void loadBannerAd(AdView adView, Activity activity) {
  // Get a BannerAdRequest for a 360 wide large anchored adaptive banner ad.
  AdSize adSize = AdSize.getLargeAnchoredAdaptiveBannerAdSize(activity, 360);
  BannerAdRequest adRequest = new BannerAdRequest.Builder(AD_UNIT_ID, adSize).build();

  adView.loadAd(
      adRequest,
      new AdLoadCallback<BannerAd>() {
        @Override
        public void onAdLoaded(@NonNull BannerAd bannerAd) {
          Log.d(TAG, "Banner ad loaded.");
        }

        @Override
        public void onAdFailedToLoad(@NonNull LoadAdError adError) {
          Log.d(TAG, "Banner ad failed to load: " + adError);
        }
      });
}
```

### Set ad event callbacks

Sets `BannerAdEventCallback` on the loaded banner ad to handle impressions, clicks, and full-screen content events.

```kotlin
override fun onAdLoaded(ad: BannerAd) {
  ad.adEventCallback =
    object : BannerAdEventCallback {
      override fun onAdImpression() {
        // Banner ad recorded an impression.
        Log.d(TAG, "Banner ad recorded an impression.")
      }

      override fun onAdClicked() {
        // Banner ad recorded a click.
        Log.d(TAG, "Banner ad clicked.")
      }

      override fun onAdShowedFullScreenContent() {
        // Banner ad showed.
        Log.d(TAG, "Banner ad showed full screen content.")
      }

      override fun onAdDismissedFullScreenContent() {
        // Banner ad dismissed.
        Log.d(TAG, "Banner ad dismissed full screen content.")
      }

      override fun onAdFailedToShowFullScreenContent(
        fullScreenContentError: FullScreenContentError
      ) {
        // Banner ad failed to show.
        Log.w(TAG, "Banner ad failed to show full screen content: $fullScreenContentError")
      }
    }
}
```

### Set ad event callbacks (Java)

Sets `BannerAdEventCallback` on the loaded banner ad to handle impressions, clicks, and full-screen content events.

```java
@Override
public void onAdLoaded(@NonNull BannerAd bannerAd) {
  bannerAd.setAdEventCallback(
      new BannerAdEventCallback() {
        @Override
        public void onAdImpression() {
          // Banner ad recorded an impression.
          Log.d(TAG, "Banner ad recorded an impression.");
        }

        @Override
        public void onAdClicked() {
          // Banner ad recorded a click.
          Log.d(TAG, "Banner ad clicked.");
        }

        @Override
        public void onAdShowedFullScreenContent() {
          // Banner ad showed.
          Log.d(TAG, "Banner ad showed full screen content.");
        }

        @Override
        public void onAdDismissedFullScreenContent() {
          // Banner ad dismissed.
          Log.d(TAG, "Banner ad dismissed full screen content.");
        }

        @Override
        public void onAdFailedToShowFullScreenContent(
            @NonNull FullScreenContentError fullScreenContentError) {
          // Banner ad failed to show.
          Log.w(
              TAG,
              "Banner ad failed to show full screen content: " + fullScreenContentError);
        }
      });
}
```

---

## 19. Inline Adaptive

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/inline-adaptive.md

Guide for implementing inline adaptive banners, which are larger and taller than anchored adaptive banners with variable heights, placed in scrolling content.

### Implement inline adaptive banners

Creates an inline adaptive ad size, loads the ad via `BannerAd.load()`, and adds the ad view to the view hierarchy on the UI thread.

```kotlin
private fun loadAd() {
  // Create an inline adaptive ad size. 320 is a placeholder value.
  // Replace 320 with your banner container width.
  val adSize = AdSize.getCurrentOrientationInlineAdaptiveBannerAdSize(this, 320)

  // Step 1 - Create a BannerAdRequest object with ad unit ID and size.
  val adRequest = BannerAdRequest.Builder("AD_UNIT_ID", adSize).build()

  // Step 2 - Load the ad.
  BannerAd.load(
    adRequest,
    object : AdLoadCallback<BannerAd> {
      override fun onAdLoaded(ad: BannerAd) {
        // Assign the loaded ad to the BannerAd object.
        bannerAd = ad
        // Step 3 - Call BannerAd.getView() to get the View and add it
        // to view hierarchy on the UI thread.
        activity?.runOnUiThread {
          binding.bannerViewContainer.addView(ad.getView(requireActivity()))
        }
      }

      override fun onAdFailedToLoad(loadAdError: LoadAdError) {
        bannerAd = null
      }
    }
  )
}
```

### Implement inline adaptive banners (Java)

Creates an inline adaptive ad size, loads the ad via `BannerAd.load()`, and adds the ad view to the view hierarchy on the UI thread.

```java
private void loadAd() {
  // Create an inline adaptive ad size. 320 is a placeholder value.
  // Replace 320 with your banner container width.
  AdSize adSize = AdSize.getCurrentOrientationInlineAdaptiveBannerAdSize(this, 320);

  // Step 1 - Create a BannerAdRequest object with ad unit ID and size.
  BannerAdRequest adRequest = new BannerAdRequest.Builder("AD_UNIT_ID",
      adSize).build();

  // Step 2 - Load the ad.
  BannerAd.load(
      adRequest,
      new AdLoadCallback<BannerAd>() {
        @Override
        public void onAdLoaded(@NonNull BannerAd ad) {
          // Assign the loaded ad to the BannerAd object.
          bannerAd = ad;
          // Step 3 - Call BannerAd.getView() to get the View and add it
          // to view hierarchy on the UI thread.
          if (getActivity() != null) {
            getActivity()
                .runOnUiThread(() ->
                    binding.bannerViewContainer.addView(ad.getView(getActivity())));
          }
        }

        @Override
        public void onAdFailedToLoad(@NonNull LoadAdError adError) {
          bannerAd = null;
        }
      });
}
```

---

## 20. Collapsible

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/collapsible.md

Guide for implementing collapsible banner ads, which are initially presented as a larger overlay with a button to collapse them to the originally requested banner size.

### Load a collapsible banner ad

Loads a collapsible banner ad by passing a `Bundle` extra with the `collapsible` placement key in the ad request.

```kotlin
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
```

### Load a collapsible banner ad (Java)

Loads a collapsible banner ad by passing a `Bundle` extra with the `collapsible` placement key in the ad request.

```java
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
```

### Check if a loaded ad is collapsible

Checks whether the last loaded banner ad is collapsible using the `isCollapsible()` method.

```kotlin
override fun onAdLoaded(ad: BannerAd) {
  // ...
  Log.i(
    TAG,
    "The last loaded banner is ${if (ad.isCollapsible()) "" else "not "}collapsible."
  )
}
```

### Check if a loaded ad is collapsible (Java)

Checks whether the last loaded banner ad is collapsible using the `isCollapsible()` method.

```java
@Override
public void onAdLoaded(@NonNull BannerAd ad) {
  // ...
  Log.i(TAG, String.format("The last loaded banner is %scollapsible.",
      ad.isCollapsible() ? "" : "not "));
}
```

---

## 21. Fixed Size

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/fixed-size.md

Lists the standard fixed banner ad sizes (BANNER, LARGE_BANNER, MEDIUM_RECTANGLE, FULL_BANNER, LEADERBOARD) supported by the GMA Next-Gen SDK when adaptive banners don't meet your needs. The container must be at least as large as the banner.

*No code samples in this guide.*

---

## 22. Interstitial

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/interstitial.md

Guide for integrating full-screen interstitial ads that cover the host app's interface, displayed at natural transition points in the app flow.

### Load an interstitial ad

Loads a single interstitial ad with `InterstitialAd.load()` and handles the `AdLoadCallback` for success and failure.

```kotlin
import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback
import com.google.android.libraries.ads.mobile.sdk.common.AdRequest
import com.google.android.libraries.ads.mobile.sdk.common.FullScreenContentError
import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError
import com.google.android.libraries.ads.mobile.sdk.interstitial.InterstitialAd
import com.google.android.libraries.ads.mobile.sdk.interstitial.InterstitialAdEventCallback
import com.google.android.libraries.ads.mobile.sdk.MobileAds

class InterstitialActivity : Activity() {
  private var interstitialAd: InterstitialAd? = null

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // Load ads after you initialize GMA Next-Gen SDK.
    InterstitialAd.load(
      AdRequest.Builder(AD_UNIT_ID).build(),
      object : AdLoadCallback<InterstitialAd> {
        override fun onAdLoaded(ad: InterstitialAd) {
          // Interstitial ad loaded.
          interstitialAd = ad
        }

        override fun onAdFailedToLoad(adError: LoadAdError) {
          // Interstitial ad failed to load.
          interstitialAd = null
        }
      },
    )
  }

  companion object {
    // Sample interstitial ad unit ID.
    const val AD_UNIT_ID = "ca-app-pub-3940256099942544/1033173712"
  }
}
```

### Load an interstitial ad (Java)

Loads a single interstitial ad with `InterstitialAd.load()` and handles the `AdLoadCallback` for success and failure.

```java
import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback;
import com.google.android.libraries.ads.mobile.sdk.common.AdRequest;
import com.google.android.libraries.ads.mobile.sdk.common.FullScreenContentError;
import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError;
import com.google.android.libraries.ads.mobile.sdk.interstitial.InterstitialAd;
import com.google.android.libraries.ads.mobile.sdk.interstitial.InterstitialAdEventCallback;
import com.google.android.libraries.ads.mobile.sdk.MobileAds;

class InterstitialActivity extends Activity {
  // Sample interstitial ad unit ID.
  private static final String AD_UNIT_ID = "ca-app-pub-3940256099942544/1033173712";
  private InterstitialAd interstitialAd;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Load ads after you initialize GMA Next-Gen SDK.
    InterstitialAd.load(
        new AdRequest.Builder(AD_UNIT_ID).build(),
        new AdLoadCallback<InterstitialAd>() {
          @Override
          public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
            // Interstitial ad loaded.
            AdLoadCallback.super.onAdLoaded(interstitialAd);
            InterstitialActivity.this.interstitialAd = interstitialAd;
          }

          @Override
          public void onAdFailedToLoad(@NonNull LoadAdError adError) {
            // Interstitial ad failed to load.
            AdLoadCallback.super.onAdFailedToLoad(adError);
            interstitialAd = null;
          }
        }
    );
  }
}
```

### Set the InterstitialAdEventCallback

Sets the `InterstitialAdEventCallback` on the loaded interstitial ad to handle show, dismiss, impression, and click events.

```kotlin
// Listen for ad events.
interstitialAd?.adEventCallback =
  object : InterstitialAdEventCallback {
    override fun onAdShowedFullScreenContent() {
      // Interstitial ad did show.
    }

    override fun onAdDismissedFullScreenContent() {
      // Interstitial ad did dismiss.
      interstitialAd = null
    }

    override fun onAdFailedToShowFullScreenContent(
      fullScreenContentError: FullScreenContentError
    ) {
      // Interstitial ad failed to show.
      interstitialAd = null
    }

    override fun onAdImpression() {
      // Interstitial ad did record an impression.
    }

    override fun onAdClicked() {
      // Interstitial ad did record a click.
    }
  }
```

### Set the InterstitialAdEventCallback (Java)

Sets the `InterstitialAdEventCallback` on the loaded interstitial ad to handle show, dismiss, impression, and click events.

```java
// Listen for ad events.
interstitialAd.setAdEventCallback(
    new InterstitialAdEventCallback() {
      @Override
      public void onAdShowedFullScreenContent() {
        // Interstitial ad did show.
        InterstitialAdEventCallback.super.onAdShowedFullScreenContent();
      }

      @Override
      public void onAdDismissedFullScreenContent() {
        // Interstitial ad did dismiss.
        InterstitialAdEventCallback.super.onAdDismissedFullScreenContent();
        interstitialAd = null;
      }

      @Override
      public void onAdFailedToShowFullScreenContent(
          @NonNull FullScreenContentError fullScreenContentError) {
        // Interstitial ad failed to show.
        InterstitialAdEventCallback.super.onAdFailedToShowFullScreenContent(
            fullScreenContentError);
        initerstitialAd = null;
      }

      @Override
      public void onAdImpression() {
        // Interstitial ad did record an impression.
        InterstitialAdEventCallback.super.onAdImpression();
      }

      @Override
      public void onAdClicked() {
        // Interstitial ad did record a click.
        InterstitialAdEventCallback.super.onAdClicked();
      }
    }
);
```

### Show the ad

Shows the interstitial ad using the `show()` method.

```kotlin
// Show the ad.
interstitialAd?.show(this@InterstitialActivity)
```

### Show the ad (Java)

Shows the interstitial ad using the `show()` method.

```java
// Show the ad.
interstitialAd.show(InterstitialActivity.this);
```

---

## 23. Native

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/native.md

Guide for loading native ads, which are ad assets presented through UI components native to the platform and formatted to match your app's visual design.

### Load a native ad

Loads a native ad using `NativeAdLoader.load()` with a `NativeAdRequest` and `NativeAdLoaderCallback`.

```kotlin
import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError
import com.google.android.libraries.ads.mobile.sdk.nativead.NativeAd
import com.google.android.libraries.ads.mobile.sdk.nativead.NativeAdLoader
import com.google.android.libraries.ads.mobile.sdk.nativead.NativeAdLoaderCallback
import com.google.android.libraries.ads.mobile.sdk.nativead.NativeAdRequest

class NativeFragment : Fragment() {

  private var nativeAd: NativeAd? = null

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    loadAd()
  }

  private fun loadAd() {
    // Build an ad request with native ad options to customize the ad.
    val adRequest = NativeAdRequest
      .Builder(AD_UNIT_ID, listOf(NativeAd.NativeAdType.NATIVE))
      .build()

    val adCallback =
      object : NativeAdLoaderCallback {
        override fun onNativeAdLoaded(nativeAd: NativeAd) {
          // Called when a native ad has loaded.
        }
        override fun onAdFailedToLoad(adError: LoadAdError) {
          // Called when a native ad has failed to load.
        }
      }

    // Load the native ad with our request and callback.
    NativeAdLoader.load(adRequest, adCallback)
  }

  companion object {
    // Sample native ad unit ID.
    const val AD_UNIT_ID = "ca-app-pub-3940256099942544/2247696110"
  }
}
```

### Set the native ad event callback

Sets the `NativeAdEventCallback` on the loaded native ad to handle full-screen content, impression, and click events.

```kotlin
nativeAd.adEventCallback =
  object : NativeAdEventCallback {
    override fun onAdShowedFullScreenContent() {
      // Native ad showed full screen content.
    }
    override fun onAdDismissedFullScreenContent() {
      // Native ad dismissed full screen content.
    }
    override fun onAdFailedToShowFullScreenContent {
      // Native ad failed to show full screen content.
    }
    override fun onAdImpression() {
      // Native ad recorded an impression.
    }
    override fun onAdClicked() {
      // Native ad recorded a click.
    }
  }
```

---

## 24. Advanced

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/advanced.md

Guide for displaying loaded native ads using `NativeAdView`, including populating asset views, registering the ad, and handling clicks and impressions.

### Handle the loaded native ad

Handles the `onNativeAdLoaded` callback, inflates the `NativeAdView`, and adds it to the view hierarchy on the UI thread.

```kotlin
// Build an ad request with native ad options to customize the ad.
val adTypes = listOf(NativeAd.NativeAdType.NATIVE)
val adRequest = NativeAdRequest
  .Builder("ca-app-pub-3940256099942544/2247696110", adTypes)
  .build()

val adCallback =
  object : NativeAdLoaderCallback {
    override fun onNativeAdLoaded(nativeAd: NativeAd) {
      activity?.runOnUiThread {

        val nativeAdBinding = NativeAdBinding.inflate(layoutInflater)
        val adView = nativeAdBinding.root
        val frameLayout = myActivityLayout.nativeAdPlaceholder

        // Populate and register the native ad asset views.
        displayNativeAd(nativeAd, nativeAdBinding)

        // Remove all old ad views and add the new native ad
        // view to the view hierarchy.
        frameLayout.removeAllViews()
        frameLayout.addView(adView)
      }
    }
  }

// Load the native ad with our request and callback.
NativeAdLoader.load(adRequest, adCallback)
```

### Handle the loaded native ad (Java)

Handles the `onNativeAdLoaded` callback, inflates the `NativeAdView`, and adds it to the view hierarchy on the UI thread.

```java
// Build an ad request with native ad options to customize the ad.
List<NativeAd.NativeAdType> adTypes = Arrays.asList(NativeAd.NativeAdType.NATIVE);
NativeAdRequest adRequest = new NativeAdRequest
                                .Builder("ca-app-pub-3940256099942544/2247696110", adTypes)
                                .build();

NativeAdLoaderCallback adCallback = new NativeAdLoaderCallback() {
    @Override
    public void onNativeAdLoaded(NativeAd nativeAd) {
      if (getActivity() != null) {
        getActivity()
          .runOnUiThread(() -> {
            // Inflate the native ad view and add it to the view hierarchy.
            NativeAdBinding nativeAdBinding = NativeAdBinding.inflate(getLayoutInflater());
            NativeAdView adView = (NativeAdView) nativeAdBinding.getRoot();
            FrameLayout frameLayout = myActivityLayout.nativeAdPlaceholder;

            // Populate and register the native ad asset views.
            displayNativeAd(nativeAd, nativeAdBinding);

            // Remove all old ad views and add the new native ad
            // view to the view hierarchy.
            frameLayout.removeAllViews();
            frameLayout.addView(adView);
        });
      }
    }
};

// Load the native ad with our request and callback.
NativeAdLoader.load(adRequest, adCallback);
```

### Display the native ad

Populates and registers native ad asset views with the loaded `NativeAd` object, setting visibility for available assets.

```kotlin
private fun displayNativeAd(nativeAd: NativeAd, nativeAdBinding : NativeAdBinding) {
  // Set the native ad view elements.
  val nativeAdView = nativeAdBinding.root
  nativeAdView.advertiserView = nativeAdBinding.adAdvertiser
  nativeAdView.bodyView = nativeAdBinding.adBody
  nativeAdView.callToActionView = nativeAdBinding.adCallToAction
  nativeAdView.headlineView = nativeAdBinding.adHeadline
  nativeAdView.iconView = nativeAdBinding.adAppIcon
  nativeAdView.priceView = nativeAdBinding.adPrice
  nativeAdView.starRatingView = nativeAdBinding.adStars
  nativeAdView.storeView = nativeAdBinding.adStore

  // Set the view element with the native ad assets.
  nativeAdBinding.adAdvertiser.text = nativeAd.advertiser
  nativeAdBinding.adBody.text = nativeAd.body
  nativeAdBinding.adCallToAction.text = nativeAd.callToAction
  nativeAdBinding.adHeadline.text = nativeAd.headline
  nativeAdBinding.adAppIcon.setImageDrawable(nativeAd.icon?.drawable)
  nativeAdBinding.adPrice.text = nativeAd.price
  nativeAd.starRating?.toFloat().let { value ->
    nativeAdBinding.adStars.rating = value
  }
  nativeAdBinding.adStore.text = nativeAd.store

  // Hide views for assets that don't have data.
  nativeAdBinding.adAdvertiser.visibility = getAssetViewVisibility(nativeAd.advertiser)
  nativeAdBinding.adBody.visibility = getAssetViewVisibility(nativeAd.body)
  nativeAdBinding.adCallToAction.visibility = getAssetViewVisibility(nativeAd.callToAction)
  nativeAdBinding.adHeadline.visibility = getAssetViewVisibility(nativeAd.headline)
  nativeAdBinding.adAppIcon.visibility = getAssetViewVisibility(nativeAd.icon)
  nativeAdBinding.adPrice.visibility = getAssetViewVisibility(nativeAd.price)
  nativeAdBinding.adStars.visibility = getAssetViewVisibility(nativeAd.starRating)
  nativeAdBinding.adStore.visibility = getAssetViewVisibility(nativeAd.store)

  // Inform GMA Next-Gen SDK that you have finished populating
  // the native ad views with this native ad.
  nativeAdView.registerNativeAd(nativeAd, nativeAdBinding.adMedia)
}

/**
* Determines the visibility of an asset view based on the presence of its asset.
*
* @param asset The native ad asset to check for nullability.
* @return [View.VISIBLE] if the asset is not null, [View.INVISIBLE] otherwise.
*/
private fun getAssetViewVisibility(asset: Any?): Int {
  return if (asset == null) View.INVISIBLE else View.VISIBLE
}
```

### Display the native ad (Java)

Populates and registers native ad asset views with the loaded `NativeAd` object, setting visibility for available assets.

```java
private void displayNativeAd(ad: NativeAd, nativeAdBinding : NativeAdBinding) {
  // Set the native ad view elements.
  NativeAdView nativeAdView = nativeAdBinding.getRoot();
  nativeAdView.setAdvertiserView(nativeAdBinding.adAdvertiser);
  nativeAdView.setBodyView(nativeAdBinding.adBody);
  nativeAdView.setCallToActionView(nativeAdBinding.adCallToAction);
  nativeAdView.setHeadlineView(nativeAdBinding.adHeadline);
  nativeAdView.setIconView(nativeAdBinding.adAppIcon);
  nativeAdView.setPriceView(nativeAdBinding.adPrice);
  nativeAdView.setStarRatingView(nativeAdBinding.adStars);
  nativeAdView.setStoreView(nativeAdBinding.adStore);

  // Set the view element with the native ad assets.
  nativeAdBinding.adAdvertiser.setText(nativeAd.getAdvertiser());
  nativeAdBinding.adBody.setText(nativeAd.getBody());
  nativeAdBinding.adCallToAction.setText(nativeAd.getCallToAction());
  nativeAdBinding.adHeadline.setText(nativeAd.getHeadline());
  if (nativeAd.getIcon() != null) {
      nativeAdBinding.adAppIcon.setImageDrawable(nativeAd.getIcon().getDrawable());
  }
  nativeAdBinding.adPrice.setText(nativeAd.getPrice());
  if (nativeAd.getStarRating() != null) {
      nativeAdBinding.adStars.setRating(nativeAd.getStarRating().floatValue());
  }
  nativeAdBinding.adStore.setText(nativeAd.getStore());

  // Hide views for assets that don't have data.
  nativeAdBinding.adAdvertiser.setVisibility(getAssetViewVisibility(nativeAd.getAdvertiser()));
  nativeAdBinding.adBody.setVisibility(getAssetViewVisibility(nativeAd.getBody()));
  nativeAdBinding.adCallToAction.setVisibility(getAssetViewVisibility(nativeAd.getCallToAction()));
  nativeAdBinding.adHeadline.setVisibility(getAssetViewVisibility(nativeAd.getHeadline()));
  nativeAdBinding.adAppIcon.setVisibility(getAssetViewVisibility(nativeAd.getIcon()));
  nativeAdBinding.adPrice.setVisibility(getAssetViewVisibility(nativeAd.getPrice()));
  nativeAdBinding.adStars.setVisibility(getAssetViewVisibility(nativeAd.getStarRating()));
  nativeAdBinding.adStore.setVisibility(getAssetViewVisibility(nativeAd.getStore()));

  // Inform GMA Next-Gen SDK that you have finished populating
  // the native ad views with this native ad.
  nativeAdView.registerNativeAd(nativeAd, nativeAdBinding.adMedia);
}

/**
* Determines the visibility of an asset view based on the presence of its asset.
*
* @param asset The native ad asset to check for nullability.
* @return {@link View#VISIBLE} if the asset is not null, {@link View#INVISIBLE} otherwise.
*/
private int getAssetViewVisibility(Object asset) {
    return (asset == null) ? View.INVISIBLE : View.VISIBLE;
}
```

---
## 25. Full Screen

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/full-screen.md

Best practices for creating full-screen native ad experiences (popular in social/entertainment apps), covering AdChoices placement, unique ad unit IDs, consistent media view sizing, and requesting specific media aspect ratios.

### Request a specific media aspect ratio (Kotlin)

Instruct the SDK to prefer portrait, landscape, or square media assets to match your full-screen layout.

```kotlin
    val adRequest = NativeAdRequest.Builder(adUnitId, listOf(NativeAd.NativeAdType.NATIVE))
       .setMediaAspectRatio(NativeAd.NativeMediaAspectRatio.PORTRAIT)
       .build()
```

### Request a specific media aspect ratio (Java)

Instruct the SDK to prefer portrait, landscape, or square media assets to match your full-screen layout.

```java
    List<NativeAd.NativeAdType> adTypes = Arrays.asList(NativeAd.NativeAdType.NATIVE);
    NativeAdRequest adRequest = new NativeAdRequest.Builder(adUnitId, adTypes)
       .setMediaAspectRatio(NativeAd.NativeMediaAspectRatio.PORTRAIT)
       .build();
```

---

## 26. Options

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/options.md

Advanced native ad features: preferred media aspect ratio controls, image download control, AdChoices placements, video start-mute behavior, and custom click gestures.

### Preferred media aspect ratio (Kotlin)

Specify a preferred aspect ratio for ad creatives via `NativeAdRequest.Builder.setMediaAspectRatio()`.

```kotlin
    val adRequest = NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .setMediaAspectRatio(NativeAd.NATIVE_MEDIA_ASPECT_RATIO_LANDSCAPE)
      .build()
```

### Preferred media aspect ratio (Java)

Specify a preferred aspect ratio for ad creatives via `NativeAdRequest.Builder.setMediaAspectRatio()`.

```java
    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .setMediaAspectRatio(NativeAd.NATIVE_MEDIA_ASPECT_RATIO_LANDSCAPE)
      .build();
```

### Disable image download and read the image URI (Kotlin)

Use `disableImageDownloading()` so the SDK returns only the image URI, then retrieve it in the load callback.

```kotlin
    val adRequest = NativeAdRequest.Builder(
        "ca-app-pub-3940256099942544/2247696110",
        listOf(NativeAd.NativeAdType.NATIVE))
        .setMediaAspectRatio(NativeAd.NATIVE_MEDIA_ASPECT_RATIO_LANDSCAPE)
        .disableImageDownloading()
        .build()

    val adCallback: NativeAdLoaderCallback =
      object : NativeAdLoaderCallback {
        override fun onNativeAdLoaded(nativeAd: NativeAd) {
          // Get the image uri.
          val imageUri = nativeAd.image?.uri
        }
      };

    // Load the native ad with the ad request and callback.
    NativeAdLoader.load(adRequest, adLoaderCallback);
```

### Disable image download and read the image URI (Java)

Use `disableImageDownloading()` so the SDK returns only the image URI, then retrieve it in the load callback.

```java
    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .disableImageDownloading()
      .build();

    NativeAdLoaderCallback adLoaderCallback =
      new NativeAdLoaderCallback() {
        @Override
        public void onNativeAdLoaded(@NonNull NativeAd nativeAd) {
          // Get the image uri.
          Uri imageUri = nativeAd.getImage().getUri();
        }
      };

    // Load the native ad with the ad request and callback.
    NativeAdLoader.load(adRequest, adLoaderCallback);
```

### Set AdChoices placement (Kotlin)

Position the AdChoices icon in one of the four corners using `setAdChoicesPlacement()`.

```kotlin
    val adRequest = NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .setAdChoicesPlacement(NativeAdOptions.ADCHOICES_BOTTOM_RIGHT)
      .build()
```

### Set AdChoices placement (Java)

Position the AdChoices icon in one of the four corners using `setAdChoicesPlacement()`.

```java
    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .setAdChoicesPlacement(NativeAdOptions.ADCHOICES_BOTTOM_RIGHT)
      .build();
```

### Start video unmuted (Kotlin)

Configure `VideoOptions` with `setStartMuted(false)` and attach it to the native ad request.

```kotlin
    val videoOptions = VideoOptions.Builder()
      .setStartMuted(false)
      .build()

    val adRequest = NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .setVideoOptions(videoOptions)
      .build()
```

### Start video unmuted (Java)

Configure `VideoOptions` with `setStartMuted(false)` and attach it to the native ad request.

```java
    VideoOptions videoOptions = VideoOptions.Builder()
      .setStartMuted(false)
      .build()

    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .setVideoOptions(videoOptions)
      .build()
```

### Enable custom click gestures (Kotlin)

Register swipe gestures as ad clicks via `enableCustomClickGestureDirection()`, optionally preserving normal tap behavior.

```kotlin
    val adOptions = NativeAdOptions
      .Builder()
      .enableCustomClickGestureDirection(
        /* swipeDirection */ NativeAdOptions.SWIPE_GESTURE_DIRECTION_RIGHT,
        /* tapsAllowed= */ true)
      .build();

    // You can use the following sample ad unit ID to test custom click gestures.
    val adRequest = NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .withNativeAdOptions(adOptions)
      .build();
```

### Enable custom click gestures (Java)

Register swipe gestures as ad clicks via `enableCustomClickGestureDirection()`, optionally preserving normal tap behavior.

```java
    NativeAdOptions adOptions = new NativeAdOptions
      .Builder()
      .enableCustomClickGestureDirection(
        /* swipeDirection */ NativeAdOptions.SWIPE_GESTURE_DIRECTION_RIGHT,
        /* tapsAllowed= */ true)
      .build();

    // You can use the following sample ad unit ID to test custom click gestures.
    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .withNativeAdOptions(adOptions)
      .build();
```

---

## 27. Validator

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/validator.md

Native Validator catches policy violations in native ad placements before shipping. It appears only on test ads (never live ads) and is enabled by default; it can be disabled during SDK initialization.

### Disable Native Validator during initialization (Kotlin)

Call `setNativeValidatorDisabled()` on the `InitializationConfig` passed to `MobileAds.initialize()`.

```kotlin
    MobileAds.initialize(
      this@MainActivity,
      // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
      InitializationConfig.Builder("SAMPLE_APP_ID")
        .setNativeValidatorDisabled()
        .build()
      ) {
        // Adapter initialization is complete.
    }
```

### Disable Native Validator during initialization (Java)

Call `setNativeValidatorDisabled()` on the `InitializationConfig` passed to `MobileAds.initialize()`.

```java
    MobileAds.initialize(
        this,
        // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
        new InitializationConfig.Builder("SAMPLE_APP_ID")
            .setNativeValidatorDisabled()
            .build(),
        initializationStatus -> {
            // Adapter initialization is complete.
        });
```

---

## 28. Video Ads

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/video-ads.md

Access the `MediaContent` object on a native ad to read media info (aspect ratio, duration) and to control video playback via `VideoController` lifecycle callbacks.

### Get media aspect ratio and duration (Java)

Retrieve the `MediaContent` from a `NativeAd` and read its aspect ratio and video duration.

```java
    if (nativeAd.getMediaContent() != null) {
      MediaContent mediaContent = nativeAd.getMediaContent();
      float mediaAspectRatio = mediaContent.getAspectRatio();
      if (mediaContent.hasVideoContent()) {
        float duration = mediaContent.getDuration();
      }
    }
```

### Get media aspect ratio and duration (Kotlin)

Retrieve the `MediaContent` from a `NativeAd` and read its aspect ratio and video duration.

```kotlin
    nativeAd.mediaContent?.let { mediaContent ->
      val mediaAspectRatio: Float = mediaContent.aspectRatio
      if (mediaContent.hasVideoContent()) {
        val duration: Float = mediaContent.duration
      }
    }
```

### Handle video lifecycle callbacks (Java)

Extend `VideoController.VideoLifecycleCallbacks` and register it on the `VideoController` to observe video start/play/pause/end/mute events.

```java
    if (nativeAd.getMediaContent() != null) {
      VideoController videoController = nativeAd.getMediaContent().getVideoController();
      if (videoController != null) {
        videoController.setVideoLifecycleCallbacks(
            new VideoController.VideoLifecycleCallbacks() {
              @Override
              public void onVideoStart() {
                Log.d(TAG, "Video started.");
              }

              @Override
              public void onVideoPlay() {
                Log.d(TAG, "Video played.");
              }

              @Override
              public void onVideoPause() {
                Log.d(TAG, "Video paused.");
              }

              @Override
              public void onVideoEnd() {
                Log.d(TAG, "Video ended.");
              }

              @Override
              public void onVideoMute(boolean isMuted) {
                Log.d(TAG, "Video isMuted: " + isMuted + ".");
              }
            });
      }
    }
```

### Handle video lifecycle callbacks (Kotlin)

Extend `VideoController.VideoLifecycleCallbacks` and register it on the `VideoController` to observe video start/play/pause/end/mute events.

```kotlin
    val videoLifecycleCallbacks =
      object : VideoController.VideoLifecycleCallbacks() {
        override fun onVideoStart() {
          Log.d(TAG, "Video started.")
        }

        override fun onVideoPlay() {
          Log.d(TAG, "Video played.")
        }

        override fun onVideoPause() {
          Log.d(TAG, "Video paused.")
        }

        override fun onVideoEnd() {
          Log.d(TAG, "Video ended.")
        }

        override fun onVideoMute(isMuted: Boolean) {
          Log.d(TAG, "Video isMuted: $isMuted.")
        }
      }
    nativeAd.mediaContent?.videoController?.videoLifecycleCallbacks = videoLifecycleCallbacks
```

---

## 29. Rewarded

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/rewarded.md

Rewarded ads reward users with in-app items for interacting with video ads, playable ads, and surveys. Covers the single ad loading API, the ad preloading API, the full-screen event callback, and showing the ad with an `OnUserEarnedRewardListener`.

### Load a rewarded ad with the single ad loading API (Kotlin)

Call `RewardedAd.load()` with an `AdRequest` and an `AdLoadCallback` to receive the loaded ad or load error.

```kotlin
    import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback
    import com.google.android.libraries.ads.mobile.sdk.common.AdRequest
    import com.google.android.libraries.ads.mobile.sdk.common.FullScreenContentError
    import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError
    import com.google.android.libraries.ads.mobile.sdk.rewarded.RewardedAd
    import com.google.android.libraries.ads.mobile.sdk.rewarded.RewardedAdEventCallback
    import com.google.android.libraries.ads.mobile.sdk.MobileAds

    class RewardedActivity : Activity() {
      private var rewardedAd: RewardedAd? = null

      override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Load ads after you inititalize GMA Next-Gen SDK.
        RewardedAd.load(
          AdRequest.Builder(AD_UNIT_ID).build(),
          object : AdLoadCallback<RewardedAd> {
            override fun onAdLoaded(ad: RewardedAd) {
              // Rewarded ad loaded.
              rewardedAd = ad
            }

            override fun onAdFailedToLoad(adError: LoadAdError) {
              // Rewarded ad failed to load.
              rewardedAd = null
            }
          },
        )
      }

      companion object {
        // Sample rewarded ad unit ID.
        const val AD_UNIT_ID = "ca-app-pub-3940256099942544/5224354917"
      }
    }
```

### Load a rewarded ad with the single ad loading API (Java)

Call `RewardedAd.load()` with an `AdRequest` and an `AdLoadCallback` to receive the loaded ad or load error.

```java
    import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback;
    import com.google.android.libraries.ads.mobile.sdk.common.AdRequest;
    import com.google.android.libraries.ads.mobile.sdk.common.FullScreenContentError;
    import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError;
    import com.google.android.libraries.ads.mobile.sdk.rewarded.RewardedAd;
    import com.google.android.libraries.ads.mobile.sdk.rewarded.RewardedAdEventCallback;
    import com.google.android.libraries.ads.mobile.sdk.MobileAds;

    class RewardedActivity extends Activity {
      // Sample rewarded ad unit ID.
      private static final String AD_UNIT_ID = "ca-app-pub-3940256099942544/5224354917";
      private RewardedAd rewardedAd;

      @Override
      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Load ads after you initialize GMA Next-Gen SDK.
        RewardedAd.load(
            new AdRequest.Builder(AD_UNIT_ID).build(),
            new AdLoadCallback<RewardedAd>() {
              @Override
              public void onAdLoaded(@NonNull RewardedAd rewardedAd) {
                // Rewarded ad loaded.
                AdLoadCallback.super.onAdLoaded(rewardedAd);
                RewardedActivity.this.rewardedAd = rewardedAd;
              }

              @Override
              public void onAdFailedToLoad(@NonNull LoadAdError adError) {
                // Rewarded ad failed to load.
                AdLoadCallback.super.onAdFailedToLoad(adError);
                rewardedAd = null;
              }
            }
        );
      }
    }
```

### Set the RewardedAdEventCallback (Kotlin)

Before showing the ad, assign a `RewardedAdEventCallback` to handle show/dismiss/failure/impression/click events.

```kotlin
    // Listen for ad events.
    rewardedAd?.adEventCallback =
      object : RewardedAdEventCallback {
        override fun onAdShowedFullScreenContent() {
          // Rewarded ad did show.
        }

        override fun onAdDismissedFullScreenContent() {
          // Rewarded ad did dismiss.
          rewardedAd = null
        }

        override fun onAdFailedToShowFullScreenContent(
          fullScreenContentError: FullScreenContentError
        ) {
          // Rewarded ad failed to show.
          rewardedAd = null
        }

        override fun onAdImpression() {
          // Rewarded ad did record an impression.
        }

        override fun onAdClicked() {
          // Rewarded ad did record a click.
        }
      }
```

### Set the RewardedAdEventCallback (Java)

Before showing the ad, assign a `RewardedAdEventCallback` to handle show/dismiss/failure/impression/click events.

```java
    // Listen for ad events.
    rewardedAd.setAdEventCallback(
        new RewardedAdEventCallback() {
          @Override
          public void onAdShowedFullScreenContent() {
            // Rewarded ad did show.
            RewardedAdEventCallback.super.onAdShowedFullScreenContent();
          }

          @Override
          public void onAdDismissedFullScreenContent() {
            // Rewarded ad did dismiss.
            RewardedAdEventCallback.super.onAdDismissedFullScreenContent();
            rewardedAd = null;
          }

          @Override
          public void onAdFailedToShowFullScreenContent(
              @NonNull FullScreenContentError fullScreenContentError) {
            // Rewarded ad failed to show.
            RewardedAdEventCallback.super.onAdFailedToShowFullScreenContent(
                fullScreenContentError);
            rewardedAd = null;
          }

          @Override
          public void onAdImpression() {
            // Rewarded ad did record an impression.
            RewardedAdEventCallback.super.onAdImpression();
          }

          @Override
          public void onAdClicked() {
            // Rewarded ad did record a click.
            RewardedAdEventCallback.super.onAdClicked();
          }
        }
    );
```

### Show the rewarded ad and handle the reward (Kotlin)

Call `show()` with an `OnUserEarnedRewardListener` to display the ad and receive the earned `RewardItem`.

```kotlin
    // Show the ad.
    rewardedAd?.show(
      this@RewardedActivity,
      object : OnUserEarnedRewardListener {
        override fun onUserEarnedReward(rewardItem: RewardItem) {
          // User earned the reward.
          val rewardAmount = rewardItem.amount
          val rewardType = rewardItem.type
        }
      },
    )
```

### Show the rewarded ad and handle the reward (Java)

Call `show()` with an `OnUserEarnedRewardListener` to display the ad and receive the earned `RewardItem`.

```java
    // Show the ad.
    rewardedAd.show(
        RewardedActivity.this,
        new OnUserEarnedRewardListener() {
          @Override
          public void onUserEarnedReward(@NonNull RewardItem rewardItem) {
            // User earned the reward.
            int rewardAmount = rewardItem.getAmount();
            String rewardType = rewardItem.getType();
        }
    });
```

---

## 30. Rewarded Interstitial

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/rewarded-interstitial.md

Rewarded interstitial is an incentivized ad format shown during natural app transitions where users aren't required to opt in. Covers single loading, preloading, the full-screen event callback, and showing the ad with an `OnUserEarnedRewardListener`.

### Load a rewarded interstitial ad (Kotlin)

Call `RewardedInterstitialAd.load()` with an `AdRequest` and an `AdLoadCallback`.

```kotlin
    import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback
    import com.google.android.libraries.ads.mobile.sdk.common.AdRequest
    import com.google.android.libraries.ads.mobile.sdk.common.FullScreenContentError
    import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError
    import com.google.android.libraries.ads.mobile.sdk.rewardedinterstitial.RewardedInterstitialAd
    import com.google.android.libraries.ads.mobile.sdk.rewardedinterstitial.RewardedInterstitialAdEventCallback
    import com.google.android.libraries.ads.mobile.sdk.MobileAds

    class RewardedInterstitialActivity : Activity() {
      private var rewardedInterstitialAd: RewardedInterstitialAd? = null

      override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Load ads after you initialize GMA Next-Gen SDK.
        RewardedInterstitialAd.load(
          AdRequest.Builder(AD_UNIT_ID).build(),
          object : AdLoadCallback<RewardedInterstitialAd> {
            override fun onAdLoaded(ad: RewardedInterstitialAd) {
              // Rewarded interstitial ad loaded.
              rewardedInterstitialAd = ad
            }

            override fun onAdFailedToLoad(adError: LoadAdError) {
              // Rewarded interstitial ad failed to load.
              rewardedInterstitialAd = null
            }
          },
        )
      }

      companion object {
        // Sample rewarded interstitial ad unit ID.
        const val AD_UNIT_ID = "ca-app-pub-3940256099942544/5354046379"
      }
    }
```

### Load a rewarded interstitial ad (Java)

Call `RewardedInterstitialAd.load()` with an `AdRequest` and an `AdLoadCallback`.

```java
    import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback;
    import com.google.android.libraries.ads.mobile.sdk.common.AdRequest;
    import com.google.android.libraries.ads.mobile.sdk.common.FullScreenContentError;
    import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError;
    import com.google.android.libraries.ads.mobile.sdk.rewardedinterstitial.RewardedInterstitialAd;
    import com.google.android.libraries.ads.mobile.sdk.rewardedinterstitial.RewardedInterstitialAdEventCallback;
    import com.google.android.libraries.ads.mobile.sdk.MobileAds;

    class RewardedActivity extends Activity {
      // Sample rewarded interstitial ad unit ID.
      private static final String AD_UNIT_ID = "ca-app-pub-3940256099942544/5354046379";
      private RewardedInterstitialAd rewardedInterstitialAd;

      @Override
      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Load ads after you initialize GMA Next-Gen SDK.
        RewardedInterstitialAd.load(
            new AdRequest.Builder(AD_UNIT_ID).build(),
            new AdLoadCallback<RewardedInterstitialAd>() {
              @Override
              public void onAdLoaded(@NonNull RewardedInterstitialAd rewardedInterstitialAd) {
                // Rewarded interstitial ad loaded.
                AdLoadCallback.super.onAdLoaded(rewardedInterstitialAd);
                RewardedActivity.this.rewardedInterstitialAd = rewardedInterstitialAd;
              }

              @Override
              public void onAdFailedToLoad(@NonNull LoadAdError adError) {
                // Rewarded interstitial ad failed to load.
                AdLoadCallback.super.onAdFailedToLoad(adError);
                rewardedInterstitialAd = null;
              }
            }
        );
      }
    }
```

### Set the RewardedInterstitialAdEventCallback (Kotlin)

Before showing the ad, assign a `RewardedInterstitialAdEventCallback` to handle full-screen show/dismiss/failure/impression/click events.

```kotlin
    // Listen for ad events.
    rewardedInterstitialAd?.adEventCallback =
      object : RewardedInterstitialAdEventCallback {
        override fun onAdShowedFullScreenContent() {
          // Rewarded interstitial ad did show.
        }

        override fun onAdDismissedFullScreenContent() {
          // Rewarded interstitial ad did dismiss.
          rewardedInterstitialAd = null
        }

        override fun onAdFailedToShowFullScreenContent(
          fullScreenContentError: FullScreenContentError
        ) {
          // Rewarded interstitial ad failed to show.
          rewardedInterstitialAd = null
        }

        override fun onAdImpression() {
          // Rewarded interstitial ad did record an impression.
        }

        override fun onAdClicked() {
          // Rewarded interstitial ad did record a click.
        }
      }
```

### Set the RewardedInterstitialAdEventCallback (Java)

Before showing the ad, assign a `RewardedInterstitialAdEventCallback` to handle full-screen show/dismiss/failure/impression/click events.

```java
    // Listen for ad events.
    rewardedInterstitialAd.setAdEventCallback(
        new RewardedInterstitialAdEventCallback() {
          @Override
          public void onAdShowedFullScreenContent() {
            // Rewarded interstitial ad did show.
            RewardedInterstitialAdEventCallback.super.onAdShowedFullScreenContent();
          }

          @Override
          public void onAdDismissedFullScreenContent() {
            // Rewarded interstitial ad did dismiss.
            RewardedInterstitialAdEventCallback.super.onAdDismissedFullScreenContent();
            rewardedInterstitialAd = null;
          }

          @Override
          public void onAdFailedToShowFullScreenContent(
              @NonNull FullScreenContentError fullScreenContentError) {
            // Rewarded interstitial ad failed to show.
            RewardedInterstitialAdEventCallback.super.onAdFailedToShowFullScreenContent(
                fullScreenContentError);
            rewardedInterstitialAd = null;
          }

          @Override
          public void onAdImpression() {
            // Rewarded interstitial ad did record an impression.
            RewardedInterstitialAdEventCallback.super.onAdImpression();
          }

          @Override
          public void onAdClicked() {
            // Rewarded interstitial ad did record a click.
            RewardedInterstitialAdEventCallback.super.onAdClicked();
          }
        }
    );
```

### Show the rewarded interstitial ad (Kotlin)

Call `show()` with an `OnUserEarnedRewardListener` to display the ad and receive the earned `RewardItem`.

```kotlin
    // Show the ad.
    rewardedInterstitialAd?.show(
      this@RewardedActivity,
      object : OnUserEarnedRewardListener {
        override fun onUserEarnedReward(rewardItem: RewardItem) {
          // User earned the reward.
          val rewardAmount = rewardItem.amount
          val rewardType = rewardItem.type
        }
      },
    )
```

### Show the rewarded interstitial ad (Java)

Call `show()` with an `OnUserEarnedRewardListener` to display the ad and receive the earned `RewardItem`.

```java
    // Show the ad.
    rewardedInterstitialAd.show(
        RewardedActivity.this,
        new OnUserEarnedRewardListener() {
          @Override
          public void onUserEarnedReward(@NonNull RewardItem rewardItem) {
            // User earned the reward.
            int rewardAmount = rewardItem.getAmount();
            String rewardType = rewardItem.getType();
        }
    });
```

---

## 31. Mediation

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/mediation.md

AdMob Mediation serves ads from multiple sources (AdMob Network and third-party ad sources) to maximize fill rate. Covers SDK initialization with adapter status checks, excluding legacy `com.google.android.gms` modules to avoid duplicate symbols, and checking which ad network adapter loaded an ad.

### Initialize GMA Next-Gen SDK and log adapter status (Kotlin)

Initialize on a background thread and iterate `initializationStatus.adapterStatusMap` to verify each adapter's state before loading ads.

```kotlin
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
```

### Initialize GMA Next-Gen SDK and log adapter status (Java)

Initialize on a background thread and iterate `initializationStatus.getAdapterStatusMap()` to verify each adapter's state before loading ads.

```java
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
```

### Exclude legacy play-services-ads modules (Kotlin)

Globally exclude `play-services-ads` and `play-services-ads-lite` from all dependencies in the app-level `build.gradle` to avoid duplicate symbol compile errors.

```kotlin
configurations.configureEach {
    exclude(group = "com.google.android.gms", module = "play-services-ads")
    exclude(group = "com.google.android.gms", module = "play-services-ads-lite")
}
```

### Exclude legacy play-services-ads modules (Groovy)

Globally exclude `play-services-ads` and `play-services-ads-lite` from all dependencies in the app-level `build.gradle` to avoid duplicate symbol compile errors.

```groovy
configurations.configureEach {
    exclude group: "com.google.android.gms", module = "play-services-ads"
    exclude group: "com.google.android.gms", module = "play-services-ads-lite"
}
```

### Check which ad network adapter loaded the ad (Kotlin)

Log the mediation adapter class name from `ResponseInfo` in the banner ad load callback.

```kotlin
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
```

### Check which ad network adapter loaded the ad (Java)

Log the mediation adapter class name from `ResponseInfo` in the banner ad load callback.

```java
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
```

---

## 32. Choose Networks

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/choose-networks.md

Lists the ad sources AdMob Mediation supports, including required open-source/versioned adapters, supported ad formats, bidding support, and ad source optimization support. Also covers adapter versioning and custom events for unsupported networks.

*No code samples in this guide.*

---

## 33. Meta

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/meta.md

Integrating Meta Audience Network (formerly Facebook) with AdMob Mediation via bidding. Covers adding gradle dependencies (with legacy GMS exclusion), privacy/GDPR settings, and extracting Meta-specific native ad asset extras.

### Add Meta Audience Network SDK and adapter dependencies (Kotlin)

Add the `ads-mobile-sdk` and `facebook` mediation adapter to the app-level gradle file, excluding legacy `play-services-ads` modules.

```kotlin
dependencies {
    implementation("com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk:1.2.1")
    implementation("com.google.ads.mediation:facebook:6.21.0.3")
}

configurations.configureEach {
    exclude(group = "com.google.android.gms", module = "play-services-ads")
    exclude(group = "com.google.android.gms", module = "play-services-ads-lite")
}
```

### Add Meta Audience Network SDK and adapter dependencies (Groovy)

Add the `ads-mobile-sdk` and `facebook` mediation adapter to the app-level gradle file, excluding legacy `play-services-ads` modules.

```groovy
dependencies {
    implementation 'com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk:1.2.1'
    implementation 'com.google.ads.mediation:facebook:6.21.0.3'
}

configurations.configureEach {
    exclude group: 'com.google.android.gms', module = 'play-services-ads'
    exclude group: 'com.google.android.gms', module = 'play-services-ads-lite'
}
```

### Extract Meta-specific native ad extras (Kotlin)

Meta native ad assets that don't map one-to-one to Google native ad assets are passed via `NativeAd.getExtras()`, e.g. the social context asset.

```kotlin
    val extras = nativeAd.getExtras()
    if (extras.containsKey(FacebookMediationAdapter.KEY_SOCIAL_CONTEXT_ASSET)) {
      var socialContext = extras.getString(FacebookMediationAdapter.KEY_SOCIAL_CONTEXT_ASSET)
      // ...
    }
```

### Extract Meta-specific native ad extras (Java)

Meta native ad assets that don't map one-to-one to Google native ad assets are passed via `NativeAd.getExtras()`, e.g. the social context asset.

```java
    Bundle extras = nativeAd.getExtras();
    if (extras.containsKey(FacebookMediationAdapter.KEY_SOCIAL_CONTEXT_ASSET)) {
        String socialContext = extras.getString(FacebookMediationAdapter.KEY_SOCIAL_CONTEXT_ASSET);
        // ...
    }
```

---

## 34. Troubleshoot Bidding

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/troubleshoot-bidding.md

Checklist for diagnosing improper bidding partner integrations (e.g. fewer ad requests than expected or a missing `a3p` parameter). Covers AdMob UI configuration, ad unit mappings, manifest app ID, and adapter/SDK version checks.

*No code samples in this guide.*

---

## 35. Setup

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/setup.md

Custom events let you add waterfall mediation for ad networks that aren't natively supported, by implementing a custom event adapter. Covers creating the adapter in the AdMob UI, implementing `initialize()`, and reporting adapter and SDK version numbers via `VersionInfo`.

### Initialize the custom event adapter (Java)

Extend `Adapter` and override `initialize()` to set up the third-party SDK, then call the completion callback on success.

```java
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
```

### Initialize the custom event adapter (Kotlin)

Extend `Adapter` and override `initialize()` to set up the third-party SDK, then call the completion callback on success.

```kotlin
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
```

### Report adapter and SDK version numbers (Java)

Override `getVersionInfo()` and `getSDKVersionInfo()` to return `VersionInfo` objects for the adapter and the third-party SDK.

```java
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
```

### Report adapter and SDK version numbers (Kotlin)

Override `getVersionInfo()` and `getSDKVersionInfo()` to return `VersionInfo` objects for the adapter and the third-party SDK.

```kotlin
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
```

---

## 36. Custom Events Banner

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/custom_events_banner.md

Implementing custom events for banner ads: the `Adapter.loadBannerAd()` entry point, the `SampleBannerCustomEventLoader` that loads from the third-party network and implements `MediationBannerAd`, and forwarding ad event callbacks back to GMA Next-Gen SDK.

### Implement loadBannerAd in the adapter (Java)

When the custom event line item is reached, `loadBannerAd()` is called on the adapter, which delegates to the loader.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationBannerAd;
import com.google.android.gms.ads.mediation.MediationBannerAdCallback;
...

public class SampleCustomEvent extends Adapter {
  private SampleBannerCustomEventLoader bannerLoader;
  @Override
  public void loadBannerAd(
      @NonNull MediationBannerAdConfiguration adConfiguration,
      @NonNull MediationAdLoadCallback<MediationBannerAd, MediationBannerAdCallback> callback) {
    bannerLoader = new SampleBannerCustomEventLoader(adConfiguration, callback);
    bannerLoader.loadAd();
  }
}
```

### Load the banner ad from the third-party network (Java)

`SampleBannerCustomEventLoader` reads the server parameter, configures the `SampleAdView` size from display metrics, and calls `fetchAd()`. It implements `MediationBannerAd`.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationBannerAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationBannerAd;
import com.google.android.gms.ads.mediation.MediationBannerAdCallback;
...

public class SampleBannerCustomEventLoader extends SampleAdListener implements MediationBannerAd {

  /** View to contain the sample banner ad. */
  private SampleAdView sampleAdView;

  /** Configuration for requesting the banner ad from the third-party network. */
  private final MediationBannerAdConfiguration mediationBannerAdConfiguration;

  /** Callback that fires on loading success or failure. */
  private final MediationAdLoadCallback<MediationBannerAd, MediationBannerAdCallback>
      mediationAdLoadCallback;

  /** Callback for banner ad events. */
  private MediationBannerAdCallback bannerAdCallback;

  /** Constructor. */
  public SampleBannerCustomEventLoader(
      @NonNull MediationBannerAdConfiguration mediationBannerAdConfiguration,
      @NonNull MediationAdLoadCallback<MediationBannerAd, MediationBannerAdCallback>
              mediationAdLoadCallback) {
    this.mediationBannerAdConfiguration = mediationBannerAdConfiguration;
    this.mediationAdLoadCallback = mediationAdLoadCallback;
  }

  /** Loads a banner ad from the third-party ad network. */
  public void loadAd() {
    // All custom events have a server parameter named "parameter" that returns
    // back the parameter entered into the UI when defining the custom event.
    Log.i("BannerCustomEvent", "Begin loading banner ad.");
    String serverParameter =
        mediationBannerAdConfiguration.getServerParameters().getString(
        MediationConfiguration.CUSTOM_EVENT_SERVER_PARAMETER_FIELD);

    Log.d("BannerCustomEvent", "Received server parameter.");

    Context context = mediationBannerAdConfiguration.getContext();
    sampleAdView = new SampleAdView(context);

    // Assumes that the serverParameter is the ad unit of the Sample Network.
    sampleAdView.setAdUnit(serverParameter);
    AdSize size = mediationBannerAdConfiguration.getAdSize();

    // Internally, smart banners use constants to represent their ad size, which
    // means a call to AdSize.getHeight could return a negative value. You can
    // accommodate this by using AdSize.getHeightInPixels and
    // AdSize.getWidthInPixels instead, and then adjusting to match the device's
    // display metrics.
    int widthInPixels = size.getWidthInPixels(context);
    int heightInPixels = size.getHeightInPixels(context);
    DisplayMetrics displayMetrics = Resources.getSystem().getDisplayMetrics();
    int widthInDp = Math.round(widthInPixels / displayMetrics.density);
    int heightInDp = Math.round(heightInPixels / displayMetrics.density);

    sampleAdView.setSize(new SampleAdSize(widthInDp, heightInDp));
    sampleAdView.setAdListener(this);

    SampleAdRequest request = createSampleRequest(mediationBannerAdConfiguration);
    Log.i("BannerCustomEvent", "Start fetching banner ad.");
    sampleAdView.fetchAd(request);
  }

  public SampleAdRequest createSampleRequest(
      MediationAdConfiguration mediationAdConfiguration) {
    SampleAdRequest request = new SampleAdRequest();
    request.setTestMode(mediationAdConfiguration.isTestRequest());
    request.setKeywords(mediationAdConfiguration.getMediationExtras().keySet());
    return request;
  }
}
```

### Report load success/failure, return the view, and forward events (Java)

Call `onSuccess`/`onFailure` from the third-party SDK listener, implement `getView()` for `MediationBannerAd`, and forward open/click/close callbacks via `MediationBannerAdCallback`.

```java
@Override
public void onAdFetchSucceeded() {
  bannerAdCallback = mediationAdLoadCallback.onSuccess(this);
}

@Override
public void onAdFetchFailed(SampleErrorCode errorCode) {
  mediationAdLoadCallback.onFailure(SampleCustomEventError.createSampleSdkError(errorCode));
}
```

```java
@Override
@NonNull
public View getView() {
  return sampleAdView;
}
```

```java
@Override
public void onAdFullScreen() {
  bannerAdCallback.onAdOpened();
  bannerAdCallback.reportAdClicked();
}

@Override
public void onAdClosed() {
  bannerAdCallback.onAdClosed();
}
```

---
## 37. Custom Events Interstitial

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/custom_events_interstitial.md

How to implement a custom event adapter to mediate interstitial ads from a third-party SDK into the GMA Next-Gen SDK waterfall.

### Implement loadInterstitialAd on the Adapter (Java)

The `SampleCustomEvent` extends `Adapter` and delegates to a loader class to fetch the interstitial.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationInterstitialAd;
import com.google.android.gms.ads.mediation.MediationInterstitialAdCallback;
...

public class SampleCustomEvent extends Adapter {
  private SampleInterstitialCustomEventLoader interstitialLoader;
  @Override
  public void loadInterstitialAd(
      @NonNull MediationInterstitialAdConfiguration adConfiguration,
      @NonNull
          MediationAdLoadCallback<MediationInterstitialAd, MediationInterstitialAdCallback>
              callback) {
    interstitialLoader = new SampleInterstitialCustomEventLoader(adConfiguration, callback);
    interstitialLoader.loadAd();
  }
}
```

### SampleInterstitialCustomEventLoader class (Java)

Loads the interstitial from the third-party network, reads the server parameter, and implements `MediationInterstitialAd`.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationInterstitialAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationInterstitialAd;
import com.google.android.gms.ads.mediation.MediationInterstitialAdCallback;
...

public class SampleInterstitialCustomEventLoader extends SampleAdListener
    implements MediationInterstitialAd {

  /** A sample third-party SDK interstitial ad. */
  private SampleInterstitial sampleInterstitialAd;

  /** Configuration for requesting the interstitial ad from the third-party network. */
  private final MediationInterstitialAdConfiguration mediationInterstitialAdConfiguration;

  /** Callback for interstitial ad events. */
  private MediationInterstitialAdCallback interstitialAdCallback;

  /** Callback that fires on loading success or failure. */
  private final MediationAdLoadCallback<MediationInterstitialAd, MediationInterstitialAdCallback>
      mediationAdLoadCallback;

  /** Constructor. */
  public SampleInterstitialCustomEventLoader(
      @NonNull MediationInterstitialAdConfiguration mediationInterstitialAdConfiguration,
      @NonNull MediationAdLoadCallback<MediationInterstitialAd, MediationInterstitialAdCallback>
              mediationAdLoadCallback) {
    this.mediationInterstitialAdConfiguration = mediationInterstitialAdConfiguration;
    this.mediationAdLoadCallback = mediationAdLoadCallback;
  }

  /** Loads the interstitial ad from the third-party ad network. */
  public void loadAd() {
    // All custom events have a server parameter named "parameter" that returns
    // back the parameter entered into the UI when defining the custom event.
    Log.i("InterstitialCustomEvent", "Begin loading interstitial ad.");
    String serverParameter = mediationInterstitialAdConfiguration.getServerParameters().getString(
        MediationConfiguration.CUSTOM_EVENT_SERVER_PARAMETER_FIELD);
    Log.d("InterstitialCustomEvent", "Received server parameter.");

    sampleInterstitialAd =
        new SampleInterstitial(mediationInterstitialAdConfiguration.getContext());
    sampleInterstitialAd.setAdUnit(serverParameter);

    // Implement a SampleAdListener and forward callbacks to mediation.
    sampleInterstitialAd.setAdListener(this);

    // Make an ad request.
    Log.i("InterstitialCustomEvent", "start fetching interstitial ad.");
    sampleInterstitialAd.fetchAd(
        SampleCustomEvent.createSampleRequest(mediationInterstitialAdConfiguration));
  }

public SampleAdRequest createSampleRequest(
      MediationAdConfiguration mediationAdConfiguration) {
    SampleAdRequest request = new SampleAdRequest();
    request.setTestMode(mediationAdConfiguration.isTestRequest());
    request.setKeywords(mediationAdConfiguration.getMediationExtras().keySet());
    return request;
  }
}
```

### Report load success or failure (Java)

Invoke `onSuccess()` or `onFailure()` from the third-party SDK's listener callbacks.

```java
@Override
public void onAdFetchSucceeded() {
  interstitialAdCallback = mediationAdLoadCallback.onSuccess(this);
}

@Override
public void onAdFetchFailed(SampleErrorCode errorCode) {
  mediationAdLoadCallback.onFailure(SampleCustomEventError.createSampleSdkError(errorCode));
}
```

### Implement showAd (Java)

`MediationInterstitialAd` requires a `showAd()` method to display the ad.

```java
@Override
public void showAd(@NonNull Context context) {
  sampleInterstitialAd.show();
}
```

### Forward mediation events to GMA Next-Gen SDK (Java)

Forward presentation events (open, close, impression) from the third-party SDK to the Google Mobile Ads SDK via the `MediationInterstitialAdCallback`.

```java
@Override
public void onAdFullScreen() {
  interstitialAdCallback.reportAdImpression();
  interstitialAdCallback.onAdOpened();
}

@Override
public void onAdClosed() {
  interstitialAdCallback.onAdClosed();
}
```

---

## 38. Custom Events Native

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/custom_events_native.md

How to implement a custom event adapter for native ads, including the `UnifiedNativeAdMapper` to reconcile a third-party native ad with the GMA Next-Gen SDK format.

### Implement loadNativeAd on the Adapter (Java)

The `SampleCustomEvent` extends `Adapter` and delegates native ad loading to `SampleNativeCustomEventLoader`.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;

import com.google.android.gms.ads.mediation.MediationNativeAdCallback;
...
public class SampleCustomEvent extends Adapter {
  private SampleNativeCustomEventLoader nativeLoader;

  @Override
  public void loadNativeAd(
      @NonNull MediationNativeAdConfiguration adConfiguration,
      @NonNull MediationAdLoadCallback<UnifiedNativeAdMapper, MediationNativeAdCallback> callback) {
    nativeLoader = new SampleNativeCustomEventLoader(adConfiguration, callback);
    nativeLoader.loadAd();
  }
}
```

### SampleNativeCustomEventLoader loadAd (Java)

Loads the native ad from the third-party network, reads the server parameter, and respects `NativeAdOptions` for image asset handling.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationNativeAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationNativeAdCallback;
...

public class SampleNativeCustomEventLoader extends SampleNativeAdListener {
  /** Configuration for requesting the native ad from the third-party network. */
  private final MediationNativeAdConfiguration mediationNativeAdConfiguration;

  /** Callback that fires on loading success or failure. */
  private final MediationAdLoadCallback<UnifiedNativeAdMapper, MediationNativeAdCallback>
      mediationAdLoadCallback;

  /** Callback for native ad events. */
  private MediationNativeAdCallback nativeAdCallback;

  /** Constructor */
  public SampleNativeCustomEventLoader(
      @NonNull MediationNativeAdConfiguration mediationNativeAdConfiguration,
      @NonNull MediationAdLoadCallback<UnifiedNativeAd, MediationNativeAdCallback>
              mediationAdLoadCallback) {
    this.mediationNativeAdConfiguration = mediationNativeAdConfiguration;
    this.mediationAdLoadCallback = mediationAdLoadCallback;
  }

  /** Loads the native ad from the third-party ad network. */
  public void loadAd() {
    // Create one of the Sample SDK's ad loaders to request ads.
    Log.i("NativeCustomEvent", "Begin loading native ad.");
    SampleNativeAdLoader loader =
        new SampleNativeAdLoader(mediationNativeAdConfiguration.getContext());

    // All custom events have a server parameter named "parameter" that returns
    // back the parameter entered into the UI when defining the custom event.
    String serverParameter = mediationNativeAdConfiguration
        .getServerParameters()
        .getString(MediationConfiguration
        .CUSTOM_EVENT_SERVER_PARAMETER_FIELD);
    Log.d("NativeCustomEvent", "Received server parameter.");

    loader.setAdUnit(serverParameter);

    // Create a native request to give to the SampleNativeAdLoader.
    SampleNativeAdRequest request = new SampleNativeAdRequest();
    NativeAdOptions options = mediationNativeAdConfiguration.getNativeAdOptions();
    if (options != null) {
      // If the NativeAdOptions' shouldReturnUrlsForImageAssets is true, the adapter should
      // send just the URLs for the images.
      request.setShouldDownloadImages(!options.shouldReturnUrlsForImageAssets());

      request.setShouldDownloadMultipleImages(options.shouldRequestMultipleImages());
      switch (options.getMediaAspectRatio()) {
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_LANDSCAPE:
          request.setPreferredImageOrientation(SampleNativeAdRequest.IMAGE_ORIENTATION_LANDSCAPE);
          break;
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_PORTRAIT:
          request.setPreferredImageOrientation(SampleNativeAdRequest.IMAGE_ORIENTATION_PORTRAIT);
          break;
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_SQUARE:
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_ANY:
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_UNKNOWN:
        default:
          request.setPreferredImageOrientation(SampleNativeAdRequest.IMAGE_ORIENTATION_ANY);
      }
    }

    loader.setNativeAdListener(this);

    // Begin a request.
    Log.i("NativeCustomEvent", "Start fetching native ad.");
    loader.fetchAd(request);
  }
}
```

### Report native ad load success or failure (Java)

On a successful fetch, wrap the third-party ad in a `UnifiedNativeAdMapper` and pass it to `onSuccess()`.

```java
@Override
public void onNativeAdFetched(SampleNativeAd ad) {
  SampleUnifiedNativeAdMapper mapper = new SampleUnifiedNativeAdMapper(ad);
  mediationNativeAdCallback = mediationAdLoadCallback.onSuccess(mapper);
}

@Override
public void onAdFetchFailed(SampleErrorCode errorCode) {
  mediationAdLoadCallback.onFailure(SampleCustomEventError.createSampleSdkError(errorCode));
}
```

### SampleUnifiedNativeAdMapper class (Java)

Maps the third-party native ad's assets (headline, body, icon, images, AdChoices, extras) onto the `UnifiedNativeAdMapper` and overrides click/impression tracking.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.UnifiedNativeAdMapper;
import com.google.android.gms.ads.nativead.NativeAd;
...

public class SampleUnifiedNativeAdMapper extends UnifiedNativeAdMapper {

  private final SampleNativeAd sampleAd;

  public SampleUnifiedNativeAdMapper(SampleNativeAd ad) {
    sampleAd = ad;
    setHeadline(sampleAd.getHeadline());
    setBody(sampleAd.getBody());
    setCallToAction(sampleAd.getCallToAction());
    setStarRating(sampleAd.getStarRating());
    setStore(sampleAd.getStoreName());
    setIcon(
        new SampleNativeMappedImage(
            ad.getIcon(), ad.getIconUri(), SampleCustomEvent.SAMPLE_SDK_IMAGE_SCALE));
    setAdvertiser(ad.getAdvertiser());

    List<NativeAd.Image> imagesList = new ArrayList<NativeAd.Image>();
    imagesList.add(new SampleNativeMappedImage(ad.getImage(), ad.getImageUri(),
        SampleCustomEvent.SAMPLE_SDK_IMAGE_SCALE));
    setImages(imagesList);

    if (sampleAd.getPrice() != null) {
      NumberFormat formatter = NumberFormat.getCurrencyInstance();
      String priceString = formatter.format(sampleAd.getPrice());
      setPrice(priceString);
    }

    Bundle extras = new Bundle();
    extras.putString(SampleCustomEvent.DEGREE_OF_AWESOMENESS, ad.getDegreeOfAwesomeness());
    this.setExtras(extras);

    setOverrideClickHandling(false);
    setOverrideImpressionRecording(false);

    setAdChoicesContent(sampleAd.getInformationIcon());
  }

  @Override
  public void recordImpression() {
    sampleAd.recordImpression();
  }

  @Override
  public void handleClick(View view) {
    sampleAd.handleClick(view);
  }

  // The Sample SDK doesn't do its own impression/click tracking, instead relies on its
  // publishers calling the recordImpression and handleClick methods on its native ad object. So
  // there's no need to pass a reference to the View being used to display the native ad. If
  // your mediated network does need a reference to the view, the following method can be used
  // to provide one.

  @Override
  public void trackViews(View containerView, Map<String, View> clickableAssetViews,
      Map<String, View> nonClickableAssetViews) {
    super.trackViews(containerView, clickableAssetViews, nonClickableAssetViews);
    // If your ad network SDK does its own impression tracking, here is where you can track the
    // top level native ad view and its individual asset views.
  }

  @Override
  public void untrackView(View view) {
    super.untrackView(view);
    // Here you would remove any trackers from the View added in trackView.
  }
}
```

### SampleNativeMappedImage class (Java)

Subclass of `NativeAd.Image` to map image assets (drawable, URI, scale) from the mediated SDK.

```java
public class SampleNativeMappedImage extends NativeAd.Image {

  private Drawable drawable;
  private Uri imageUri;
  private double scale;

  public SampleNativeMappedImage(Drawable drawable, Uri imageUri, double scale) {
    this.drawable = drawable;
    this.imageUri = imageUri;
    this.scale = scale;
  }

  @Override
  public Drawable getDrawable() {
    return drawable;
  }

  @Override
  public Uri getUri() {
    return imageUri;
  }

  @Override
  public double getScale() {
    return scale;
  }
}
```

---

## 39. Custom Events Rewarded

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/custom_events_rewarded.md

How to implement a custom event adapter to mediate rewarded ads, including forwarding reward and presentation callbacks to the GMA Next-Gen SDK.

### Implement loadRewardedAd on the Adapter (Java)

The `SampleCustomEvent` extends `Adapter` and delegates rewarded ad loading to `SampleRewardedCustomEventLoader`.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationRewardedAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationRewardedAd;
import com.google.android.gms.ads.mediation.MediationRewardedAdCallback;
...

public class SampleCustomEvent extends Adapter {

  private SampleNativeCustomEventLoader nativeLoader;

  @Override
  public void loadRewardedAd(
      @NonNull MediationRewardedAdConfiguration mediationRewardedAdConfiguration,
      @NonNull
          MediationAdLoadCallback<MediationRewardedAd, MediationRewardedAdCallback>
              mediationAdLoadCallback) {
    rewardedLoader =
        new SampleRewardedCustomEventLoader(
            mediationRewardedAdConfiguration, mediationAdLoadCallback);
    rewardedLoader.loadAd();
  }
}
```

### SampleRewardedCustomEventLoader class (Java)

Loads the rewarded ad from the third-party network, reads the server parameter, builds the sample request, and implements `MediationRewardedAd`.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationRewardedAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationRewardedAd;
import com.google.android.gms.ads.mediation.MediationRewardedAdCallback;
...

public class SampleRewardedCustomEventLoader extends SampleRewardedAdListener
    implements MediationRewardedAd {

  /** Configuration for requesting the rewarded ad from the third-party network. */
  private final MediationRewardedAdConfiguration mediationRewardedAdConfiguration;

  /**
   * A {@link MediationAdLoadCallback} that handles any callback when a Sample
   * rewarded ad finishes loading.
   */
  private final MediationAdLoadCallback<MediationRewardedAd, MediationRewardedAdCallback>
      mediationAdLoadCallback;

  /** Callback for rewarded ad events. */
  private MediationRewardedAdCallback rewardedAdCallback;

  /** Constructor. */
  public SampleRewardedCustomEventLoader(
      @NonNull MediationRewardedAdConfiguration mediationRewardedAdConfiguration,
      @NonNull MediationAdLoadCallback<MediationRewardedAd, MediationRewardedAdCallback>
              mediationAdLoadCallback) {
    this.mediationRewardedAdConfiguration = mediationRewardedAdConfiguration;
    this.mediationAdLoadCallback = mediationAdLoadCallback;
  }

  /** Loads the rewarded ad from the third-party ad network. */
  public void loadAd() {
    // All custom events have a server parameter named "parameter" that returns
    // back the parameter entered into the AdMob UI when defining the custom event.
    Log.i("RewardedCustomEvent", "Begin loading rewarded ad.");
    String serverParameter = mediationRewardedAdConfiguration
        .getServerParameters()
        .getString(MediationConfiguration
        .CUSTOM_EVENT_SERVER_PARAMETER_FIELD);
    Log.d("RewardedCustomEvent", "Received server parameter.");
    SampleAdRequest request = createSampleRequest(mediationRewardedAdConfiguration);
    sampleRewardedAd = new SampleRewardedAd(serverParameter);
    sampleRewardedAd.setListener(this);
    Log.i("RewardedCustomEvent", "Start fetching rewarded ad.");
    sampleRewardedAd.loadAd(request);
  }

  public SampleAdRequest createSampleRequest(
      MediationAdConfiguration mediationAdConfiguration) {
    SampleAdRequest request = new SampleAdRequest();
    request.setTestMode(mediationAdConfiguration.isTestRequest());
    request.setKeywords(mediationAdConfiguration.getMediationExtras().keySet());
    return request;
  }
}
```

### Report rewarded ad load success or failure (Java)

Invoke `onSuccess()` or `onFailure()` from the third-party SDK's rewarded ad listener callbacks.

```java
@Override
public void onRewardedAdLoaded() {
  rewardedAdCallback = mediationAdLoadCallback.onSuccess(this);
}

@Override
public void onRewardedAdFailedToLoad(SampleErrorCode errorCode) {
    mediationAdLoadCallback.onFailure(SampleCustomEventError.createSampleSdkError(errorCode));
}
```

### Implement showAd for rewarded ads (Java)

`MediationRewardedAd` requires a `showAd()` method; validate the Activity context and ad availability before showing.

```java
@Override
public void showAd(Context context) {
  if (!(context instanceof Activity)) {
    rewardedAdCallback.onAdFailedToShow(
        SampleCustomEventError.createCustomEventNoActivityContextError());
    return;
  }
  Activity activity = (Activity) context;

  if (!sampleRewardedAd.isAdAvailable()) {
    rewardedAdCallback.onAdFailedToShow(
        SampleCustomEventError.createCustomEventAdNotAvailableError());
    return;
  }
  sampleRewardedAd.showAd(activity);
}
```

### Forward mediation events to GMA Next-Gen SDK (Java)

Forward reward, click, open/close, impression, and video events from the third-party SDK to the Google Mobile Ads SDK via the `MediationRewardedAdCallback`.

```java
@Override
public void onAdRewarded(final String rewardType, final int amount) {
  RewardItem rewardItem =
      new RewardItem() {
        @Override
        public String getType() {
          return rewardType;
        }

        @Override
        public int getAmount() {
          return amount;
        }
      };
  rewardedAdCallback.onUserEarnedReward(rewardItem);
}

@Override
public void onAdClicked() {
  rewardedAdCallback.reportAdClicked();
}

@Override
public void onAdFullScreen() {
  rewardedAdCallback.onAdOpened();
  rewardedAdCallback.onVideoStart();
  rewardedAdCallback.reportAdImpression();
}

@Override
public void onAdClosed() {
  rewardedAdCallback.onAdClosed();
}

@Override
public void onAdCompleted() {
  rewardedAdCallback.onVideoComplete();
}
```

---

## 40. Strategies

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/strategies.md

Covers Publisher first-party ID, which is enabled by default in the GMA Next-Gen SDK to deliver more relevant and personalized ads using app-collected data.

### Disable Publisher first-party ID (Kotlin)

Disables Publisher first-party ID by calling `MobileAds.putPublisherFirstPartyIdEnabled(false)`.

```kotlin
// Disables Publisher first-party ID.
MobileAds.putPublisherFirstPartyIdEnabled(false)
```

### Disable Publisher first-party ID (Java)

Disables Publisher first-party ID by calling `MobileAds.putPublisherFirstPartyIdEnabled(false)`.

```java
// Disables Publisher first-party ID.
MobileAds.putPublisherFirstPartyIdEnabled(false);
```

---

## 41. Ad Serving Modes

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/ad-serving-modes.md

Describes the three ad serving modes (personalized, non-personalized, limited) and the IAB TCF purpose consent choices Google uses to determine which mode applies under the EU User Consent Policy.

*No code samples in this guide.*

---

## 42. Play Data Disclosure

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/play-data-disclosure.md

Helps complete Google Play's Data safety section disclosure for apps using the GMA Next-Gen SDK, listing data the SDK collects automatically (IP address, user interactions, diagnostics, device/account identifiers) and how it is handled.

*No code samples in this guide.*

---

## 43. Precise Location

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/precise-location.md

Shows an example consent overlay to inform users when precise location data may be shared with third parties for personalized advertising, analytics, and attribution.

### Present a location data consent overlay (Kotlin)

Example `AlertDialog` that informs users about location data sharing and acknowledges their consent.

```kotlin
protected fun presentConsentOverlay(context: Context) {
  AlertDialog.Builder(context)
      .setTitle("Location data")
      .setMessage("We may use your location, " +
          "and share it with third parties, " +
          "for the purposes of personalized advertising, " +
          "analytics, and attribution. " +
          "To learn more, visit our privacy policy " +
          "at https://myapp.com/privacy.")
      .setNeutralButton("OK") { dialog, which ->
        dialog.cancel()
        // TODO: replace the below log statement with code that specifies how
        // you want to handle the user's acknowledgement.
        Log.d("MyApp", "Got consent.")
      }
      .show()
}

// To use the above function:
presentConsentOverlay(this)
```

### Present a location data consent overlay (Java)

Example `AlertDialog` that informs users about location data sharing and acknowledges their consent.

```java
protected void presentConsentOverlay(Context context) {
  new AlertDialog.Builder(context)
      .setTitle("Location data")
      .setMessage("We may use your location, " +
          "and share it with third parties, " +
          "for the purposes of personalized advertising, " +
          "analytics, and attribution. " +
          "To learn more, visit our privacy policy " +
          "at https://myapp.com/privacy.")
  .setNeutralButton("OK", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int which) {
      dialog.cancel();
      // TODO: replace the below log statement with code that specifies how
      // you want to handle the user's acknowledgement.
      Log.d("MyApp", "Got consent.");
    }
  })
  .show();
}

// To use the above method:
presentConsentOverlay(this);
```

---

## 44. US States

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/us-states.md

Explains how to enable Google's Restricted Data Processing (RDP) signal and use the IAB Global Privacy Platform (GPP) signal to comply with U.S. state privacy laws.

### Enable the RDP signal (Java)

Write the key `gad_rdp` with value `1` to `SharedPreferences` to notify Google to enable RDP during ad loading.

```java
    SharedPreferences sharedPref = PreferenceManager.getDefaultSharedPreferences(context);
    sharedPref.edit().putInt("gad_rdp", 1).apply();
```

### Enable the RDP signal (Kotlin)

Write the key `gad_rdp` with value `1` to `SharedPreferences` to notify Google to enable RDP during ad loading.

```kotlin
    val sharedPref = PreferenceManager.getDefaultSharedPreferences(context)
    sharedPref.edit().putInt("gad_rdp", 1).apply()
```

---

## 45. Privacy

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/privacy.md

Walks through integrating the User Messaging Platform (UMP) SDK: getting consent information, loading and presenting the consent form, presenting privacy options, and requesting ads with consent.

### Initialize ConsentInformation (Java)

Declare and initialize a `ConsentInformation` instance via `UserMessagingPlatform.getConsentInformation(context)`.

```java
    private final ConsentInformation consentInformation;
```

```java
    consentInformation = UserMessagingPlatform.getConsentInformation(context);
```

### Initialize ConsentInformation (Kotlin)

Declare and initialize a `ConsentInformation` instance via `UserMessagingPlatform.getConsentInformation(context)`.

```kotlin
    private lateinit val consentInformation: ConsentInformation
```

```kotlin
    consentInformation = UserMessagingPlatform.getConsentInformation(context)
```

### Request consent info update (Kotlin)

Request an update of the user's consent information on every app launch, providing success and failure listeners.

```kotlin
    // Requesting an update to consent information should be called on every app launch.
    consentInformation.requestConsentInfoUpdate(
      activity,
      params,
      {
        // Called when consent information is successfully updated.
      },
      { requestConsentError ->
        // Called when there's an error updating consent information.
      },
    )
```

### Load and show the consent form if required (Java)

Call `UserMessagingPlatform.loadAndShowConsentFormIfRequired()` to load any forms required to collect user consent; the form presents immediately after loading.

```java
    UserMessagingPlatform.loadAndShowConsentFormIfRequired(
        activity,
        formError -> {
          // Consent gathering process is complete.
        });
```

### Load and show the consent form if required (Kotlin)

Call `UserMessagingPlatform.loadAndShowConsentFormIfRequired()` to load any forms required to collect user consent; the form presents immediately after loading.

```kotlin
    UserMessagingPlatform.loadAndShowConsentFormIfRequired(activity) { formError ->
      // Consent gathering process is complete.
    }
```

### Check if privacy options entry point is required (Kotlin)

Helper that returns `true` if the privacy options form is required, based on `ConsentInformation.PrivacyOptionsRequirementStatus`.

```kotlin
    /** Helper variable to determine if the privacy options form is required. */
    val isPrivacyOptionsRequired: Boolean
      get() =
        consentInformation.privacyOptionsRequirementStatus ==
          ConsentInformation.PrivacyOptionsRequirementStatus.REQUIRED
```

### Present the privacy options form (Java)

Call `UserMessagingPlatform.showPrivacyOptionsForm()` when the user interacts with the privacy options entry point.

```java
    UserMessagingPlatform.showPrivacyOptionsForm(activity, onConsentFormDismissedListener);
```

### Present the privacy options form (Kotlin)

Call `UserMessagingPlatform.showPrivacyOptionsForm()` when the user interacts with the privacy options entry point.

```kotlin
    UserMessagingPlatform.showPrivacyOptionsForm(activity, onConsentFormDismissedListener)
```

### Check if ads can be requested (Java)

Before requesting ads, call `consentInformation.canRequestAds()` to verify consent has been obtained.

```java
    consentInformation.canRequestAds();
```

### Check if ads can be requested (Kotlin)

Before requesting ads, call `consentInformation.canRequestAds()` to verify consent has been obtained.

```kotlin
    consentInformation.canRequestAds()
```

### Configure debug settings for testing (Kotlin)

Use `ConsentDebugSettings` with a test device hashed ID and a debug geography to test the app's consent behavior in different regions.

```kotlin
    val debugSettings = ConsentDebugSettings.Builder(this)
        .setDebugGeography(ConsentDebugSettings.DebugGeography.DEBUG_GEOGRAPHY_EEA)
        .addTestDeviceHashedId("TEST-DEVICE-HASHED-ID")
        .build()

    val params = ConsentRequestParameters
        .Builder()
        .setConsentDebugSettings(debugSettings)
        .build()

    consentInformation = UserMessagingPlatform.getConsentInformation(this)
    // Include the ConsentRequestParameters in your consent request.
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        ...
    )
```

### Reset consent state for testing (Kotlin)

Call `consentInformation.reset()` to simulate a first-install experience; intended for testing only.

```kotlin
    consentInformation.reset()
```

---

## 46. GDPR

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/gdpr.md

Outlines GDPR IAB TCF v2 message support in the UMP SDK, including tagging for under-age-of-consent users and reading consent choices from local storage.

### Set tag for under age of consent (TFUA) (Java)

Set `setTagForUnderAgeOfConsent(true)` on `ConsentRequestParameters` so the UMP SDK does not request consent from the user.

```java
    ConsentRequestParameters params = new ConsentRequestParameters
        .Builder()
        // Indicate the user is under age of consent.
    .setTagForUnderAgeOfConsent(true)
        .build();

    consentInformation = UserMessagingPlatform.getConsentInformation(this);
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        (OnConsentInfoUpdateSuccessListener) () -> {
          // ...
        },
        (OnConsentInfoUpdateFailureListener) requestConsentError -> {
          // ...
        });
```

### Set tag for under age of consent (TFUA) (Kotlin)

Set `setTagForUnderAgeOfConsent(true)` on `ConsentRequestParameters` so the UMP SDK does not request consent from the user.

```kotlin
    val params = ConsentRequestParameters
        .Builder()
        // Indicate the user is under age of consent.
    .setTagForUnderAgeOfConsent(true)
        .build()

    consentInformation = UserMessagingPlatform.getConsentInformation(this)
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        ConsentInformation.OnConsentInfoUpdateSuccessListener {
          // ...
        },
        ConsentInformation.OnConsentInfoUpdateFailureListener {
          requestConsentError ->
          // ...
        })
```

### Read TCF Purpose 1 consent (Java)

Read the `IABTCF_PurposeConsents` string from `SharedPreferences` and check the zero-indexed character for Purpose 1 consent.

```java
    SharedPreferences sharedPref = PreferenceManager.getDefaultSharedPreferences(context);
    // Example value: "1111111111"
    String purposeConsents = sharedPref.getString("IABTCF_PurposeConsents", "");
    // Purposes are zero-indexed. Index 0 contains information about Purpose 1.
    if (!purposeConsents.isEmpty()) {
      String purposeOneString = purposeConsents.charAt(0).toString();
      boolean hasConsentForPurposeOne = purposeOneString.equals("1");
    }
```

### Read TCF Purpose 1 consent (Kotlin)

Read the `IABTCF_PurposeConsents` string from `SharedPreferences` and check the zero-indexed character for Purpose 1 consent.

```kotlin
    val sharedPref = PreferenceManager.getDefaultSharedPreferences(context)
    // Example value: "1111111111"
    val purposeConsents = sharedPref.getString("IABTCF_PurposeConsents", "")
    // Purposes are zero-indexed. Index 0 contains information about Purpose 1.
    if (purposeConsents?.isEmpty() == false) {
      val purposeOneString = purposeConsents.first().toString()
      val hasConsentForPurposeOne = purposeOneString == "1"
    }
```

### Detect Additional Consent (AC) String version 2 (Java)

Check the `IABTCF_AddtlConsent` key to determine whether a user consented to AC String version 2.

```java
        SharedPreferences sharedPref = PreferenceManager.getDefaultSharedPreferences(context);
        // Example value: "2~1.35.41.101~dv.9.21.81"
        String additionalConsent = sharedPref.getString("IABTCF_AddtlConsent", "");
        // Index 0 contains information about the specification version number.
        if (!additionalConsent.isEmpty()) {
          String specACVersion = additionalConsent.charAt(0);
          boolean isACVersion2 = purposeOneString.equals("2");
        }
```

### Detect Additional Consent (AC) String version 2 (Kotlin)

Check the `IABTCF_AddtlConsent` key to determine whether a user consented to AC String version 2.

```kotlin
        val sharedPref = PreferenceManager.getDefaultSharedPreferences(context)
        // Example value: "2~1.35.41.101~dv.9.21.81"
        val additionalConsent = sharedPref.getString("IABTCF_AddtlConsent", "")
        // Index 0 contains information about the specification version number.
        if (!additionalConsent.isEmpty()) {
          val specACVersion = additionalConsent.first()
          val isACVersion2 = specACVersion == "2"
        }
```

---

## 47. US IAB Support

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/us-iab-support.md

Covers supporting the US states regulations message in the UMP SDK, including tagging for under-age-of-consent users and reading GPP consent choices.

### Set tag for under age of consent (TFUA) (Java)

Set `setTagForUnderAgeOfConsent(true)` on `ConsentRequestParameters` so the UMP SDK does not request consent from the user.

```java
    ConsentRequestParameters params = new ConsentRequestParameters
        .Builder()
        // Indicate the user is under age of consent.
    .setTagForUnderAgeOfConsent(true)
        .build();

    consentInformation = UserMessagingPlatform.getConsentInformation(this);
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        (OnConsentInfoUpdateSuccessListener) () -> {
          // ...
        },
        (OnConsentInfoUpdateFailureListener) requestConsentError -> {
          // ...
        });
```

### Set tag for under age of consent (TFUA) (Kotlin)

Set `setTagForUnderAgeOfConsent(true)` on `ConsentRequestParameters` so the UMP SDK does not request consent from the user.

```kotlin
    val params = ConsentRequestParameters
        .Builder()
        // Indicate the user is under age of consent.
    .setTagForUnderAgeOfConsent(true)
        .build()

    consentInformation = UserMessagingPlatform.getConsentInformation(this)
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        ConsentInformation.OnConsentInfoUpdateSuccessListener {
          // ...
        },
        ConsentInformation.OnConsentInfoUpdateFailureListener {
          requestConsentError ->
          // ...
        })
```

---

## 48. Consent Mode

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/consent-mode.md

Describes how the UMP SDK interprets GDPR consent choices for Google Consent Mode (ad storage, ad personalization, ad user data, analytics storage) and how to configure basic versus advanced mode with the Google Analytics for Firebase SDK.

*No code samples in this guide.*

---
## 49. Sync Consent

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/sync-consent.md

Guide to syncing GDPR consent from the User Messaging Platform (UMP) SDK across multiple apps using a consent sync identifier.

### Set the consent sync identifier (Kotlin)

Example fetching App Set ID to identify the user across apps and passing it as the consent sync ID.

```kotlin
import com.google.android.gms.appset.AppSet
import com.google.android.gms.appset.AppSetIdInfo

// Example fetching App Set ID to identify the user across apps.
val client = AppSet.getClient(this)
client.appSetIdInfo.addOnSuccessListener { info: AppSetIdInfo ->
  val appSetId = info.id
  val params = ConsentRequestParameters.Builder().setConsentSyncId(appSetId).build()
}
```

### Set the consent sync identifier (Java)

Example fetching App Set ID to identify the user across apps and passing it as the consent sync ID.

```java
import com.google.android.gms.appset.AppSet;
import com.google.android.gms.appset.AppSetIdClient;

// Example fetching App Set ID to identify the user across apps.
AppSetIdClient client = AppSet.getClient(this);
client.getAppSetIdInfo().addOnSuccessListener(
  info -> {
    String appSetId = info.getId();
    ConsentRequestParameters params =
        new ConsentRequestParameters.Builder().setConsentSyncId(appSetId).build();
  }
);
```

---

## 50. Release Notes

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/release-notes.md

Versioned release notes table for the UMP SDK, listing changes from 1.0.0 (2020-07-07) through 4.0.0 (2025-10-31), including the addition of `setConsentSyncId()` and minimum API level bumps.

*No code samples in this guide.*

---

## 51. Ad Inspector

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/ad-inspector.md

Overview of ad inspector, an in-app overlay for real-time analysis of test ad requests. Covers prerequisites including AdMob account setup, test device registration, and GMA Next-Gen SDK 0.9.0-alpha01+.

*No code samples in this guide.*

---

## 52. Launch Ad Inspector

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/launch-ad-inspector.md

Covers launching ad inspector via gestures configured in the AdMob UI or programmatically through the GMA Next-Gen SDK using `MobileAds.openAdInspector`.

### Launch programmatically (Java)

Open ad inspector programmatically with a closed-listener callback that receives an optional error.

```java
MobileAds.openAdInspector(context, new OnAdInspectorClosedListener() {
  public void onAdInspectorClosed(@Nullable AdInspectorError error) {
    // Error will be non-null if ad inspector closed due to an error.
  }
});
```

### Launch programmatically (Kotlin)

Open ad inspector programmatically with a lambda callback that receives an optional error.

```kotlin
MobileAds.openAdInspector(context) { error ->
  // Error will be non-null if ad inspector closed due to an error.
}
```

---

## 53. Verify Adapter Integrations

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/verify-adapter-integrations.md

Steps to view and verify adapters associated with ad sources in the Ad inspector **Adapters** tab, including initialization statuses, adapter versions, and custom event adapters.

*No code samples in this guide.*

---

## 54. Test Ad Units

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/test-ad-units.md

Covers in-context and out-of-context ad unit testing, custom test ad configuration, and single ad source testing to verify third-party adapter integrations.

*No code samples in this guide.*

---

## 55. Troubleshoot Ad Units

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/troubleshoot-ad-units.md

Covers viewing waterfall and bidding details in the SDK request log, troubleshooting ad units by exporting ad request/response data, and viewing native ad details.

*No code samples in this guide.*

---

## 56. Troubleshoot Privacy Settings

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/troubleshoot-privacy-settings.md

Covers reviewing Consent Management Platform (CMP) details, analyzing missing ad partners, and viewing privacy signals (`IABTCF_gdprApplies`, `IABTCF_AddtlConsent`, `IABTCF_TCString`) in ad requests.

*No code samples in this guide.*

---

## 57. Copy Troubleshooting Output

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/copy-troubleshooting-output.md

Instructions for copying a JSON troubleshooting string (AdMob app details, adapter status, ad unit test results) to clipboard by tapping the app icon seven times.

*No code samples in this guide.*

---

## 58. Test Creative Types

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/test-creative-types.md

Covers the `ft_ctype` extras parameter to specify a creative type for test queries, restricting which ads are retrieved and rendered (works only in Test Mode).

### Specify a creative type (Kotlin)

Pass `ft_ctype` in a Google extras bundle on the AdRequest to restrict creatives (e.g., to `video_app_install`).

```kotlin
val extras = Bundle()
extras.putString("ft_ctype", "video_app_install")

val request = AdRequest
  .Builder(AD_UNIT_ID)
  .setGoogleExtrasBundle(extras)
  .build()
```

### Specify a creative type (Java)

Pass `ft_ctype` in a Google extras bundle on the AdRequest to restrict creatives (e.g., to `video_app_install`).

```java
Bundle extras = new Bundle();
extras.putString("ft_ctype", "video_app_install");

AdRequest request = new AdRequest
  .Builder(AD_UNIT_ID)
  .setGoogleExtrasBundle(extras)
  .build();
```

---

## 59. Ad Load Errors

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/ad-load-errors.md

Covers the `LoadAdError` object provided in the `onAdFailedToLoad` callback, including error code, message, and response info retrieval.

### Handle ad load failure (Kotlin)

Inspect the `LoadAdError` for code, message, and response info when an ad fails to load.

```kotlin
override fun onAdFailedToLoad(adError: LoadAdError) {
  // Gets the error code. See
  // https://developers.google.com/admob/android/early-access/nextgen/reference/com/google/android/libraries/ads/mobile/sdk/common/LoadAdError.ErrorCode
  // for a list of possible codes.
  val errorCode = adError.code
  // Gets an error message.
  // For example "Account not approved yet". See
  // https://support.google.com/admob/answer/9905175 for explanations of
  // common errors.
  val errorMessage = adError.message
  // Gets additional response information about the request. See
  // https://developers.google.com/admob/android/next-gen/response-info
  // for more information.
  val responseInfo = adError.responseInfo
  // All of this information is available using the error's toString() method.
  Log.d("Ads", adError.toString())
}
```

### Handle ad load failure (Java)

Inspect the `LoadAdError` for code, message, and response info when an ad fails to load.

```java
@Override
public void onAdFailedToLoad(@NonNull LoadAdError adError) {
  // Gets the error code. See
  // https://developers.google.com/admob/android/early-access/nextgen/reference/com/google/android/libraries/ads/mobile/sdk/common/LoadAdError.ErrorCode
  // for a list of possible codes.
  ErrorCode errorCode = adError.getCode();
  // Gets an error message.
  // For example "Account not approved yet". See
  // https://support.google.com/admob/answer/9905175 for explanations of
  // common errors.
  String errorMessage = adError.getMessage();
  // Gets additional response information about the request. See
  // https://developers.google.com/admob/android/next-gen/response-info
  // for more information.
  ResponseInfo responseInfo = adError.getResponseInfo();
  // All of this information is available using the error's toString() method.
  Log.d("Ads", adError.toString());
}
```

---

## 60. Response Info

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/response-info.md

Covers the `ResponseInfo` object for debugging loaded ads and mediation waterfalls, including `AdSourceResponseInfo` metadata for each ad source in the response.

### Retrieve response info on success and failure (Kotlin)

Get `ResponseInfo` from a loaded `InterstitialAd` or from a `LoadAdError` on failure.

```kotlin
override fun onAdLoaded(interstitialAd: InterstitialAd)) {
  val responseInfo = interstitialAd.responseInfo
  Log.d(TAG, responseInfo.toString())
}

override fun onAdFailedToLoad(adError: LoadAdError) {
  val responseInfo = adError.responseInfo
  Log.d(TAG, responseInfo.toString())
}
```

### Retrieve response info on success and failure (Java)

Get `ResponseInfo` from a loaded `InterstitialAd` or from a `LoadAdError` on failure.

```java
@Override
public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
  ResponseInfo responseInfo = interstitialAd.getResponseInfo();
  Log.d(TAG, responseInfo.toString());
}

@Override
public void onAdFailedToLoad(LoadAdError loadAdError) {
  ResponseInfo responseInfo = loadAdError.getResponseInfo();
  Log.d(TAG, responseInfo.toString());
}
```

### Access ResponseInfo fields (Kotlin)

Read response ID, adapter class name, ad source responses, loaded ad source response, and response extras (mediation group / A/B test).

```kotlin
override fun onAdLoaded(interstitialAd: InterstitialAd) {
  val responseInfo = interstitialAd.responseInfo

  val responseId = responseInfo.responseId
  val adapterClassName = responseInfo.adapterClassName
  val adSourceResponses = responseInfo.adSourceResponses
  val loadedAdSourceResponse = responseInfo.loadedAdSourceResponse
  val extras = responseInfo.responseExtras
  val mediationGroupName = extras.getString("mediation_group_name")
  val mediationABTestName = extras.getString("mediation_ab_test_name")
  val mediationABTestVariant = extras.getString("mediation_ab_test_variant")
}
```

### Access ResponseInfo fields (Java)

Read response ID, adapter class name, ad source responses, loaded ad source response, and response extras (mediation group / A/B test).

```java
@Override
public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
  MyActivity.this.interstitialAd = interstitialAd;

  ResponseInfo responseInfo = interstitialAd.getResponseInfo();
  String responseId = responseInfo.getResponseId();
  String adapterClassName = responseInfo.getAdapterClassName();
  List<AdSourceResponseInfo> adSourceResponses = responseInfo.getAdSourceResponses();
  AdSourceResponseInfo loadedAdSourceResponse = responseInfo.getLoadedAdSourceResponse();
  Bundle extras = responseInfo.getResponseExtras();
  String mediationGroupName = extras.getString("mediation_group_name");
  String mediationABTestName = extras.getString("mediation_ab_test_name");
  String mediationABTestVariant = extras.getString("mediation_ab_test_variant");
}
```

### Access AdSourceResponseInfo fields (Kotlin)

Read the loaded ad source's error, IDs, instance name, adapter class name, credentials, and latency.

```kotlin
override fun onAdLoaded(interstitialAd: InterstitialAds) {
  val loadedAdSourceResponseInfo = interstitialAd.responseInfo.loadedAdSourceResponse

  val adError = loadedAdSourceResponseInfo.adError
  val adSourceId = loadedAdSourceResponseInfo.id
  val adSourceInstanceId = loadedAdSourceResponseInfo.instanceId
  val adSourceInstanceName = loadedAdSourceResponseInfo.instanceName
  val adSourceName = loadedAdSourceResponseInfo.name
  val adapterClassName = loadedAdSourceResponseInfo.adapterClassName
  val credentials = loadedAdSourceResponseInfo.credentials
  val latencyMillis = loadedAdSourceResponseInfo.latencyMillis
}
```

### Access AdSourceResponseInfo fields (Java)

Read the loaded ad source's error, IDs, instance name, adapter class name, credentials, and latency.

```java
@Override
public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
  AdSourceResponseInfo loadedAdSourceResponseInfo =
      interstitialAd.getResponseInfo().getLoadedAdSourceResponse();

  AdError adError = loadedAdSourceResponseInfo.getAdError();
  String adSourceId = loadedAdSourceResponseInfo.getId();
  String adSourceInstanceId = loadedAdSourceResponseInfo.getInstanceId();
  String adSourceInstanceName = loadedAdSourceResponseInfo.getInstanceName();
  String adSourceName = loadedAdSourceResponseInfo.getName();
  String adapterClassName = loadedAdSourceResponseInfo.getAdapterClassName();
  Bundle credentials = loadedAdSourceResponseInfo.getCredentials();
  long latencyMillis = loadedAdSourceResponseInfo.getLatencyMillis();
}
```

---
## 61. Charles

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/charles.md

Setting up Charles proxy to inspect ad calls on Android N or higher, including SSL certificate installation and network security configuration.

### Network Security Configuration XML

Declares that the app can trust user-provided SSL certificates for Charles SSL proxying.

```xml
<network-security-config>
    <debug-overrides>
        <trust-anchors>
            <!-- Trust user added CAs while debuggable only -->
            <certificates src="user" />
        </trust-anchors>
    </debug-overrides>
</network-security-config>
```

### AndroidManifest.xml Network Security Config

References the network security configuration XML in the app manifest.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest ... >
    <application ...
                 android:networkSecurityConfig="@xml/network_security_config"
                 ... >
        ...
    </application>
</manifest>
```

---

## 62. App Ads

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/app-ads.md

Setting up app-ads.txt to identify authorized sellers and protect app ad inventory from fraud, including publishing via Firebase Hosting.

### app-ads.txt Content

Add this line to your app-ads.txt file, replacing the publisher ID with your own from AdMob console > Settings.

```text
google.com, pub-00000000000000, DIRECT, f08c47fec0942fa0
```

### Firebase CLI Commands

Initialize and deploy your Firebase project to host app-ads.txt.

```bash
firebase init
```

```bash
firebase deploy --only hosting
```

### Firebase Hosting Redirect Configuration

Optional redirect configuration in firebase.json to redirect the landing page to an existing website.

```json
"hosting": {
  ...
  "redirects": [
    {
      "source": "/",
      "destination": "https://www.example.com",
      "type": 301
    }
  ]
}
```

---

## 63. Global Settings

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/global-settings.md

Using the MobileAds class for global settings including video ad volume control and consent for cookies.

### Set User Controlled App Volume (Kotlin)

Initialize the SDK on a background thread and set the app volume to half of the current device volume.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)

  val backgroundScope = CoroutineScope(Dispatchers.IO)
  backgroundScope.launch {
    // Initialize GMA Next-Gen SDK on a background thread.
    MobileAds.initialize(
      this@MainActivity,
      // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
      InitializationConfig.Builder("SAMPLE_APP_ID").build()
    ) {}
    
    // Set app volume to be half of current device volume.
    MobileAds.setUserControlledAppVolume(0.5f)
  }
}
```

### Set User Controlled App Volume (Java)

Initialize the SDK on a background thread and set the app volume to half of the current device volume.

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);

  new Thread(
          () -> {
            // Initialize GMA Next-Gen SDK on a background thread.
            MobileAds.initialize(
                this,
                // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
                new InitializationConfig.Builder("SAMPLE_APP_ID")
                    .build(),
                initializationStatus -> {
                });
            
            // Set app volume to be half of current device volume.
            MobileAds.setUserControlledAppVolume(0.5f);
          })
      .start();
}
```

### Mute App Volume (Kotlin)

Inform the SDK that the app volume has been muted.

```kotlin
MobileAds.setUserMutedApp(true)
```

### Mute App Volume (Java)

Inform the SDK that the app volume has been muted.

```java
MobileAds.setUserMutedApp(true);
```

### Consent for Cookies / Limited Ads (Kotlin)

Set the SharedPreferences value to 0 to enable limited ads (LTD).

```kotlin
val sharedPrefs = PreferenceManager.getDefaultSharedPreferences(context)
// Set the value to 0 to enable limited ads.
sharedPrefs.edit().putInt("gad_has_consent_for_cookies", 0).apply()
```

### Consent for Cookies / Limited Ads (Java)

Set the SharedPreferences value to 0 to enable limited ads (LTD).

```java
Context activity = getActivity();
SharedPreferences sharedPreferences =
  PreferenceManager.getDefaultSharedPreferences(activity);
// Set the value to 0 to enable limited ads.
sharedPreferences.edit().putInt("gad_has_consent_for_cookies", 0).apply();
```

---

## 64. Impression-Level Ad Revenue

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/impression-level-ad-revenue.md

Capturing impression-level ad revenue data using the onAdPaid callback and forwarding to analytics partners.

### onAdPaid Event Handler (Kotlin)

Handle paid events for a rewarded ad by extracting AdValue and response info including mediation details.

```kotlin
ad.adEventCallback =
  object : RewardedAdEventCallback {
    override fun onAdPaid(adValue: AdValue) {
      // Send the impression-level ad revenue information to your
      // preferred analytics server directly within this callback.

      // Extract the impression-level ad revenue data.
      val valueMicros = adValue.valueMicros
      val currencyCode = adValue.currencyCode
      val precisionType = adValue.precisionType

      val loadedAdSourceResponseInfo = ad.getResponseInfo().loadedAdSourceResponseInfo
      val adSourceName = loadedAdSourceResponseInfo?.name
      val adSourceId = loadedAdSourceResponseInfo?.id
      val adSourceInstanceName = loadedAdSourceResponseInfo?.instanceName
      val adSourceInstanceId = loadedAdSourceResponseInfo?.instanceId
      val extras = ad.getResponseInfo().responseExtras
      val mediationGroupName = extras.getString("mediation_group_name")
      val mediationABTestName = extras.getString("mediation_ab_test_name")
      val mediationABTestVariant = extras.getString("mediation_ab_test_variant")
    }
  }
```

### onAdPaid Event Handler (Java)

Handle paid events for a rewarded ad by extracting AdValue and response info including mediation details.

```java
ad.setAdEventCallback(
    new RewardedAdEventCallback() {
      @Override
      public void onAdPaid(@NonNull AdValue value) {
        // Send the impression-level ad revenue information to your preferred
        // analytics server directly within this callback.

        // Extract the impression-level ad revenue data.
        long valueMicros = value.getValueMicros();
        String currencyCode = value.getCurrencyCode();
        PrecisionType precisionType = value.getPrecisionType();

        AdSourceResponseInfo loadedAdSourceResponseInfo =
            ad.getResponseInfo().getLoadedAdSourceResponseInfo();
        String adSourceName = loadedAdSourceResponseInfo.getName();
        String adSourceId = loadedAdSourceResponseInfo.getId();
        String adSourceInstanceName = loadedAdSourceResponseInfo.getInstanceName();
        String adSourceInstanceId = loadedAdSourceResponseInfo.getInstanceId();

        Bundle extras = ad.getResponseInfo().getResponseExtras();
        String mediationGroupName = extras.getString("mediation_group_name");
        String mediationABTestName = extras.getString("mediation_ab_test_name");
        String mediationABTestVariant = extras.getString("mediation_ab_test_variant");
      }
    });
```

### Adjust Integration (Kotlin)

Send impression-level ad revenue data to Adjust via the onAdPaid callback.

```kotlin
rewardedAd.adEventCallback =
  object : RewardedAdEventCallback {
    override fun onAdPaid(value: AdValue) {
      // Send ad revenue info to Adjust.
      val adRevenue = AdjustAdRevenue("admob_sdk")
      adRevenue.setRevenue(value.valueMicros / 1000000.0, value.currencyCode)
      val loadedAdSourceResponseInfo = rewardedAd.getResponseInfo().loadedAdSourceResponseInfo
      loadedAdSourceResponseInfo?.let { adRevenue.setAdRevenueNetwork(it.name) }
      Adjust.trackAdRevenue(adRevenue)
    }
  }
```

### Adjust Integration (Java)

Send impression-level ad revenue data to Adjust via the onAdPaid callback.

```java
rewardedAd.setAdEventCallback(
    new RewardedAdEventCallback() {
      @Override
      public void onAdPaid(@NonNull AdValue value) {
        // Send ad revenue info to Adjust.
        AdjustAdRevenue adRevenue = new AdjustAdRevenue("admob_sdk");
        adRevenue.setRevenue(value.getValueMicros() / 1000000.0, value.getCurrencyCode());
        if (rewardedAd.getResponseInfo().getLoadedAdSourceResponseInfo() != null) {
          adRevenue.setAdRevenueNetwork(
              rewardedAd.getResponseInfo().getLoadedAdSourceResponseInfo().getName());
        }
        Adjust.trackAdRevenue(adRevenue);
      }
    });
```

---

## 65. SSV

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/ssv.md

Verifying rewarded server-side verification (SSV) callbacks using the Tink Java Apps library, including setting custom data and manual verification steps.

### RewardedAdsVerifier Usage (Java)

Verify a rewarded SSV callback URL using the Tink Java Apps RewardedAdsVerifier helper class.

```java
RewardedAdsVerifier verifier = new RewardedAdsVerifier.Builder()
    .fetchVerifyingPublicKeysWith(
        RewardedAdsVerifier.KEYS_DOWNLOADER_INSTANCE_PROD)
    .build();
String rewardUrl = ...;
verifier.verify(rewardUrl);
```

### Set Server-Side Verification Options (Kotlin)

Set SSV options with user ID and custom data after the rewarded ad is loaded.

```kotlin
RewardedAd.load(
    AdRequest.Builder("AD_UNIT_ID").build(),
    object : AdLoadCallback<RewardedAd> {
      override fun onAdLoaded(ad: RewardedAd) {
        // Rewarded ad loaded.
        rewardedAd = ad;
        rewardedAd.setServerSideVerificationOptions(
ServerSideVerificationOptions("userId", "customData")
)
      }

      override fun onAdFailedToLoad(adError: LoadAdError) {
        // Rewarded ad failed to load.
        rewardedAd = null
      }
    },
  )
```

### Set Server-Side Verification Options (Java)

Set SSV options with user ID and custom data after the rewarded ad is loaded.

```java
RewardedAd.load(
      new AdRequest.Builder("AD_UNIT_ID").build(),
      new AdLoadCallback<RewardedAd>() {
        @Override
        public void onAdLoaded(@NonNull RewardedAd ad) {
          // Rewarded ad loaded.
          rewardedAd = ad;
          rewardedAd.setServerSideVerificationOptions(
new ServerSideVerificationOptions("userId", "customData")
);
        }

        @Override
        public void onAdFailedToLoad(@NonNull LoadAdError adError) {
          // Rewarded ad failed to load.
          rewardedAd = null;
        }
      }
  );
```

### Parse Public Keys from JSON (Java)

Parse the AdMob key server JSON response into a map of key IDs to ECPublicKey objects.

```java
private static Map<Integer, ECPublicKey> parsePublicKeysJson(String publicKeysJson)
    throws GeneralSecurityException {
  Map<Integer, ECPublicKey> publicKeys = new HashMap<>();
  try {
    JSONArray keys = new JSONObject(publicKeysJson).getJSONArray("keys");
    for (int i = 0; i < keys.length(); i++) {
      JSONObject key = keys.getJSONObject(i);
      publicKeys.put(
          key.getInt("keyId"),
          EllipticCurves.getEcPublicKey(Base64.decode(key.getString("base64"))));
    }
  } catch (JSONException e) {
    throw new GeneralSecurityException("failed to extract trusted signing public keys", e);
  }
  if (publicKeys.isEmpty()) {
    throw new GeneralSecurityException("No trusted keys are available.");
  }
  return publicKeys;
}
```

### Verify SSV Callback Signature (Java)

Verify the callback URL content with the appropriate public key using ECDSA.

```java
private void verify(final byte[] dataToVerify, int keyId, final byte[] signature)
    throws GeneralSecurityException {
  Map<Integer, ECPublicKey> publicKeys = parsePublicKeysJson();
  if (publicKeys.containsKey(keyId)) {
    foundKeyId = true;
    ECPublicKey publicKey = publicKeys.get(keyId);
    EcdsaVerifyJce verifier = new EcdsaVerifyJce(publicKey, HashType.SHA256, EcdsaEncoding.DER);
    verifier.verify(signature, dataToVerify);
  } else {
    throw new GeneralSecurityException("cannot find verifying key with key ID: " + keyId);
  }
}
```

---

## 66. Targeting

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/targeting.md

Providing targeting information using RequestConfiguration for child-directed treatment, age treatment, ad content filtering, and publisher privacy.

### RequestConfiguration for Child-Directed Treatment (Kotlin)

Create a RequestConfiguration with child-directed treatment tag and set it globally via MobileAds.setRequestConfiguration().

```kotlin
val requestConfiguration = RequestConfiguration
  .Builder()
  // Set your targeting tags.
  .setTagForChildDirectedTreatment(RequestConfiguration.TagForChildDirectedTreatment.TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE)
  .build()

MobileAds.setRequestConfiguration(requestConfiguration)
```

### RequestConfiguration for Child-Directed Treatment (Java)

Create a RequestConfiguration with child-directed treatment tag and set it globally via MobileAds.setRequestConfiguration().

```java
RequestConfiguration requestConfiguration = new RequestConfiguration
  .Builder()
  // Set your targeting tags.
  .setTagForChildDirectedTreatment(TagForChildDirectedTreatment.TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE)
  .build();

MobileAds.setRequestConfiguration(requestConfiguration);
```

### Set Age Restricted Treatment (Kotlin)

Indicate that ad requests should receive child age treatment using the new setAgeRestrictedTreatment API.

```kotlin
val requestConfiguration =
  RequestConfiguration.Builder()
    // Indicate that ad requests should have child age treatment.
    .setAgeRestrictedTreatment(AgeRestrictedTreatment.CHILD)
    .build()
MobileAds.setRequestConfiguration(requestConfiguration)
```

### Set Age Restricted Treatment (Java)

Indicate that ad requests should receive child age treatment using the new setAgeRestrictedTreatment API.

```java
RequestConfiguration requestConfiguration =
    new RequestConfiguration.Builder()
        // Indicate that ad requests should have child age treatment.
        .setAgeRestrictedTreatment(AgeRestrictedTreatment.CHILD)
        .build();
MobileAds.setRequestConfiguration(requestConfiguration);
```

### Set Max Ad Content Rating (Kotlin)

Configure the RequestConfiguration to specify a maximum ad content rating of G.

```kotlin
val requestConfiguration = RequestConfiguration
  .Builder()
  .setMaxAdContentRating(RequestConfiguration.MaxAdContentRating.MAX_AD_CONTENT_RATING_G)
  .build()

MobileAds.setRequestConfiguration(requestConfiguration)
```

### Set Max Ad Content Rating (Java)

Configure the RequestConfiguration to specify a maximum ad content rating of G.

```java
RequestConfiguration requestConfiguration = new RequestConfiguration
  .Builder()
  .setMaxAdContentRating(MaxAdContentRating.MAX_AD_CONTENT_RATING_G)
  .build();

MobileAds.setRequestConfiguration(requestConfiguration);
```

### Network Extras in Ad Request (Kotlin)

Set an extra parameter for a specific ad source using setGoogleExtrasBundle in a NativeAdRequest.

```kotlin
val extras = Bundle()
extras.putString("collapsible", "bottom")
val adRequest =
  NativeAdRequest.Builder("AD_UNIT_ID", listOf(NativeAd.NativeAdType.NATIVE))
    .setGoogleExtrasBundle(extras)
    .build()
NativeAdLoader.load(adRequest, adCallback)
```

### Network Extras in Ad Request (Java)

Set an extra parameter for a specific ad source using setGoogleExtrasBundle in a NativeAdRequest.

```java
Bundle extras = new Bundle();
extras.putString("collapsible", "bottom");
NativeAdRequest adRequest =
  new NativeAdRequest.Builder("AD_UNIT_ID", Arrays.asList(NativeAd.NativeAdType.NATIVE))
    .setGoogleExtrasBundle(extras)
    .build();
NativeAdLoader.load(adRequest, adCallback);
```

---

## 67. Open Measurement

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/open-measurement.md

Open Measurement is an IAB standard for third-party viewability verification, integrated automatically by the GMA Next-Gen SDK using "Google" as the partner name for all ads.

*No code samples in this guide.*

---

## 68. Browser

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/browser.md

In-app browsers powered by Custom Tabs and WebView can be optimized for ad monetization using the web view APIs for ads.

*No code samples in this guide.*

---

## 69. Custom Tabs

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/custom-tabs.md

Using web view APIs for ads with Chrome Custom Tabs to improve monetization by registering a CustomTabsSession with the GMA Next-Gen SDK.

### Initialize SDK with WebView Application ID (Kotlin)

Initialize the GMA Next-Gen SDK with WEBVIEW_APIS_FOR_ADS_APPLICATION_ID when you don't have an AdMob application ID.

```kotlin
MobileAds.initialize(
    this@MainActivity,
    // Use this application ID to initialize the GMA Next-Gen SDK if
    // you don't have an AdMob application ID.
    InitializationConfig.Builder(InitializationConfig.WEBVIEW_APIS_FOR_ADS_APPLICATION_ID)
        .build(),
  ) {
    // Adapter initialization complete.
  }
```

### Initialize SDK with WebView Application ID (Java)

Initialize the GMA Next-Gen SDK with WEBVIEW_APIS_FOR_ADS_APPLICATION_ID when you don't have an AdMob application ID.

```java
MobileAds.initialize(
    this,
    // Use this application ID to initialize the GMA Next-Gen SDK if
    // you don't have an AdMob application ID.
    new InitializationConfig.Builder(InitializationConfig.WEBVIEW_APIS_FOR_ADS_APPLICATION_ID)
        .build(),
        initializationStatus -> {
          // Adapter initialization is complete.
        });
```

### Register Custom Tabs Session (Kotlin)

Register a CustomTabsSession using MobileAds.registerCustomTabsSession within the onCustomTabsServiceConnected callback to establish a postMessage channel.

```kotlin
import com.google.android.libraries.ads.mobile.sdk.MobileAds

class MainActivity : ComponentActivity() {
  private var customTabsClient: CustomTabsClient? = null
  private var customTabsSession: CustomTabsSession? = null

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    // Get the default browser package name, this will be null if
    // the default browser does not provide a CustomTabsService.
    val packageName = CustomTabsClient.getPackageName(applicationContext, null);
    if (packageName == null) {
      // Do nothing as service connection is not supported.
      return
    }

    CustomTabsClient.bindCustomTabsService(
      applicationContext,
      packageName,
      object : CustomTabsServiceConnection() {
        override fun onCustomTabsServiceConnected(
          name: ComponentName, client: CustomTabsClient,
          ) {
            customTabsClient = client

            // Warm up the browser process.
            customTabsClient?.warmup(0L)
            // Create a new browser session using the GMA Next-Gen SDK.
customTabsSession = MobileAds.INSTANCE.registerCustomTabsSession(
client,
// Checks the "Digital Asset Link" to connect the postMessage channel.
ORIGIN,
// Optional parameter to receive the delegated callbacks.
customTabsCallback
)
// Create a new browser session if the GMA Next-Gen SDK is
// unable to create one.
if (customTabsSession == null) {
customTabsSession = client.newSession(customTabsCallback)
}

            // Pass the custom tabs session into the intent.
            val customTabsIntent = CustomTabsIntent.Builder(customTabsSession).build()
            customTabsIntent.launchUrl(this@MainActivity,
                                      Uri.parse("YOUR_URL"))
          }

          override fun onServiceDisconnected(componentName: ComponentName) {
            // Remove the custom tabs client and custom tabs session.
            customTabsClient = null
            customTabsSession = null
          }
      })
  }

  // Listen for events from the CustomTabsSession delegated by the GMA Next-Gen SDK.
  private val customTabsCallback: CustomTabsCallback = object : CustomTabsCallback() {
    @Synchronized
    override fun onNavigationEvent(navigationEvent: Int, extras: Bundle?) {
      // Called when a navigation event happens.
    }

    @Synchronized
    override fun onMessageChannelReady(extras: Bundle?) {
      // Called when the channel is ready for sending and receiving messages on both
      // ends.
      // This frequently happens, such as each time the SDK requests a
// new channel.
    }

    @Synchronized
    override fun onPostMessage(message: String, extras: Bundle?) {
      // Called when a tab controlled by this CustomTabsSession has sent a postMessage.
    }

    override fun onRelationshipValidationResult(
      relation: Int, requestedOrigin: Uri, result: Boolean, extras: Bundle?
    ) {
      // Called when a relationship validation result is available.
    }

    override fun onActivityResized(height: Int, width: Int, extras: Bundle) {
      // Called when the tab is resized.
    }

    override fun extraCallback(callbackName: String, args: Bundle?) {

    }

    override fun extraCallbackWithResult(callbackName: String, args: Bundle?): Bundle? {
      return null
    }
  }

  companion object {
    // Replace this URL with an associated website.
const val ORIGIN = "https://www.google.com"
  }
}
```

### Register Custom Tabs Session (Java)

Register a CustomTabsSession using MobileAds.registerCustomTabsSession within the onCustomTabsServiceConnected callback to establish a postMessage channel.

```java
import com.google.android.libraries.ads.mobile.sdk.MobileAds;

class MainActivity extends ComponentActivity {
  // Replace this URL with an associated website.
private static final String ORIGIN = "https://www.google.com";
  private CustomTabsClient customTabsClient;
  private CustomTabsSession customTabsSession;

  @Override
  protected void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // Get the default browser package name, this will be null if
    // the default browser does not provide a CustomTabsService.
    String packageName = CustomTabsClient.getPackageName(getApplicationContext(), null);
    if (packageName == null) {
      // Do nothing as service connection is not supported.
      return;
    }

    CustomTabsClient.bindCustomTabsService(
        getApplicationContext(),
        packageName,
        new CustomTabsServiceConnection() {
          @Override
          public void onCustomTabsServiceConnected(@NonNull ComponentName name,
              @NonNull CustomTabsClient client) {
            customTabsClient = client;

            // Warm up the browser process.
            customTabsClient.warmup(0);
            // Create a new browser session using the GMA Next-Gen SDK.
customTabsSession = MobileAds.INSTANCE.registerCustomTabsSession(
client,
// Checks the "Digital Asset Link" to connect the postMessage channel.
ORIGIN,
// Optional parameter to receive the delegated callbacks.
customTabsCallback);
// Create a new browser session if the GMA Next-Gen SDK is
// unable to create one.
if (customTabsSession == null) {
customTabsSession = client.newSession(customTabsCallback);
}

            // Pass the custom tabs session into the intent.
            CustomTabsIntent intent = new CustomTabsIntent.Builder(customTabsSession).build();
            intent.launchUrl(MainActivity.this, Uri.parse("YOUR_URL"));
          }

          @Override
          public void onServiceDisconnected(ComponentName componentName) {
            // Remove the custom tabs client and custom tabs session.
            customTabsClient = null;
            customTabsSession = null;
          }
        }

    );
  }

  // Listen for events from the CustomTabsSession delegated by the GMA Next-Gen SDK.
  private final CustomTabsCallback customTabsCallback = new CustomTabsCallback() {
    @Override
    public void onNavigationEvent(int navigationEvent, @Nullable Bundle extras) {
      // Called when a navigation event happens.
      super.onNavigationEvent(navigationEvent, extras);
    }

    @Override
    public void onMessageChannelReady(@Nullable Bundle extras) {
      // Called when the channel is ready for sending and receiving messages on both
      // ends.
      // This frequently happens, such as each time the SDK requests a
// new channel.
      super.onMessageChannelReady(extras);
    }

    @Override
    public void onPostMessage(@NonNull String message, @Nullable Bundle extras) {
      // Called when a tab controlled by this CustomTabsSession has sent a postMessage.
      super.onPostMessage(message, extras);
    }

    @Override
    public void onRelationshipValidationResult(int relation, @NonNull Uri requestedOrigin,
        boolean result, @Nullable Bundle extras) {
      // Called when a relationship validation result is available.
      super.onRelationshipValidationResult(relation, requestedOrigin, result, extras);
    }

    @Override
    public void onActivityResized(int height, int width, @NonNull Bundle extras) {
      // Called when the tab is resized.
      super.onActivityResized(height, width, extras);
    }

    @Override
    public void extraCallback(@NonNull String callbackName, @Nullable Bundle args) {
      super.extraCallback(callbackName, args);
    }

    @Nullable
    @Override
    public Bundle extraCallbackWithResult(@NonNull String callbackName, @Nullable Bundle args) {
      return super.extraCallbackWithResult(callbackName, args);
    }
  };
}
```

---

## 70. WebView

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/webview.md

Configuring WebView for optimal ad monetization by enabling third-party cookies, JavaScript, DOM storage, and automatic video playback.

### WebView Configuration and Load URL (Java)

Configure WebView settings for ads monetization and load a network-based URL for optimized performance.

```java
import android.webkit.CookieManager;
import android.webkit.WebView;

public class MainActivity extends AppCompatActivity {
  private WebView webView;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    webView = findViewById(R.id.webview);

    // Let the web view accept third-party cookies.
    CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true);
    // Let the web view use JavaScript.
    webView.getSettings().setJavaScriptEnabled(true);
    // Let the web view access local storage.
    webView.getSettings().setDomStorageEnabled(true);
    // Let HTML videos play automatically.
    webView.getSettings().setMediaPlaybackRequiresUserGesture(false);

    // Load the URL for optimized web view performance.
webView.loadUrl("https://google.github.io/webview-ads/test/");
  }
}
```

### WebView Configuration and Load URL (Kotlin)

Configure WebView settings for ads monetization and load a network-based URL for optimized performance.

```kotlin
import android.webkit.CookieManager
import android.webkit.WebView

class MainActivity : AppCompatActivity() {
  lateinit var webView: WebView

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    webView = findViewById(R.id.webview)

    // Let the web view accept third-party cookies.
    CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true)
    // Let the web view use JavaScript.
    webView.settings.javaScriptEnabled = true
    // Let the web view access local storage.
    webView.settings.domStorageEnabled = true
    // Let HTML videos play automatically.
    webView.settings.mediaPlaybackRequiresUserGesture = false

    // Load the URL for optimized web view performance.
webView.loadUrl("https://google.github.io/webview-ads/test/")
  }
}
```

---

## 71. API for Ads

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/api-for-ads.md

Making app signals available to WebView tags by registering WebView with the GMA Next-Gen SDK for improved monetization and spam protection.

### Initialize SDK with WebView Application ID (Kotlin)

Initialize the GMA Next-Gen SDK with WEBVIEW_APIS_FOR_ADS_APPLICATION_ID when you don't have an AdMob application ID.

```kotlin
MobileAds.initialize(
    this@MainActivity,
    // Use this application ID to initialize the GMA Next-Gen SDK if
    // you don't have an AdMob application ID.
    InitializationConfig.Builder(InitializationConfig.WEBVIEW_APIS_FOR_ADS_APPLICATION_ID)
        .build(),
  ) {
    // Adapter initialization complete.
  }
```

### Initialize SDK with WebView Application ID (Java)

Initialize the GMA Next-Gen SDK with WEBVIEW_APIS_FOR_ADS_APPLICATION_ID when you don't have an AdMob application ID.

```java
MobileAds.initialize(
    this,
    // Use this application ID to initialize the GMA Next-Gen SDK if
    // you don't have an AdMob application ID.
    new InitializationConfig.Builder(InitializationConfig.WEBVIEW_APIS_FOR_ADS_APPLICATION_ID)
        .build(),
        initializationStatus -> {
          // Adapter initialization is complete.
        });
```

### Register WebView (Kotlin)

Configure the WebView with ads-optimized settings and call MobileAds.registerWebView() to establish a connection with JavaScript ad handlers.

```kotlin
import android.webkit.CookieManager
import android.webkit.WebView
import com.google.android.libraries.ads.mobile.sdk.MobileAds

class MainActivity : AppCompatActivity() {
  lateinit var webView: WebView

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    webView = findViewById(R.id.webview)

    // Let the web view accept third-party cookies.
    CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true)
    // Let the web view use JavaScript.
    webView.settings.javaScriptEnabled = true
    // Let the web view access local storage.
    webView.settings.domStorageEnabled = true
    // Let HTML videos play automatically.
    webView.settings.mediaPlaybackRequiresUserGesture = false

    // Register the web view.
MobileAds.registerWebView(webView)
  }
}
```

### Register WebView (Java)

Configure the WebView with ads-optimized settings and call MobileAds.registerWebView() to establish a connection with JavaScript ad handlers.

```java
import android.webkit.CookieManager;
import android.webkit.WebView;
import com.google.android.libraries.ads.mobile.sdk.MobileAds;

public class MainActivity extends AppCompatActivity {
  private WebView webView;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    webView = findViewById(R.id.webview);

    // Let the web view accept third-party cookies.
    CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true);
    // Let the web view use JavaScript.
    webView.getSettings().setJavaScriptEnabled(true);
    // Let the web view access local storage.
    webView.getSettings().setDomStorageEnabled(true);
    // Let HTML videos play automatically.
    webView.getSettings().setMediaPlaybackRequiresUserGesture(false);

    // Register the web view.
MobileAds.registerWebView(webView);
  }
}
```

---

## 72. Click Behavior

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/click-behavior.md

Optimizing WebView click behavior by overriding shouldOverrideUrlLoading to handle custom URL schemes and cross-domain navigation with Custom Tabs.

### Add Custom Tabs Dependency

Add the androidx.browser dependency to your module-level build.gradle file for Custom Tabs support.

```groovy
dependencies {
  implementation 'androidx.browser:browser:1.5.0'
}
```

### Override shouldOverrideUrlLoading (Java)

Implement WebViewClient.shouldOverrideUrlLoading to handle custom URL schemes, same-domain navigation, and cross-domain navigation via Custom Tabs.

```java
public class MainActivity extends AppCompatActivity {

  private WebView webView;

  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    // ... Register the WebView.

    webView = new WebView(this);
    WebSettings webSettings = webView.getSettings();
    webSettings.setJavaScriptEnabled(true);
    webView.setWebViewClient(
        new WebViewClient() {
          // 1. Implement the web view click handler.
          @Override
          public boolean shouldOverrideUrlLoading(
              WebView view,
              WebResourceRequest request) {
            // 2. Determine whether to override the behavior of the URL.
            // If the target URL has no host and no scheme, return early.
            if (request.getUrl().getHost() == null && request.getUrl().getScheme() == null) {
              return false;
            }

            // Handle custom URL schemes such as market:// by attempting to
            // launch the corresponding application in a new intent.
            if (!request.getUrl().getScheme().equals("http")
                && !request.getUrl().getScheme().equals("https")) {
              Intent intent = new Intent(Intent.ACTION_VIEW, request.getUrl());
              // If the URL cannot be opened, return early.
              try {
                MainActivity.this.startActivity(intent);
              } catch (ActivityNotFoundException exception) {
                Log.d("TAG", "Failed to load URL with scheme:" + request.getUrl().getScheme());
              }
              return true;
            }

            String currentDomain;
            // If the current URL's host cannot be found, return early.
            try {
              currentDomain = new URI(view.getUrl()).toURL().getHost();
            } catch (URISyntaxException | MalformedURLException exception) {
              // Malformed URL.
              return false;
            }
            String targetDomain = request.getUrl().getHost();

            // If the current domain equals the target domain, the
            // assumption is the user is not navigating away from
            // the site. Reload the URL within the existing web view.
            if (currentDomain.equals(targetDomain)) {
              return false;
            }

            // 3. User is navigating away from the site, open the URL in
            // Custom Tabs to preserve the state of the web view.
            CustomTabsIntent intent = new CustomTabsIntent.Builder().build();
            intent.launchUrl(MainActivity.this, request.getUrl());
            return true;
          }
        });
  }
}
```

### Override shouldOverrideUrlLoading (Kotlin)

Implement WebViewClient.shouldOverrideUrlLoading to handle custom URL schemes, same-domain navigation, and cross-domain navigation via Custom Tabs.

```kotlin
class MainActivity : AppCompatActivity() {

  private lateinit var webView: WebView

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // ... Register the WebView.

    webView.webViewClient = object : WebViewClient() {
      // 1. Implement the web view click handler.
      override fun shouldOverrideUrlLoading(
          view: WebView?,
          request: WebResourceRequest?
      ): Boolean {
        // 2. Determine whether to override the behavior of the URL.
        // If the target URL has no host and no scheme, return early.
        if (request?.url?.host == null && request.url.scheme == null) {
          return false
        }
        val currentDomain = URI(view?.url).toURL().host

        // Handle custom URL schemes such as market:// by attempting to
        // launch the corresponding application in a new intent.
        if (!request.url.scheme.equals("http") &&
            !request.url.scheme.equals("https")) {
          val intent = Intent(Intent.ACTION_VIEW, request.url)
          // If the URL cannot be opened, return early.
          try {
            this@MainActivity.startActivity(intent)
          } catch (exception: ActivityNotFoundException) {
            Log.d("TAG", "Failed to load URL with scheme: ${request.url.scheme}")
          }
          return true
        }

        val targetDomain = request.url.host

        // If the current domain equals the target domain, the
        // assumption is the user is not navigating away from
        // the site. Reload the URL within the existing web view.
        if (currentDomain.equals(targetDomain)) {
          return false
        }

        // 3. User is navigating away from the site, open the URL in
        // Custom Tabs to preserve the state of the web view.
        val customTabsIntent = CustomTabsIntent.Builder().build()
        customTabsIntent.launchUrl(this@MainActivity, request.url)
        return true
      }
    }
  }
}
```

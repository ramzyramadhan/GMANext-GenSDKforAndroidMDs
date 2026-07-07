Select platform: [Android](https://developers.google.com/admob/android/next-gen/banner "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/banner "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/banner "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/banner "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/banner "View this page for the Android (Legacy) platform docs.")

<br />

![](https://developers.google.com/static/admob/images/format-banner.svg)
[Banner ads](https://support.google.com/admob/answer/9993556) are rectangular ads that occupy a portion of an app's layout. Anchored adaptive banners are fixed aspect ratio ads that stay on screen while users are interacting with the app, either anchored at the top or bottom of the screen.

<br />

This guide covers loading an anchored adaptive banner ad into an Android app.

## Prerequisites

- [Set up GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start).

## Always test with test ads

When building and testing your apps, make sure you use test ads rather than
live, production ads. Failure to do so can lead to suspension of your account.

The easiest way to load test ads is to use our dedicated test ad unit ID for
Android banners:

`ca-app-pub-3940256099942544/9214589741`

It's been specially configured to return test ads for every request, and you can
use it in your own apps while coding, testing, and debugging. Just make sure you
replace it with your own ad unit ID before publishing your app.

For more information about how GMA Next-Gen SDK test ads work, see
[Enable test ads](https://developers.google.com/admob/android/next-gen/test-ads).

## Create an `AdView` object

To display banners, do the following:

### Kotlin

1. Create an [`AdView`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/banner/AdView) object.
2. Add the `AdView` object to your app's layout.

The following example creates and adds the `AdView` object to an app layout:

    private fun createAdView(adViewContainer: FrameLayout, activity: Activity) {
      val adView = AdView(activity)
      adViewContainer.addView(adView)
    }

### Java

1. Create an [`AdView`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/banner/AdView) object.
2. Add the `AdView` object to your app's layout.

The following example creates and adds the `AdView` object to an app layout:

    private void createAdView(FrameLayout adViewContainer, Activity activity) {
      AdView adView = new AdView(activity);
      adViewContainer.addView(adView);
    }

### XML Layout

Add an `AdView` element to your layout XML file:

    <?xml version="1.0" encoding="utf-8"?>
    <androidx.constraintlayout.widget.ConstraintLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

      <com.google.android.libraries.ads.mobile.sdk.banner.AdView
          android:id="@+id/adView"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          app:layout_constraintBottom_toBottomOf="parent"
          app:layout_constraintEnd_toEndOf="parent"
          app:layout_constraintStart_toStartOf="parent" />
    </androidx.constraintlayout.widget.ConstraintLayout>

### Jetpack Compose

1. Include an `AndroidView` element in the Compose UI.
2. Define a `mutableStateOf<BannerAd?>` variable for holding the banner ad:

    // Initialize required variables.
    val context = LocalContext.current
    var bannerAdState by remember { mutableStateOf<BannerAd?>(null) }

    // The AdView is placed at the bottom of the screen.
    Column(modifier = modifier.fillMaxSize(), verticalArrangement = Arrangement.Bottom) {
      bannerAdState?.let { bannerAd ->
        Box(modifier = Modifier.fillMaxWidth()) {
          // Display the ad within an AndroidView.
          AndroidView(
            modifier = modifier.wrapContentSize(),
            factory = { bannerAd.getView(requireActivity()) },
          )
        }
      }
    }

## Load an ad

The following example loads a 360-width anchored adaptive banner ad into an
`AdView` object:

### Kotlin

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

### Java

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

### Jetpack Compose

    // Request an large anchored adaptive banner with a width of 360.
    val adSize = AdSize.getLargeAnchoredAdaptiveBannerAdSize(requireContext(), 360)

    // Load the ad when the screen is active.
    val coroutineScope = rememberCoroutineScope()
    val isPreviewMode = LocalInspectionMode.current
    LaunchedEffect(context) {
      bannerAdState?.destroy()
      if (!isPreviewMode) {
        coroutineScope.launch {
          when (val result = BannerAd.load(BannerAdRequest.Builder(AD_UNIT_ID, adSize).build())) {
            is AdLoadResult.Success -> {
              bannerAdState = result.ad
            }
            is AdLoadResult.Failure -> {
              showToast("Banner failed to load.")
              Log.w(Constant.TAG, "Banner ad failed to load: $result.error")
            }
          }
        }
      }
    }

## Refresh an ad

If you configured your ad unit to refresh, you don't need to request another ad
when the ad fails to load. GMA Next-Gen SDK respects any refresh rate
you specified in the AdMob UI. If you haven't enabled
refresh, issue a new request. For more details on ad unit refresh, such as
setting a refresh rate, see

[Use automatic refresh for Banner ads](https://support.google.com/admob/answer/3245199).

> [!NOTE]
> **Note:** When setting a refresh rate in the AdMob UI, the automatic refresh occurs only if the banner is visible on screen.

## Release an ad resource

When you are finished using a banner ad, you can release the banner ad's
resources.

To release the ad's resource, you remove the ad from the view hierarchy and
drop all its references:

### Kotlin

    // Remove banner from view hierarchy.
    val parentView = adView?.parent
    if (parentView is ViewGroup) {
      parentView.removeView(adView)
    }

    // Destroy the banner ad resources.
    adView?.destroy()

    // Drop reference to the banner ad.
    adView = null

### Java

    // Remove banner from view hierarchy.
    if (adView.getParent() instanceof ViewGroup) {
      ((ViewGroup) adView.getParent()).removeView(adView);
    }
    // Destroy the banner ad resources.
    adView.destroy();
    // Drop reference to the banner ad.
    adView = null;

### Jetpack Compose


    // Destroy the ad when the screen is disposed.
    DisposableEffect(Unit) { onDispose { bannerAdState?.destroy() } }

## Ad events

You can listen for a number of events in the ad's lifecycle, including ad
impression and click, as well as ad opening and closing. It is recommended to
set the callback before showing the banner.


### Kotlin

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

### Java

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



### Ad refresh callback

The `BannerAdRefreshCallback` handles ad refreshing events if you use [automatic refresh](https://support.google.com/admob/answer/3245199)
for banner ads. Make sure to set the callback before the you add the ad view to
your view hierarchy. For details on ad refreshing, see
[Refresh an ad](https://developers.google.com/admob/android/next-gen/banner#refresh_an_ad).

### Kotlin

    BannerAd.load(
      BannerAdRequest.Builder("ca-app-pub-3940256099942544/9214589741", adSize).build(),
      object : AdLoadCallback<BannerAd> {
        override fun onAdLoaded(ad: BannerAd) {
          ad.bannerAdRefreshCallback =
            object : BannerAdRefreshCallback {
              // Set the ad refresh callbacks.
              override fun onAdRefreshed() {
                // Called when the ad refreshes.
              }

              override fun onAdFailedToRefresh(loadAdError: LoadAdError) {
                // Called when the ad fails to refresh.
              }
            }

          // ...
        }
      }
    )

### Java

    BannerAd.load(
        new BannerAdRequest.Builder("ca-app-pub-3940256099942544/9214589741", adSize).build(),
        new AdLoadCallback<BannerAd>() {
          @Override
          public void onAdLoaded(@NonNull BannerAd ad) {
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
            // ...
          }
        });

## Hardware acceleration for video ads

In order for video ads to show successfully in your banner ad views, [hardware
acceleration](https://developer.android.com/guide/topics/graphics/hardware-accel) must
be enabled.

Hardware acceleration is enabled by default, but some apps may choose to disable
it. If this applies to your app, we recommend enabling hardware acceleration for
`Activity` classes that use ads.

#### Enable hardware acceleration

If your app does not behave properly with hardware acceleration turned on
globally, you can control it for individual activities as well. To enable or
disable hardware acceleration, you can use the `android:hardwareAccelerated`
attribute for the
[`<application>`](https://developer.android.com/guide/topics/manifest/application-element)
and
[`<activity>`](https://developer.android.com/guide/topics/manifest/activity-element)
elements in your `AndroidManifest.xml`. The following example enables hardware
acceleration for the entire app but disables it for one activity:

    <application android:hardwareAccelerated="true">
        <!-- For activities that use ads, hardwareAcceleration should be true. -->
        <activity android:hardwareAccelerated="true" />
        <!-- For activities that don't use ads, hardwareAcceleration can be false. -->
        <activity android:hardwareAccelerated="false" />
    </application>

See the [Hardware acceleration
guide](https://developer.android.com/guide/topics/graphics/hardware-accel) for more
information about options for controlling hardware acceleration. Note that
individual ad views cannot be enabled for hardware acceleration if the activity
is disabled, so the activity itself must have hardware acceleration enabled.

Download and run the
[example app](https://github.com/googleads/gma-next-gen-sdk-android-examples)
that demonstrates the use of the GMA Next-Gen SDK.
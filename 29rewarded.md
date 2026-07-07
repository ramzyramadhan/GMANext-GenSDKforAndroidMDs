Select platform: [Android](https://developers.google.com/admob/android/next-gen/rewarded "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/rewarded "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/rewarded "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/rewarded "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/rewarded "View this page for the Android (Legacy) platform docs.")

<br />

[Rewarded ads](https://support.google.com/admob/answer/7372450) allow you to reward users with in-app items for interacting with video ads, playable ads, and surveys.

<br />

## Prerequisites

- [Set up GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start).

## Always test with test ads

- When building and testing your apps, make sure you use test ads rather than live, production ads. Failure to do so can lead to suspension of your account.
- The easiest way to load test ads is to use our dedicated test ad unit ID for Android rewarded ads:
- `ca-app-pub-3940256099942544/5224354917`
- It's been specially configured to return test ads for every request, and you're free to use it in your own apps while coding, testing, and debugging. Just make sure you replace it with your own ad unit ID before publishing your app.
- For details on GMA Next-Gen SDK test ads, see [Enable test ads](https://developers.google.com/admob/android/next-gen/test-ads).

## Load an ad

- To load an ad, GMA Next-Gen SDK offers the following:
  - Load with the [single ad loading API](https://developers.google.com/admob/android/next-gen/rewarded#load-with-single-load).

  - Load with the [ad preloading API](https://developers.google.com/admob/android/next-gen/rewarded#load-with-preloading), which eliminates
    the need for manual ad loading and caching.

### Load with the single ad loading API

- The following example shows you how to load a single ad:

### Kotlin

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

### Java

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

### Load with the ad preloading API

- To start preloading, do the following:
  1. Initialize a preload configuration with an ad request.

  2. Start the preloader for rewarded ads with your ad unit ID and preload
     configuration:

### Kotlin

    private fun startPreloading(adUnitId: String) {
      val adRequest = AdRequest.Builder(adUnitId).build()
      val preloadConfig = PreloadConfiguration(adRequest)
      RewardedAdPreloader.start(adUnitId, preloadConfig)
    }

### Java

    private void startPreloading(String adUnitId) {
      AdRequest adRequest = new AdRequest.Builder(adUnitId).build();
      PreloadConfiguration preloadConfig = new PreloadConfiguration(adRequest);
      RewardedAdPreloader.start(adUnitId, preloadConfig);
    }

- When you're ready to show the ad, poll the ad from the preloader:

### Kotlin

    // Polling returns the next available ad and loads another ad in the background.
    val ad = RewardedAdPreloader.pollAd(adUnitId)

### Java

    // Polling returns the next available ad and loads another ad in the background.
    final RewardedAd ad = RewardedAdPreloader.pollAd(adUnitId);

## Set the RewardedAdEventCallback

- The `RewardedAdEventCallback` handles events related to displaying your `RewardedAd`. Before showing the rewarded ad, make sure to set the callback:

### Kotlin

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

### Java

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

## Show the ad

- To show a rewarded ad, use the `show()` method. Use an `OnUserEarnedRewardListener` object to handle reward events.

### Kotlin

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

### Java

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

## FAQ

Is there a timeout for the initialization call?
:   After 10 seconds, GMA Next-Gen SDK invokes the
    `OnInitializationCompleteListener` even if a mediation network still hasn't
    completed initialization.

What if some mediation networks aren't ready when I get the initialization callback?

:   We recommend loading an ad inside the callback of the
    `OnInitializationCompleteListener`. Even if a mediation network is not ready,
    GMA Next-Gen SDK still asks that network for an ad. So if a
    mediation network finishes initializing after the timeout, it can still
    service future ad requests in that session.

    You can continue to poll the initialization status of all adapters throughout
    your app session by calling `MobileAds.getInitializationStatus()`.

How do I find out why a particular mediation network isn't ready?

:   `AdapterStatus.getDescription()` describes why an adapter is not ready to
    service ad requests.

Does the [`onUserEarnedReward()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/rewarded/OnUserEarnedRewardListener#onUserEarnedReward(com.google.android.libraries.ads.mobile.sdk.rewarded.RewardItem)) callback always get called before the [`onAdDismissedFullScreenContent()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/AdEventCallback#onAdDismissedFullScreenContent()) callback?

:   For Google ads, all `onUserEarnedReward()` calls occur before
    `onAdDismissedFullScreenContent()`. For ads served through
    [mediation](https://developers.google.com/admob/android/next-gen/mediation), the third-party ad
    network SDK's implementation determines the callback order. For ad network
    SDKs that provide a single close callback with reward information, the
    mediation adapter invokes `onUserEarnedReward()` before
    `onAdDismissedFullScreenContent()`.

## Example

- Download and run the [example app](https://github.com/googleads/gma-next-gen-sdk-android-examples) that demonstrates the use of the GMA Next-Gen SDK.
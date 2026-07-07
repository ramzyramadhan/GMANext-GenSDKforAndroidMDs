Select platform: [Android](https://developers.google.com/admob/android/next-gen/rewarded-interstitial "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/rewarded-interstitial "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/rewarded-interstitial "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/rewarded-interstitial "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/rewarded-interstitial "View this page for the Android (Legacy) platform docs.")

<br />

[Rewarded interstitial](https://support.google.com/admob/answer/9884467) is a type
of incentivized ad format that lets you offer rewards for ads that appear
automatically during natural app transitions. Unlike rewarded ads, users aren't
required to opt in to view a rewarded interstitial.

## Prerequisites

Before you continue, [set up GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start).

## Always test with test ads

When building and testing your apps, make sure you use test ads rather than
live, production ads. Failure to do so can lead to suspension of your account.

The easiest way to load test ads is to use our dedicated test ad unit ID for
Android rewarded interstitial ads:

`ca-app-pub-3940256099942544/5354046379`

It's been specially configured to return test ads for every request, and you're
free to use it in your own apps while coding, testing, and debugging. Just make
sure you replace it with your own ad unit ID before publishing your app.

For details on GMA Next-Gen SDK test ads, see
[Enable test ads](https://developers.google.com/admob/android/next-gen/test-ads).

## Load an ad

To load an ad, GMA Next-Gen SDK offers the following:

- Load with the [single ad loading API](https://developers.google.com/admob/android/next-gen/rewarded-interstitial#load-with-single-load).

- Load with the [ad preloading API](https://developers.google.com/admob/android/next-gen/rewarded-interstitial#load-with-preloading), which eliminates
  the need for manual ad loading and caching.

### Load with the single ad loading API

The following example shows you how to load a single ad:

### Kotlin

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

### Java

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

### Load with the ad preloading API

To start preloading, do the following:

1. Initialize a preload configuration with an ad request.

2. Start the preloader for rewarded interstitial ads with your ad unit ID and
   preload configuration:

### Kotlin

    private fun startPreloading(adUnitId: String) {
      val adRequest = AdRequest.Builder(adUnitId).build()
      val preloadConfig = PreloadConfiguration(adRequest)
      RewardedInterstitialAdPreloader.start(adUnitId, preloadConfig)
    }

### Java

    private void startPreloading(String adUnitId) {
      AdRequest adRequest = new AdRequest.Builder(adUnitId).build();
      PreloadConfiguration preloadConfig = new PreloadConfiguration(adRequest);
      RewardedInterstitialAdPreloader.start(adUnitId, preloadConfig);
    }

When you're ready to show the ad, poll the ad from the preloader:

### Kotlin

    // Polling returns the next available ad and loads another ad in the background.
    val ad = RewardedInterstitialAdPreloader.pollAd(adUnitId)

### Java

    // Polling returns the next available ad and loads another ad in the background.
    final RewardedInterstitialAd ad = RewardedInterstitialAdPreloader.pollAd(adUnitId);

## Set the RewardedInterstitialAdEventCallback

The `RewardedInterstitialAdEventCallback` handles events related to displaying
your `RewardedInterstitialAd`. Before showing the rewarded interstitial ad, make
sure to set the callback:

### Kotlin

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

### Java

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

## Show the ad

To show a rewarded interstitial ad, use the `show()` method. Use an
`OnUserEarnedRewardListener` object to handle reward events.

### Kotlin

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

### Java

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

## Example

Download and run the
[example app](https://github.com/googleads/gma-next-gen-sdk-android-examples)
that demonstrates the use of the GMA Next-Gen SDK.
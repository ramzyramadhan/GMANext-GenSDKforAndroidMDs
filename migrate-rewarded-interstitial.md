This page covers the differences in loading and showing a rewarded
interstitial ad.

## Load an ad

The following examples load a rewarded interstitial ad:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin RewardedInterstitialAd.load( this@RewardedInterstitialActivity, "AD_UNIT_ID", AdRequest.Builder().build(), object : RewardedInterstitialAdLoadCallback() { override fun onAdLoaded(ad: RewardedInterstitialAd) { // Called when an ad has loaded. ad.fullScreenContentCallback = object : FullScreenContentCallback() { } rewardedInterstitialAd = ad } override fun onAdFailedToLoad(loadAdError: LoadAdError) { // Called when ad fails to load. } } ) ``` ### Java ```java RewardedInterstitialAd.load( this, "AD_UNIT_ID", new AdRequest.Builder().build(), new RewardedInterstitialAdLoadCallback() { @Override public void onAdLoaded(@NonNull RewardedInterstitialAd ad) { // Called when an ad has loaded. ad.setFullScreenContentCallback(new FullScreenContentCallback() {}); rewardedInterstitialAd = ad; } @Override public void onAdFailedToLoad(@NonNull LoadAdError loadAdError) { // Called when ad fails to load. } } ); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin RewardedInterstitialAd.load( AdRequest.Builder("AD_UNIT_ID").build(), object : AdLoadCallback<RewardedInterstitialAd> { override fun onAdLoaded(ad: RewardedInterstitialAd) { // Called when an ad has loaded. ad.adEventCallback = object : RewardedInterstitialAdEventCallback { } rewardedInterstitialAd = ad } override fun onAdFailedToLoad(loadAdError: LoadAdError) { // Called when ad fails to load. } } ) ``` ### Java ```java RewardedInterstitialAd.load( new AdRequest.Builder("AD_UNIT_ID").build(), new AdLoadCallback<RewardedInterstitialAd>() { @Override public void onAdLoaded(@NonNull RewardedInterstitialAd ad) { // Called when an ad has loaded. ad.setAdEventCallback(new RewardedInterstitialAdEventCallback() {}); rewardedInterstitialAd = ad; } @Override public void onAdFailedToLoad(@NonNull LoadAdError adError) { // Called when ad fails to load. } }); ``` |

## Show an ad

The following examples show a rewarded interstitial ad:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin rewardedInterstitialAd?.show( this@RewardedInterstitialActivity, object : OnUserEarnedRewardListener { override fun onUserEarnedReward(rewardItem: RewardItem) { // User earned the reward. val rewardAmount = rewardItem.amount val rewardType = rewardItem.type } } ) ``` ### Java ```java rewardedInterstitialAd.show( this, new OnUserEarnedRewardListener() { @Override public void onUserEarnedReward(@NonNull RewardItem rewardItem) { // User earned the reward. int rewardAmount = rewardItem.getAmount(); String rewardType = rewardItem.getType(); } }); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin rewardedInterstitialAd?.show( this@RewardedInterstitialActivity, object : OnUserEarnedRewardListener { override fun onUserEarnedReward(rewardItem: RewardItem) { // User earned the reward. val rewardAmount = rewardItem.amount val rewardType = rewardItem.type } } ) ``` ### Java ```java rewardedInterstitialAd.show( this, new OnUserEarnedRewardListener() { @Override public void onUserEarnedReward(@NonNull RewardItem rewardItem) { // User earned the reward. int rewardAmount = rewardItem.getAmount(); String rewardType = rewardItem.getType(); } }); ``` |
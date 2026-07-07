This page covers the differences in loading and showing a rewarded ad.

## Load an ad

The following examples load a rewarded ad:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin RewardedAd.load( this@RewardedActivity, "AD_UNIT_ID", AdRequest.Builder().build(), object : RewardedAdLoadCallback() { override fun onAdLoaded(ad: RewardedAd) { // Called when an ad has loaded. ad.fullScreenContentCallback = object : FullScreenContentCallback() { } rewardedAd = ad } override fun onAdFailedToLoad(loadAdError: LoadAdError) { // Called when ad fails to load. } } ) ``` ### Java ```java RewardedAd.load( this, "AD_UNIT_ID", new AdRequest.Builder().build(), new RewardedAdLoadCallback() { @Override public void onAdLoaded(@NonNull RewardedAd ad) { // Called when an ad has loaded. ad.setFullScreenContentCallback(new FullScreenContentCallback() {}); rewardedAd = ad; } @Override public void onAdFailedToLoad(@NonNull LoadAdError loadAdError) { // Called when ad fails to load. } } ); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin RewardedAd.load( AdRequest.Builder("AD_UNIT_ID").build(), object : AdLoadCallback<RewardedAd> { override fun onAdLoaded(ad: RewardedAd) { // Called when an ad has loaded. ad.adEventCallback = object : RewardedAdEventCallback { } rewardedAd = ad } override fun onAdFailedToLoad(loadAdError: LoadAdError) { // Called when ad fails to load. } } ) ``` ### Java ```java RewardedAd.load( new AdRequest.Builder("AD_UNIT_ID").build(), new AdLoadCallback<RewardedAd>() { @Override public void onAdLoaded(@NonNull RewardedAd ad) { // Called when an ad has loaded. ad.setAdEventCallback(new RewardedAdEventCallback() {}); rewardedAd = ad; } @Override public void onAdFailedToLoad(@NonNull LoadAdError adError) { // Called when ad fails to load. } }); ``` |

## Show an ad

The following examples show a rewarded ad:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin rewardedAd?.show( this@RewardedActivity, object : OnUserEarnedRewardListener { override fun onUserEarnedReward(rewardItem: RewardItem) { // User earned the reward. val rewardAmount = rewardItem.amount val rewardType = rewardItem.type } } ) ``` ### Java ```java rewardedAd.show( this, new OnUserEarnedRewardListener() { @Override public void onUserEarnedReward(@NonNull RewardItem rewardItem) { // User earned the reward. int rewardAmount = rewardItem.getAmount(); String rewardType = rewardItem.getType(); } }); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin rewardedAd?.show( this@RewardedActivity, object : OnUserEarnedRewardListener { override fun onUserEarnedReward(rewardItem: RewardItem) { // User earned the reward. val rewardAmount = rewardItem.amount val rewardType = rewardItem.type } } ) ``` ### Java ```java rewardedAd.show( this, new OnUserEarnedRewardListener() { @Override public void onUserEarnedReward(@NonNull RewardItem rewardItem) { // User earned the reward. int rewardAmount = rewardItem.getAmount(); String rewardType = rewardItem.getType(); } }); ``` |
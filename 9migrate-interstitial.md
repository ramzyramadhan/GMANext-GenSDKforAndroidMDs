[Video](https://www.youtube.com/watch?v=a94TRVp-b1k)

<br />


<br />

This page covers the differences in loading and showing an interstitial ad
between Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK.

## Load an ad

The following examples load an interstitial ad:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin InterstitialAd.load( this@InterstitialActivity, "AD_UNIT_ID", AdRequest.Builder().build(), object : InterstitialAdLoadCallback() { override fun onAdLoaded(ad: InterstitialAd) { // Called when an ad has loaded. ad.fullScreenContentCallback = object : FullScreenContentCallback() { } interstitialAd = ad } override fun onAdFailedToLoad(loadAdError: LoadAdError) { // Called when ad fails to load. } } ) ``` ### Java ```java InterstitialAd.load( this, "AD_UNIT_ID", new AdRequest.Builder().build(), new InterstitialAdLoadCallback() { @Override public void onAdLoaded(@NonNull InterstitialAd ad) { // Called when an ad has loaded. ad.setFullScreenContentCallback(new FullScreenContentCallback() {}); interstitialAd = ad; } @Override public void onAdFailedToLoad(@NonNull LoadAdError loadAdError) { // Called when ad fails to load. } } ); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin InterstitialAd.load( AdRequest.Builder("AD_UNIT_ID").build(), object : AdLoadCallback<InterstitialAd> { override fun onAdLoaded(ad: InterstitialAd) { // Called when an ad has loaded. ad.adEventCallback = object : InterstitialAdEventCallback { } interstitialAd = ad } override fun onAdFailedToLoad(loadAdError: LoadAdError) { // Called when ad fails to load. } } ) ``` ### Java ```java InterstitialAd.load( new AdRequest.Builder("AD_UNIT_ID").build(), new AdLoadCallback<InterstitialAd>() { @Override public void onAdLoaded(@NonNull InterstitialAd ad) { // Called when an ad has loaded. ad.setAdEventCallback(new InterstitialAdEventCallback() {}); interstitialAd = ad; } @Override public void onAdFailedToLoad(@NonNull LoadAdError adError) { // Called when ad fails to load. } }); ``` |

## Show an ad

The following examples show an interstitial ad:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin interstitialAd?.show(this@InterstitialActivity) ``` ### Java ```java interstitialAd.show(this); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin interstitialAd?.show(this@InterstitialActivity) ``` ### Java ```java interstitialAd.show(this); ``` |
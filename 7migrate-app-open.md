This page covers the differences in loading and showing an app open ad
between Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK.

## Load an ad

The following examples load an app open ad:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin AppOpenAd.load( this@AppOpenActivity, "AD_UNIT_ID", AdRequest.Builder().build(), object : AppOpenAdLoadCallback() { override fun onAdLoaded(ad: AppOpenAd) { // Called when an ad has loaded. ad.fullScreenContentCallback = object : FullScreenContentCallback() { } appOpenAd = ad } override fun onAdFailedToLoad(loadAdError: LoadAdError) { // Called when ad fails to load. } } ) ``` ### Java ```java AppOpenAd.load( this, "AD_UNIT_ID", new AdRequest.Builder().build(), new AppOpenAdLoadCallback() { @Override public void onAdLoaded(@NonNull AppOpenAd ad) { // Called when an ad has loaded. ad.setFullScreenContentCallback(new FullScreenContentCallback() {}); appOpenAd = ad; } @Override public void onAdFailedToLoad(@NonNull LoadAdError loadAdError) { // Called when ad fails to load. } } ); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin AppOpenAd.load( AdRequest.Builder("AD_UNIT_ID").build(), object : AdLoadCallback<AppOpenAd> { override fun onAdLoaded(ad: AppOpenAd) { // Called when an ad has loaded. ad.adEventCallback = object : AppOpenAdEventCallback { } appOpenAd = ad } override fun onAdFailedToLoad(loadAdError: LoadAdError) { // Called when ad fails to load. } } ) ``` ### Java ```java AppOpenAd.load( new AdRequest.Builder("AD_UNIT_ID").build(), new AdLoadCallback<AppOpenAd>() { @Override public void onAdLoaded(@NonNull AppOpenAd ad) { // Called when an ad has loaded. ad.setAdEventCallback(new AppOpenAdEventCallback() {}); appOpenAd = ad; } @Override public void onAdFailedToLoad(@NonNull LoadAdError adError) { // Called when ad fails to load. } }); ``` |

## Show an ad

The following examples show an app open ad:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin appOpenAd?.show(this@AppOpenActivity) ``` ### Java ```java appOpenAd.show(this); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin appOpenAd?.show(this@AppOpenActivity) ``` ### Java ```java appOpenAd.show(this); ``` |
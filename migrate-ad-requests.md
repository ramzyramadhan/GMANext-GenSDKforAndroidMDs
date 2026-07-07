**Last updated:** January 10, 2025

<br />

This page covers the instructions to migrate ad requests.

GMA Next-Gen SDK requires you to pass the
AdMob ad unit ID directly to the `AdRequest` object,
rather than passing it to the load ad method.

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin val adRequest = AdRequest.Builder().build() InterstitialAd.load( this, "AD_UNIT_ID", adRequest, object : InterstitialAdLoadCallback() { } ) ``` ### Java ```java AdRequest adRequest = new AdRequest.Builder().build(); InterstitialAd.load( this, "AD_UNIT_ID", adRequest, new InterstitialAdLoadCallback() { } ); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin val adRequest = AdRequest.Builder("AD_UNIT_ID").build() InterstitialAd.load(adRequest, object : AdLoadCallback<InterstitialAd> {}) ``` ### Java ```java AdRequest adRequest = new AdRequest.Builder("AD_UNIT_ID").build(); InterstitialAd.load(adRequest, new AdLoadCallback<InterstitialAd>() {}); ``` |

## Pass extra parameters to AdMob

The following examples pass extra parameters to AdMob to request
non-personalized ads:

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin val extras = Bundle() extras.putInt("npa", 1) val request = AdRequest.Builder() .addNetworkExtrasBundle(AdMobAdapter::class.java, extras) .build() ``` ### Java ```java Bundle extras = new Bundle(); extras.putInt("npa", 1); AdRequest request = new AdRequest.Builder() .addNetworkExtrasBundle(AdMobAdapter.class, extras) .build(); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin val extras = Bundle() extras.putInt("npa", 1) val request = AdRequest.Builder("AD_UNIT_ID") .setGoogleExtrasBundle(extras) .build() ``` ### Java ```java Bundle extras = new Bundle(); extras.putInt("npa", 1); AdRequest request = new AdRequest.Builder("AD_UNIT_ID") .setGoogleExtrasBundle(extras) .build(); ``` |

## Pass extra parameters to an ad source adapter

The following examples pass extra parameters to a sample ad source adapter. For
details on passing extra parameters to a specific ad source adapter, see the
corresponding
[ad source integration guide](https://developers.google.com/admob/android/next-gen/mediation/choose-networks#network_details).

|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin val extras = Bundle() extras.putString("exampleKey", "exampleValue") val request = AdRequest.Builder() .addNetworkExtrasBundle(SampleAdapter::class, extras) .build() ``` ### Java ```java Bundle extras = new Bundle(); extras.putString("exampleKey", "exampleValue"); AdRequest request = new AdRequest.Builder() .addNetworkExtrasBundle(SampleAdapter.class, extras) .build(); ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin val extras = Bundle() extras.putString("exampleKey", "exampleValue") val request = AdRequest.Builder("AD_UNIT_ID") .putAdSourceExtrasBundle(SampleAdapter::class.java, extras) .build() ``` ### Java ```java Bundle extras = new Bundle(); extras.putString("exampleKey", "exampleValue"); AdRequest request = new AdRequest.Builder("AD_UNIT_ID") .putAdSourceExtrasBundle(SampleAdapter.class, extras) .build(); ``` |
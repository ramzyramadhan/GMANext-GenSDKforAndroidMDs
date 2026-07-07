Select platform: [Android](https://developers.google.com/admob/android/next-gen/custom-events/interstitial "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/custom-events/interstitial "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/custom-events/interstitial "View this page for the Android (Legacy) platform docs.")

<br />

## Prerequisites

Complete the [custom events setup](https://developers.google.com/admob/android/next-gen/custom-events/setup).

## Request an interstitial ad

When the custom event line item is reached in the waterfall mediation chain,
the `loadInterstitialAd()` method is called on the class name you provided when
[creating a custom
event](https://developers.google.com/admob/android/next-gen/custom-events/setup#create). In this case,
that method is in `SampleCustomEvent`, which then calls the
`loadInterstitialAd()` method in `SampleInterstitialCustomEventLoader`.

To request an interstitial ad, create or modify a class that extends `Adapter`
to implement `loadInterstitialAd()`. Additionally, create a new class to
implement `MediationInterstitialAd`.

In our [custom event example](https://github.com/googleads/googleads-mobile-android-mediation/blob/main/Example/customevent/src/main/java/com/google/ads/mediation/sample/customevent/SampleCustomEvent.java),
`SampleCustomEvent` extends the `Adapter` class and then delegates to
`SampleInterstitialCustomEventLoader`.

### Java

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

`SampleInterstitialCustomEventLoader` is responsible for the following tasks:

- Loading the interstitial ad and invoking a
  [`MediationAdLoadCallback`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationAdLoadCallback)
  method once loading completes.

- Implementing the `MediationInterstitialAd` interface.

- Receiving and reporting ad event callbacks to GMA Next-Gen SDK.

The optional parameter defined in the AdMob UI is
included in the ad configuration. The parameter can be accessed through
`adConfiguration.getServerParameters().getString(MediationConfiguration.CUSTOM_EVENT_SERVER_PARAMETER_FIELD)`.
This parameter is typically an ad unit identifier that an ad network SDK
requires when instantiating an ad object.

### Java

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

Depending on whether the ad is successfully fetched or encounters an error, you
would call either
[`onSuccess()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationAdLoadCallback#onSuccess(MediationAdT))
or
[`onFailure()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationAdLoadCallback#onFailure(com.google.android.gms.ads.AdError)).
`onSuccess()` is called by passing in an instance of the class that implements
`MediationInterstitialAd`.

Typically, these methods are implemented inside callbacks from the
third-party SDK your adapter implements. For this example, the Sample SDK
has a `SampleAdListener` with relevant callbacks:

### Java

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

`MediationInterstitialAd` requires implementing a `showAd()` method to display
the ad:

### Java

```java
@Override
public void showAd(@NonNull Context context) {
  sampleInterstitialAd.show();
}
```

## Forward mediation events to GMA Next-Gen SDK

Once `onSuccess()` is called, the returned `MediationInterstitialAdCallback`
object can then be used by the adapter to forward presentation events from the
third-party SDK to GMA Next-Gen SDK. The
`SampleInterstitialCustomEventLoader` class extends the `SampleAdListener`
interface to forward callbacks from the sample ad network to the Google Mobile
Ads SDK.

It's important that your custom event forwards as many of these callbacks as
possible, so that your app receives these equivalent events from the Google
Mobile Ads SDK. Here's an example of using callbacks:

### Java

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

This completes the custom events implementation for interstitial ads. The full
example is available on
[GitHub](https://github.com/googleads/googleads-mobile-android-mediation/tree/master/Example/customevent/src/main/java/com/google/ads/mediation/sample/customevent).
You can use it with an ad network that is already supported or modify it to
display custom event interstitial ads.
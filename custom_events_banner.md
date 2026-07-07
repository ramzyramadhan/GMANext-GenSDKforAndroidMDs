Select platform: [Android](https://developers.google.com/admob/android/next-gen/custom-events/banner "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/custom-events/banner "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/custom-events/banner "View this page for the Android (Legacy) platform docs.")

<br />

## Prerequisites

Complete the [custom events setup](https://developers.google.com/admob/android/next-gen/custom-events/setup).

## Request a banner ad

When the custom event line item is reached in the waterfall mediation chain,
the `loadBannerAd()` method is called on the class name you provided when
[creating a custom
event](https://developers.google.com/admob/android/next-gen/custom-events/custom-events/setup#create). In this case,
that method is in `SampleCustomEvent`, which then calls the `loadBannerAd()`
method in `SampleBannerCustomEventLoader`.

To request a banner ad, create or modify a class that extends `Adapter` to
implement `loadBannerAd()`. Additionally, create a new class to implement
`MediationBannerAd`.

In our [custom event example](https://github.com/googleads/googleads-mobile-android-mediation/blob/main/Example/customevent/src/main/java/com/google/ads/mediation/sample/customevent/SampleCustomEvent.java),
`SampleCustomEvent` extends the `Adapter` class and then delegates to
`SampleBannerCustomEventLoader`.

### Java

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

`SampleBannerCustomEventLoader` is responsible for the following tasks:

- Loading the banner ad and invoking a
  [`MediationAdLoadCallback`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationAdLoadCallback)
  method once loading completes.

- Implementing the `MediationBannerAd` interface.

- Receiving and reporting ad event callbacks to GMA Next-Gen SDK.

The optional parameter defined in the UI is
included in the ad configuration. The parameter can be accessed through
`adConfiguration.getServerParameters().getString(MediationConfiguration.CUSTOM_EVENT_SERVER_PARAMETER_FIELD)`.
This parameter is typically an ad unit identifier that an ad network SDK
requires when instantiating an ad object.

### Java

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

Depending on whether the ad is successfully fetched or encounters an error, you
would call either
[`onSuccess()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationAdLoadCallback#onSuccess(MediationAdT))
or
[`onFailure()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationAdLoadCallback#onFailure(com.google.android.gms.ads.AdError)).
`onSuccess()` is called by passing in an instance of the class that implements
`MediationBannerAd`.

Typically, these methods are implemented inside callbacks from the
third-party SDK your adapter implements. For this example, the Sample SDK
has a `SampleAdListener` with relevant callbacks:

### Java

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

`MediationBannerAd` requires implementing a `View` getter method:

### Java

```java
@Override
@NonNull
public View getView() {
  return sampleAdView;
}
```

## Forward mediation events to GMA Next-Gen SDK

Once `onSuccess()` is called, the returned `MediationBannerAdCallback` object
can then be used by the adapter to forward presentation events from the
third-party SDK to GMA Next-Gen SDK. The
`SampleBannerCustomEventLoader` class extends the `SampleAdListener` interface
to forward callbacks from the sample ad network to GMA Next-Gen SDK.

It's important that your custom event forwards as many of these callbacks as
possible, so that your app receives these equivalent events from the Google
Mobile Ads SDK. Here's an example of using callbacks:

### Java

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

This completes the custom events implementation for banner ads. The full example
is available on
[GitHub](https://github.com/googleads/googleads-mobile-android-mediation/tree/master/Example/customevent/src/main/java/com/google/ads/mediation/sample/customevent).
You can use it with an ad network that is already supported or modify it to
display custom event banner ads.
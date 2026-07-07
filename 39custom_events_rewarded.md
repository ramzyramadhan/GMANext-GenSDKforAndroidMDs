Select platform: [Android](https://developers.google.com/admob/android/next-gen/custom-events/rewarded "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/custom-events/rewarded "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/custom-events/rewarded "View this page for the Android (Legacy) platform docs.")

<br />

## Prerequisites

Complete the [custom events setup](https://developers.google.com/admob/android/next-gen/custom-events/setup).

## Request a rewarded ad

When the custom event line item is reached in the waterfall mediation chain,
the `loadRewardedAd()` method is called on the class name you
provided when [creating a custom
event](https://developers.google.com/admob/android/next-gen/custom-events/setup#create). In this case,
that method is in `SampleCustomEvent`, which then calls the `loadRewardedAd()`
method in `SampleRewardedCustomEventLoader`.

To request a rewarded ad, create or modify a class that extends `Adapter` to
implement `loadRewardedAd()`. Additionally, create a new class to implement
`MediationRewardedAd`.

In our [custom event example](https://github.com/googleads/googleads-mobile-android-mediation/blob/main/Example/customevent/src/main/java/com/google/ads/mediation/sample/customevent/SampleCustomEvent.java),
`SampleCustomEvent` extends the `Adapter` class and then delegates to
`SampleRewardedCustomEventLoader`.

### Java

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationRewardedAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationRewardedAd;
import com.google.android.gms.ads.mediation.MediationRewardedAdCallback;
...

public class SampleCustomEvent extends Adapter {

  private SampleNativeCustomEventLoader nativeLoader;

  @Override
  public void loadRewardedAd(
      @NonNull MediationRewardedAdConfiguration mediationRewardedAdConfiguration,
      @NonNull
          MediationAdLoadCallback<MediationRewardedAd, MediationRewardedAdCallback>
              mediationAdLoadCallback) {
    rewardedLoader =
        new SampleRewardedCustomEventLoader(
            mediationRewardedAdConfiguration, mediationAdLoadCallback);
    rewardedLoader.loadAd();
  }
}
```

`SampleRewardedCustomEventLoader` is responsible for the following tasks:

- Loading the rewarded ad

- Implementing the `MediationRewardedAd` interface.

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
import com.google.android.gms.ads.mediation.MediationRewardedAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationRewardedAd;
import com.google.android.gms.ads.mediation.MediationRewardedAdCallback;
...

public class SampleRewardedCustomEventLoader extends SampleRewardedAdListener
    implements MediationRewardedAd {

  /** Configuration for requesting the rewarded ad from the third-party network. */
  private final MediationRewardedAdConfiguration mediationRewardedAdConfiguration;

  /**
   * A {@link MediationAdLoadCallback} that handles any callback when a Sample
   * rewarded ad finishes loading.
   */
  private final MediationAdLoadCallback<MediationRewardedAd, MediationRewardedAdCallback>
      mediationAdLoadCallback;

  /** Callback for rewarded ad events. */
  private MediationRewardedAdCallback rewardedAdCallback;

  /** Constructor. */
  public SampleRewardedCustomEventLoader(
      @NonNull MediationRewardedAdConfiguration mediationRewardedAdConfiguration,
      @NonNull MediationAdLoadCallback<MediationRewardedAd, MediationRewardedAdCallback>
              mediationAdLoadCallback) {
    this.mediationRewardedAdConfiguration = mediationRewardedAdConfiguration;
    this.mediationAdLoadCallback = mediationAdLoadCallback;
  }

  /** Loads the rewarded ad from the third-party ad network. */
  public void loadAd() {
    // All custom events have a server parameter named "parameter" that returns
    // back the parameter entered into the AdMob UI when defining the custom event.
    Log.i("RewardedCustomEvent", "Begin loading rewarded ad.");
    String serverParameter = mediationRewardedAdConfiguration
        .getServerParameters()
        .getString(MediationConfiguration
        .CUSTOM_EVENT_SERVER_PARAMETER_FIELD);
    Log.d("RewardedCustomEvent", "Received server parameter.");
    SampleAdRequest request = createSampleRequest(mediationRewardedAdConfiguration);
    sampleRewardedAd = new SampleRewardedAd(serverParameter);
    sampleRewardedAd.setListener(this);
    Log.i("RewardedCustomEvent", "Start fetching rewarded ad.");
    sampleRewardedAd.loadAd(request);
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
`MediationRewardedAd`.

Typically, these methods are implemented inside callbacks from the
third-party SDK your adapter implements. For this example, the Sample SDK
has a `SampleAdListener` with relevant callbacks:

### Java

```java
@Override
public void onRewardedAdLoaded() {
  rewardedAdCallback = mediationAdLoadCallback.onSuccess(this);
}

@Override
public void onRewardedAdFailedToLoad(SampleErrorCode errorCode) {
    mediationAdLoadCallback.onFailure(SampleCustomEventError.createSampleSdkError(errorCode));
}
```

`MediationRewardedAd` requires implementing a `showAd()` method to display
the ad:

### Java

```java
@Override
public void showAd(Context context) {
  if (!(context instanceof Activity)) {
    rewardedAdCallback.onAdFailedToShow(
        SampleCustomEventError.createCustomEventNoActivityContextError());
    return;
  }
  Activity activity = (Activity) context;

  if (!sampleRewardedAd.isAdAvailable()) {
    rewardedAdCallback.onAdFailedToShow(
        SampleCustomEventError.createCustomEventAdNotAvailableError());
    return;
  }
  sampleRewardedAd.showAd(activity);
}
```

## Forward mediation events to GMA Next-Gen SDK

Once `onSuccess()` is called, the returned `MediationRewardedAdCallback`
object can then be used by the adapter to forward presentation events from the
third-party SDK to GMA Next-Gen SDK. The
`SampleRewardedCustomEventLoader` class extends the `SampleAdListener`
interface to forward callbacks from the sample ad network to the Google Mobile
Ads SDK.

It's important that your custom event forwards as many of these callbacks as
possible, so that your app receives these equivalent events from the
GMA Next-Gen SDK. Here's an example of using callbacks:

### Java

```java
@Override
public void onAdRewarded(final String rewardType, final int amount) {
  RewardItem rewardItem =
      new RewardItem() {
        @Override
        public String getType() {
          return rewardType;
        }

        @Override
        public int getAmount() {
          return amount;
        }
      };
  rewardedAdCallback.onUserEarnedReward(rewardItem);
}

@Override
public void onAdClicked() {
  rewardedAdCallback.reportAdClicked();
}

@Override
public void onAdFullScreen() {
  rewardedAdCallback.onAdOpened();
  rewardedAdCallback.onVideoStart();
  rewardedAdCallback.reportAdImpression();
}

@Override
public void onAdClosed() {
  rewardedAdCallback.onAdClosed();
}

@Override
public void onAdCompleted() {
  rewardedAdCallback.onVideoComplete();
}
```

This completes the custom events implementation for rewarded ads. The full
example is available on
[GitHub](https://github.com/googleads/googleads-mobile-android-mediation/tree/master/Example/customevent/src/main/java/com/google/ads/mediation/sample/customevent).
You can use it with an ad network that is already supported or modify it to
display custom event rewarded ads.
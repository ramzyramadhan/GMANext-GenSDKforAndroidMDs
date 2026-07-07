Select platform: [Android](https://developers.google.com/admob/android/next-gen/custom-events/native "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/custom-events/native "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/custom-events/native "View this page for the Android (Legacy) platform docs.")

<br />

## Prerequisites

Complete the [custom events setup](https://developers.google.com/admob/android/next-gen/custom-events/setup).

## Request a native ad

When the custom event line item is reached in the waterfall mediation chain,
the `loadNativeAd()` method is called on the class name you provided when
[creating a custom
event](https://developers.google.com/admob/android/next-gen/custom-events/setup#create). In this case,
that method is in `SampleCustomEvent`, which then calls
the `loadNativeAd()` method in `SampleNativeCustomEventLoader`.

To request a native ad, create or modify a class that extends `Adapter` to
implement `loadNativeAd()`. If a class that extends `Adapter` already exists,
implement `loadNativeAd()` there. Additionally, create a new class to implement
`UnifiedNativeAdMapper`.

In our [custom event example](https://github.com/googleads/googleads-mobile-android-mediation/blob/main/Example/customevent/src/main/java/com/google/ads/mediation/sample/customevent/SampleCustomEvent.java),
`SampleCustomEvent` extends the `Adapter` class and then delegates to
`SampleNativeCustomEventLoader`.

### Java

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.MediationAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;

import com.google.android.gms.ads.mediation.MediationNativeAdCallback;
...
public class SampleCustomEvent extends Adapter {
  private SampleNativeCustomEventLoader nativeLoader;

  @Override
  public void loadNativeAd(
      @NonNull MediationNativeAdConfiguration adConfiguration,
      @NonNull MediationAdLoadCallback<UnifiedNativeAdMapper, MediationNativeAdCallback> callback) {
    nativeLoader = new SampleNativeCustomEventLoader(adConfiguration, callback);
    nativeLoader.loadAd();
  }
}
```

`SampleNativeCustomEventLoader` is responsible for the following tasks:

- Loading the native ad.

- Implementing the `UnifiedNativeAdMapper` class.

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
import com.google.android.gms.ads.mediation.MediationNativeAdConfiguration;
import com.google.android.gms.ads.mediation.MediationAdLoadCallback;
import com.google.android.gms.ads.mediation.MediationNativeAdCallback;
...

public class SampleNativeCustomEventLoader extends SampleNativeAdListener {
  /** Configuration for requesting the native ad from the third-party network. */
  private final MediationNativeAdConfiguration mediationNativeAdConfiguration;

  /** Callback that fires on loading success or failure. */
  private final MediationAdLoadCallback<UnifiedNativeAdMapper, MediationNativeAdCallback>
      mediationAdLoadCallback;

  /** Callback for native ad events. */
  private MediationNativeAdCallback nativeAdCallback;

  /** Constructor */
  public SampleNativeCustomEventLoader(
      @NonNull MediationNativeAdConfiguration mediationNativeAdConfiguration,
      @NonNull MediationAdLoadCallback<MediationNativeAd, MediationNativeAdCallback>
              mediationAdLoadCallback) {
    this.mediationNativeAdConfiguration = mediationNativeAdConfiguration;
    this.mediationAdLoadCallback = mediationAdLoadCallback;
  }

  /** Loads the native ad from the third-party ad network. */
  public void loadAd() {
    // Create one of the Sample SDK's ad loaders to request ads.
    Log.i("NativeCustomEvent", "Begin loading native ad.");
    SampleNativeAdLoader loader =
        new SampleNativeAdLoader(mediationNativeAdConfiguration.getContext());

    // All custom events have a server parameter named "parameter" that returns
    // back the parameter entered into the UI when defining the custom event.
    String serverParameter = mediationNativeAdConfiguration
        .getServerParameters()
        .getString(MediationConfiguration
        .CUSTOM_EVENT_SERVER_PARAMETER_FIELD);
    Log.d("NativeCustomEvent", "Received server parameter.");

    loader.setAdUnit(serverParameter);

    // Create a native request to give to the SampleNativeAdLoader.
    SampleNativeAdRequest request = new SampleNativeAdRequest();
    NativeAdOptions options = mediationNativeAdConfiguration.getNativeAdOptions();
    if (options != null) {
      // If the NativeAdOptions' shouldReturnUrlsForImageAssets is true, the adapter should
      // send just the URLs for the images.
      request.setShouldDownloadImages(!options.shouldReturnUrlsForImageAssets());

      request.setShouldDownloadMultipleImages(options.shouldRequestMultipleImages());
      switch (options.getMediaAspectRatio()) {
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_LANDSCAPE:
          request.setPreferredImageOrientation(SampleNativeAdRequest.IMAGE_ORIENTATION_LANDSCAPE);
          break;
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_PORTRAIT:
          request.setPreferredImageOrientation(SampleNativeAdRequest.IMAGE_ORIENTATION_PORTRAIT);
          break;
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_SQUARE:
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_ANY:
        case NativeAdOptions.NATIVE_MEDIA_ASPECT_RATIO_UNKNOWN:
        default:
          request.setPreferredImageOrientation(SampleNativeAdRequest.IMAGE_ORIENTATION_ANY);
      }
    }

    loader.setNativeAdListener(this);

    // Begin a request.
    Log.i("NativeCustomEvent", "Start fetching native ad.");
    loader.fetchAd(request);
  }
}
```

Depending on whether the ad is successfully fetched or encounters an error, you
would call either
[`onSuccess()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationAdLoadCallback#onSuccess(MediationAdT))
or
[`onFailure()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationAdLoadCallback#onFailure(com.google.android.gms.ads.AdError)).
`onSuccess()` is called by passing in an instance of the class that implements
`MediationNativeAd`.

Typically, these methods are implemented inside callbacks from the
third-party SDK your adapter implements. For this example, the Sample SDK
has a `SampleAdListener` with relevant callbacks:

### Java

```java
@Override
public void onNativeAdFetched(SampleNativeAd ad) {
  SampleUnifiedNativeAdMapper mapper = new SampleUnifiedNativeAdMapper(ad);
  mediationNativeAdCallback = mediationAdLoadCallback.onSuccess(mapper);
}

@Override
public void onAdFetchFailed(SampleErrorCode errorCode) {
  mediationAdLoadCallback.onFailure(SampleCustomEventError.createSampleSdkError(errorCode));
}
```

## Map native ads

Different SDKs have their own unique formats for native ads. One might return
objects that contain a "title" field, for example, while another might have
"headline". Additionally, the methods used to track impressions and process
clicks can vary from one SDK to another.

The `UnifiedNativeAdMapper` is responsible for reconciling these differences and
adapting a mediated SDK's native ad object to match the interface expected by
GMA Next-Gen SDK. Custom events should extend this class to create
their own mappers specific to their mediated SDK. Here's a sample ad mapper from
our example custom event project:

### Java

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.UnifiedNativeAdMapper;
import com.google.android.gms.ads.nativead.NativeAd;
...

public class SampleUnifiedNativeAdMapper extends UnifiedNativeAdMapper {

  private final SampleNativeAd sampleAd;

  public SampleUnifiedNativeAdMapper(SampleNativeAd ad) {
    sampleAd = ad;
    setHeadline(sampleAd.getHeadline());
    setBody(sampleAd.getBody());
    setCallToAction(sampleAd.getCallToAction());
    setStarRating(sampleAd.getStarRating());
    setStore(sampleAd.getStoreName());
    setIcon(
        new SampleNativeMappedImage(
            ad.getIcon(), ad.getIconUri(), SampleCustomEvent.SAMPLE_SDK_IMAGE_SCALE));
    setAdvertiser(ad.getAdvertiser());

    List<NativeAd.Image> imagesList = new ArrayList<NativeAd.Image>();
    imagesList.add(new SampleNativeMappedImage(ad.getImage(), ad.getImageUri(),
        SampleCustomEvent.SAMPLE_SDK_IMAGE_SCALE));
    setImages(imagesList);

    if (sampleAd.getPrice() != null) {
      NumberFormat formatter = NumberFormat.getCurrencyInstance();
      String priceString = formatter.format(sampleAd.getPrice());
      setPrice(priceString);
    }

    Bundle extras = new Bundle();
    extras.putString(SampleCustomEvent.DEGREE_OF_AWESOMENESS, ad.getDegreeOfAwesomeness());
    this.setExtras(extras);

    setOverrideClickHandling(false);
    setOverrideImpressionRecording(false);

    setAdChoicesContent(sampleAd.getInformationIcon());
  }

  @Override
  public void recordImpression() {
    sampleAd.recordImpression();
  }

  @Override
  public void handleClick(View view) {
    sampleAd.handleClick(view);
  }

  // The Sample SDK doesn't do its own impression/click tracking, instead relies on its
  // publishers calling the recordImpression and handleClick methods on its native ad object. So
  // there's no need to pass a reference to the View being used to display the native ad. If
  // your mediated network does need a reference to the view, the following method can be used
  // to provide one.

  @Override
  public void trackViews(View containerView, Map<String, View> clickableAssetViews,
      Map<String, View> nonClickableAssetViews) {
    super.trackViews(containerView, clickableAssetViews, nonClickableAssetViews);
    // If your ad network SDK does its own impression tracking, here is where you can track the
    // top level native ad view and its individual asset views.
  }

  @Override
  public void untrackView(View view) {
    super.untrackView(view);
    // Here you would remove any trackers from the View added in trackView.
  }
}
```

We now take a closer look at the constructor code.

### Hold a reference to the mediated native ad object

The constructor accepts the `SampleNativeAd` parameter, the native ad class used
by the Sample SDK for its native ads. The mapper needs a reference to the
mediated ad so it can pass on click and impression events. `SampleNativeAd` is
stored as a local variable.

### Set mapped asset properties

The constructor uses the `SampleNativeAd` object to populate assets in the
`UnifiedNativeAdMapper`.

This snippet gets the mediated ad's price data and uses it to set the mapper's
price:

### Java

```java
if (sampleAd.getPrice() != null) {
    NumberFormat formatter = NumberFormat.getCurrencyInstance();
    String priceString = formatter.format(sampleAd.getPrice());
    setPrice(priceString);
}
```

In this example, the mediated ad stores the price as a `double`, while
AdMob uses a `String` for the same asset. The
mapper is responsible for handling these types of conversions.

### Map image assets

Mapping image assets is more complicated than mapping data types such as
`double` or `String`. Images might be downloaded automatically or
returned as URL values. Their pixel-to-dpi scales can also vary.

To help you manage these details, GMA Next-Gen SDK provides the
`NativeAd.Image` class. In much the same way that you need to create a subclass
of `UnifiedNativeAdMapper` to map a mediated native ad, you should also create a
subclass of `NativeAd.Image` when mapping image assets.

Here's an example for the custom event's `SampleNativeMappedImage` class:

### Java

```java
public class SampleNativeMappedImage extends NativeAd.Image {

  private Drawable drawable;
  private Uri imageUri;
  private double scale;

  public SampleNativeMappedImage(Drawable drawable, Uri imageUri, double scale) {
    this.drawable = drawable;
    this.imageUri = imageUri;
    this.scale = scale;
  }

  @Override
  public Drawable getDrawable() {
    return drawable;
  }

  @Override
  public Uri getUri() {
    return imageUri;
  }

  @Override
  public double getScale() {
    return scale;
  }
}
```

The `SampleNativeAdMapper` uses its mapped image class in this line to set the
mapper's icon image asset:

### Java

```java
setIcon(new SampleNativeMappedImage(ad.getAppIcon(), ad.getAppIconUri(),
    SampleCustomEvent.SAMPLE_SDK_IMAGE_SCALE));
```

### Add fields to the extras Bundle

Some mediated SDKs provide additional assets beyond those in the
AdMob native ad format. The `UnifiedNativeAdMapper`
class includes a `setExtras()` method that is used to pass these assets to
publishers. The `SampleNativeAdMapper` makes use of this for the Sample SDK's
"degree of awesomeness" asset:

### Java

```java
Bundle extras = new Bundle();
extras.putString(SampleCustomEvent.DEGREE_OF_AWESOMENESS, ad.getDegreeOfAwesomeness());
this.setExtras(extras);
```

Publishers can retrieve the data using the `NativeAd` class' `getExtras()`
method.

### AdChoices

Your custom event is responsible for providing an AdChoices icon using the
`setAdChoicesContent()` method on `UnifiedNativeAdMapper`. Here's a snippet from
`SampleNativeAdMapper` showing how to provide the AdChoices icon:

### Java

```java
public SampleNativeAdMapper(SampleNativeAd ad) {
    ...
    setAdChoicesContent(sampleAd.getInformationIcon());
}
```

## Impression and click events

Both GMA Next-Gen SDK and the mediated SDK need to know when an
impression or click occurs, but only one SDK needs to track these events. There
are two different approaches custom events can use, depending on whether the
mediated SDK supports tracking impressions and clicks on its own.

### Track clicks and impressions with GMA Next-Gen SDK

If the mediated SDK doesn't perform its own impression and click tracking but
provides methods to record clicks and impressions, GMA Next-Gen SDK can
track these events and notify the adapter. The
`UnifiedNativeAdMapper` class includes two methods:
`recordImpression()` and `handleClick()` that
custom events can implement to call the corresponding method on the mediated
native ad object:

### Java

```java
@Override
public void recordImpression() {
  sampleAd.recordImpression();
}

@Override
public void handleClick(View view) {
  sampleAd.handleClick(view);
}
```

Because the `SampleNativeAdMapper` holds a reference to the Sample SDK's native
ad object, it can call the appropriate method on that object to report a click
or impression. Note that the `handleClick()` method takes a
single parameter: the `View` object corresponding to the native ad asset that
received the click.

### Track clicks and impressions with the mediated SDK

Some mediated SDKs might prefer to track clicks and impressions on their own. In
that case, you should override the default click and impression tracking by
making the following two calls in the constructor of your
`UnifiedNativeAdMapper`:

### Java

```java
setOverrideClickHandling(true);
setOverrideImpressionRecording(true);
```

Custom events that override click and impression tracking are required to
report the `onAdClicked()` and `onAdImpression()` events to the Google Mobile
Ads SDK.

To track impressions and clicks, the mediated SDK probably needs access to the
views to enable the tracking. The custom event should override the
`trackViews()` method and use it to pass the native ad's view to the mediated
SDK to track. The sample SDK from our custom event example project (from which
this guide's code snippets have been taken) doesn't use this approach; but if
it did, the custom event code would look something like this:

### Java

```java
@Override
public void trackViews(View containerView,
    Map<String, View> clickableAssetViews,
    Map<String, View> nonClickableAssetViews) {
  sampleAd.setNativeAdViewForTracking(containerView);
}
```

If the mediated SDK supports tracking individual assets, it can look inside
`clickableAssetViews` to see which views should be made clickable. This map is
keyed by an asset name in `NativeAdAssetNames`. The `UnifiedNativeAdMapper`
offers a corresponding `untrackView()` method that custom events can override
to release any references to the view and disassociate it from the native ad
object.

## Forward mediation events to GMA Next-Gen SDK

You can find all the callbacks that mediation supports in the
[`MediationNativeAdCallback` docs](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/mediation/MediationNativeAdCallback).

It's important that your custom event forwards as many of these callbacks as
possible, so that your app receives these equivalent events from GMA Next-Gen SDK.
Here's an example of using callbacks:

This completes the custom events implementation for native ads. The full example
is available on
[GitHub](https://github.com/googleads/googleads-mobile-android-mediation/tree/master/Example/customevent/src/main/java/com/google/ads/mediation/sample/customevent).
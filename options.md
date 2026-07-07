Select platform: [Android](https://developers.google.com/admob/android/next-gen/native/options "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/native/options "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/native/options "View this page for the Android (Legacy) platform docs.")

<br />

Native ads have many advanced features that allow you to make additional
customizations and make the best possible ad experience. This guide shows you
how to use the advanced features of native ads.

## Prerequisites

- Integrate the [Native ad format](https://developers.google.com/admob/android/next-gen/native).

## Asset controls

This section details how to customize the creative assets in your native ads.
You have the option to specify a preferred aspect ratio for media assets and
how the image assets are downloaded and displayed.

### Preferred media aspect ratio controls

Media Aspect Ratio Controls let you specify a preference for the aspect ratio of
ad creatives.

> [!IMPORTANT]
> **Important:** Use of this API does not guarantee that all ad creatives will meet the preference specified.

Call [`NativeAdRequest.Builder.setMediaAspectRatio()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAdRequest.Builder#setMediaAspectRatio(com.google.android.libraries.ads.mobile.sdk.nativead.NativeAd.NativeMediaAspectRatio))
with a [`NativeAd.NativeMediaAspectRatio`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAd.NativeMediaAspectRatio)
value.

- When unset, the returned ad can have any media aspect ratio.

- When set, you will be able to improve the user experience by specifying the
  preferred type of aspect ratio.

The following example instructs the SDK to prefer a return image or video with a
specific aspect ratio.

### Kotlin

    val adRequest = NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .setMediaAspectRatio(NativeAd.NATIVE_MEDIA_ASPECT_RATIO_LANDSCAPE)
      .build()

### Java

    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .setMediaAspectRatio(NativeAd.NATIVE_MEDIA_ASPECT_RATIO_LANDSCAPE)
      .build();

<br />

### Image download control

Image download control lets you decide if image assets or only URIs are
returned by the SDK.

Call [`NativeAdRequest.Builder.disableImageDownloading()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAdRequest.Builder#disableImageDownloading()).

- Image download control are disabled by default.

- When disabled, GMA Next-Gen SDK populates both the image and the URI for you.

- When enabled, the SDK instead populates just the URI, allowing you to download
  the actual images at your discretion.

The following example instructs the SDK to return just the URI.

### Kotlin

    val adRequest = NativeAdRequest.Builder(
        "ca-app-pub-3940256099942544/2247696110",
        listOf(NativeAd.NativeAdType.NATIVE))
        .setMediaAspectRatio(NativeAd.NATIVE_MEDIA_ASPECT_RATIO_LANDSCAPE)
        .disableImageDownloading()
        .build()

    val adCallback: NativeAdLoaderCallback =
      object : NativeAdLoaderCallback {
        override fun onNativeAdLoaded(nativeAd: NativeAd) {
          // Get the image uri.
          val imageUri = nativeAd.image?.uri
        }
      };

    // Load the native ad with the ad request and callback.
    NativeAdLoader.load(adRequest, adLoaderCallback);

### Java

    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .disableImageDownloading()
      .build();

    NativeAdLoaderCallback adLoaderCallback =
      new NativeAdLoaderCallback() {
        @Override
        public void onNativeAdLoaded(@NonNull NativeAd nativeAd) {
          // Get the image uri.
          Uri imageUri = nativeAd.getImage().getUri();
        }
      };

    // Load the native ad with the ad request and callback.
    NativeAdLoader.load(adRequest, adLoaderCallback);

### Image payload controls

Some ads have a series of images rather than just one. Use this feature to
indicate whether your app is prepared to display all the images or just one.

<br />

- Image payload controls are disabled by default.

- When disabled, your app instructs the SDK to provide just the
  first image for any assets that contain a series.

- When enabled, your app indicates that it is prepared to display all the images
  for any assets that have more than one.

The following example instructs the SDK to return multiple image assets.

<br />

## AdChoices placements

This section details how to position the AdChoices overlay. You have the option
to set its placement to one of the four corners or render it within a custom
view.

### AdChoices position controls

The AdChoices position controls lets you choose which corner to render the
AdChoices icon.

Call [`NativeAdRequest.Builder.setAdChoicesPlacement()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAdRequest.Builder#setAdChoicesPlacement(com.google.android.libraries.ads.mobile.sdk.common.AdChoicesPlacement))
with a [`NativeAdRequest.AdChoicesPlacement`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/AdChoicesPlacement)
value.

- If unset, the AdChoices icon position is set to the top right.

- If set, AdChoices is placed at the custom position as requested.

The following example demonstrates how to set a custom AdChoices image position.

### Kotlin

    val adRequest = NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .setAdChoicesPlacement(NativeAdOptions.ADCHOICES_BOTTOM_RIGHT)
      .build()

### Java

    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .setAdChoicesPlacement(NativeAdOptions.ADCHOICES_BOTTOM_RIGHT)
      .build();

<br />

### AdChoices custom view


> [!WARNING]
> Warning: Access to this feature is limited. To request access, reach out to your account manager.

<br />

The AdChoices custom view feature lets you position the AdChoices icon in a
custom location. This is different from AdChoices position controls, which only
allows specification of one of the four corners.

Call [`NativeAdView.setAdChoicesView()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/BaseAdAssetViewContainer#setAdChoicesView(com.google.android.libraries.ads.mobile.sdk.common.AdChoicesView))
with a [`AdChoicesView`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/AdChoicesView) value.

<br />

The following example demonstrates how to set a custom AdChoices view, with the
AdChoices icon rendered inside the `AdChoicesView`.

### Kotlin

    override fun onNativeAdLoaded(nativeAd: NativeAd) {
      val nativeAdView = NativeAdView(applicationContext)
      val adChoicesView = AdChoicesView(this)
      nativeAdView.adChoicesView = adChoicesView
    }

### Java

    public void onNativeAdLoaded(@NonNull NativeAd nativeAd) {
      NativeAdView nativeAdView = new NativeAdView(getApplicationContext());
      AdChoicesView adChoicesView = new AdChoicesView(this);
      nativeAdView.setAdChoicesView(adChoicesView);
    }

<br />

<br />

## Video controls

This section details how customize the playback experience for video ads. You
have the option to set the initial mute state and implement custom playback
controls.

### Start mute behavior

The start muted behavior lets you disable or enable a video's starting audio.

Call [`VideoOptions.Builder.setStartMuted()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/VideoOptions.Builder#setStartMuted(kotlin.Boolean))
with a `boolean` value and call [`NativeAdOptions.Builder.setVideoOptions()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAdOptions.Builder#setVideoOptions(com.google.android.libraries.ads.mobile.sdk.common.VideoOptions)).

- The start muted behavior is enabled by default.

- When disabled, your app requests the video should begin with
  audio.

- When enabled, your app requests that the video should begin with audio muted.

The following example shows how to start the video with un-muted audio.

### Kotlin

    val videoOptions = VideoOptions.Builder()
      .setStartMuted(false)
      .build()

    val adRequest = NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .setVideoOptions(videoOptions)
      .build()

### Java

    VideoOptions videoOptions = VideoOptions.Builder()
      .setStartMuted(false)
      .build()

    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .setVideoOptions(videoOptions)
      .build()

### Custom playback controls


> [!WARNING]
> Warning: Access to this feature is limited. To request access, reach out to your account manager.

<br />

This lets you request custom video input controls to play, pause, or mute the
video.

To set the ads starting mute state, call [`VideoOptions.Builder.setCustomControlsRequested()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/VideoOptions.Builder#setCustomControlsRequested(kotlin.Boolean)).

<br />

- Custom playback control are disabled by default.

- When disabled, your video will show SDK rendered input controls.

<br />

If the ad does have video content and custom controls are enabled, you should
then display your custom controls along with the ad, as the ad won't show any
controls itself. The controls can then call the relevant methods on the

[`VideoOptions.Builder.setCustomControlsRequested()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/VideoOptions.Builder#setCustomControlsRequested(kotlin.Boolean)).

The following example shows how request a video with custom playback controls.

### Kotlin

    val videoOptions: VideoOptions.Builder()
      .setCustomControlsRequested(true)
      .build()

    val adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .setVideoOptions(videoOptions)
      .build()

### Java

    VideoOptions VideoOptions = VideoOptions.Builder()
      .setCustomControlsRequested(true)
      .build()

    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .setVideoOptions(videoOptions)
      .build()

#### Check if custom controls are enabled

Because it's not known at request time whether the returned ad will allow
custom video controls, you must check whether it has custom controls enabled.

### Kotlin

      val adCallback: NativeAdLoaderCallback =
        object : NativeAdLoaderCallback {
          override fun onNativeAdLoaded(nativeAd: NativeAd) {
            val mediaContent = nativeAd.mediaContent;
            if (mediaContent != null) {
              val videoController = mediaContent.videoController;
              val canShowCustomControls = videoController?.isCustomControlsEnabled();
            }
          }
        };

### Java

    NativeAdLoaderCallback adCallback =
      new NativeAdLoaderCallback() {
        @Override
        public void onNativeAdLoaded(@NonNull NativeAd nativeAd) {
          MediaContent mediaContent = nativeAd.getMediaContent();
          if (mediaContent != null) {
            VideoController videoController = mediaContent.getVideoController();
            if (videoController != null) {
              boolean canShowCustomControls = videoController.isCustomControlsEnabled();
            }
          }
        }
      };

#### Render custom video controls

Render custom video controls using the following best practices:

1. Render the custom controls view as a child of the native ad view. This approach lets open measurement viewability calculations consider the custom controls as a friendly obstruction.
2. Avoid rendering an invisible overlay over the entire media view. Overlays block clicks on the media view, negatively impacting native ads performance. Instead, create a small overlay that is just large enough to fit the controls.

> [!NOTE]
> **Note:** For information on measurement and verification providers that have committed to the development of the open measurement viewability standard, see the [Open Measurement Working Group](https://iabtechlab.com/working-groups/open-measurement-working-group/).

## Custom click gestures


> [!WARNING]
> Warning: Access to this feature is limited. To request access, reach out to your account manager.

<br />

Custom click gestures is a native ads feature that lets you register swipes
on ad views as ad clicks. It works with apps that use swipe gestures for
content navigation. This guide shows you how to enable custom click gestures
on your native ads.

Call [`NativeAdRequest.Builder.enableCustomClickGestureDirection()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAdRequest.Builder#enableCustomClickGestureDirection(com.google.android.libraries.ads.mobile.sdk.nativead.NativeAd.SwipeGestureDirection,kotlin.Boolean))
with a [`NativeAd.SwipeGestureDirection`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAd.SwipeGestureDirection)
and a `boolean` value.

<br />

- Custom click gestures are disabled by default.

- When disabled, your app supports normal click behavior.

- When enabled, your app supports custom swipe gestures.

The following example implements a custom swipe gesture to the right and
preserves normal tap behavior.

### Kotlin

    val adOptions = NativeAdOptions
      .Builder()
      .enableCustomClickGestureDirection(
        /* swipeDirection */ NativeAdOptions.SWIPE_GESTURE_DIRECTION_RIGHT,
        /* tapsAllowed= */ true)
      .build();

    // You can use the following sample ad unit ID to test custom click gestures.
    val adRequest = NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      listOf(NativeAd.NativeAdType.NATIVE))
      .withNativeAdOptions(adOptions)
      .build();

### Java

    NativeAdOptions adOptions = new NativeAdOptions
      .Builder()
      .enableCustomClickGestureDirection(
        /* swipeDirection */ NativeAdOptions.SWIPE_GESTURE_DIRECTION_RIGHT,
        /* tapsAllowed= */ true)
      .build();

    // You can use the following sample ad unit ID to test custom click gestures.
    NativeAdRequest adRequest = new NativeAdRequest.Builder(
      "ca-app-pub-3940256099942544/2247696110",
      List.of(NativeAd.NativeAdType.NATIVE))
      .withNativeAdOptions(adOptions)
      .build();

> [!WARNING]
> **Warning:** If your account doesn't have custom click gestures enabled, ad requests return a no-fill.

#### Listen for swipe gesture events

To listen to swipe gesture events, call [`NativeAd.setAdEventCallback()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAd#setAdEventCallback(com.google.android.libraries.ads.mobile.sdk.nativead.NativeAd.AdEventCallback))
with a [`NativeAdEventCallback`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAdEventCallback) and
implement the [`onAdSwipeGestureClicked()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAdEventCallback#onAdSwipeGestureClicked())
method.

### Kotlin

      val adCallback: NativeAdLoaderCallback =
        object : NativeAdLoaderCallback {
          override fun onNativeAdLoaded(nativeAd: NativeAd) {
            // Implement the onAdSwipeGestureClicked() method.
            val nativeAdCallback: NativeAdEventCallback = object : NativeAdEventCallback {
              override fun onAdSwipeGestureClicked() {
                // A swipe gesture click has occurred.
              }
            }
          }
        }
      // Load the native ad with the ad request and callback.
      NativeAdLoader.load(adRequest, adCallback)

### Java

      NativeAdLoaderCallback adCallback =
        new NativeAdLoaderCallback() {
          @Override
          public void onNativeAdLoaded(@NonNull NativeAd nativeAd) {
            // Implement the onAdSwipeGestureClicked() method.
            NativeAdEventCallback nativeAdCallback = new NativeAdEventCallback() {
              @Override
              public void onAdSwipeGestureClicked() {
                // A swipe gesture click has occurred.
              }
            };
          }
        };
      // Load the native ad with the ad request and callback.
      NativeAdLoader.load(adRequest, adCallback);

#### Mediation

Custom click gestures only work on native ads that Google Mobile
Ads SDK renders. Ad sources that
[require third-party SDKs](https://developers.google.com/admob/android/next-gen/choose-networks) for
rendering, don't respond to the custom click directions setting.
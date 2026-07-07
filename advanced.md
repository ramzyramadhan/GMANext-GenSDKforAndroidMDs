Select platform: [Android](https://developers.google.com/admob/android/next-gen/native/advanced "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/native/advanced "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/native/advanced "View this page for the Android (Legacy) platform docs.")

<br />

<br />

When a native ad loads, GMA Next-Gen SDK invokes the listener for the
corresponding ad format. Your app is then responsible for displaying the ad,
though it doesn't necessarily have to do so immediately. To make displaying
system-defined ad formats easier, the SDK offers some useful resources, as
described below.

> [!IMPORTANT]
> **Key Point:** Learn more about native ads in our [Native Ads
> Playbook](https://admob.google.com/home/resources/native-ads-playbook/). Also, review the [Native ads policies and guidelines](https://support.google.com/admob/answer/6329638) for guidance on how to render your native ads.

### Define the `NativeAdView` class

Define a [`NativeAdView`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAdView) class. This class is a
[`ViewGroup`](https://developer.android.com/reference/android/view/ViewGroup.html)
class and is the top level container for a `NativeAdView` class. Each native ad
view contains native ad assets, such as the `MediaView` view element or the
`Title` view element, which must be a child of the `NativeAdView` object.

### XML Layout


Add a XML `NativeAdView` to your project:

    <com.google.android.libraries.ads.mobile.sdk.nativead.NativeAdView
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <LinearLayout
        android:orientation="vertical">
            <LinearLayout
            android:orientation="horizontal">
              <ImageView
              android:id="@+id/ad_app_icon" />
              <TextView
                android:id="@+id/ad_headline" />
            </LinearLayout>
            <!--Add remaining assets such as the image and media view.-->
        </LinearLayout>
    </com.google.android.libraries.ads.mobile.sdk.nativead.NativeAdView>

### Jetpack Compose

1. Include the [JetpackCompose Utilities](https://github.com/googleads/googleads-mobile-android-examples/tree/main/kotlin/advanced/JetpackComposeDemo/app/src/main/java/com/google/android/gms/example/jetpackcomposedemo/formats/compose_utils/) folder. This folder includes helpers for composing the `NativeAdView` object and assets.

Compose a `NativeAdView`:

      import com.google.android.gms.compose_util.NativeAdAttribution
      import com.google.android.gms.compose_util.NativeAdView

      @Composable
      /** Display a native ad with a user defined template. */
      fun DisplayNativeAdView(nativeAd: NativeAd) {
          NativeAdView {
              // Display the ad attribution.
              NativeAdAttribution(text = context.getString("Ad"))
              // Add remaining assets such as the image and media view.
            }
        }

### Handle the loaded native ad

When a native ad loads, handle the callback event, inflate the native ad view,
and add it to the view hierarchy:

### Kotlin

    // Build an ad request with native ad options to customize the ad.
    val adTypes = listOf(NativeAd.NativeAdType.NATIVE)
    val adRequest = NativeAdRequest
      .Builder("ca-app-pub-3940256099942544/2247696110", adTypes)
      .build()

    val adCallback =
      object : NativeAdLoaderCallback {
        override fun onNativeAdLoaded(nativeAd: NativeAd) {
          activity?.runOnUiThread {

            val nativeAdBinding = NativeAdBinding.inflate(layoutInflater)
            val adView = nativeAdBinding.root
            val frameLayout = myActivityLayout.nativeAdPlaceholder

            // Populate and register the native ad asset views.
            displayNativeAd(nativeAd, nativeAdBinding)

            // Remove all old ad views and add the new native ad
            // view to the view hierarchy.
            frameLayout.removeAllViews()
            frameLayout.addView(adView)
          }
        }
      }

    // Load the native ad with our request and callback.
    NativeAdLoader.load(adRequest, adCallback)

### Java

    // Build an ad request with native ad options to customize the ad.
    List<NativeAd.NativeAdType> adTypes = Arrays.asList(NativeAd.NativeAdType.NATIVE);
    NativeAdRequest adRequest = new NativeAdRequest
                                    .Builder("ca-app-pub-3940256099942544/2247696110", adTypes)
                                    .build();

    NativeAdLoaderCallback adCallback = new NativeAdLoaderCallback() {
        @Override
        public void onNativeAdLoaded(NativeAd nativeAd) {
          if (getActivity() != null) {
            getActivity()
              .runOnUiThread(() -> {
                // Inflate the native ad view and add it to the view hierarchy.
                NativeAdBinding nativeAdBinding = NativeAdBinding.inflate(getLayoutInflater());
                NativeAdView adView = (NativeAdView) nativeAdBinding.getRoot();
                FrameLayout frameLayout = myActivityLayout.nativeAdPlaceholder;

                // Populate and register the native ad asset views.
                displayNativeAd(nativeAd, nativeAdBinding);

                // Remove all old ad views and add the new native ad
                // view to the view hierarchy.
                frameLayout.removeAllViews();
                frameLayout.addView(adView);
            });
          }
        }
    };

    // Load the native ad with our request and callback.
    NativeAdLoader.load(adRequest, adCallback);

Note that all assets for a given native ad should be rendered inside the
`NativeAdView` layout. GMA Next-Gen SDK attempts to log a warning when
native assets are rendered outside of a native ad view layout.

The ad view classes also provide methods used to register the view used for
each individual asset, and one to register the `NativeAd` object itself.
Registering the views in this way allows the SDK to automatically handle tasks
such as:

- Recording clicks
- Recording impressions when the first pixel is visible on the screen
- Displaying the AdChoices overlay

### Display the native ad

The following example demonstrates how to display a native ad:

### Kotlin

    private fun displayNativeAd(nativeAd: NativeAd, nativeAdBinding : NativeAdBinding) {
      // Set the native ad view elements.
      val nativeAdView = nativeAdBinding.root
      nativeAdView.advertiserView = nativeAdBinding.adAdvertiser
      nativeAdView.bodyView = nativeAdBinding.adBody
      nativeAdView.callToActionView = nativeAdBinding.adCallToAction
      nativeAdView.headlineView = nativeAdBinding.adHeadline
      nativeAdView.iconView = nativeAdBinding.adAppIcon
      nativeAdView.priceView = nativeAdBinding.adPrice
      nativeAdView.starRatingView = nativeAdBinding.adStars
      nativeAdView.storeView = nativeAdBinding.adStore

      // Set the view element with the native ad assets.
      nativeAdBinding.adAdvertiser.text = nativeAd.advertiser
      nativeAdBinding.adBody.text = nativeAd.body
      nativeAdBinding.adCallToAction.text = nativeAd.callToAction
      nativeAdBinding.adHeadline.text = nativeAd.headline
      nativeAdBinding.adAppIcon.setImageDrawable(nativeAd.icon?.drawable)
      nativeAdBinding.adPrice.text = nativeAd.price
      nativeAd.starRating?.toFloat().let { value ->
        nativeAdBinding.adStars.rating = value
      }
      nativeAdBinding.adStore.text = nativeAd.store

      // Hide views for assets that don't have data.
      nativeAdBinding.adAdvertiser.visibility = getAssetViewVisibility(nativeAd.advertiser)
      nativeAdBinding.adBody.visibility = getAssetViewVisibility(nativeAd.body)
      nativeAdBinding.adCallToAction.visibility = getAssetViewVisibility(nativeAd.callToAction)
      nativeAdBinding.adHeadline.visibility = getAssetViewVisibility(nativeAd.headline)
      nativeAdBinding.adAppIcon.visibility = getAssetViewVisibility(nativeAd.icon)
      nativeAdBinding.adPrice.visibility = getAssetViewVisibility(nativeAd.price)
      nativeAdBinding.adStars.visibility = getAssetViewVisibility(nativeAd.starRating)
      nativeAdBinding.adStore.visibility = getAssetViewVisibility(nativeAd.store)

      // Inform GMA Next-Gen SDK that you have finished populating
      // the native ad views with this native ad.
      nativeAdView.registerNativeAd(nativeAd, nativeAdBinding.adMedia)
    }

    /**
    * Determines the visibility of an asset view based on the presence of its asset.
    *
    * @param asset The native ad asset to check for nullability.
    * @return [View.VISIBLE] if the asset is not null, [View.INVISIBLE] otherwise.
    */
    private fun getAssetViewVisibility(asset: Any?): Int {
      return if (asset == null) View.INVISIBLE else View.VISIBLE
    }

### Java

    private void displayNativeAd(ad: NativeAd, nativeAdBinding : NativeAdBinding) {
      // Set the native ad view elements.
      NativeAdView nativeAdView = nativeAdBinding.getRoot();
      nativeAdView.setAdvertiserView(nativeAdBinding.adAdvertiser);
      nativeAdView.setBodyView(nativeAdBinding.adBody);
      nativeAdView.setCallToActionView(nativeAdBinding.adCallToAction);
      nativeAdView.setHeadlineView(nativeAdBinding.adHeadline);
      nativeAdView.setIconView(nativeAdBinding.adAppIcon);
      nativeAdView.setPriceView(nativeAdBinding.adPrice);
      nativeAdView.setStarRatingView(nativeAdBinding.adStars);
      nativeAdView.setStoreView(nativeAdBinding.adStore);

      // Set the view element with the native ad assets.
      nativeAdBinding.adAdvertiser.setText(nativeAd.getAdvertiser());
      nativeAdBinding.adBody.setText(nativeAd.getBody());
      nativeAdBinding.adCallToAction.setText(nativeAd.getCallToAction());
      nativeAdBinding.adHeadline.setText(nativeAd.getHeadline());
      if (nativeAd.getIcon() != null) {
          nativeAdBinding.adAppIcon.setImageDrawable(nativeAd.getIcon().getDrawable());
      }
      nativeAdBinding.adPrice.setText(nativeAd.getPrice());
      if (nativeAd.getStarRating() != null) {
          nativeAdBinding.adStars.setRating(nativeAd.getStarRating().floatValue());
      }
      nativeAdBinding.adStore.setText(nativeAd.getStore());

      // Hide views for assets that don't have data.
      nativeAdBinding.adAdvertiser.setVisibility(getAssetViewVisibility(nativeAd.getAdvertiser()));
      nativeAdBinding.adBody.setVisibility(getAssetViewVisibility(nativeAd.getBody()));
      nativeAdBinding.adCallToAction.setVisibility(getAssetViewVisibility(nativeAd.getCallToAction()));
      nativeAdBinding.adHeadline.setVisibility(getAssetViewVisibility(nativeAd.getHeadline()));
      nativeAdBinding.adAppIcon.setVisibility(getAssetViewVisibility(nativeAd.getIcon()));
      nativeAdBinding.adPrice.setVisibility(getAssetViewVisibility(nativeAd.getPrice()));
      nativeAdBinding.adStars.setVisibility(getAssetViewVisibility(nativeAd.getStarRating()));
      nativeAdBinding.adStore.setVisibility(getAssetViewVisibility(nativeAd.getStore()));

      // Inform GMA Next-Gen SDK that you have finished populating
      // the native ad views with this native ad.
      nativeAdView.registerNativeAd(nativeAd, nativeAdBinding.adMedia);
    }

    /**
    * Determines the visibility of an asset view based on the presence of its asset.
    *
    * @param asset The native ad asset to check for nullability.
    * @return {@link View#VISIBLE} if the asset is not null, {@link View#INVISIBLE} otherwise.
    */
    private int getAssetViewVisibility(Object asset) {
        return (asset == null) ? View.INVISIBLE : View.VISIBLE;
    }

> [!NOTE]
> **Note:** If you prefer to render your native ad without a `MediaView` instance, pass the `null` value for the media view parameter when calling the `registerNativeAd()` method.

### AdChoices overlay

An AdChoices overlay is added to each ad view by the SDK. Leave space in your
[preferred corner](https://developers.google.com/admob/android/next-gen/native/options#adchoices-placement) of your
native ad view for the automatically inserted AdChoices logo. Also, it's
important that the AdChoices overlay be seen, so choose background colors
and images appropriately. For more information on the overlay's appearance and
function, see [Native ads field
descriptions](https://support.google.com/admob/answer/6329638#adchoices).

### Ad attribution

You must display an ad attribution to denote that the view is an advertisement.
Learn more in our [policy
guidelines](https://support.google.com/admob/answer/6329638#ad-attribution).

### Handle clicks

Don't implement any custom click handlers on any views over or within the
native ad view. Clicks on the ad view assets are handled by the SDK as long as
you correctly populate and register the asset views.

To listen for clicks, implement GMA Next-Gen SDK click callback:

### Kotlin

    private fun setEventCallback(nativeAd: NativeAd) {
      nativeAd.adEventCallback =
        object : NativeAdEventCallback {
          override fun onAdClicked() {
            Log.d(Constant.TAG, "Native ad recorded a click.")
          }
        }
    }

### Java

    private void setEventCallback(NativeAd nativeAd) {
      nativeAd.setAdEventCallback(new NativeAdEventCallback() {
          @Override
          public void onAdClicked() {
            Log.d(Constant.TAG, "Native ad recorded a click.");
          }
      });
    }

#### ImageScaleType

The `MediaView` class has an `ImageScaleType` property when displaying
images. If you want to change how an image is scaled in the `MediaView`, set
the corresponding `ImageView.ScaleType` using the `setImageScaleType()`
method of the `MediaView`:

### Kotlin

    nativeAdViewBinding.mediaView.imageScaleType = ImageView.ScaleType.CENTER_CROP

### Java

    nativeAdViewBinding.mediaView.setImageScaleType(ImageView.ScaleType.CENTER_CROP);

#### MediaContent

The `MediaContent` class holds the data related to the media content of the
native ad, which is displayed using the `MediaView` class. When the
`MediaView` `mediaContent` property is set with a `MediaContent` instance:

- If a video asset is available, it's buffered and starts playing inside the
  `MediaView`. You can tell if a video asset is available by checking
  `hasVideoContent()`.

- If the ad does not contain a video asset, the `mainImage` asset is downloaded
  and placed inside the `MediaView` instead.

## Destroy an ad

After you show a native ad, destroy the ad. The following example destroys a
native ad:

### Kotlin

    nativeAd.destroy()

### Java

    nativeAd.destroy();

## Next steps

Explore the following topics:

- [User privacy](https://developers.google.com/admob/android/next-gen/privacy)

## Example

Download and run the
[example app](https://github.com/googleads/gma-next-gen-sdk-android-examples)
that demonstrates the use of the GMA Next-Gen SDK.
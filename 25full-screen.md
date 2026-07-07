Select platform: [Android](https://developers.google.com/admob/android/next-gen/native/full-screen "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/native/full-screen "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/native/full-screen "View this page for the Android (Legacy) platform docs.")

<br />

The [native ad format](https://developers.google.com/admob/android/next-gen/native) can be used
to create any size of ad, including full-screen ads like those that are highly
popular in social and entertainment apps. Full-screen native ads can improve
revenue and retention, either through matching the style of existing
full-screen content experiences such as in social apps, or through providing a
means to place ads in "stories" feeds. Here are some examples of
full-screen native ads:

![](https://developers.google.com/static/admob/images/full-screen/image00.png)
![](https://developers.google.com/static/admob/images/full-screen/image01.png)
![](https://developers.google.com/static/admob/images/full-screen/image02.png)

There is no separate API to call to enable full-screen native ads to serve
beyond the instructions for [Native
Advanced](https://developers.google.com/admob/android/next-gen/native/advanced). However,
there are best practices we recommend when creating full-screen ad experiences:

Customize the AdChoices icon placement
:   By default, the AdChoices icon is placed at the top-right corner of the ad,
    but you can specify any corner where the AdChoices icon should appear by
    setting the `AdChoicesPosition` based on placement of the ad. In the
    three images in the previous section, the AdChoices icon is placed in a
    corner far away from the **Install** button, the menu button and other ad
    assets to avoid accidental clicks.

Use unique ad unit IDs for each placement

:   Be sure to [create a unique ad unit
    ID](https://support.google.com/admob/answer/7187428) for each different ad placement
    in your app, even if all ad placements are the same format. For example, if
    you have an existing native ad placement in your app for a non-full screen
    experience, use a new ad unit ID for the full screen experience. Using unique
    ad units:

    - maximizes performance
    - helps Google return ad assets that better fit your layouts
    - enables more comprehensive reporting.

Set your media view to a consistent size

:   Google always tries to serve the best-sized native assets for optimal
    performance. To facilitate this, the sizing for your native ads should be
    predictable and consistent. Your media view asset should be the same size for
    every ad request on the same device. To accomplish this, set your media view
    to a fixed size, or set the media view to `MATCH_PARENT` and make the parent
    view a fixed size. Repeat this step for every parent view of the media view
    that is not a fixed size.

Enable video ads

:   Enable the `Video` media type when

    [configuring native ads](https://support.google.com/admob/answer/7187428)

    in the AdMob UI. Allowing video ads to compete for your
    inventory can significantly improve performance.

\[Optional\] Request specific aspect ratios for the media asset

:   By default, ads of any aspect ratio may be returned. For example, you may get
    a landscape or square main creative asset when your app is in portrait
    mode. Depending on your native ad layout, you may want to serve only
    portrait, landscape, or square ads. You can request assets of specific
    [aspect ratios](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAd.NativeMediaAspectRatio) to best suit your
    layout.

    |---|---|---|
    | ![](https://developers.google.com/static/admob/images/full-screen/image03.png) Landscape | ![](https://developers.google.com/static/admob/images/full-screen/image04.png) Square | ![](https://developers.google.com/static/admob/images/full-screen/image05.png) Portrait |

    <br />

    ### Kotlin

        val adRequest = NativeAdRequest.Builder(adUnitId, listOf(NativeAd.NativeAdType.NATIVE))
           .setMediaAspectRatio(NativeAd.NativeMediaAspectRatio.PORTRAIT)
           .build()

    ### Java

        List<NativeAd.NativeAdType> adTypes = Arrays.asList(NativeAd.NativeAdType.NATIVE);
        NativeAdRequest adRequest = new NativeAdRequest.Builder(adUnitId, adTypes)
           .setMediaAspectRatio(NativeAd.NativeMediaAspectRatio.PORTRAIT)
           .build();

    > [!CAUTION]
    > **Caution:** Setting media aspect ratio to portrait, landscape, or square will limit ad availability, and may reduce revenue. To optimize revenue, we recommend leaving the media aspect ratio to the default value of `NativeAd.NativeMediaAspectRatio.ANY`.
Select platform: [Android](https://developers.google.com/admob/android/next-gen/mediation/troubleshoot-bidding "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/troubleshoot-bidding "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/troubleshoot-bidding "View this page for the Unity platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/troubleshoot-bidding "View this page for the Android (Legacy) platform docs.")

<br />

When integrating a bidding partner that requires their SDK, the following
symptoms indicate an improper integration:

- The [Ads Activity report](https://support.google.com/admob/answer/10979428) shows significantly fewer ad requests to that partner than you expect.
- The `a3p` parameter in any request after the first ad request is missing.

> [!IMPORTANT]
> **Key Point:** The first ad request may contain the `a3p` parameter even if you did not configure bidding.

Follow this checklist to make sure your setup is correct:

- In the AdMob UI:

  - Confirm that you have followed the specific partner's
    [integration guide](https://developers.google.com/admob/android/next-gen/mediation/choose-networks)
    to configure third-party bidding demand.

  - Confirm that you have an ad unit mapping for each creative format.

- In your app code:

  - [Update your `AndroidManifest.xml`
    file](https://developers.google.com/admob/android/next-gen/quick-start#import_the_mobile_ads_sdk) with the same app ID that your ad unit mapping targets.

  - Make sure your ad unit IDs match those in the
    AdMob UI as they must match exactly.

  - [Initialize the Google Mobile Ads
    SDK](https://developers.google.com/admob/android/next-gen/quick-start#initialize_the_mobile_ads_sdk)
    and verify that the adapter status is `READY` prior to loading an ad.

  - Use the latest version of the adapter and SDK binaries for the ad source
    you're trying to integrate with.
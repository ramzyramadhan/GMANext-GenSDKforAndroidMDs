This guide explains how to provide targeting information to the Google Mobile
Ads SDK.

## Prerequisite

Before you continue, [set up GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start).

## RequestConfiguration

[`RequestConfiguration`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration)
collects targeting information applied globally to every ad request. For
available targeting tags, refer to the
[`RequestConfiguration.Builder`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration.Builder)
documentation.

Create a `RequestConfiguration` object with the targeting tags you need using
its builder, then set the configuration by calling
[`MobileAds.setRequestConfiguration()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/MobileAds#setRequestConfiguration(com.google.android.libraries.ads.mobile.sdk.common.RequestConfiguration)).

> [!IMPORTANT]
> **Key Point:** Calling `MobileAds.setRequestConfiguration()` replaces any existing configuration.

### Kotlin

    val requestConfiguration = RequestConfiguration
      .Builder()
      // Set your targeting tags.
      .setTagForChildDirectedTreatment(RequestConfiguration.TagForChildDirectedTreatment.TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE)
      .build()

    MobileAds.setRequestConfiguration(requestConfiguration)

### Java

    RequestConfiguration requestConfiguration = new RequestConfiguration
      .Builder()
      // Set your targeting tags.
      .setTagForChildDirectedTreatment(TagForChildDirectedTreatment.TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE)
      .build();

    MobileAds.setRequestConfiguration(requestConfiguration);

To apply targeting tags from the first ad request, provide the request
configuration during SDK initialization:

### Kotlin

    val requestConfiguration = RequestConfiguration
    .Builder()
    // Set your targeting tags.
    .setTagForChildDirectedTreatment(RequestConfiguration.TagForChildDirectedTreatment.TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE)
    .build()

    CoroutineScope(Dispatchers.IO).launch {
      // Initialize GMA Next-Gen SDK on a background thread.
      MobileAds.initialize(
        this@MainActivity,
        InitializationConfig
          // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
          .Builder("SAMPLE_APP_ID")
          .setRequestConfiguration(requestConfiguration)
          .build()
      ) {
        // Adapter initialization is complete.
      }
      // Other methods on MobileAds can now be called.
    }

### Java

    RequestConfiguration requestConfiguration = new RequestConfiguration
    .Builder()
    // Set your targeting tags.
    .setTagForChildDirectedTreatment(TagForChildDirectedTreatment.TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE)
    .build();

    new Thread(
        () -> {
          // Initialize GMA Next-Gen SDK on a background thread.
          MobileAds.initialize(
              this,
              // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
              new InitializationConfig
                  .Builder("SAMPLE_APP_ID")
                  .setRequestConfiguration(requestConfiguration)
                  .build(),
              initializationStatus -> {
                // Adapter initialization is complete.
              });
          // Other methods on MobileAds can now be called.
        })
        .start();

### Set the age treatment

To help you manage your compliance with applicable privacy regulations related
to children and teens, GMA Next-Gen SDK provides an age treatment setting. The
age treatment setting lets you indicate whether GMA Next-Gen SDK should apply
specific ad serving protections for children, teens, or an unspecified age.

You can set age treatment with the `setAgeRestrictedTreatment()` method with the
[`RequestConfiguration.Builder`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration.Builder)
API.

The following example indicates that ad requests should receive child age
treatment:

### Kotlin

    val requestConfiguration =
      RequestConfiguration.Builder()
        // Indicate that ad requests should have child age treatment.
        .setAgeRestrictedTreatment(AgeRestrictedTreatment.CHILD)
        .build()
    MobileAds.setRequestConfiguration(requestConfiguration)

### Java

    RequestConfiguration requestConfiguration =
        new RequestConfiguration.Builder()
            // Indicate that ad requests should have child age treatment.
            .setAgeRestrictedTreatment(AgeRestrictedTreatment.CHILD)
            .build();
    MobileAds.setRequestConfiguration(requestConfiguration);

To indicate teen or an unspecified age treatment, replace the `CHILD` setting
with the following:

- `TEEN`
- `UNSPECIFIED`

When using the setting, GMA Next-Gen SDK includes a `tfat` parameter in ad
requests. Consult your legal counsel to determine the applicable age treatment
for your users based on your
legal and regulatory obligations. For more information, see
[Tag an ad request from an app for age restricted treatment](https://support.google.com/admob/answer/6219315).

#### Migrate to age treatment from TFCD and TFUA

The age treatment setting replaces the deprecated
`.setTagForChildDirectedTreatment()` (TFCD) and `.setTagForUnderAgeOfConsent()`
(TFUA) settings.

The following table shows the TFCD and TFUA settings and their age treatment
equivalents:

### TFCD

| `TFCD` | Age treatment |
|---|---|
| `TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE` | `AgeRestrictedTreatment.CHILD` |
| `TAG_FOR_CHILD_DIRECTED_TREATMENT_FALSE` | `AgeRestrictedTreatment.UNSPECIFIED` |
| `TAG_FOR_CHILD_DIRECTED_TREATMENT_UNSPECIFIED` | `AgeRestrictedTreatment.UNSPECIFIED` |
| No value assigned `.setTagForChildDirectedTreatment()` | `AgeRestrictedTreatment.UNSPECIFIED` |
| No equivalent | `AgeRestrictedTreatment.TEEN` |

### TFUA

| `TFUA` | Age treatment |
|---|---|
| `TAG_FOR_UNDER_AGE_OF_CONSENT_TRUE` | `AgeRestrictedTreatment.CHILD` |
| `TAG_FOR_UNDER_AGE_OF_CONSENT_FALSE` | `AgeRestrictedTreatment.UNSPECIFIED` |
| `TAG_FOR_UNDER_AGE_OF_CONSENT_UNSPECIFIED` | `AgeRestrictedTreatment.UNSPECIFIED` |
| No value assigned `.setTagForUnderAgeOfConsent()` | `AgeRestrictedTreatment.UNSPECIFIED` |
| No equivalent | `AgeRestrictedTreatment.TEEN` |

#### Understand age treatment interactions with TFCD and TFUA

If you set age treatment setting and TFCD or TFUA settings, Google applies the
most conservative treatment.

> [!NOTE]
> **Note:** Apps in the [Designed For Families
> Program](https://developer.android.com/distribute/google-play/families) as [Primarily child-directed apps](https://support.google.com/admob/answer/6223431) and users signed into Google Accounts managed with [Family
> Link](https://families.google.com/familylink/) automatically have all content treated as child-directed for all ad requests.

### Child-directed setting

> [!WARNING]
> **Warning:** The `setTagForChildDirectedTreatment()` method is deprecated. Instead, [set the `setAgeRestrictedTreatment()`](https://developers.google.com/admob/android/next-gen/targeting#set_the_age_treatment) method.

For purposes of the [Children's Online Privacy Protection Act
(COPPA)](https://www.ftc.gov/tips-advice/business-center/privacy-and-security/children%27s-privacy),
there is a setting called "tag for child-directed treatment".
By setting this tag, you certify that this notification is accurate
and you are authorized to act on behalf of the owner of the app.
You understand that abuse of this setting may result in termination
of your Google Account.

As an app developer, you can indicate whether you want Google to treat your
content as child-directed when you make an ad request. If you indicate that you
want Google to treat your content as child-directed, we take steps to
disable IBA and remarketing ads on that ad request.

You can apply the child-directed setting through

[`setTagForChildDirectedTreatment()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration.Builder#setTagForChildDirectedTreatment(com.google.android.libraries.ads.mobile.sdk.common.RequestConfiguration.TagForChildDirectedTreatment)):


- Call `setTagForChildDirectedTreatment` with
  `TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE` to indicate that you want your
  content treated as child-directed for purposes of COPPA. This prevents
  the transmission of the [Android advertising identifier
  (AAID)](https://support.google.com/googleplay/android-developer/answer/6048248).

- Call `setTagForChildDirectedTreatment` with
  `TAG_FOR_CHILD_DIRECTED_TREATMENT_FALSE` to indicate that you don't want your
  content treated as child-directed for purposes of COPPA.

- Call `setTagForChildDirectedTreatment` with
  `TAG_FOR_CHILD_DIRECTED_TREATMENT_UNSPECIFIED` if you don't want to indicate
  how you would like your content treated with respect to COPPA in ad requests.

The following example indicates that you want your content treated as
child-directed for purposes of COPPA:

### Kotlin

    val requestConfiguration = RequestConfiguration
      .Builder()
      .setTagForChildDirectedTreatment(RequestConfiguration.TagForChildDirectedTreatment.TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE)
      .build()

    MobileAds.setRequestConfiguration(requestConfiguration)

### Java

    RequestConfiguration requestConfiguration = new RequestConfiguration
      .Builder()
      .setTagForChildDirectedTreatment(TagForChildDirectedTreatment.TAG_FOR_CHILD_DIRECTED_TREATMENT_TRUE)
      .build();

    MobileAds.setRequestConfiguration(requestConfiguration);

> [!NOTE]
> **Note:** Apps in the [Designed For Families
> Program](https://developer.android.com/distribute/google-play/families) as [Primarily child-directed apps](https://support.google.com/admob/answer/6223431) and users signed into Google Accounts managed with [Family
> Link](https://families.google.com/familylink/) automatically have all content treated as child-directed for all ad requests.

### Users under the age of consent

> [!WARNING]
> **Warning:** The `setTagForUnderAgeOfConsent()` method is deprecated. Instead, [set
> the `setAgeRestrictedTreatment()`](https://developers.google.com/admob/android/next-gen/targeting#set_the_age_treatment) method.


You can mark your ad requests to receive treatment for users in the
European Economic Area (EEA) under the age of consent. This feature is
designed to help facilitate compliance with the [General
Data Protection Regulation (GDPR)](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32016R0679). Note that you may have other legal
obligations under GDPR. Review European Union guidance and consult with
your own legal counsel. Note that Google's tools are designed to facilitate
compliance and don't relieve any particular publisher of its obligations under
the law.


[Learn more about how the GDPR affects publishers](https://support.google.com/admob/answer/7666366).


When using this feature, a Tag For Users under the Age of Consent in Europe
(TFUA) parameter is included in the ad request. This parameter disables
personalized advertising, including remarketing, for all ad requests. It also
disables requests to third-party ad vendors, such as ad measurement pixels and
third-party ad servers.

Like child-directed settings, there is a method in
`RequestConfiguration.Builder` for setting the TFUA parameter:

[`setTagForUnderAgeOfConsent()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration.Builder#setTagForUnderAgeOfConsent(com.google.android.libraries.ads.mobile.sdk.common.RequestConfiguration.TagForUnderAgeOfConsent)),

with the following options.

- Call `setTagForUnderAgeOfConsent()` with `TAG_FOR_UNDER_AGE_OF_CONSENT_TRUE`
  to indicate that you want the ad request to receive treatment for users in
  the European Economic Area (EEA) under the age of consent. This also
  prevents the transmission of the [Android advertising identifier
  (AAID)](https://support.google.com/googleplay/android-developer/answer/6048248).

- Call `setTagForUnderAgeOfConsent()` with `TAG_FOR_UNDER_AGE_OF_CONSENT_FALSE`
  to indicate that you want the ad request to *not* receive treatment for users
  in the European Economic Area (EEA) under the age of consent.

- Call `setTagForUnderAgeOfConsent()` with
  `TAG_FOR_UNDER_AGE_OF_CONSENT_UNSPECIFIED` to indicate that you have not
  specified whether the ad request should receive treatment for users in the
  European Economic Area (EEA) under the age of consent.

The following example indicates that you want TFUA included in your ad requests:

### Kotlin

    val requestConfiguration = RequestConfiguration
      .Builder()
      .setTagForUnderAgeOfConsent(RequestConfiguration.TagForUnderAgeOfConsent.TAG_FOR_UNDER_AGE_OF_CONSENT_TRUE)
      .build()

    MobileAds.setRequestConfiguration(requestConfiguration)

### Java

    RequestConfiguration requestConfiguration = new RequestConfiguration
      .Builder()
      .setTagForUnderAgeOfConsent(TagForUnderAgeOfConsent.TAG_FOR_UNDER_AGE_OF_CONSENT_TRUE)
      .build();

    MobileAds.setRequestConfiguration(requestConfiguration);

The tags to enable the [Child-directed setting](https://developers.google.com/admob/android/next-gen/targeting#child-directed_setting)
and `setTagForUnderAgeOfConsent()` shouldn't both simultaneously be set to
`true`. If they are, the child-directed setting takes precedence.

### Ad content filtering

To comply with Google Play's [Inappropriate Ads Policy](https://support.google.com/googleplay/android-developer/answer/9857753#zippy=%2Cexamples-of-common-violations)
that includes associated offers within an ad, all ads and their associated
offers shown within your app must be appropriate for the [content
rating](https://support.google.com/googleplay/android-developer/answer/9898843) of
your app, even if the content by itself is otherwise compliant with Google
Play's policies.

Tools like maximum ad content rating can help you have more control over the
content of the ads shown to your users. You can set a maximum content rating to
help compliance with platform policies.

Apps can set a maximum ad content rating for their ad requests using the

[`setMaxAdContentRating`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration.Builder#setMaxAdContentRating(com.google.android.libraries.ads.mobile.sdk.common.RequestConfiguration.MaxAdContentRating))

method. AdMob ads returned when this is configured have a content rating at or
lower than that level. The possible values for this network extra are based on
[digital content label classifications](https://support.google.com/admob/answer/7562142), and must be one of the following
strings:

- `MAX_AD_CONTENT_RATING_G`
- `MAX_AD_CONTENT_RATING_PG`
- `MAX_AD_CONTENT_RATING_T`
- `MAX_AD_CONTENT_RATING_MA`

The following code configures an `RequestConfiguration` object to specify that
ad content returned should correspond to a digital content label designation no
higher than `G`:

### Kotlin

    val requestConfiguration = RequestConfiguration
      .Builder()
      .setMaxAdContentRating(RequestConfiguration.MaxAdContentRating.MAX_AD_CONTENT_RATING_G)
      .build()

    MobileAds.setRequestConfiguration(requestConfiguration)

### Java

    RequestConfiguration requestConfiguration = new RequestConfiguration
      .Builder()
      .setMaxAdContentRating(MaxAdContentRating.MAX_AD_CONTENT_RATING_G)
      .build();

    MobileAds.setRequestConfiguration(requestConfiguration);

Learn more about:

- [Setting the maximum content rating for each ad
  request](https://support.google.com/admob/answer/10477886)

- [Setting the maximum ad content rating for an app or
  account](https://support.google.com/admob/answer/7562142)

> [!NOTE]
> **Note:** Content rating filter settings specified through GMA Next-Gen SDK override any settings configured using the AdMob UI.

### Publisher Privacy Treatment (Beta)

The

[Publisher Privacy Treatment](https://support.google.com/admob/answer/14323214)

(PPT) API is an optional tool that lets apps indicate whether to turn off ads
personalization for all ad requests using the

[`setPublisherPrivacyPersonalizationState()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/RequestConfiguration.Builder#setPublisherPrivacyPersonalizationState(com.google.android.libraries.ads.mobile.sdk.common.RequestConfiguration.PublisherPrivacyPersonalizationState))
method. When using this feature, a publisher privacy treatment (PPT)
parameter is included in all future ad requests for the remainder of the session.

By default, ad requests to Google are served personalized ads. The following
code turns off ads personalization for all ad requests:

### Kotlin

    val requestConfiguration = RequestConfiguration
      .Builder()
      .setPublisherPrivacyPersonalizationState(RequestConfiguration.PublisherPrivacyPersonalizationState.DISABLED)
      .build()

    MobileAds.setRequestConfiguration(requestConfiguration)

### Java

    RequestConfiguration requestConfiguration = new RequestConfiguration
      .Builder()
      .setPublisherPrivacyPersonalizationState(RequestConfiguration.PublisherPrivacyPersonalizationState.DISABLED)
      .build();

    MobileAds.setRequestConfiguration(requestConfiguration);

> [!NOTE]
> **Note:** For ad requests with multiple user privacy signals, the most restrictive signal will take precedence. See the [Publisher Privacy Treatment API](https://support.google.com/admob/answer/14323214) documentation for specific examples.

## Ad request

The `AdRequest` object collects targeting information to be sent
with an ad request.

### Add network extras

Network extras are extra details sent with an ad request that are specific to
a single ad source.

The following code snippet sets an extra parameter key of `collapsible` with
a value of `bottom` to Google:

### Kotlin

    val extras = Bundle()
    extras.putString("collapsible", "bottom")
    val adRequest =
      NativeAdRequest.Builder("AD_UNIT_ID", listOf(NativeAd.NativeAdType.NATIVE))
        .setGoogleExtrasBundle(extras)
        .build()
    NativeAdLoader.load(adRequest, adCallback)

### Java

    Bundle extras = new Bundle();
    extras.putString("collapsible", "bottom");
    NativeAdRequest adRequest =
      new NativeAdRequest.Builder("AD_UNIT_ID", Arrays.asList(NativeAd.NativeAdType.NATIVE))
        .setGoogleExtrasBundle(extras)
        .build();
    NativeAdLoader.load(adRequest, adCallback);

<br />
Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy/gdpr "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy/gdpr "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/privacy/gdpr "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/privacy/gdpr "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy/gdpr "View this page for the Android (Legacy) platform docs.")

<br />

Under the Google [EU User Consent
Policy](https://www.google.com/about/company/user-consent-policy/), you must
make certain disclosures to your users in the European Economic Area (EEA),
the United Kingdom (UK), and Switzerland, and obtain their consent to use
cookies or other local storage, where legally required, and to use personal
data (such as AdID) to serve ads.

This policy reflects the requirements of the EU ePrivacy Directive and the
General Data Protection Regulation (GDPR).

This guide outlines the steps required to support the GDPR IAB TCF v2 message as
part of the UMP SDK. It is intended to be paired with [Get
started](https://developers.google.com/admob/android/next-gen/privacy) which gives an overview of how to get your app
running with the UMP SDK and the basics of setting up your message. The
following guidance is specific to the GDPR IAB TCF v2 message. For more
information, see

[How IAB requirements affect EU consent messages](https://support.google.com/admob/answer/10207733).


## Prerequisites

- [Set up UMP SDK](https://developers.google.com/admob/android/next-gen/privacy).
- Create a [European regulation message for apps](https://support.google.com/admob/answer/10113207).

## Consent revocation

GDPR requires

[consent revocation](https://support.google.com/admob/answer/10113915)

to allow users to withdraw their consent choices at any time. See
[Privacy options](https://developers.google.com/admob/android/next-gen/privacy#privacy_options)
to implement a way for users to withdraw their consent choices.

## Tag for under age of consent

To indicate whether a user is under the age of consent, set
`
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentRequestParameters.Builder#public-consentrequestparameters.builder-settagforunderageofconsent-boolean-tagforunderageofconsent` (TFUA). When you set TFUA to `true`, the UMP SDK
doesn't request consent from the user. If your app has a mixed audience, set
this parameter for child users to ensure consent is not requested.

> [!IMPORTANT]
> **Important:** The UMP SDK does not forward the TFUA tag set on consent requests to GMA Next-Gen SDK. You must explicitly set the `tagForUnderAgeOfConsent` on ad requests. If you don't set the `tagForUnderAgeOfConsent` or on ad requests, the UMP SDK does not collect any information that allows Google to determine whether or not users under the age of consent use your app. For more information about data processing restrictions for these users, see [Tag an ad request for EEA, the UK, and Switzerland users for restricted data
> processing](https://support.google.com/admob/answer/9009425).

The following example sets TFUA to true on a UMP consent request:

### Java

    ConsentRequestParameters params = new ConsentRequestParameters
        .Builder()
        // Indicate the user is under age of consent.
    .setTagForUnderAgeOfConsent(true)
        .build();

    consentInformation = UserMessagingPlatform.getConsentInformation(this);
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        (OnConsentInfoUpdateSuccessListener) () -> {
          // ...
        },
        (OnConsentInfoUpdateFailureListener) requestConsentError -> {
          // ...
        });

### Kotlin

    val params = ConsentRequestParameters
        .Builder()
        // Indicate the user is under age of consent.
    .setTagForUnderAgeOfConsent(true)
        .build()

    consentInformation = UserMessagingPlatform.getConsentInformation(this)
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        ConsentInformation.OnConsentInfoUpdateSuccessListener {
          // ...
        },
        ConsentInformation.OnConsentInfoUpdateFailureListener {
          requestConsentError ->
          // ...
        })

## Mediation

Follow the steps in

[Add ad partners to published GDPR
messages](https://support.google.com/admob/answer/10113004#adding_ad_partners_to_published_gdpr_messages)

to add your mediation partners to the ad partners list. Failure to do so can
lead to partners failing to serve ads on your app.

Mediation partners might also have additional tools to help with GDPR
compliance. See a specific partner's [integration
guide](https://developers.google.com/admob/android/next-gen/choose-networks) for more details.

## How to read consent choices

After GDPR consent has been collected, you can read consent choices from local
storage following the
[TCF v2 spec](https://github.com/InteractiveAdvertisingBureau/GDPR-Transparency-and-Consent-Framework/blob/master/TCFv2/IAB%20Tech%20Lab%20-%20CMP%20API%20v2.md#in-app-details).
The `IABTCF_PurposeConsents` key indicates consent for each of the
[TCF purposes](https://iabeurope.eu/iab-europe-transparency-consent-framework-policies/#A_Purposes).

The following code snippet shows how to check consent for Purpose 1:

### Java

    SharedPreferences sharedPref = PreferenceManager.getDefaultSharedPreferences(context);
    // Example value: "1111111111"
    String purposeConsents = sharedPref.getString("IABTCF_PurposeConsents", "");
    // Purposes are zero-indexed. Index 0 contains information about Purpose 1.
    if (!purposeConsents.isEmpty()) {
      String purposeOneString = purposeConsents.charAt(0).toString();
      boolean hasConsentForPurposeOne = purposeOneString.equals("1");
    }

### Kotlin

    val sharedPref = PreferenceManager.getDefaultSharedPreferences(context)
    // Example value: "1111111111"
    val purposeConsents = sharedPref.getString("IABTCF_PurposeConsents", "")
    // Purposes are zero-indexed. Index 0 contains information about Purpose 1.
    if (purposeConsents?.isEmpty() == false) {
      val purposeOneString = purposeConsents.first().toString()
      val hasConsentForPurposeOne = purposeOneString == "1"
    }

<br />

## Frequently asked questions

> [!IMPORTANT]
> **Key Point:** See [European regulations message FAQs](https://support.google.com/admob/answer/10113207) for additional frequently asked questions.

What happens if I take no action to meet the [Consent Management Platform Requirements for serving ads in the EEA, the UK, and Switzerland](https://support.google.com/admob/answer/13554116)?

:   Beginning January 16, 2024, if a partner doesn't adopt a

    [Google-certified CMP](https://support.google.com/admob/answer/13554116#zippy=%2Cgoogle-certified-cmps),

    only

    [Limited Ads](https://support.google.com/admob/answer/10105530)

    will be eligible to serve on EEA and UK traffic.

    Enforcement will begin January 16, 2024 on a small percentage of EEA and UK
    traffic and will ramp up until Google enforces across all EEA and UK traffic
    by the end of February 2024. Have a certified CMP in place by January 16,
    2024 to ensure your monetization is not impacted.

How can I check if the user consented?

:   Consent is not represented by a single bit, but rather a set of purposes and
    vendors as defined in the [IAB TCF specification](https://iabeurope.eu/transparency-consent-framework/). See

    [Consent Policies: Personalized \& Non-Personalized Ads](https://support.google.com/admob/answer/9760862#consent-policies)

    for Google Ads personalization criteria.

    Additionally, ad techs on Google's

    [Ad technology providers](https://support.google.com/admob/answer/9012903)

    (ATP) list that are not registered in the
    [TCF vendor list](https://iabeurope.eu/vendor-list-tcf/) use

    [Google's Additional Consent technical specification](https://support.google.com/admob/answer/9681920)

    for consent collection. Google
    publishes the list of ad technology providers not registered with the IAB
    and their IDs at the following location:
    <https://storage.googleapis.com/tcfac/additional-consent-providers.csv>.

    To debug an individual ad request, use the
    [Troubleshoot privacy settings](https://developers.google.com/admob/android/next-gen/ad-inspector/troubleshoot-privacy-settings)
    feature in ad inspector to view the following privacy signals passed in the
    ad request as part of
    [Publisher integration with the IAB Europe TCF](https://support.google.com/admob/answer/9760862):

    | Ad inspector label | Ad request query parameter | Meaning |
    |---|---|---|
    | GDPR applies (IABTCF_gdprApplies) | `gdpr` | Whether GDPR applies for this ad request. |
    | TC string (IABTCF_TCString) | `gdpr_consent` | The TC String. The IAB provides a web tool where you can manually [decode](https://iabtcf.com/#/decode) the value. |
    | AC string (IABTCF_AddtlConsent) | `addtl_consent` | The AC string from [Google's Additional Consent technical specification](https://support.google.com/admob/answer/9681920). |

    To read consent choices programmatically, see

    [How to read consent choices](https://developers.google.com/admob/android/next-gen/privacy/gdpr#how_to_read_consent_choices)

    for more information.

Do I need to use Google's UMP SDK to meet the CMP requirement?

:   No, you can use any CMP from the

    [List of Google-certified CMP](https://support.google.com/admob/answer/13554116#zippy=%2Cgoogle-certified-cmps)

    to serve ads.

How can I show the consent form again using the UMP SDK even if the user has already consented?

:   If a user has already made a consent decision, Google's consent management
    solution won't request to gather new consent until the TC string is expired
    or otherwise becomes invalid.

    GDPR requires consent modification to allow users to withdraw their consent
    choices at any time. See
    [privacy options](https://developers.google.com/admob/android/next-gen/privacy#privacy_options) to
    implement a way for users to withdraw their consent choices. To show a
    consent form again, call `
    https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/UserMessagingPlatform#showPrivacyOptionsForm(android.app.Activity,%20com.google.android.ump.ConsentForm.OnConsentFormDismissedListener)`.

I integrated a Google-certified CMP, but I'm not seeing any ad requests get made to mediation partners even from users who consented. Why is this happening?

:   Under TCF, Google checks that ad technology providers and other programmatic
    demand sources don't violate Google policy and have at least one legal basis
    for processing data prior to including them in the mediation waterfall.
    Navigate to the

    [mediation](https://support.google.com/admob/answer/10105530)

    section for more information.

    Some mediation partners in

    [Google's Ad Tech Providers (ATP) list](https://support.google.com/admob/answer/9012903)

    are not registered in the [TCF vendor list](https://iabeurope.eu/vendor-list-tcf/).
    These partners instead use

    [Google's Additional Consent technical specification](https://support.google.com/admob/answer/9681920)

    for consent collection. Google publishes the list of ad technology providers
    not registered with the IAB and their IDs at the following location:
    <https://storage.googleapis.com/tcfac/additional-consent-providers.csv>

    The UMP SDK supports storing the ACString, enabling you to

    [Add ad partners to published GDPR messages](https://support.google.com/admob/answer/10113004#adding_ad_partners_to_published_gdpr_messages)

    without needing to understand whether partners are TCF-registered. When using
    a third-party CMP, you should do the following:

    1. Confirm that the third-party CMP supports storing the ACString.
    2. Include each mediation partner in the list of ad technology providers that the third-party CMP uses to gather consent.

Can I change how my app functions if users don't consent? Is this allowed by policy?

:   Publishers can read the IAB TCF string in their apps. See

    [How to read consent choices](https://developers.google.com/admob/android/next-gen/privacy/gdpr#how_to_read_consent_choices)

    for information on reading consent choices programmatically. Publishers
    should review their obligations under relevant regulations with legal
    counsel.

When I select **Manage Options** and consent to all purposes, I'm not seeing any ads? Why is this happening?

:   In addition to collecting purposes consent you also need to collect vendor
    consent. Both purposes consent and vendor consent are required for any
    vendor, such as Google, to serve appropriate ads.


How do I implement the AC String version 2 for users who already consented to version 1?

:   Check the `IABTCF_AddtlConsent` key in local storage per

    [Google's Additional Consent technical specification](https://support.google.com/admob/answer/9681920)

    to determine whether a user has consented to AC String version 2 and if you
    need to show the consent form again.

    ### Java

        SharedPreferences sharedPref = PreferenceManager.getDefaultSharedPreferences(context);
        // Example value: "2~1.35.41.101~dv.9.21.81"
        String additionalConsent = sharedPref.getString("IABTCF_AddtlConsent", "");
        // Index 0 contains information about the specification version number.
        if (!additionalConsent.isEmpty()) {
          String specACVersion = additionalConsent.charAt(0);
          boolean isACVersion2 = purposeOneString.equals("2");
        }

    ### Kotlin

        val sharedPref = PreferenceManager.getDefaultSharedPreferences(context)
        // Example value: "2~1.35.41.101~dv.9.21.81"
        val additionalConsent = sharedPref.getString("IABTCF_AddtlConsent", "")
        // Index 0 contains information about the specification version number.
        if (!additionalConsent.isEmpty()) {
          val specACVersion = additionalConsent.first()
          val isACVersion2 = specACVersion == "2"
        }
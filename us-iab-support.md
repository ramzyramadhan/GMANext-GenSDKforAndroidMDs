Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy/us-iab-support "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy/us-iab-support "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/privacy/us-iab-support "View this page for the Unity platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy/us-iab-support "View this page for the Android (Legacy) platform docs.")

<br />

This guide outlines the steps required to support the US states regulations
message as part of the UMP SDK. Pair these instructions with
[Get started](https://developers.google.com/admob/android/next-gen/privacy), which details how to get your app running
with the UMP SDK and set up your message. The following guidance is specific to
the US states regulations message.

## Prerequisites

Before continuing, ensure you do the following:

- Update to the latest version of the UMP SDK. For US states regulations messaging support, we recommend you to use version 2.1.0 or higher.
- [Set up UMP SDK](https://developers.google.com/admob/android/next-gen/privacy). Be sure to implement a privacy options entrypoint and render it if required. By completing this guide, you have an entrypoint to serve your US states regulations message to your users.
- [Create a US states regulations message](https://support.google.com/admob/answer/10860309) for apps.
- If you're using the US states regulations message alongside other messages, consult [Available user message types](https://support.google.com/admob/answer/10114020) to understand when different messages are displayed to your users.

## Set the tag for under age of consent

To indicate whether a user is under the age of consent, set
`
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentRequestParameters.Builder#public-consentrequestparameters.builder-settagforunderageofconsent-boolean-tagforunderageofconsent` (TFUA). When you set TFUA to `true`, the UMP SDK
doesn't request consent from the user. If your app has a mixed audience, set
this parameter for child users to ensure consent is not requested.
It is your
responsibility for setting this parameter where necessary to comply with COPPA
and other relevant regulations.

> [!IMPORTANT]
> **Important:** The UMP SDK does not forward the TFUA tag set on consent requests to GMA Next-Gen SDK. You must explicitly set the `tagForUnderAgeOfConsent` or `tagForChildDirectectedTreatment` on ad requests. If you don't set the `tagForUnderAgeOfConsent` or `tagForChildDirectectedTreatment` on ad requests, the UMP SDK does not collect any information that allows Google to determine whether or not users under the age of consent use your app. For more information about data processing restrictions for these users, see [Tag an ad request from an app for child-directed treatment](https://support.google.com/admob/answer/6219315).

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

## Read consent choices

After the user has made a US states regulations decision, you can read
their choice from local storage following the Global Privacy Platform (GPP)
spec. For more details see,
[In-App Details](https://github.com/InteractiveAdvertisingBureau/Global-Privacy-Platform/blob/main/Core/CMP%20API%20Specification.md#in-app-details).
Note that the UMP SDK only populates the `IABGPP_GppSID` and
`IABGPP_HDR_GppString` keys.

## Test your US states regulations messaging

To test your US states regulations messaging, use the
`UMPDebugGeographyRegulatedUSState` `debugGeography` to force the UMP
SDK to treat your test device as if the device were located in a regulated US
state. You can also use `UMPDebugGeographyOther` to force suppression of US
states regulations messages. For more details on `debugGeography`, see
[Force a geography](https://developers.google.com/admob/android/next-gen/privacy#force_a_geography).

> [!NOTE]
> **Note:** `UMPDebugGeographyRegulatedUSState` is only available in UMP version 3.1.0 or higher.
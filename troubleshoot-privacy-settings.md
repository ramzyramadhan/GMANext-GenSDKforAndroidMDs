Select platform: [Android](https://developers.google.com/admob/android/next-gen/ad-inspector/troubleshoot-privacy-settings "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/ad-inspector/troubleshoot-privacy-settings "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/ad-inspector/troubleshoot-privacy-settings "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/ad-inspector/troubleshoot-privacy-settings "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/ad-inspector/troubleshoot-privacy-settings "View this page for the Android (Legacy) platform docs.")

<br />

[Video](https://www.youtube.com/watch?v=Gmgu4zJ8Ivo)

As part of

[Publisher integration with the IAB Europe TCF](https://support.google.com/admob/answer/9760862),

use ad inspector to view the following privacy signals in the ad request:

- GDPR applies: `IABTCF_gdprApplies`
- AC string: `IABTCF_AddtlConsent`
- TC string: `IABTCF_TCString`

## Prerequisites

Before you continue, do the following:

- Complete all items in the initial [Prerequisites](https://developers.google.com/admob/android/next-gen/ad-inspector#prerequisites) to create an AdMob account, set your test device, initialize GMA Next-Gen SDK, and install the latest version.
- [Launch ad inspector](https://developers.google.com/admob/android/next-gen/ad-inspector/launch-ad-inspector).

## View consent management platform details

Ad inspector provides you information about your consent management platform
(CMP), such as the name of the CMP and the ID, through the **Privacy** tab. The
**Privacy** tab displays your CMP information through
**Consent Management Platform (CMP)** , and a list of missing ad partners in
the **Ad partners** page.

### Review CMP information

**Consent Management Platform (CMP)** displays the following information:

| Details | Description |
|---|---|
| **CMP** | The name of the CMP. |
| **CMP ID** | The ID of the CMP. |
| **CMP Version** | The CMP version that collects user consent. |

> [!NOTE]
> **Note:** To view CMP details, you must have at least one ad request available.

![Ad inspector Consent Management Platform details](https://developers.google.com/static/admob/images/ad-inspector-cmp.png)

### Analyze missing ad partners

If you've configured an ad partner in mediation, but not in the CMP, ad
inspector warns you of missing ad partners through the **Ad partners** page.
The **Ad partners** page displays a list of missing ad partners not configured
in your CMP and not present in the TC/AC string. When testing, you must consent
to all ad partners. For more details, see
[Adding ad partners to published European regulations messages](https://support.google.com/admob/answer/10113004#adding_ad_partners_to_published_gdpr_messages).

To access the **Ad partners** page, click **View** **ad partners** in
**Consent Management Platform (CMP)** . For more details, see
[Review CMP information](https://developers.google.com/admob/android/next-gen/ad-inspector/troubleshoot-privacy-settings#review_cmp_information).

![Ad inspector ad partners page with IDs](https://developers.google.com/static/admob/images/ad-inspector-ad-partners-page-with-ids.png)

For each ad partner, you see the following labels:

| Label | Description |
|---|---|
| **Missing** | An ad partner configured in mediation, but not in the TC/AC string. |
| **IAB only** | An IAB-approved ad partner. For more details, see [GDPR IAB support](https://developers.google.com/admob/android/privacy/gdpr). |
| **Provider ID** | The company ID of an ad technology partner (ATP). For more details, see [Ad technology providers](https://support.google.com/admanager/answer/9012903). |
| **ATP ID** | The ATP within the Additional Consent (AC) string. For details, see [Google's additional content technical specification](https://support.google.com/admanager/answer/9681920). |
| **GVL ID** | The IAB global vendor list (GVL) ID assigned to a a company that has registerded with the IAB Europe Transparency and Consent Framework (TCF). For details, see [TCF vendor list](https://iabeurope.eu/vendor-list-tcf/). |

### Export missing ad partners list

You can export and share a list of missing ad partners to troubleshoot and
support your CMP configuration.

To export a list of missing ad partners, do the following:

1. Click share **Share**.
2. Select your preferred extension. For example, **Notes**.

## View privacy signals and status

After you load an ad or
[request a test ad](https://developers.google.com/admob/android/next-gen/ad-inspector/test-ad-units#request_a_test_ad),
complete the following steps to view privacy signals and status:

1. Tap your preferred request in **SDK request log**.
2. Select **Privacy signals**:


   ![](https://developers.google.com/static/admob/images/inspector/advanced-1.png)

   **Privacy signals** displays the privacy signals in the ad request:

   ![](https://developers.google.com/static/admob/images/inspector/privacy-signals-details.png)

To learn more about the AC string, see

[Google's Additional Consent technical specification](https://support.google.com/admob/answer/9681920).

For consent details in the TC string, such as the CMP ID and the IAB consented
vendor list, click **Decode** .
[IAB GPP Encoder / Decoder](https://iabgpp.com/) opens. If you see errors,
follow

[Understand privacy issues](https://support.google.com/admob/answer/12659455#understand-privacy-issues).
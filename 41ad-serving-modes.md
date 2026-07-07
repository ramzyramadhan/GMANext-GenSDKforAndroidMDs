Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy/ad-serving-modes "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy/ad-serving-modes "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/privacy/ad-serving-modes "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/privacy/ad-serving-modes "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy/ad-serving-modes "View this page for the Android (Legacy) platform docs.")

<br />

Under Google's [EU User Consent
Policy](https://www.google.com/about/company/user-consent-policy.html), you must make
certain disclosures to your users in the European Economic Area (EEA) and the
UK, and obtain their consent for the use of cookies or other local storage where
legally required, and for the collection, sharing, and use of personal data for
ads personalization. This policy reflects the requirements of the EU ePrivacy
Directive and the General Data Protection Regulation (GDPR). To comply with this
policy, publishers are required to adopt a

[Google-certified consent management platform](https://support.google.com/admob/answer/13554116#zippy=%2Cgoogle-certified-cmps)

(CMP) that has integrated with the [TCF
framework](https://iabeurope.eu/transparency-consent-framework/) such as the [User
Messaging Platform SDK](https://developers.google.com/admob/android/next-gen/privacy). Once adopted, the CMP presents
consent choices, known as purposes, in your mobile app.

The exact UI for the consent choices is kept up-to-date by Google, but here's an
earlier version for reference:

![Consent choices sample image](https://developers.google.com/static/admob/images/privacy/consent-form-purposes.png)

> [!IMPORTANT]
> **Important:** In addition to collecting purposes consent, you also need to collect vendor consent. Both purposes consent and vendor consent are required for any vendor, such as Google, to serve appropriate ads.

The different types of ads that can be served are:

- [Personalized ads](https://developers.google.com/admob/android/next-gen/privacy/ad-serving-modes#personalized_ads)
- [Non-personalized ads](https://developers.google.com/admob/android/next-gen/privacy/ad-serving-modes#non-personalized_ads)
- [Limited ads](https://developers.google.com/admob/android/next-gen/privacy/ad-serving-modes#limited_ads)

### Personalized ads

[Personalized ads](https://support.google.com/admob/answer/7676680) are ads that make inferences about a user's interests based on the sites they visit or the apps they use. Google considers ads to be personalized when they are based on previously collected or historical data to determine or influence ad selection.

<br />

Google will serve personalized ads when all of the following criteria are met.
For more information, read

[requirements for personalized ads](https://support.google.com/admob/answer/9760862#consent-policies).


| Purpose | User consent choice |
|---|---|
| Purpose 1 | ✅ |
| Purpose 2 | ✔ or ✅ |
| Purpose 3 | ✅ |
| Purpose 4 | ✅ |
| Purpose 7 | ✔ or ✅ |
| Purpose 9 | ✔ or ✅ |
| Purpose 10 | ✔ or ✅ |
[Legend: ✅ Consent ✔ Legitimate interest]

### Non-personalized ads

[Non-personalized ads](https://support.google.com/admob/answer/7676680) are not based on a user's past behavior. Although non-personalized ads don't use cookies or mobile ad identifiers for ad targeting, these ads do still use cookies or mobile ad identifiers for frequency capping and aggregated ad reporting.

<br />

Google will serve non-personalized ads when all of the following criteria are
met. For more information, see

[Requirements for non-personalized ads](https://support.google.com/admob/answer/9760862#consent-policies).


| Purpose | User consent choice |
|---|---|
| Purpose 1 | ✅ |
| Purpose 2 | ✔ or ✅ |
| Purpose 7 | ✔ or ✅ |
| Purpose 9 | ✔ or ✅ |
| Purpose 10 | ✔ or ✅ |
[Legend: ✅ Consent ✔ Legitimate interest 🚫 No consent]

### Limited ads

[Limited ads (LTD)](https://support.google.com/admob/answer/10105530) disable all personalization and features that require using a local identifier.

<br />

Google serves limited ads when all of the following criteria are met. For
more information, read

[Launched: Limited ads 2.0](https://support.google.com/admob/answer/10105530#limited-ads-update).


- Special Purposes: 1, 2
- Legitimate Interest: 7 (optional only)
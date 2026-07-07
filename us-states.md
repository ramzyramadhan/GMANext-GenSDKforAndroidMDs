Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy/us-states "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy/us-states "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/privacy/us-states "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/privacy/us-states "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy/us-states "View this page for the Android (Legacy) platform docs.")

<br />

> [!IMPORTANT]
> **Important:** Verify that you have **Account Management** permission to complete the configuration for EU Consent and GDPR, US State regulations, and User Messaging Platform. See [Manage user access to your
> account](https://support.google.com/admob/answer/2784628) for details.

To help you comply with

[U.S. states privacy laws](https://support.google.com/admob/answer/9561022),

the GMA Next-Gen SDK lets you use Google
[restricted data processing](https://business.safety.google/rdp/) (RDP) parameter to
indicate whether to enable RDP. Google also supports the

[Global Privacy Platform](https://support.google.com/admob/answer/14126918)

(GPP) for applicable US states. When the GMA Next-Gen SDK uses either
signal, the SDK restricts certain unique identifiers and other data is processed
in the provision of services to you.

You must decide how restricted data processing can support your compliance plans
and when to enable. Determine whether to use the RDP parameter
directly or signaling consent and privacy choices with the
[GPP Specification](https://github.com/InteractiveAdvertisingBureau/Global-Privacy-Platform).

This guide helps you enable RDP on a per-ad request basis and use the GPP
signal.

## Enable the RDP signal

To notify Google to enable the RDP signal, write the key `gad_rdp` with a value
of `1` to
[`SharedPreferences`](https://developer.android.com/reference/android/content/SharedPreferences)
storage. GMA Next-Gen SDK reads the `gad_rdp` key during ad loading:

### Java

    SharedPreferences sharedPref = PreferenceManager.getDefaultSharedPreferences(context);
    sharedPref.edit().putInt("gad_rdp", 1).apply();

### Kotlin

    val sharedPref = PreferenceManager.getDefaultSharedPreferences(context)
    sharedPref.edit().putInt("gad_rdp", 1).apply()

<br />

> [!TIP]
> **Tip:** You can use [network tracing](https://developers.google.com/admob/android/next-gen/network-tracing) or a proxy tool such as [Charles](https://developers.google.com/admob/android/next-gen/charles) to capture your app's HTTPS traffic and inspect the ad requests for a `&rdp=` parameter.

## Use the IAB GPP Signal

If you collect consent decisions with a consent management platform or your own
custom messaging, the GMA Next-Gen SDK respects GPP signals written to
local storage. The User Messaging Platform (UMP) SDK supports writing the GPP
signal. To gather US state consent, see
[US IAB Support](https://developers.google.com/admob/android/next-gen/privacy/us-iab-support).
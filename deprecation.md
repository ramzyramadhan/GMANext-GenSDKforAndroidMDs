Select platform: [Android](https://developers.google.com/admob/android/next-gen/deprecation "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/deprecation "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/deprecation "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/deprecation "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/deprecation "View this page for the Android (Legacy) platform docs.")

<br />

With the release of a new major GMA Next-Gen SDK version, earlier
major versions may be given a sunset date. After an SDK version is sunset,
ad traffic from that version is at risk of receiving automatic no fill due to
stopped ad serving.

Benefits of a deprecation schedule

:   Introducing a predictable deprecation schedule offers the following benefits:

    - Ability to predict and plan for SDK updates with a year of lead time.
    - Legacy SDK code that only exists to support old versions can be deleted, thereby decreasing SDK size and lowering the risk of bugs.
    - Engineering resources can focus more on support for newer SDKs and innovation of new SDK features.

## Timetable

The following table lists the specific deprecation and sunset dates for each
version. We encourage you to migrate to the newest version as soon as possible
after its release.

| Versions | Status | Release date | Deprecation date | Sunset date |
|---|---|---|---|---|
| [v1.x.x](https://developers.google.com/admob/android/next-gen/rel-notes#1.0.0) | Supported | April 14, 2026 | Q1 2028 | Q2 2029 |
| [v0.x.x-beta](https://developers.google.com/admob/android/next-gen/rel-notes#0.18.0-beta01) | Supported | July 17, 2025 | Q1 2027 | Q2 2028 |
| [v0.x.x-alpha](https://developers.google.com/admob/android/next-gen/rel-notes#0.3.0-alpha01) | Deprecated | March 20, 2024 | July 1, 2026 | June 30, 2027 |

^1^ A more specific sunset date will
be announced on the [Google Ads Developer
blog](https://ads-developers.googleblog.com/search/label/mobile_ads_sdk), and updated
on this page with two months notice.

## Differences between supported, deprecated, and sunset

| Term | Supported | Deprecated | Sunset |
|---|---|---|---|
| **SDK Versions** | All releases with major release N and N-1, where N is the latest major version. | All releases with major release N-2. | All releases with major release N-3 or below. Releases with major version N-3 will sunset approximately two months after major version N is released. |
| **Ad serving** | Ads serve to this version. | Ads serve to this version. | Ads **at risk of not serving** to this version. We will regularly review usage of all sunset versions going forward to consider disabling ad serving. The oldest versions with lower usage and higher maintenance costs will be targeted first. When ad serving is disabled, ad requests return a no fill with an error indicating that this version is sunset. |
| **Support** | Technical SDK support is available through the [contact us form](https://support.google.com/admob/contact/contact_us_gma_sdk). | Technical SDK support is no longer provided for this version. You will be asked to validate the issue in a supported version to receive full support. | Technical SDK support is no longer provided for this version. You will be asked to validate the issue in a supported version to receive full support. |

## Lifecycle of a major SDK version

![](https://developers.google.com/static/admob/images/deprecation_schedule.png)

In general, a new major version will live in the **supported** state for about
two years, and in a **deprecated** state for an additional year before moving
to a **sunset** state.

The deprecation and sunset timelines for GMA Next-Gen SDK revolve
around major SDK releases. We plan to have one major version release in the
first quarter of each year. The release of a new major version triggers changes
in support for earlier major versions.

When a new major version N is released:

- All SDK versions with major version N-2 are considered **deprecated** immediately.
- All SDKs versions with major version N-3 will **sunset** after approximately two months.

### Exceptions

This deprecation schedule provides a framework for predictable lifetimes for an
SDK version. However, there may be exceptions in the future. This schedule does
not preclude us from sunsetting an SDK version at an earlier date, but we are
committed to providing proactive communication with ample lead time for any
future changes.
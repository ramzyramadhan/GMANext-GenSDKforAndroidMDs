Select platform: [Android](https://developers.google.com/admob/android/next-gen/mediation/choose-networks "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/choose-networks "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/choose-networks "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/choose-networks "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/choose-networks "View this page for the Android (Legacy) platform docs.")

<br />

This guide shows the ad sources
AdMob Mediation supports, in addition to
details, such as required open-source and versioned adapters, and supported ad
source optimization.

If you want to integrate an ad source that
AdMob Mediation doesn't
support, see [Custom events](https://developers.google.com/admob/android/next-gen/mediation/choose-networks#custom-events).

## Integrate open-source and versioned adapters

This section includes a picker to select your preferred open-source and
versioned adapter, and view the integration statements.

To integrate an open-sourced and versioned adapter into your app, do the
following:

1. Click the checkbox of your preferred open-sourced and versioned adapter.
2. Click **Show mediation setup**.
3. In your app, copy and paste the resulting integration statements.

*** ** * ** ***

<iframe src="https://developers.devsite.google/frame/admob/android/next-gen/mediation/choose-networks_0e665548aa9ffef55f7c99d25db17d8974c7bbfd334e61a2e2012bd00fa39e83.frame" class="framebox inherit-locale " allow="clipboard-write https://developers.devsite.google" allowfullscreen is-upgraded></iframe>

For further integration instructions, see the page for each individual ad
source in the following section.

## Network details

AdMob Mediation supports several ad sources, with a mix of bidding and waterfall mediation integrations. Select an ad source for integration instructions specific to that ad source:

<br />

| Ad Source | App Open | Banner | Interstitial | Rewarded | Rewarded Interstitial | Native | Bidding | Ad source optimization support |
| Open source and versioned - third party SDKs required |||||||||
|---|---|---|---|---|---|---|---|---|
| [AppLovin](https://developers.google.com/admob/android/next-gen/mediation/applovin) |   | Yes | Yes | Yes |   |   | Yes | Country-specific |
| [BidMachine](https://developers.google.com/admob/android/next-gen/mediation/bidmachine) |   | Yes | Yes | Yes |   | Yes | Yes | None |
| [BIGO Ads SDK](https://developers.google.com/admob/android/next-gen/mediation/bigo) | Yes | Yes | Yes | Yes |   | Yes | Yes | None |
| [Chartboost](https://developers.google.com/admob/android/next-gen/mediation/chartboost) |   | Yes | Yes | Yes |   |   |   | Country-specific |
| [DT Exchange](https://developers.google.com/admob/android/next-gen/mediation/dt-exchange) (previously Fyber Marketplace) |   | Yes | Yes | Yes |   | Yes | Yes | Country-specific |
| [i-mobile](https://developers.google.com/admob/android/next-gen/mediation/imobile) |   | Yes | Yes |   |   | Yes |   | Japan only |
| [InMobi](https://developers.google.com/admob/android/next-gen/mediation/inmobi) |   | Yes | Yes | Yes | Yes | Yes | Yes | Country-specific |
| [ironSource Ads](https://developers.google.com/admob/android/next-gen/mediation/ironsource) |   | Yes | Yes | Yes | Yes |   | Yes | Country-specific |
| [Liftoff Monetize](https://developers.google.com/admob/android/next-gen/mediation/liftoff-monetize) (previously Vungle) | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Country-specific |
| [LY Ads Network](https://developers.google.com/admob/android/next-gen/mediation/line) |   | Yes | Yes | Yes | Yes | Yes | Yes | Country-specific |
| [maio](https://developers.google.com/admob/android/next-gen/mediation/maio) |   |   | Yes | Yes |   |   |   | Japan only |
| [Meta Audience Network](https://developers.google.com/admob/android/next-gen/mediation/meta) (previously Facebook) |   | Yes | Yes [^2^](https://developers.google.com/admob/android/next-gen/mediation/choose-networks#meta-footnote) | Yes | Yes | Yes | Yes | Bidding only |
| [Mintegral](https://developers.google.com/admob/android/next-gen/mediation/mintegral) | Yes | Yes | Yes | Yes |   | Yes | Yes | Country-specific |
| [Moloco](https://developers.google.com/admob/android/next-gen/mediation/moloco) |   | Yes | Yes | Yes |   | Yes | Yes | Country-specific |
| [myTarget](https://developers.google.com/admob/android/next-gen/mediation/mytarget) |   | Yes | Yes | Yes |   | Yes |   | Country-specific |
| [Pangle](https://developers.google.com/admob/android/next-gen/mediation/pangle) | Yes | Yes | Yes | Yes |   | Yes | Yes | Country-specific |
| [PubMatic OpenWrap](https://developers.google.com/admob/android/next-gen/mediation/pubmatic) |   | Yes | Yes [^3^](https://developers.google.com/admob/android/next-gen/mediation/choose-networks#format-beta-sdkb-footnote) | Yes [^3^](https://developers.google.com/admob/android/next-gen/mediation/choose-networks#format-beta-sdkb-footnote) |   | Yes [^3^](https://developers.google.com/admob/android/next-gen/mediation/choose-networks#format-beta-sdkb-footnote) | Yes | Country-specific |
| [Unity Ads](https://developers.google.com/admob/android/next-gen/mediation/unity) |   | Yes | Yes | Yes |   |   | Yes | Country-specific |
| [Vpon](https://developers.google.com/admob/android/next-gen/mediation/vpon) |   | Yes | Yes |   |   |   |   | None |
| [Zucks](https://developers.google.com/admob/android/next-gen/mediation/zucks) |   | Yes | Yes |   |   |   |   | Country-specific |
| Ad Generation |   | Yes | Yes | Yes |   |   | Yes | Bidding only |
| Bidease |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Chocolate Platform |   | Yes | Yes | Yes |   |   | Yes | Bidding only |
| Equativ (formerly Smart Adserver) |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Fluct |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Improve Digital |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Index Exchange |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| InMobi Exchange |   | Yes | Yes | Yes |   |   | Yes | Bidding only |
| Magnite DV+ |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Media.net |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| MobFox |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Nativo |   | Yes | Yes |   |   | Yes | Yes | Bidding only |
| Nexxen (previously UnrulyX) |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| OneTag Exchange |   | Yes | Yes | Yes |   |   | Yes | Bidding only |
| OpenX |   | Yes | Yes | Yes |   |   | Yes | Bidding only |
| PubMatic |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Rise |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Sharethrough |   | Yes |   |   |   | Yes | Yes | Bidding only |
| Smaato |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Sonobi |   | Yes | Yes |   |   |   | Yes | Bidding only |
| TripleLift |   | Yes | Yes | Yes |   |   | Yes | Bidding only |
| Verve Group |   | Yes | Yes | Yes |   | Yes | Yes | Bidding only |
| Yieldmo |   | Yes | Yes | Yes |   |   | Yes | Bidding only |
| YieldOne |   | Yes | Yes | Yes |   |   | Yes | Bidding only |

^1^ Bidding integration is in beta.


^2^ Meta Audience Network
doesn't support anchored and inline adaptive banners.


^3^ This format is in beta (SDK bidding).

> [!NOTE]
> **Note:** Ad sources are ordered alphabetically.

### Open source and versioned adapters

If an adapter is labeled "Open source and versioned" in the earlier table,
it means the adapter source code is open-sourced in [Google's GitHub
repository](https://github.com/googleads/googleads-mobile-android-mediation/tree/master/ThirdPartyAdapters),
enabling you to debug issues yourself should you choose to do so.

You can also find [adapter
builds](https://maven.google.com/web/index.html#com.google.ads.mediation) that can be
integrated into your app with just a single line change to your app's
`build.gradle` file.

#### Adapter versioning

The adapter versioning scheme for versioned adapters is `<third-party
SDK version>.<adapter patch version>`. For example, if an
ad network releases a new SDK version `1.2.3`, a new adapter version `1.2.3.0`
will be released to Bintray after being tested against that new SDK.

If an adapter needs updating outside the lifecycle of a third-party SDK release,
the patch version will increase. A bug fix for adapter version `1.2.3.0` will
be released in version `1.2.3.1`.

> [!IMPORTANT]
> **Important:** Make sure to disable refresh in all third-party ad source UIs for banner ad units used in AdMob Mediation. This prevents a double refresh since AdMob also triggers a refresh based on your banner ad unit's refresh rate.

### Ad source optimization

When you configure multiple ad networks for mediation, you have to
specify what order to request these networks by setting their respective
.
This can be difficult to manage, since ad network performance changes over
time.
[Ad source optimization](https://support.google.com/admob/answer/3379794) is a feature that lets you to generate the highest CPM from the ad networks in your mediation chain by automating the process of ordering the mediation chain to maximize revenue.

<br />

The [mediation networks table](https://developers.google.com/admob/android/next-gen/mediation/choose-networks#ad-networks) earlier uses the following values
for ad source optimization support:

| Ad source optimization support | What it means |
|---|---|
| `Bidding only` | The ad network only participates in bidding. Ad source optimization is not applicable. |
| `Country-specific` | eCPM values are automatically updated on your behalf on a per-country basis. This is the optimal type of optimization. |
| `None` | You must manually configure an eCPM value for that ad network. |

Click a specific ad network's guide for details on how to configure
ad source optimization for that network.

> [!NOTE]
> **Note:** Only ad networks that are **Open source and versioned** have specific instructions for setting up ad source optimization on that network.

## Custom events

If you're looking for an ad network and don't see it on the list earlier, you
can use custom events to write your own integration with that ad network. See
the [custom events guide](https://developers.google.com/admob/android/next-gen/custom-events/setup) for more details on how
to create a custom event.

> [!NOTE]
> **Note:** Custom events don't have ad source optimization or bidding support.

*[CPM]: cost per thousand impressions
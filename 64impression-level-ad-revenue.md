Select platform: [Android](https://developers.google.com/admob/android/next-gen/impression-level-ad-revenue "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/impression-level-ad-revenue "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/impression-level-ad-revenue "View this page for the Unity platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/impression-level-ad-revenue "View this page for the Android (Legacy) platform docs.")

<br />

When an impression occurs, GMA Next-Gen SDK provides ad revenue data
associated with that impression. You can use the data to calculate a user's
lifetime value, or forward the data downstream to other relevant systems.

This guide is intended to help you implement the impression-level ad revenue
data capture in your Android app.

## Prerequisites

- Make sure you have [turned on the impression-level ad revenue feature](https://support.google.com/admob/answer/11322405) in the AdMob UI.

<!-- -->

- [Set up GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start).
- Before you can receive any impression-level ad revenue, you need to implement
  at least one ad format:

  - [App open](https://developers.google.com/admob/android/next-gen/app-open)
  - [Banner](https://developers.google.com/admob/android/next-gen/banner)
  - [Interstitial](https://developers.google.com/admob/android/next-gen/interstitial)
  - [Rewarded](https://developers.google.com/admob/android/next-gen/rewarded)
  - [Rewarded interstitial](https://developers.google.com/admob/android/next-gen/rewarded-interstitial)
  - [Native](https://developers.google.com/admob/android/next-gen/native)

## Paid event handler

Each ad format has an

[`onAdPaid`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/AdEventCallback#onAdPaid(com.google.android.libraries.ads.mobile.sdk.common.AdValue))
event callback.

During the lifecycle of an ad event, GMA Next-Gen SDK monitors
impression events and invokes the handler with an earned value.

The following example handles paid events for a rewarded ad:

### Kotlin

    ad.adEventCallback =
      object : RewardedAdEventCallback {
        override fun onAdPaid(adValue: AdValue) {
          // Send the impression-level ad revenue information to your
          // preferred analytics server directly within this callback.

          // Extract the impression-level ad revenue data.
          val valueMicros = adValue.valueMicros
          val currencyCode = adValue.currencyCode
          val precisionType = adValue.precisionType

          val loadedAdSourceResponseInfo = ad.getResponseInfo().loadedAdSourceResponseInfo
          val adSourceName = loadedAdSourceResponseInfo?.name
          val adSourceId = loadedAdSourceResponseInfo?.id
          val adSourceInstanceName = loadedAdSourceResponseInfo?.instanceName
          val adSourceInstanceId = loadedAdSourceResponseInfo?.instanceId
          val extras = ad.getResponseInfo().responseExtras
          val mediationGroupName = extras.getString("mediation_group_name")
          val mediationABTestName = extras.getString("mediation_ab_test_name")
          val mediationABTestVariant = extras.getString("mediation_ab_test_variant")
        }
      }

### Java

    ad.setAdEventCallback(
        new RewardedAdEventCallback() {
          @Override
          public void onAdPaid(@NonNull AdValue value) {
            // Send the impression-level ad revenue information to your preferred
            // analytics server directly within this callback.

            // Extract the impression-level ad revenue data.
            long valueMicros = value.getValueMicros();
            String currencyCode = value.getCurrencyCode();
            PrecisionType precisionType = value.getPrecisionType();

            AdSourceResponseInfo loadedAdSourceResponseInfo =
                ad.getResponseInfo().getLoadedAdSourceResponseInfo();
            String adSourceName = loadedAdSourceResponseInfo.getName();
            String adSourceId = loadedAdSourceResponseInfo.getId();
            String adSourceInstanceName = loadedAdSourceResponseInfo.getInstanceName();
            String adSourceInstanceId = loadedAdSourceResponseInfo.getInstanceId();

            Bundle extras = ad.getResponseInfo().getResponseExtras();
            String mediationGroupName = extras.getString("mediation_group_name");
            String mediationABTestName = extras.getString("mediation_ab_test_name");
            String mediationABTestVariant = extras.getString("mediation_ab_test_variant");
          }
        });

### Identify a custom event ad source name

For custom event ad sources, the `AdSourceResponseInfo.name` property
returns the ad source name `Custom event`. If you use multiple custom
events, the ad source name isn't granular enough to distinguish between multiple
custom events. To locate a specific custom event, do:

1. Get the`AdSourceResponseInfo.name` property.
2. Set a unique ad source name.

The following example sets a unique ad source name for a custom event:

### Kotlin

    private fun getUniqueAdSourceName(loadedAdapterResponseInfo: AdSourceResponseInfo): String {
      var adSourceName = loadedAdapterResponseInfo.name
      if (adSourceName == "Custom Event") {
        if (
          loadedAdapterResponseInfo.adapterClassName ==
            "com.google.ads.mediation.sample.customevent.SampleCustomEvent"
        ) {
          adSourceName = "Sample Ad Network (Custom Event)"
        }
      }
      return adSourceName
    }

### Java

    private String getUniqueAdSourceName(@NonNull AdSourceResponseInfo loadedAdapterResponseInfo) {
      String adSourceName = loadedAdapterResponseInfo.getName();
      if (adSourceName.equals("Custom Event")) {
        if (loadedAdapterResponseInfo
            .getAdapterClassName()
            .equals("com.google.ads.mediation.sample.customevent.SampleCustomEvent")) {
          adSourceName = "Sample Ad Network (Custom Event)";
        }
      }
      return adSourceName;
    }

For more information on the winning ad source, see [Retrieve information about
the ad response](https://developers.google.com/admob/android/next-gen/response-info).

### App Attribution Partners (AAP) integration

For complete details on forwarding ads revenue data to analytics platforms,
refer to the partner's guide:

| Partner SDK |
|---|
| [Adjust](https://dev.adjust.com/en/sdk/android/integrations/admob) |
| [AppsFlyer](https://dev.appsflyer.com/hc/docs/ad-revenue-1) |
| [Singular](https://support.singular.net/hc/en-us/articles/360037635452-Unity-SDK-Integration-Guide#42_Adding_Ad_Revenue_Attribution_Support) |
| [Tenjin](https://tenjin.com/docs/android-admob/) |

The partner's guide may not be updated to reference GMA Next-Gen SDK.

The following examples show how to send impression level revenue data to AAP
partners using GMA Next-Gen SDK:

### Adjust

### Java

    rewardedAd.setAdEventCallback(
        new RewardedAdEventCallback() {
          @Override
          public void onAdPaid(@NonNull AdValue value) {
            // Send ad revenue info to Adjust.
            AdjustAdRevenue adRevenue = new AdjustAdRevenue("admob_sdk");
            adRevenue.setRevenue(value.getValueMicros() / 1000000.0, value.getCurrencyCode());
            if (rewardedAd.getResponseInfo().getLoadedAdSourceResponseInfo() != null) {
              adRevenue.setAdRevenueNetwork(
                  rewardedAd.getResponseInfo().getLoadedAdSourceResponseInfo().getName());
            }
            Adjust.trackAdRevenue(adRevenue);
          }
        });

### Kotlin

    rewardedAd.adEventCallback =
      object : RewardedAdEventCallback {
        override fun onAdPaid(value: AdValue) {
          // Send ad revenue info to Adjust.
          val adRevenue = AdjustAdRevenue("admob_sdk")
          adRevenue.setRevenue(value.valueMicros / 1000000.0, value.currencyCode)
          val loadedAdSourceResponseInfo = rewardedAd.getResponseInfo().loadedAdSourceResponseInfo
          loadedAdSourceResponseInfo?.let { adRevenue.setAdRevenueNetwork(it.name) }
          Adjust.trackAdRevenue(adRevenue)
        }
      }

### AppsFlyer

### Java

    rewardedAd.setAdEventCallback(
        new RewardedAdEventCallback() {
          @Override
          public void onAdPaid(@NonNull AdValue value) {
            long valueMicros = value.getValueMicros();
            String currencyCode = value.getCurrencyCode();

            AFAdRevenueData adRevenueData =
                new AFAdRevenueData(
                    "AdMob Mediation", // monetizationNetwork
                    MediationNetwork.GOOGLE_ADMOB, // mediationNetwork
                    currencyCode, // currencyIso4217Code
                    (double) valueMicros // revenue
                    );

            Map<String, Object> additionalParameters = new HashMap<>();
            additionalParameters.put(COUNTRY, "US");
            additionalParameters.put(AD_UNIT, AD_UNIT_ID);
            additionalParameters.put(AD_TYPE, AdFormat.REWARDED);

            AppsFlyerLib.getInstance().logAdRevenue(adRevenueData, additionalParameters);
          }
        });

### Kotlin

    rewardedAd.adEventCallback =
      object : RewardedAdEventCallback {
        override fun onAdPaid(value: AdValue) {
          val valueMicros = value.valueMicros
          val currencyCode = value.currencyCode

          val adRevenueData =
            AFAdRevenueData(
              "AdMob Mediation", // monetizationNetwork
              MediationNetwork.GOOGLE_ADMOB, // mediationNetwork
              currencyCode, // currencyIso4217Code
              valueMicros.toDouble(), // revenue
            )

          val additionalParameters: MutableMap<String?, Any?> = HashMap()
          additionalParameters[COUNTRY] = "US"
          additionalParameters[AD_UNIT] = AD_UNIT_ID
          additionalParameters[AD_TYPE] = AdFormat.REWARDED

          appsflyer.logAdRevenue(adRevenueData, additionalParameters)
        }
      }

### Singular

### Java

    rewardedAd.setAdEventCallback(
        new RewardedAdEventCallback() {
          @Override
          public void onAdPaid(@NonNull AdValue value) {
            // Convert revenue from micros to standard units.
            double revenue = value.getValueMicros() / 1000000.0;
            String currency = value.getCurrencyCode();

            // Validate ad revenue data before sending.
            if (revenue > 0 && !currency.isEmpty()) {
              SingularAdData adData = new SingularAdData("AdMob", currency, revenue);
              Singular.adRevenue(adData);
            }
          }
        });

### Kotlin

    rewardedAd.adEventCallback =
      object : RewardedAdEventCallback {
        override fun onAdPaid(value: AdValue) {
          // Convert revenue from micros to standard units.
          val revenue = value.valueMicros / 1000000.0
          val currency = value.currencyCode

          // Validate ad revenue data before sending.
          if (revenue > 0 && currency.isNotEmpty()) {
            val adData = SingularAdData("AdMob", currency, revenue)
            Singular.adRevenue(adData)
          }
        }
      }

### Tenjin

For GMA Next-Gen SDK, you must integrate using a JSON object. The following
example sends impression level revenue data to Tenjin using a JSON object:

### Java

    rewardedAd.setAdEventCallback(
        new RewardedAdEventCallback() {
          @Override
          public void onAdPaid(@NonNull AdValue value) {
            ResponseInfo responseInfo = rewardedAd.getResponseInfo();

            // Extract the impression-level ad revenue data.
            long valueMicros = value.getValueMicros();
            String currencyCode = value.getCurrencyCode();
            PrecisionType precisionType = value.getPrecisionType();

            JSONObject json = new JSONObject();
            try {
              json.put("ad_unit_id", AD_UNIT_ID);
              json.put("currency_code", currencyCode);
              json.put("response_id", responseInfo.getResponseId());
              json.put("value_micros", valueMicros);
              if (responseInfo.getLoadedAdSourceResponseInfo() != null) {
                json.put(
                    "mediation_adapter_class_name",
                    responseInfo.getLoadedAdSourceResponseInfo().getAdapterClassName());
              }
              json.put("precision_type", precisionType);

              tenjinInstance.eventAdImpressionAdMob(json);
            } catch (JSONException e) {
              // Handle error.
            }
          }
        });

### Kotlin

    rewardedAd.adEventCallback =
      object : RewardedAdEventCallback {
        override fun onAdPaid(value: AdValue) {
          val responseInfo = rewardedAd.getResponseInfo()

          // Extract the impression-level ad revenue data.
          val valueMicros = value.valueMicros
          val currencyCode = value.currencyCode
          val precisionType = value.precisionType

          val json = JSONObject()
          try {
            json.put("ad_unit_id", AD_UNIT_ID)
            json.put("currency_code", currencyCode)
            json.put("response_id", responseInfo.responseId)
            json.put("value_micros", valueMicros)
            responseInfo.loadedAdSourceResponseInfo?.let {
              json.put("mediation_adapter_class_name", it.adapterClassName)
            }
            json.put("precision_type", precisionType)

            tenjinInstance.eventAdImpressionAdMob(json)
          } catch (_: JSONException) {
            // Handle error.
          }
        }
      }

### Implementation best practices

- Set the listener immediately once you create or get access to the ad object, and definitely before showing the ad. This makes sure that you don't miss any paid event callbacks.
- Send the impression-level ad revenue information to your preferred analytics server immediately at the time the paid event callback is called. This makes sure that you don't accidentally drop any callbacks and avoids data discrepancies.

## AdValue

`AdValue` is a class that represents the monetary value earned for an ad,
including the value's currency code and its precision type encoded as follows.

> [!IMPORTANT]
> **Key Point:** [`getValueMicros()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/AdValue#getValueMicros()) returns the value of the ad in micro units. For example, a `getValueMicros()` returned value of 5,000 means the ad is estimated to be worth $0.005.

| PrecisionType | Description |
|---|---|
| `UNKNOWN` | An ad value that's unknown. This gets returned when LTV pingback is enabled but there isn't enough data available. |
| `ESTIMATED` | An ad value estimated from aggregated data. |
| `PUBLISHER_PROVIDED` | A publisher provided ad value, such as manual CPMs in a mediation group. |
| `PRECISE` | The precise value paid for this ad. |

In the case of AdMob Mediation, AdMob tries to
provide an `ESTIMATED` value for ad sources that

are [optimized](https://support.google.com/admob/answer/7374110).

For non-optimized ad sources, or in cases where there aren't enough aggregated
data to report a meaningful estimation, the `PUBLISHER_PROVIDED` value is
returned.

### Test impressions from bidding ad sources

After an impression-level ad revenue event occurs for
a bidding ad source through a test request, you receive only the following
values:

- `UNKNOWN`: indicates the precision type.

<!-- -->

- `0`: indicates the ad value.

Previously, you might have seen the precision type as a value other than
`UNKNOWN` and an ad value more than `0`.

For details on sending a test ad request, see
[Enable test devices](https://developers.google.com/admob/android/next-gen/test-ads#enable-test-devices).
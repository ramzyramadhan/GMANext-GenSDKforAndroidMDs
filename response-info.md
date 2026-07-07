Select platform: [Android](https://developers.google.com/admob/android/next-gen/response-info "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/response-info "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/response-info "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/response-info "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/response-info "View this page for the Android (Legacy) platform docs.")

<br />

For debugging and logging purposes, successfully loaded ads provide a
[`ResponseInfo`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/ResponseInfo)
object. This object contains information about the ad it loaded, in addition to
information about the mediation waterfall used to load the ad.

For cases where an ad loads successfully, the ad object has a

[`getResponseInfo()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/Ad#getResponseInfo())

method. For example,

`InterstitialAd.getResponseInfo()`

gets the response info for a loaded interstitial ad.

For cases where ads fail to load and only an error is available, the
response info is available through
[`LoadAdError.getResponseInfo()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/LoadAdError#getResponseInfo()).

### Kotlin

    override fun onAdLoaded(interstitialAd: InterstitialAd)) {
      val responseInfo = interstitialAd.responseInfo
      Log.d(TAG, responseInfo.toString())
    }

    override fun onAdFailedToLoad(adError: LoadAdError) {
      val responseInfo = adError.responseInfo
      Log.d(TAG, responseInfo.toString())
    }

### Java

    @Override
    public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
      ResponseInfo responseInfo = interstitialAd.getResponseInfo();
      Log.d(TAG, responseInfo.toString());
    }

    @Override
    public void onAdFailedToLoad(LoadAdError loadAdError) {
      ResponseInfo responseInfo = loadAdError.getResponseInfo();
      Log.d(TAG, responseInfo.toString());
    }

## Response Info

Here is sample output returned by `ResponseInfo.toString()` showing the
debugging data returned for a loaded ad:

    {
      "Response ID": "COOllLGxlPoCFdAx4Aod-Q4A0g",
      "Mediation Adapter Class Name": "com.google.ads.mediation.admob.AdMobAdapter",
      "Adapter Responses": [
        {
          "Adapter": "com.google.ads.mediation.admob.AdMobAdapter",
          "Latency": 328,
          "Ad Source Name": "Reservation campaign",
          "Ad Source ID": "7068401028668408324",
          "Ad Source Instance Name": "[DO NOT EDIT] Publisher Test Interstitial",
          "Ad Source Instance ID": "4665218928925097",
          "Credentials": {},
          "Ad Error": "null"
        }
      ],
      "Loaded Adapter Response": {
        "Adapter": "com.google.ads.mediation.admob.AdMobAdapter",
        "Latency": 328,
        "Ad Source Name": "Reservation campaign",
        "Ad Source ID": "7068401028668408324",
        "Ad Source Instance Name": "[DO NOT EDIT] Publisher Test Interstitial",
        "Ad Source Instance ID": "4665218928925097",
        "Credentials": {},
        "Ad Error": "null"
      },
      "Response Extras": {
        "mediation_group_name": "Campaign"
      }
    }

Methods on the `ResponseInfo` object include the following:

| Method | Description |
|---|---|
| ` getAdSourceResponses` | Returns the list of `https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/AdSourceResponseInfo` containing metadata for each ad source included in the ad response. Can be used to debug the waterfall mediation and bidding execution. The order of the list matches the order of the mediation waterfall for this ad request. See [Ad source response info](https://developers.google.com/admob/android/next-gen/response-info#ad_source_response_info) for more information. |
| ` getLoadedAdSourceResponse` | Returns the `AdSourceResponseInfo` corresponding to the ad source that loaded the ad. |
| `getAdapterClassName ` | Returns the mediation adapter class name of the ad source that loaded the ad. |
| `getResponseId` | The response identifier is a unique identifier for the ad response. This identifier can be used to identify and block the ad in the [Ad Review Center (ARC)](https://support.google.com/admob/answer/3480906). |
| `getResponseExtras` | Returns extra information about the ad response. Extras may return the following keys: - `mediation_group_name`: The name of the mediation group - `mediation_ab_test_name`: The name of the [mediation A/B test](https://support.google.com/admob/answer/9572326), if applicable - `mediation_ab_test_variant`: The variant used in the mediation A/B test, if applicable |

### Kotlin

    override fun onAdLoaded(interstitialAd: InterstitialAd) {
      val responseInfo = interstitialAd.responseInfo

      val responseId = responseInfo.responseId
      val adapterClassName = responseInfo.adapterClassName
      val adSourceResponses = responseInfo.adSourceResponses
      val loadedAdSourceResponse = responseInfo.loadedAdSourceResponse
      val extras = responseInfo.responseExtras
      val mediationGroupName = extras.getString("mediation_group_name")
      val mediationABTestName = extras.getString("mediation_ab_test_name")
      val mediationABTestVariant = extras.getString("mediation_ab_test_variant")
    }

### Java

    @Override
    public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
      MyActivity.this.interstitialAd = interstitialAd;

      ResponseInfo responseInfo = interstitialAd.getResponseInfo();
      String responseId = responseInfo.getResponseId();
      String adapterClassName = responseInfo.getAdapterClassName();
      List<AdSourceResponseInfo> adSourceResponses = responseInfo.getAdSourceResponses();
      AdSourceResponseInfo loadedAdSourceResponse = responseInfo.getLoadedAdSourceResponse();
      Bundle extras = responseInfo.getResponseExtras();
      String mediationGroupName = extras.getString("mediation_group_name");
      String mediationABTestName = extras.getString("mediation_ab_test_name");
      String mediationABTestVariant = extras.getString("mediation_ab_test_variant");
    }

## Ad source response info

[`AdSourceResponseInfo`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/AdSourceResponseInfo)
contains response information for an individual ad source in an ad response.

The following sample `AdSourceResponseInfo` output shows the metadata
for a loaded ad:

    {
      "Adapter": "com.google.ads.mediation.admob.AdMobAdapter",
      "Latency": 328,
      "Ad Source Name": "Reservation campaign",
      "Ad Source ID": "7068401028668408324",
      "Ad Source Instance Name": "[DO NOT EDIT] Publisher Test Interstitial",
      "Ad Source Instance ID": "4665218928925097",
      "Credentials": {},
      "Ad Error": "null"
    }

For each ad source, `AdSourceResponseInfo` provides the following
methods:

| Method | Description |
|---|---|
| `getAdError` | Gets the error associated with the request to the ad source. Returns `null` if the ad source successfully loaded an ad or if the ad source was not attempted. |
| `getId` | Gets the ad source ID associated with this ad source response. For campaigns, `6060308706800320801` is returned for a mediated ads [campaign goal type](https://support.google.com/admob/answer/9152820), and `7068401028668408324` is returned for impression and click goal types. See [Ad sources](https://developers.google.com/admob/api/v1/ad-sources-reference) for the list of possible ad source IDs when an ad source serves the ad. |
| `getInstanceId` | Gets the ad source instance ID associated with this adapter response. |
| `getInstanceName` | Gets the ad source instance name associated with this adapter response. |
| `getName` | Gets the ad source name associated with this adapter response. For campaigns, `Mediated House Ads` is returned for a mediated ads [campaign goal type](https://support.google.com/admob/answer/9152820), and `Reservation Campaign` is returned for impression and click goal types. See [Ad sources](https://developers.google.com/admob/api/v1/ad-sources-reference) for the list of possible ad source names when an ad source serves the ad. |
| `getAdapterClassName` | Gets the class name of the ad source adapter that loaded the ad. |
| `getCredentials` | Gets the ad source adapter credentials specified in the AdMob UI. |
| `getLatencyMillis` | Gets the amount of time the ad source adapter spent loading an ad. Returns `0` if the ad source was not attempted. |

### Kotlin

    override fun onAdLoaded(interstitialAd: InterstitialAds) {
      val loadedAdSourceResponseInfo = interstitialAd.responseInfo.loadedAdSourceResponse

      val adError = loadedAdSourceResponseInfo.adError
      val adSourceId = loadedAdSourceResponseInfo.id
      val adSourceInstanceId = loadedAdSourceResponseInfo.instanceId
      val adSourceInstanceName = loadedAdSourceResponseInfo.instanceName
      val adSourceName = loadedAdSourceResponseInfo.name
      val adapterClassName = loadedAdSourceResponseInfo.adapterClassName
      val credentials = loadedAdSourceResponseInfo.credentials
      val latencyMillis = loadedAdSourceResponseInfo.latencyMillis
    }

### Java

    @Override
    public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
      AdSourceResponseInfo loadedAdSourceResponseInfo =
          interstitialAd.getResponseInfo().getLoadedAdSourceResponse();

      AdError adError = loadedAdSourceResponseInfo.getAdError();
      String adSourceId = loadedAdSourceResponseInfo.getId();
      String adSourceInstanceId = loadedAdSourceResponseInfo.getInstanceId();
      String adSourceInstanceName = loadedAdSourceResponseInfo.getInstanceName();
      String adSourceName = loadedAdSourceResponseInfo.getName();
      String adapterClassName = loadedAdSourceResponseInfo.getAdapterClassName();
      Bundle credentials = loadedAdSourceResponseInfo.getCredentials();
      long latencyMillis = loadedAdSourceResponseInfo.getLatencyMillis();
    }
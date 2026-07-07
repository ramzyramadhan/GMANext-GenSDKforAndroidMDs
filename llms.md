### Start Rewarded Ad Preloading (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/rewarded.md

Starts the preloading process for rewarded ads in Java using `RewardedAdPreloader.start`. This method prepares ads in advance for a smoother user experience. It requires an `AdRequest` and a `PreloadConfiguration`.

```java
private void startPreloading(String adUnitId) {
  AdRequest adRequest = new AdRequest.Builder(adUnitId).build();
  PreloadConfiguration preloadConfig = new PreloadConfiguration(adRequest);
  RewardedAdPreloader.start(adUnitId, preloadConfig);
}
```

--------------------------------

### Send Ad Revenue to AppsFlyer (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/impression-level-ad-revenue.md

This Kotlin example demonstrates sending ad revenue to AppsFlyer. Ensure the AppsFlyer SDK is initialized.

```Kotlin
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
```

--------------------------------

### Initialize SDK with Targeting (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/targeting.md

Initialize the GMA Next-Gen SDK on a background thread, applying specific targeting configurations like child-directed treatment from the start.

```java
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
```

--------------------------------

### Start Rewarded Interstitial Ad Preloading

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/rewarded-interstitial.md

Initiate ad preloading by creating a preload configuration with an ad request and then starting the preloader with the ad unit ID and configuration. This API eliminates the need for manual ad loading.

```kotlin
private fun startPreloading(adUnitId: String) {
  val adRequest = AdRequest.Builder(adUnitId).build()
  val preloadConfig = PreloadConfiguration(adRequest)
  RewardedInterstitialAdPreloader.start(adUnitId, preloadConfig)
}
```

```java
private void startPreloading(String adUnitId) {
  AdRequest adRequest = new AdRequest.Builder(adUnitId).build();
  PreloadConfiguration preloadConfig = new PreloadConfiguration(adRequest);
  RewardedInterstitialAdPreloader.start(adUnitId, preloadConfig);
}
```

--------------------------------

### Initialize SDK with Targeting (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/targeting.md

Initialize the GMA Next-Gen SDK on a background thread, applying specific targeting configurations like child-directed treatment from the start.

```kotlin
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
```

--------------------------------

### Ad Source Response Metadata Example

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/response-info.md

This JSON output shows example metadata for a loaded ad from an ad source.

```json
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
```

--------------------------------

### Start Rewarded Ad Preloading (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/rewarded.md

Initiates the preloading of rewarded ads using the `RewardedAdPreloader.start` method. This is useful for ensuring ads are ready when needed, reducing latency. Requires an `AdRequest` and `PreloadConfiguration`.

```kotlin
private fun startPreloading(adUnitId: String) {
  val adRequest = AdRequest.Builder(adUnitId).build()
  val preloadConfig = PreloadConfiguration(adRequest)
  RewardedAdPreloader.start(adUnitId, preloadConfig)
}
```

--------------------------------

### Set Video to Start Unmuted (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/options.md

Use this snippet to configure video ads to start with audio unmuted in Java. This is useful when you want the user to hear the video immediately.

```java
VideoOptions videoOptions = VideoOptions.Builder()
  .setStartMuted(false)
  .build()

NativeAdRequest adRequest = new NativeAdRequest.Builder(
  "ca-app-pub-3940256099942544/2247696110",
  List.of(NativeAd.NativeAdType.NATIVE))
  .setVideoOptions(videoOptions)
  .build()
```

--------------------------------

### Set Video to Start Unmuted (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/options.md

Use this snippet to configure video ads to start with audio unmuted. This is useful when you want the user to hear the video immediately.

```kotlin
val videoOptions = VideoOptions.Builder()
  .setStartMuted(false)
  .build()

val adRequest = NativeAdRequest.Builder(
  "ca-app-pub-3940256099942544/2247696110",
  listOf(NativeAd.NativeAdType.NATIVE))
  .setVideoOptions(videoOptions)
  .build()
```

--------------------------------

### Start Interstitial Ad Preloading (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/interstitial.md

Initiates the preloading of interstitial ads using the `InterstitialAdPreloader.start()` method. This API continuously makes ads available, eliminating the need for manual loading.

```kotlin
private fun startPreloading(adUnitId: String) {
  val adRequest = AdRequest.Builder(adUnitId).build()
  val preloadConfig = PreloadConfiguration(adRequest)
  InterstitialAdPreloader.start(adUnitId, preloadConfig)
}
```

--------------------------------

### Start Interstitial Ad Preloading (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/interstitial.md

Initiates the preloading of interstitial ads using the `InterstitialAdPreloader.start()` method in Java. This API continuously makes ads available, eliminating the need for manual loading.

```java
private void startPreloading(String adUnitId) {
  AdRequest adRequest = new AdRequest.Builder(adUnitId).build();
  PreloadConfiguration preloadConfig = new PreloadConfiguration(adRequest);
  InterstitialAdPreloader.start(adUnitId, preloadConfig);
}
```

--------------------------------

### Get Consent Information

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/privacy.md

Initializes the ConsentInformation instance to retrieve user consent details. This should be done at every app launch.

```APIDOC
## Get Consent Information

### Description
Initializes the `ConsentInformation` instance to retrieve user consent details. This should be done at every app launch.

### Method
`UserMessagingPlatform.getConsentInformation(context)`

### Parameters
#### Path Parameters
None

#### Query Parameters
None

#### Request Body
None

### Request Example
```java
// Java
consentInformation = UserMessagingPlatform.getConsentInformation(context);
```

```kotlin
// Kotlin
consentInformation = UserMessagingPlatform.getConsentInformation(context)
```

### Response
#### Success Response
Returns an initialized `ConsentInformation` object.

#### Response Example
None
```

--------------------------------

### Load with the ad preloading API

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/interstitial.md

This section explains how to use the ad preloading API to continuously make ads available, eliminating the need for manual ad loading. It covers starting the preloader and polling for ads.

```APIDOC
## Load with the ad preloading API

### Description
Preloads interstitial ads to ensure they are available when needed.

### Start Preloading

1. Initialize a preload configuration with an ad request.
2. Start the preloader for interstitial ads with your ad unit ID and preload configuration.

#### Request Example (Kotlin)
```kotlin
private fun startPreloading(adUnitId: String) {
  val adRequest = AdRequest.Builder(adUnitId).build()
  val preloadConfig = PreloadConfiguration(adRequest)
  InterstitialAdPreloader.start(adUnitId, preloadConfig)
}
```

#### Request Example (Java)
```java
private void startPreloading(String adUnitId) {
  AdRequest adRequest = new AdRequest.Builder(adUnitId).build();
  PreloadConfiguration preloadConfig = new PreloadConfiguration(adRequest);
  InterstitialAdPreloader.start(adUnitId, preloadConfig);
}
```

### Poll for Ads

Ads are continuously made available as you show them. The following example polls for an ad from the preloader.

#### Request Example (Kotlin)
```kotlin
// Polling returns the next available ad and loads another ad in the background.
val ad = InterstitialAdPreloader.pollAd(adUnitId)
```

#### Request Example (Java)
```java
// Polling returns the next available ad and loads another ad in the background.
final InterstitialAd ad = InterstitialAdPreloader.pollAd(adUnitId);
```
```

--------------------------------

### Configure and Load URL in WebView (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/webview.md

This Java snippet demonstrates how to initialize a WebView, configure settings for cookies, JavaScript, DOM storage, and media playback, and load a network URL. Ensure the WebView is properly initialized and its ID matches the layout.

```java
import android.webkit.CookieManager;
import android.webkit.WebView;

public class MainActivity extends AppCompatActivity {
  private WebView webView;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    webView = findViewById(R.id.webview);

    // Let the web view accept third-party cookies.
    CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true);
    // Let the web view use JavaScript.
    webView.getSettings().setJavaScriptEnabled(true);
    // Let the web view access local storage.
    webView.getSettings().setDomStorageEnabled(true);
    // Let HTML videos play automatically.
    webView.getSettings().setMediaPlaybackRequiresUserGesture(false);

    // Load the URL for optimized web view performance.
    webView.loadUrl("https://google.github.io/webview-ads/test/");
  }
}
```

--------------------------------

### Configure and Load URL in WebView (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/webview.md

This Kotlin snippet shows how to initialize a WebView, set configurations for cookies, JavaScript, DOM storage, and media playback, and then load a network URL. Verify that the WebView is correctly instantiated and linked to its ID in the layout.

```kotlin
import android.webkit.CookieManager
import android.webkit.WebView

class MainActivity : AppCompatActivity() {
  lateinit var webView: WebView

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    webView = findViewById(R.id.webview)

    // Let the web view accept third-party cookies.
    CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true)
    // Let the web view use JavaScript.
    webView.settings.javaScriptEnabled = true
    // Let the web view access local storage.
    webView.settings.domStorageEnabled = true
    // Let HTML videos play automatically.
    webView.settings.mediaPlaybackRequiresUserGesture = false

    // Load the URL for optimized web view performance.
    webView.loadUrl("https://google.github.io/webview-ads/test/")
  }
}
```

--------------------------------

### Install All Agent Skills for GMA Next-Gen SDK

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/agent-skills.md

Use this command to install all available agent skills for the GMA Next-Gen SDK. The `--agent universal` flag ensures skills are placed in the standard agent skills directory.

```bash
npx skills add google/skills/ads/google-mobile-ads --skill '*' --agent universal --yes
```

--------------------------------

### Install GMA Next-Gen SDK Migration Skill

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-with-ai-tools.md

Install the Google Mobile Ads migration skill into your project's standard agent skills folder. This command ensures your AI tool has the necessary context for migration tasks.

```bash
npx skills add google/skills/skills/ads/google-mobile-ads --skill google-mobile-ads-android-migrate-to-next-gen --agent universal --yes
```

--------------------------------

### Initialize GMA Next-Gen SDK (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migration.md

Initializes the GMA Next-Gen SDK on a background thread with a sample app ID using Kotlin. Must be called before loading ads.

```kotlin
import com.google.android.libraries.ads.mobile.sdk.MobileAds
import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val backgroundScope = CoroutineScope(Dispatchers.IO)
        backgroundScope.launch {
            // Initialize GMA Next-Gen SDK on a background thread.
            MobileAds.initialize(
                this@MainActivity,
                // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
                InitializationConfig.Builder("SAMPLE_APP_ID").build()
            ) {
                // Adapter initialization is complete.
            }
            // SDK initialization is complete. If you don't want to wait for bidding adapters to finish
            // initializing, start loading ads now.
        }
    }
}
```

--------------------------------

### Get Response Info on Ad Failure (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/response-info.md

Retrieve the ResponseInfo object when an ad fails to load. This helps in diagnosing the cause of the failure.

```kotlin
override fun onAdFailedToLoad(adError: LoadAdError) {
  val responseInfo = adError.responseInfo
  Log.d(TAG, responseInfo.toString())
}
```

--------------------------------

### Java MyApplication Initialization

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/app-open.md

Initializes the AppOpenAdManager and the GMA Next-Gen SDK within the MyApplication class. The SDK initialization is performed on a background thread to avoid blocking the main thread.

```java
public class MyApplication extends Application {

  private AppOpenAdManager appOpenAdManager;

  @Override
  public void onCreate() {
    super.onCreate();
    new Thread(
            () -> {
              // Initialize GMA Next-Gen SDK on a background thread.
              MobileAds.initialize(this, initializationStatus -> {});
            }
        )
        .start();
    appOpenAdManager = new AppOpenAdManager(this);
  }
}
```

--------------------------------

### Report Adapter and SDK Versions in Java

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/setup.md

Implement getVersionInfo() and getSDKVersionInfo() to report adapter and SDK versions respectively. Ensure versions are parsed correctly from strings.

```Java
package com.google.ads.mediation.sample.customevent;

public class SampleCustomEvent extends Adapter {

  @Override
  public VersionInfo getVersionInfo() {
    String versionString = new VersionInfo(1, 2, 3).toString();
    String[] splits = versionString.split("\\.");

    if (splits.length >= 4) {
      int major = Integer.parseInt(splits[0]);
      int minor = Integer.parseInt(splits[1]);
      int micro = Integer.parseInt(splits[2]) * 100 + Integer.parseInt(splits[3]);
      return new VersionInfo(major, minor, micro);
    }

    return new VersionInfo(0, 0, 0);
  }

  @Override
  public VersionInfo getSDKVersionInfo() {
    String versionString = SampleAdRequest.getSDKVersion();
    String[] splits = versionString.split("\\.");

    if (splits.length >= 3) {
      int major = Integer.parseInt(splits[0]);
      int minor = Integer.parseInt(splits[1]);
      int micro = Integer.parseInt(splits[2]);
      return new VersionInfo(major, minor, micro);
    }

    return new VersionInfo(0, 0, 0);
  }
}
```

--------------------------------

### Get Response Info on Ad Load (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/response-info.md

Retrieve the ResponseInfo object after a successful ad load. This is useful for debugging and logging ad details.

```kotlin
override fun onAdLoaded(interstitialAd: InterstitialAd)) {
  val responseInfo = interstitialAd.responseInfo
  Log.d(TAG, responseInfo.toString())
}
```

--------------------------------

### Kotlin MyApplication Initialization

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/app-open.md

Initializes the AppOpenAdManager and the GMA Next-Gen SDK within the MyApplication class. The SDK initialization is performed on a background thread using a CoroutineScope.

```kotlin
class MyApplication : Application() {

  private lateinit var appOpenAdManager: AppOpenAdManager

  override fun onCreate() {
    super.onCreate()
    val backgroundScope = CoroutineScope(Dispatchers.IO)
    backgroundScope.launch {
      // Initialize GMA Next-Gen SDK on a background thread.
      MobileAds.initialize(this@MyApplication) {}
    }
    appOpenAdManager = AppOpenAdManager()
  }
}
```

--------------------------------

### Get Mediation Adapter Class Name

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/test-ads.md

Use this method to determine which ad network served the current ad. This is useful for debugging mediation configurations.

```kotlin
responseInfo.mediationAdapterClassName
```

--------------------------------

### Initialize GMA Next-Gen SDK (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migration.md

Initializes the GMA Next-Gen SDK on a background thread with a sample app ID using Java. Must be called before loading ads.

```java
import com.google.android.libraries.ads.mobile.sdk.MobileAds;
import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig;

public class MainActivity extends AppCompatActivity {
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        new Thread( () -> {
            // Initialize GMA Next-Gen SDK on a background thread.
            MobileAds.initialize(
                this,
                // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
                new InitializationConfig.Builder("SAMPLE_APP_ID")
                    .build(),
                initializationStatus -> {
                    // Adapter initialization is complete.
                });
            // SDK initialization is complete. If you don't want to wait for bidding adapters to
            // finish initializing, start loading ads now.
        }) .start();
    }
}
```

--------------------------------

### Initialize Legacy Google Mobile Ads SDK (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migration.md

Initializes the Google Mobile Ads SDK on a background thread using Kotlin. Recommended for reducing ANRs.

```kotlin
import com.google.android.gms.ads.MobileAds
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        val backgroundScope = CoroutineScope(Dispatchers.IO)
        backgroundScope.launch {
            // Initialize the Google Mobile Ads SDK on a background thread.
            MobileAds.initialize(this@MainActivity) {}
        }
    }
}
```

--------------------------------

### Get Response Info on Ad Failure (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/response-info.md

Retrieve the ResponseInfo object when an ad fails to load using Java. This helps in diagnosing the cause of the failure.

```java
@Override
public void onAdFailedToLoad(LoadAdError loadAdError) {
  ResponseInfo responseInfo = loadAdError.getResponseInfo();
  Log.d(TAG, responseInfo.toString());
}
```

--------------------------------

### Load with the single ad loading API

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/interstitial.md

This section demonstrates how to load a single interstitial ad using the `InterstitialAd.load` method. It includes examples for both Kotlin and Java.

```APIDOC
## Load with the single ad loading API

### Description
Loads a single interstitial ad.

### Method
`InterstitialAd.load()`

### Parameters
- `context` (Context): The Android context.
- `adRequest` (AdRequest): The ad request object.
- `adLoadCallback` (AdLoadCallback<InterstitialAd>): A callback to handle ad loading success or failure.

### Request Example (Kotlin)
```kotlin
import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback
import com.google.android.libraries.ads.mobile.sdk.common.AdRequest
import com.google.android.libraries.ads.mobile.sdk.interstitial.InterstitialAd

InterstitialAd.load(
  context,
  AdRequest.Builder(AD_UNIT_ID).build(),
  object : AdLoadCallback<InterstitialAd> {
    override fun onAdLoaded(ad: InterstitialAd) {
      interstitialAd = ad
    }

    override fun onAdFailedToLoad(adError: LoadAdError) {
      interstitialAd = null
    }
  },
)
```

### Request Example (Java)
```java
import com.google.android.libraries.ads.mobile.sdk.common.AdLoadCallback;
import com.google.android.libraries.ads.mobile.sdk.common.AdRequest;
import com.google.android.libraries.ads.mobile.sdk.interstitial.InterstitialAd;

InterstitialAd.load(
    context,
    new AdRequest.Builder(AD_UNIT_ID).build(),
    new AdLoadCallback<InterstitialAd>() {
      @Override
      public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
        InterstitialActivity.this.interstitialAd = interstitialAd;
      }

      @Override
      public void onAdFailedToLoad(@NonNull LoadAdError adError) {
        interstitialAd = null;
      }
    }
);
```
```

--------------------------------

### Initialize ConsentInformation (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/privacy.md

Initialize the ConsentInformation instance in Java using the UserMessagingPlatform SDK.

```java
consentInformation = UserMessagingPlatform.getConsentInformation(context);
```

--------------------------------

### Initialize GMA Next-Gen SDK and Check Adapter Status (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/mediation.md

Use this Kotlin code to initialize the GMA Next-Gen SDK on a background thread and log the initialization status of each mediation adapter. This ensures all adapters are ready before the first ad request.

```kotlin
import com.google.android.libraries.ads.mobile.sdk.MobileAds
import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig
import kotlinx.coroutines.CoroutineScope
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.launch

class MainActivity : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    val backgroundScope = CoroutineScope(Dispatchers.IO)
    backgroundScope.launch {
    // Initialize GMA Next-Gen SDK on a background thread.
    MobileAds.initialize(this@MainActivity, InitializationConfig.Builder("SAMPLE_APP_ID").build()) {
    initializationStatus ->
    for ((adapterName, adapterStatus) in initializationStatus.adapterStatusMap) {
    Log.d(
    "MyApp",
    String.format(
    "Adapter name: %s, Status code: %s, Status string: %s, Latency: %d",
    adapterName,
    adapterStatus.initializationState,
    adapterStatus.description,
    adapterStatus.latency,
    ),
    )
    }
    // Adapter initialization is complete.
    }
    // Other methods on MobileAds can now be called.
    }
      }
    }
```

--------------------------------

### Get Test Device ID from Logcat

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/test-ads.md

When an ad request is made, check the logcat output for a message containing your device ID and instructions on how to add it as a test device.

```logcat
I/Ads: Use RequestConfiguration.Builder.setTestDeviceIds(Arrays.asList("33BE2250B43518CCDA7DE426D04EE231"))
to get test ads on this device.

```

--------------------------------

### Get Response Info on Ad Load (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/response-info.md

Retrieve the ResponseInfo object after a successful ad load using Java. This is useful for debugging and logging ad details.

```java
@Override
public void onAdLoaded(@NonNull InterstitialAd interstitialAd) {
  ResponseInfo responseInfo = interstitialAd.getResponseInfo();
  Log.d(TAG, responseInfo.toString());
}
```

--------------------------------

### Implement Lifecycle Observer for App Open Ad (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/app-open.md

Implement DefaultLifecycleObserver in your Application class to show an app open ad when the app starts. Ensure the ProcessLifecycleOwner is observed.

```kotlin
class MyApplication :
  Application(), Application.ActivityLifecycleCallbacks, DefaultLifecycleObserver {

  private var currentActivity: Activity? = null

  override fun onCreate() {
    super<Application>.onCreate()
    registerActivityLifecycleCallbacks(this)
    ProcessLifecycleOwner.get().lifecycle.addObserver(this)
  }

  /**
   * DefaultLifecycleObserver method that shows the app open ad when the app moves to foreground.
   */
  override fun onStart(owner: LifecycleOwner) {
    currentActivity?.let { activity ->
      AppOpenAdManager.showAdIfAvailable(activity, null)
    }
  }

  // ...
}
```

--------------------------------

### Initialize SDK and Set App Volume (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/global-settings.md

Initializes the GMA SDK on a background thread and sets the app volume to half of the current device volume. Use this when your app has custom volume controls.

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);

      new Thread(
              () -> {
                // Initialize GMA Next-Gen SDK on a background thread.
                MobileAds.initialize(
                    this,
                    // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
                    new InitializationConfig.Builder("SAMPLE_APP_ID")
                        .build(),
                    initializationStatus -> { });
                
                // Set app volume to be half of current device volume.
                MobileAds.setUserControlledAppVolume(0.5f);
              })
          .start();
    }
```

--------------------------------

### Initialize ConsentInformation (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/privacy.md

Initialize the ConsentInformation instance in Kotlin using the UserMessagingPlatform SDK.

```kotlin
consentInformation = UserMessagingPlatform.getConsentInformation(context)
```

--------------------------------

### Load with the single ad loading API

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/rewarded.md

This section demonstrates how to load a single rewarded ad using the GMA Next-Gen SDK. It provides code examples in both Kotlin and Java.

```APIDOC
## Load with the single ad loading API

### Description
Loads a single rewarded ad.

### Method
`RewardedAd.load()`

### Parameters
- `adRequest` (AdRequest) - Required - An `AdRequest` object configured with the ad unit ID.
- `adLoadCallback` (AdLoadCallback<RewardedAd>) - Required - A callback to handle the ad loading result.

### Request Example (Kotlin)
```kotlin
RewardedAd.load(
  AdRequest.Builder(AD_UNIT_ID).build(),
  object : AdLoadCallback<RewardedAd> {
    override fun onAdLoaded(ad: RewardedAd) {
      rewardedAd = ad
    }

    override fun onAdFailedToLoad(adError: LoadAdError) {
      rewardedAd = null
    }
  },
)
```

### Request Example (Java)
```java
RewardedAd.load(
    new AdRequest.Builder(AD_UNIT_ID).build(),
    new AdLoadCallback<RewardedAd>() {
      @Override
      public void onAdLoaded(@NonNull RewardedAd rewardedAd) {
        RewardedActivity.this.rewardedAd = rewardedAd;
      }

      @Override
      public void onAdFailedToLoad(@NonNull LoadAdError adError) {
        rewardedAd = null;
      }
    }
);
```

### Response
#### Success Response
- `RewardedAd` (RewardedAd) - The loaded rewarded ad object.

#### Error Response
- `LoadAdError` (LoadAdError) - An error object if the ad fails to load.
```

--------------------------------

### Initialize Legacy Google Mobile Ads SDK (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migration.md

Initializes the Google Mobile Ads SDK on a background thread using Java. Recommended for reducing ANRs.

```java
import com.google.android.gms.ads.MobileAds;
import com.google.android.gms.ads.initialization.InitializationStatus;
import com.google.android.gms.ads.initialization.OnInitializationCompleteListener;

public class MainActivity extends AppCompatActivity {
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        new Thread( () -> {
            // Initialize the Google Mobile Ads SDK on a background thread.
            MobileAds.initialize(this, initializationStatus -> {});
        }) .start();
    }
}
```

--------------------------------

### Implement Lifecycle Observer for App Open Ad (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/app-open.md

Implement DefaultLifecycleObserver in your Application class to show an app open ad when the app starts. Ensure the ProcessLifecycleOwner is observed.

```java
public class MyApplication extends Application
  implements Application.ActivityLifecycleCallbacks, DefaultLifecycleObserver {

  private Activity currentActivity;

  @Override
  public void onCreate() {
    super.onCreate();
    registerActivityLifecycleCallbacks(this);
    ProcessLifecycleOwner.get().getLifecycle().addObserver(this);
  }

  /**
   * DefaultLifecycleObserver method that shows the app open ad when the app moves to foreground.
   */
  @Override
  public void onStart(@NonNull LifecycleOwner owner) {
    if (currentActivity == null) {
      return;
    }
    AppOpenAdManager.getInstance().showAdIfAvailable(currentActivity, null);
  }

  // ...
}
```

--------------------------------

### Report Adapter and SDK Versions in Kotlin

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/setup.md

Implement getVersionInfo() and getSDKVersionInfo() to report adapter and SDK versions. Ensure versions are parsed correctly from strings.

```Kotlin
package com.google.ads.mediation.sample.customevent

class SampleCustomEvent : Adapter() {
  override fun getVersionInfo(): VersionInfo {
    val versionString = VersionInfo(1,2,3).toString()
    val splits: List<String> = versionString.split("\\.")

    if (splits.count() >= 4) {
      val major = splits[0].toInt()
      val minor = splits[1].toInt()
      val micro = (splits[2].toInt() * 100) + splits[3].toInt()
      return VersionInfo(major, minor, micro)
    }

    return VersionInfo(0, 0, 0)
  }

  override fun getSDKVersionInfo(): VersionInfo {
    val versionString = VersionInfo(1,2,3).toString()
    val splits: List<String> = versionString.split("\\.")

    if (splits.count() >= 3) {
      val major = splits[0].toInt()
      val minor = splits[1].toInt()
      val micro = splits[2].toInt()
      return VersionInfo(major, minor, micro)
    }

    return VersionInfo(0, 0, 0)
  }
}
```

--------------------------------

### Initialize SDK and Set App Volume (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/global-settings.md

Initializes the GMA SDK on a background thread and sets the app volume to half of the current device volume. Use this when your app has custom volume controls.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
      super.onCreate(savedInstanceState)

      val backgroundScope = CoroutineScope(Dispatchers.IO)
      backgroundScope.launch {
        // Initialize GMA Next-Gen SDK on a background thread.
        MobileAds.initialize(
          this@MainActivity,
          // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713
          InitializationConfig.Builder("SAMPLE_APP_ID").build()
        ) {}
        
        // Set app volume to be half of current device volume.
        MobileAds.setUserControlledAppVolume(0.5f)
      }
    }
```

--------------------------------

### Update All Agent Skills for GMA Next-Gen SDK

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/agent-skills.md

Run this command to update all installed agent skills for the GMA Next-Gen SDK, ensuring your AI assistant has the latest features and recommendations.

```bash
npx skills update --all
```

--------------------------------

### Send Impression-Level Revenue Data (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/impression-level-ad-revenue.md

This Kotlin code demonstrates how to set an ad event callback and transmit impression-level ad revenue to Tenjin. It's crucial to set the listener early and submit data promptly to avoid discrepancies.

```kotlin
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
```

--------------------------------

### Register Media Asset with NativeAdView (Legacy SDK)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-native.md

For the Google Mobile Ads SDK (Legacy), register the media view with the `NativeAdView` before registering the native ad. This Kotlin example demonstrates the process.

```kotlin
private fun displayNativeAd(nativeAd: NativeAd) {
// Inflate the NativeAdView layout.
val nativeAdBinding = NativeAdBinding.inflate(layoutInflater)
// Add the NativeAdView to the view hierarchy.
binding.nativeViewContainer.addView(nativeAdBinding.root)
val nativeAdView = nativeAdBinding.root
// Populate and register the asset views.
nativeAdView.mediaView = nativeAdBinding.adMedia
// ...
// Register the native ad with the NativeAdView.
nativeAdView.setNativeAd(nativeAd)
}
```

```java
private void displayNativeAd(NativeAd nativeAd) {
// Inflate the NativeAdView layout
NativeAdBinding nativeAdBinding = NativeAdBinding.inflate(getLayoutInflater());
// Add the NativeAdView to the view hierarchy
binding.nativeViewContainer.addView(nativeAdBinding.getRoot());
NativeAdView nativeAdView = nativeAdBinding.getRoot();
// Populate and register the asset views
nativeAdView.setMediaView(nativeAdBinding.adMedia);
// ...
// Register the native ad with the NativeAdView
nativeAdView.setNativeAd(nativeAd);
}
```

--------------------------------

### Initialize Custom Event Adapter (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/setup.md

Implement the `initialize()` method in your custom event adapter to set up the required third-party SDK. Call `onInitializationSucceeded()` upon completion.

```java
package com.google.ads.mediation.sample.customevent;

import com.google.android.gms.ads.mediation.Adapter;
import com.google.android.gms.ads.mediation.InitializationCompleteCallback;
import com.google.android.gms.ads.mediation.MediationConfiguration;

public class SampleAdNetworkCustomEvent extends Adapter {
  private static final String SAMPLE_AD_UNIT_KEY = "parameter";

  @Override
  public void initialize(Context context,
      InitializationCompleteCallback initializationCompleteCallback,
      List<MediationConfiguration> mediationConfigurations) {
    // This is where you will initialize the SDK that this custom
    // event is built for. Upon finishing the SDK initialization,
    // call the completion handler with success.
    initializationCompleteCallback.onInitializationSucceeded();
  }
}
```

--------------------------------

### Load a Native Ad

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/native.md

Demonstrates how to load a native ad using the `NativeAdLoader.load()` method. It includes building a `NativeAdRequest` and implementing a `NativeAdLoaderCallback` to handle ad loading success or failure.

```APIDOC
## Load an Ad

### Description
Loads a native ad using the `NativeAdLoader.load()` method. This involves creating a `NativeAdRequest` with specific ad options and providing a `NativeAdLoaderCallback` to manage the ad loading process.

### Method
`NativeAdLoader.load(NativeAdRequest request, NativeAdLoaderCallback callback)`

### Parameters
#### Request Body
- **request** (`NativeAdRequest`) - Required - An object containing the ad request details, including the ad unit ID and native ad types.
- **callback** (`NativeAdLoaderCallback`) - Required - An object that defines methods to be called when the ad has loaded successfully or failed to load.

### Request Example
```kotlin
import com.google.android.libraries.ads.mobile.sdk.common.LoadAdError
import com.google.android.libraries.ads.mobile.sdk.nativead.NativeAd
import com.google.android.libraries.ads.mobile.sdk.nativead.NativeAdLoader
import com.google.android.libraries.ads.mobile.sdk.nativead.NativeAdLoaderCallback
import com.google.android.libraries.ads.mobile.sdk.nativead.NativeAdRequest

class NativeFragment : Fragment() {

  private var nativeAd: NativeAd? = null

  override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)
    loadAd()
  }

  private fun loadAd() {
    // Build an ad request with native ad options to customize the ad.
    val adRequest = NativeAdRequest
      .Builder(AD_UNIT_ID, listOf(NativeAd.NativeAdType.NATIVE))
      .build()

    val adCallback =
      object : NativeAdLoaderCallback {
        override fun onNativeAdLoaded(nativeAd: NativeAd) {
          // Called when a native ad has loaded.
        }
        override fun onAdFailedToLoad(adError: LoadAdError) {
          // Called when a native ad has failed to load.
        }
      }

    // Load the native ad with our request and callback.
    NativeAdLoader.load(adRequest, adCallback)
  }

  companion object {
    // Sample native ad unit ID.
    const val AD_UNIT_ID = "ca-app-pub-3940256099942544/2247696110"
  }
}
```

### Response
#### Success Response
- **nativeAd** (`NativeAd`) - The loaded native ad object.

#### Error Response
- **adError** (`LoadAdError`) - An error object detailing the reason for the ad load failure.
```

--------------------------------

### Resetting Consent State in Kotlin

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/privacy.md

Call this Kotlin method to reset the UMP SDK's consent state. This is useful for simulating a user's first install experience during testing. Do not use in production.

```Kotlin
consentInformation.reset()
```

--------------------------------

### Register Media Asset with NativeAdView (GMA Next-Gen SDK)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-native.md

The GMA Next-Gen SDK enforces registration of the media view with the `NativeAdView` at the same time as the native ad. This Kotlin example shows the `registerNativeAd` method.

```kotlin
private fun displayNativeAd(nativeAd: NativeAd) {
// Inflate the NativeAdView layout.
val nativeAdBinding = NativeAdBinding.inflate(layoutInflater)
// Add the NativeAdView to the view hierarchy.
binding.nativeViewContainer.addView(nativeAdBinding.root)
val nativeAdView = nativeAdBinding.root
// Populate and register the asset views.
// ...
// Register the native ad and media content asset with the NativeAdView.
val mediaView = nativeAdBinding.adMedia
nativeAdView.registerNativeAd(nativeAd, mediaView)
}
```

```java
private void displayNativeAd(NativeAd nativeAd) {
// Inflate the NativeAdView layout.
NativeAdBinding nativeAdBinding = NativeAdBinding.inflate(getLayoutInflater());
// Add the NativeAdView to the view hierarchy.
binding.nativeViewContainer.addView(nativeAdBinding.getRoot());
NativeAdView nativeAdView = nativeAdBinding.getRoot();
// Populate and register the asset views.
// ...
// Register the native ad and media content asset with the NativeAdView.
MediaView mediaView = nativeAdBinding.adMedia;
nativeAdView.registerNativeAd(nativeAd, mediaView);
}
```

--------------------------------

### Resetting Consent State in Java

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/privacy.md

Call this Java method to reset the UMP SDK's consent state. This is useful for simulating a user's first install experience during testing. Do not use in production.

```Java
consentInformation.reset();
```

--------------------------------

### Initialize Custom Event Adapter (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/setup.md

Implement the `initialize()` method in your custom event adapter to set up the required third-party SDK. Call `onInitializationSucceeded()` upon completion.

```kotlin
package com.google.ads.mediation.sample.customevent

import com.google.android.gms.ads.mediation.Adapter
import com.google.android.gms.ads.mediation.InitializationCompleteCallback
import com.google.android.gms.ads.mediation.MediationConfiguration

class SampleCustomEvent : Adapter() {
  private val SAMPLE_AD_UNIT_KEY = "parameter"

  override fun initialize(
    context: Context,
    initializationCompleteCallback: InitializationCompleteCallback,
    mediationConfigurations: List<MediationConfiguration>
  ) {
    // This is where you will initialize the SDK that this custom
    // event is built for. Upon finishing the SDK initialization,
    // call the completion handler with success.
    initializationCompleteCallback.onInitializationSucceeded()
  }
}
```

--------------------------------

### Initialize GMA Next-Gen SDK and Check Adapter Status (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/mediation.md

This Java code initializes the GMA Next-Gen SDK on a background thread and logs the initialization status of each mediation adapter. It's crucial for ensuring adapter readiness before the first ad request.

```java
import com.google.android.libraries.ads.mobile.sdk.MobileAds;
import com.google.android.libraries.ads.mobile.sdk.initialization.AdapterStatus;
import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig;

public class MainActivity extends AppCompatActivity {
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    new Thread(
    () -> {
    // Initialize GMA Next-Gen SDK on a background thread.
    MobileAds.initialize(
    this,
    new InitializationConfig.Builder("SAMPLE_APP_ID")
    .build(),
    initializationStatus -> {
    Map<String, AdapterStatus> adapterStatusMap =
    initializationStatus.getAdapterStatusMap();
    for (String adapterClass : adapterStatusMap.keySet()) {
    AdapterStatus adapterStatus = adapterStatusMap.get(adapterClass);
    Log.d(
    "MyApp",
    String.format(
    "Adapter name: %s, Status code: %s, Status description: %s,"
    + " Latency: %d",
    adapterClass,
    adapterStatus.getInitializationState(),
    adapterStatus.getDescription(),
    adapterStatus.getLatency()));
    }
    // Adapter initialization is complete.
    });
    // Other methods on MobileAds can now be called.
    })
    .start();
      }
    }
```

--------------------------------

### Load Collapsible Banner Ad (Bottom Placement)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/collapsible.md

This Java snippet demonstrates how to load a collapsible banner ad with the expanded region aligning to the bottom of the banner view. Ensure you have the necessary AdMob SDK setup.

```java
private void loadBannerAd() {
  // ...

  Bundle extras = new Bundle();
  extras.putString("collapsible", "bottom");

  BannerAdRequest bannerAdRequest = new BannerAdRequest.Builder("AD_UNIT_ID", adSize)
      .setGoogleExtrasBundle(extras)
      .build();

  BannerAd.load(
      bannerAdRequest,
      new AdLoadCallback<BannerAd>() {
        @Override
        public void onAdLoaded(@NonNull BannerAd ad) {
          // ...
        }

        @Override
        public void onAdFailedToLoad(@NonNull LoadAdError adError) {
          // ...
        }
      });
}
```

--------------------------------

### Load and Show Consent Form (Kotlin)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/privacy.md

Loads and displays the consent form if required. The callback is invoked immediately if no forms are needed.

```kotlin
UserMessagingPlatform.loadAndShowConsentFormIfRequired(activity) { formError ->
  // Consent gathering process is complete.
}
```

--------------------------------

### Load Adaptive Banner Ad in Jetpack Compose

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/banner.md

Load a 360-width large anchored adaptive banner ad within a Jetpack Compose UI. This example uses LaunchedEffect for loading and remembers the ad state.

```kotlin
// Request an large anchored adaptive banner with a width of 360.
    val adSize = AdSize.getLargeAnchoredAdaptiveBannerAdSize(requireContext(), 360)

    // Load the ad when the screen is active.
    val coroutineScope = rememberCoroutineScope()
    val isPreviewMode = LocalInspectionMode.current
    LaunchedEffect(context) {
      bannerAdState?.destroy()
      if (!isPreviewMode) {
        coroutineScope.launch {
          when (val result = BannerAd.load(BannerAdRequest.Builder(AD_UNIT_ID, adSize).build())) {
            is AdLoadResult.Success -> {
              bannerAdState = result.ad
            }
            is AdLoadResult.Failure -> {
              showToast("Banner failed to load.")
              Log.w(Constant.TAG, "Banner ad failed to load: $result.error")
            }
          }
        }
      }
    }
```

--------------------------------

### Load Collapsible Banner Ad (Bottom Placement)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/collapsible.md

This Kotlin snippet demonstrates how to load a collapsible banner ad with the expanded region aligning to the bottom of the banner view. Ensure you have the necessary AdMob SDK setup.

```kotlin
private fun loadBannerAd() {
  // ...

  // Create an extra parameter that aligns the bottom of the expanded ad to
// the bottom of the bannerView.
  val extras = Bundle()
  extras.putString("collapsible", "bottom")

  val bannerAdRequest = BannerAdRequest.Builder("AD_UNIT_ID", adSize)
    .setGoogleExtrasBundle(extras)
    .build()

  BannerAd.load(
    bannerAdRequest,
    object : AdLoadCallback<BannerAd> {
      override fun onAdLoaded(ad: BannerAd) {
        // ...
      }

      override fun onAdFailedToLoad(loadAdError: LoadAdError) {
        // ...
      }
    },
  )
}
```

--------------------------------

### Load Banner Ad with Legacy Google Mobile Ads SDK (Java)

Source: https://github.com/ramzyramadhan/gmanext-gensdkforandroidmds/blob/master/migrate-banner.md

This Java snippet demonstrates loading a banner ad using the legacy SDK. It outlines the steps for initializing an AdView and initiating the ad load process.

```java
import com.google.android.gms.ads.AdRequest;
import com.google.android.gms.ads.AdSize;
import com.google.android.gms.ads.AdView;
public class MainActivity extends AppCompatActivity {
    private ActivityMainBinding binding;
    private AdView adView;
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        binding = ActivityMainBinding.inflate(getLayoutInflater());
        setContentView(binding.getRoot());

        // Step 1 - Create an AdView object with ad unit ID and size.
        adView = new AdView(this);
        adView.setAdUnitId("AD_UNIT_ID");
        adView.setAdSize(AdSize.getLargeAnchoredAdaptiveBannerAdSize(this, 320));

        // Step 2 - Add the AdView to view hierarchy.
        binding.bannerViewContainer.addView(adView);

        // Step 3 - Load the ad.
        AdRequest adRequest = new AdRequest.Builder().build();
        adView.loadAd(adRequest);
    }
}
```
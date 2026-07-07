**Last updated:** January 10, 2025

<br />

This page covers the instructions to initialize GMA Next-Gen SDK.

## Before you begin

To use GMA Next-Gen SDK, you must either integrate without mediation
or use AdMob as the mediation platform. The
other mediation platforms are not compatible with GMA Next-Gen SDK.
[Video](https://www.youtube.com/watch?v=a94TRVp-b1k)

## Configure your build for GMA Next-Gen SDK

The following sections show you the necessary steps to configure GMA Next-Gen SDK.

### Include GMA Next-Gen SDK dependency

The GMA Next-Gen SDK requires a different Gradle dependency. In your
app-level build file, remove the reference to the Google Mobile Ads SDK (Legacy) dependency
and include the new artifact.

| Gradle dependencies ||
|---|---|
| Google Mobile Ads SDK (Legacy) | ### Kotlin ```kotlin dependencies { // ... implementation("com.google.android.gms:play-services-ads:25.4.0") } ``` ### Groovy ```groovy dependencies { // ... implementation 'com.google.android.gms:play-services-ads:25.4.0' } ``` |
| GMA Next-Gen SDK | ### Kotlin ```kotlin dependencies { // ... // Comment out/remove play-services-ads. // implementation("com.google.android.gms:play-services-ads:25.4.0") implementation("com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk:1.2.1") } ``` ### Groovy ```groovy dependencies { // ... // Comment out/remove play-services-ads. // implementation 'com.google.android.gms:play-services-ads:25.4.0' implementation 'com.google.android.libraries.ads.mobile.sdk:ads-mobile-sdk:1.2.1' } ``` |

### Exclude `com.google.android.gms` modules in mediation integrations

Mediation adapters continue to depend on Google Mobile Ads SDK (Legacy). However,
GMA Next-Gen SDK includes all classes required by mediation adapters.
To avoid compile errors related to duplicate symbols, you need to exclude
Google Mobile Ads SDK (Legacy) from being pulled in as a dependency by mediation
adapters.

In your app-level build file, exclude both `play-services-ads` and
`play-services-ads-lite` modules globally from all dependencies.

### Kotlin

```kotlin
configurations.configureEach {
    exclude(group = "com.google.android.gms", module = "play-services-ads")
    exclude(group = "com.google.android.gms", module = "play-services-ads-lite")
}
```

### Groovy

```groovy
configurations.configureEach {
    exclude group: "com.google.android.gms", module: "play-services-ads"
    exclude group: "com.google.android.gms", module: "play-services-ads-lite"
}
```

## Set the minimum and compile Android API levels

GMA Next-Gen SDK requires a minimum Android API level of 24 and a
compile Android API level of 34. Adjust the `minSdk` and `compileSdk` values in
your app-level build file to 24 or higher and 34 or higher, respectively.

## Initialize GMA Next-Gen SDK

[Video](https://www.youtube.com/watch?v=a94TRVp-b1k)

GMA Next-Gen SDK requires initialization before loading ads, a change
from Google Mobile Ads SDK (Legacy) where initialization is optional but recommended.
Update your code if you weren't previously initializing the SDK before loading
ads.

This section covers the differences in SDK initialization implementation between
Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK.

### Set the AdMob app ID

The following examples set the
AdMob app ID in Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK:

|---|---|
| Google Mobile Ads SDK (Legacy) | Integration requires a `<meta-data>` tag with `android:name="com.google.android.gms.ads.APPLICATION_ID"` containing your AdMob app ID within your app's AndroidManifest.xml file. ```xml <manifest> <application> <!-- Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713 --> <meta-data android:name="com.google.android.gms.ads.APPLICATION_ID" android:value="SAMPLE_APP_ID"/> </application> </manifest> ``` |
| GMA Next-Gen SDK | Provide your AdMob app ID programmatically as part of SDK initialization. ### Kotlin ```kotlin // Initialize the Google Mobile Ads SDK. val initConfig = InitializationConfig.Builder("SAMPLE_APP_ID").build() MobileAds.initialize(this@MainActivity, initConfig) {} ``` ### Java ```java // Initialize GMA Next-Gen SDK. InitializationConfig initConfig = new InitializationConfig.Builder("SAMPLE_APP_ID").build(); MobileAds.initialize(this, initConfig, initializationStatus -> {}); ``` |

> [!NOTE]
> **Note:** The Google User Messaging Platform (UMP) SDK still requires the app ID in your app's `AndroidManifest.xml`. For more information, see [Add the application ID](https://developers.google.com/admob/android/next-gen/privacy#add_the_application_id).

### Review implementation changes

The following examples initialize Google Mobile Ads SDK (Legacy) and GMA Next-Gen SDK:

|---|---|
| Google Mobile Ads SDK (Legacy) | Call `MobileAds.initialize()` to initialize the Google Mobile Ads SDK. Initialization on a background thread is recommended to reduce ANRs. ### Kotlin ```kotlin import com.google.android.gms.ads.MobileAds import kotlinx.coroutines.CoroutineScope import kotlinx.coroutines.Dispatchers import kotlinx.coroutines.launch class MainActivity : AppCompatActivity() { override fun onCreate(savedInstanceState: Bundle?) { super.onCreate(savedInstanceState) setContentView(R.layout.activity_main) val backgroundScope = CoroutineScope(Dispatchers.IO) backgroundScope.launch { // Initialize the Google Mobile Ads SDK on a background thread. MobileAds.initialize(this@MainActivity) {} } } } ``` ### Java ```java import com.google.android.gms.ads.MobileAds; import com.google.android.gms.ads.initialization.InitializationStatus; import com.google.android.gms.ads.initialization.OnInitializationCompleteListener; public class MainActivity extends AppCompatActivity { protected void onCreate(Bundle savedInstanceState) { super.onCreate(savedInstanceState); setContentView(R.layout.activity_main); new Thread( () -> { // Initialize the Google Mobile Ads SDK on a background thread. MobileAds.initialize(this, initializationStatus -> {}); }) .start(); } } ``` |
| GMA Next-Gen SDK | > [!IMPORTANT] > **Key Point** : You must initialize GMA Next-Gen SDK before loading ads and interacting with other [`MobileAds`](https://developers.google.com/admob/android/next-gen/reference/kotlin/com/google/android/libraries/ads/mobile/sdk/MobileAds) methods, unless explicitly noted in API reference docs (see [`getVersion()`](https://developers.google.com/admob/android/next-gen/reference/kotlin/com/google/android/libraries/ads/mobile/sdk/MobileAds#getVersion())). Otherwise, an [`UninitializedPropertyAccessException`](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-uninitialized-property-access-exception/) may be thrown. Call [`MobileAds.initialize()`](https://developers.google.com/admob/android/next-gen/reference/kotlin/com/google/android/libraries/ads/mobile/sdk/MobileAds#initialize(android.content.Context,com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig,com.google.android.libraries.ads.mobile.sdk.initialization.OnInitializationCompleteListener)) to initialize GMA Next-Gen SDK. This must be called on a background thread, failure to do so may cause an "Application Not Responding" (ANR) error. ### Kotlin ```kotlin import com.google.android.libraries.ads.mobile.sdk.MobileAds import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig import kotlinx.coroutines.CoroutineScope import kotlinx.coroutines.Dispatchers import kotlinx.coroutines.launch class MainActivity : AppCompatActivity() { override fun onCreate(savedInstanceState: Bundle?) { super.onCreate(savedInstanceState) setContentView(R.layout.activity_main) val backgroundScope = CoroutineScope(Dispatchers.IO) backgroundScope.launch { // Initialize GMA Next-Gen SDK on a background thread. MobileAds.initialize( this@MainActivity, // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713 InitializationConfig.Builder("SAMPLE_APP_ID").build() ) { // Adapter initialization is complete. } // SDK initialization is complete. If you don't want to wait for bidding adapters to finish // initializing, start loading ads now. } } } ``` ### Java ```java import com.google.android.libraries.ads.mobile.sdk.MobileAds; import com.google.android.libraries.ads.mobile.sdk.initialization.InitializationConfig; public class MainActivity extends AppCompatActivity { protected void onCreate(Bundle savedInstanceState) { super.onCreate(savedInstanceState); setContentView(R.layout.activity_main); new Thread( () -> { // Initialize GMA Next-Gen SDK on a background thread. MobileAds.initialize( this, // Sample AdMob app ID: ca-app-pub-3940256099942544~3347511713 new InitializationConfig.Builder("SAMPLE_APP_ID") .build(), initializationStatus -> { // Adapter initialization is complete. }); // SDK initialization is complete. If you don't want to wait for bidding adapters to // finish initializing, start loading ads now. }) .start(); } } ``` |
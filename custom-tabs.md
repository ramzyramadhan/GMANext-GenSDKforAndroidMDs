Select platform: [Android](https://developers.google.com/admob/android/next-gen/browser/custom-tabs "View this page for the Android platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/browser/custom-tabs "View this page for the Android (Legacy) platform docs.")

<br />

The web view APIs for ads makes app signals available to the tags in your
[`CustomTabSession`](https://developer.android.com/reference/androidx/browser/customtabs/CustomTabsSession),
helping to improve monetization for the web publishers that provided the content
and to protect advertisers from spam.

![](https://developers.google.com/static/ad-manager/mobile-ads-sdk/images/webview/monetization-paths-paw-cct.svg)

## How it works

Communication with the GMA Next-Gen SDK only happens in response to ad
events triggered by any of the following:

- [AdSense code](https://support.google.com/adsense/answer/9274634)
- [Google Publisher Tag](https://support.google.com/admanager/answer/181073)
- [IMA for HTML5](https://support.google.com/adsense/answer/6391192)

The GMA Next-Gen SDK facilitates communication by opening a postMessage
channel with the `CustomTabsSession` provided by the Android framework.

> [!WARNING]
> **Warning:** Don't call `CustomTabsSession.requestPostMessageChannel()` from your `CustomTabsSession` instance as this has unintended side effects. Use [`CustomTabsSession.postMessage()`](https://developer.android.com/reference/androidx/browser/customtabs/CustomTabsSession#postMessage(java.lang.String,android.os.Bundle)) to send postMessages and [`CustomTabsCallback.onPostMessage()`](https://developer.android.com/reference/androidx/browser/customtabs/CustomTabsCallback#onPostMessage(java.lang.String,android.os.Bundle)) to listen for these events.

## Prerequisites

- GMA Next-Gen SDK 0.6.0-alpha01 or higher.
- An existing [Chrome Custom Tabs](https://developer.chrome.com/docs/android/custom-tabs/guide-get-started) implementation in your mobile app. See [Warm-up and pre-fetch: using the
  Custom Tabs Service](https://developer.chrome.com/docs/android/custom-tabs/guide-warmup-prefetch).
- Associate your mobile app to a website using [Digital Asset Links](https://developers.google.com/digital-asset-links/v1/getting-started). For instructions, see [PostMessage for TWA](https://developer.chrome.com/docs/android/post-message-twa).

## Pass the application ID to the SDK

If you already have an AdMob application ID,
[initialize the GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start#initialize_the_mobile_ads_sdk) with your existing
application ID.

If you don't have an AdMob application ID, pass in
`InitializationConfig.WEBVIEW_APIS_FOR_ADS_APPLICATION_ID` as the application ID
when you [initialize the GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start#initialize_the_mobile_ads_sdk):

### Kotlin

    MobileAds.initialize(
        this@MainActivity,
        // Use this application ID to initialize the GMA Next-Gen SDK if
        // you don't have an AdMob application ID.
        InitializationConfig.Builder(InitializationConfig.WEBVIEW_APIS_FOR_ADS_APPLICATION_ID)
            .build(),
      ) {
        // Adapter initialization complete.
      }

### Java

    MobileAds.initialize(
        this,
        // Use this application ID to initialize the GMA Next-Gen SDK if
        // you don't have an AdMob application ID.
        new InitializationConfig.Builder(InitializationConfig.WEBVIEW_APIS_FOR_ADS_APPLICATION_ID)
            .build(),
            initializationStatus -> {
              // Adapter initialization is complete.
              });

## Implementation

Within your Chrome Custom Tabs implementation, modify the code in the
[`onCustomTabsServiceConnected()`](https://developer.android.com/reference/androidx/browser/customtabs/CustomTabsServiceConnection#onCustomTabsServiceConnected(android.content.ComponentName,androidx.browser.customtabs.CustomTabsClient))
callback. Replace [`newSession()`](https://developer.android.com/reference/androidx/browser/customtabs/CustomTabsClient#newSession(androidx.browser.customtabs.CustomTabsCallback))
with [`MobileAds.registerCustomTabsSession()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/MobileAds#registerCustomTabsSession(androidx.browser.customtabs.CustomTabsClient,kotlin.String,androidx.browser.customtabs.CustomTabsCallback)). This establishes a connection with the
postMessage channel to enrich the ad tags with SDK signals. A Digital Asset Link
is needed to connect the postMessage channel. Optionally, you can set the
[CustomTabsCallback](https://developer.android.com/reference/androidx/browser/customtabs/CustomTabsCallback)
parameter to listen for Chrome Custom Tab events.

You should still be able to send messages from the `CustomTabsSession` received
from the GMA Next-Gen SDK but the port could change when the SDK requests
a new channel that overrides the existing one.

The following code snippet shows how to register a `CustomTabsSession` using the
GMA Next-Gen SDK:

### Kotlin

    import com.google.android.libraries.ads.mobile.sdk.MobileAds

    class MainActivity : ComponentActivity() {
      private var customTabsClient: CustomTabsClient? = null
      private var customTabsSession: CustomTabsSession? = null

      override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Get the default browser package name, this will be null if
        // the default browser does not provide a CustomTabsService.
        val packageName = CustomTabsClient.getPackageName(applicationContext, null);
        if (packageName == null) {
          // Do nothing as service connection is not supported.
          return
        }

        CustomTabsClient.bindCustomTabsService(
          applicationContext,
          packageName,
          object : CustomTabsServiceConnection() {
            override fun onCustomTabsServiceConnected(
              name: ComponentName, client: CustomTabsClient,
              ) {
                customTabsClient = client

                // Warm up the browser process.
                customTabsClient?.warmup(0L)
                // Create a new browser session using the GMA Next-Gen SDK.
    customTabsSession = MobileAds.INSTANCE.registerCustomTabsSession(
    client,
    // Checks the "Digital Asset Link" to connect the postMessage channel.
    ORIGIN,
    // Optional parameter to receive the delegated callbacks.
    customTabsCallback
    )
    // Create a new browser session if the GMA Next-Gen SDK is
    // unable to create one.
    if (customTabsSession == null) {
    customTabsSession = client.newSession(customTabsCallback)
    }

                // Pass the custom tabs session into the intent.
                val customTabsIntent = CustomTabsIntent.Builder(customTabsSession).build()
                customTabsIntent.launchUrl(this@MainActivity,
                                          Uri.parse("YOUR_URL"))
              }

              override fun onServiceDisconnected(componentName: ComponentName) {
                // Remove the custom tabs client and custom tabs session.
                customTabsClient = null
                customTabsSession = null
              }
          })
      }

      // Listen for events from the CustomTabsSession delegated by the GMA Next-Gen SDK.
      private val customTabsCallback: CustomTabsCallback = object : CustomTabsCallback() {
        @Synchronized
        override fun onNavigationEvent(navigationEvent: Int, extras: Bundle?) {
          // Called when a navigation event happens.
        }

        @Synchronized
        override fun onMessageChannelReady(extras: Bundle?) {
          // Called when the channel is ready for sending and receiving messages on both
          // ends.
          // This frequently happens, such as each time the SDK requests a
    // new channel.
        }

        @Synchronized
        override fun onPostMessage(message: String, extras: Bundle?) {
          // Called when a tab controlled by this CustomTabsSession has sent a postMessage.
        }

        override fun onRelationshipValidationResult(
          relation: Int, requestedOrigin: Uri, result: Boolean, extras: Bundle?
        ) {
          // Called when a relationship validation result is available.
        }

        override fun onActivityResized(height: Int, width: Int, extras: Bundle) {
          // Called when the tab is resized.
        }

        override fun extraCallback(callbackName: String, args: Bundle?) {

        }

        override fun extraCallbackWithResult(callbackName: String, args: Bundle?): Bundle? {
          return null
        }
      }

      companion object {
        // Replace this URL with an associated website.
    const val ORIGIN = "https://www.google.com"
      }
    }

### Java

    import com.google.android.libraries.ads.mobile.sdk.MobileAds;

    class MainActivity extends ComponentActivity {
      // Replace this URL with an associated website.
    private static final String ORIGIN = "https://www.google.com";
      private CustomTabsClient customTabsClient;
      private CustomTabsSession customTabsSession;

      @Override
      protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Get the default browser package name, this will be null if
        // the default browser does not provide a CustomTabsService.
        String packageName = CustomTabsClient.getPackageName(getApplicationContext(), null);
        if (packageName == null) {
          // Do nothing as service connection is not supported.
          return;
        }

        CustomTabsClient.bindCustomTabsService(
            getApplicationContext(),
            packageName,
            new CustomTabsServiceConnection() {
              @Override
              public void onCustomTabsServiceConnected(@NonNull ComponentName name,
                  @NonNull CustomTabsClient client) {
                customTabsClient = client;

                // Warm up the browser process.
                customTabsClient.warmup(0);
                // Create a new browser session using the GMA Next-Gen SDK.
    customTabsSession = MobileAds.INSTANCE.registerCustomTabsSession(
    client,
    // Checks the "Digital Asset Link" to connect the postMessage channel.
    ORIGIN,
    // Optional parameter to receive the delegated callbacks.
    customTabsCallback);
    // Create a new browser session if the GMA Next-Gen SDK is
    // unable to create one.
    if (customTabsSession == null) {
    customTabsSession = client.newSession(customTabsCallback);
    }

                // Pass the custom tabs session into the intent.
                CustomTabsIntent intent = new CustomTabsIntent.Builder(customTabsSession).build();
                intent.launchUrl(MainActivity.this, Uri.parse("YOUR_URL"));
              }

              @Override
              public void onServiceDisconnected(ComponentName componentName) {
                // Remove the custom tabs client and custom tabs session.
                customTabsClient = null;
                customTabsSession = null;
              }
            }

        );
      }

      // Listen for events from the CustomTabsSession delegated by the GMA Next-Gen SDK.
      private final CustomTabsCallback customTabsCallback = new CustomTabsCallback() {
        @Override
        public void onNavigationEvent(int navigationEvent, @Nullable Bundle extras) {
          // Called when a navigation event happens.
          super.onNavigationEvent(navigationEvent, extras);
        }

        @Override
        public void onMessageChannelReady(@Nullable Bundle extras) {
          // Called when the channel is ready for sending and receiving messages on both
          // ends.
          // This frequently happens, such as each time the SDK requests a
    // new channel.
          super.onMessageChannelReady(extras);
        }

        @Override
        public void onPostMessage(@NonNull String message, @Nullable Bundle extras) {
          // Called when a tab controlled by this CustomTabsSession has sent a postMessage.
          super.onPostMessage(message, extras);
        }

        @Override
        public void onRelationshipValidationResult(int relation, @NonNull Uri requestedOrigin,
            boolean result, @Nullable Bundle extras) {
          // Called when a relationship validation result is available.
          super.onRelationshipValidationResult(relation, requestedOrigin, result, extras);
        }

        @Override
        public void onActivityResized(int height, int width, @NonNull Bundle extras) {
          // Called when the tab is resized.
          super.onActivityResized(height, width, extras);
        }

        @Override
        public void extraCallback(@NonNull String callbackName, @Nullable Bundle args) {
          super.extraCallback(callbackName, args);
        }

        @Nullable
        @Override
        public Bundle extraCallbackWithResult(@NonNull String callbackName, @Nullable Bundle args) {
          return super.extraCallbackWithResult(callbackName, args);
        }
      };
    }
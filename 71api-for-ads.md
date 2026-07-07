Select platform: [Android](https://developers.google.com/admob/android/next-gen/browser/webview/api-for-ads "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/browser/webview/api-for-ads "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/browser/webview/api-for-ads "View this page for the Android (Legacy) platform docs.")

<br />

The web view APIs for ads makes app signals available to the tags in your

[`WebView`](https://developer.android.com/reference/android/webkit/WebView), helping to improve monetization for the
publishers that provided the content and protect advertisers from spam.


![](https://developers.google.com/static/ad-manager/mobile-ads-sdk/images/webview/monetization-paths-paw.svg)

### How it works

Communication with the GMA Next-Gen SDK only happens in response to ad
events triggered by any of the following:

- [AdSense code](https://support.google.com/adsense/answer/9274634)
- [Google Publisher Tag](https://support.google.com/admanager/answer/181073)
- [IMA for HTML5](https://support.google.com/adsense/answer/6391192)

The SDK adds message handlers to the registered `WebView` to listen for
these ad events. For a better sense of how this works, view the
[source code](https://github.com/google/webview-ads/blob/main/test/index.html) of the
test page.

### Prerequisites

- [GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start#import_the_mobile_ads_sdk) version `1.0.0` or later.

## Pass the application ID to the SDK

If you already have an AdMob application ID,
[initialize the GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start#initialize_the_mobile_ads_sdk) with
your existing application ID.

If you don't have an AdMob application ID, pass in
`InitializationConfig.WEBVIEW_APIS_FOR_ADS_APPLICATION_ID` as the application ID
when you [initialize the GMA Next-Gen SDK](https://developers.google.com/admob/android/next-gen/quick-start#initialize_the_mobile_ads_sdk).

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

### Register the web view

Call

[`registerWebView()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/MobileAds#registerWebView(android.webkit.WebView))

on the main thread to establish a connection with the JavaScript handlers in the
AdSense code or Google Publisher Tag within each `WebView` instance. This
should be done as early as possible, such as in the

`onCreate()` method of your `MainActivity`.


### Kotlin

    import android.webkit.CookieManager
    import android.webkit.WebView
    import com.google.android.libraries.ads.mobile.sdk.MobileAds

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

        // Register the web view.
    MobileAds.registerWebView(webView)
      }
    }

### Java

    import android.webkit.CookieManager;
    import android.webkit.WebView;
    import com.google.android.libraries.ads.mobile.sdk.MobileAds;

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

        // Register the web view.
    MobileAds.registerWebView(webView);
      }
    }

### Test your integration

Before using your own URL, we recommend that you load the following URL to test
the integration:

    https://google.github.io/webview-ads/test#api-for-ads-tests

The test URL shows green status bars for a successful integration if the
following conditions apply:

- `WebView` connected to the GMA Next-Gen SDK

### Next steps

- Gather consent in `WebView`. The Web view APIs for Ads doesn't propagate consent collected in the mobile app context using [IAB TCF v2.3](https://iabeurope.eu/transparency-consent-framework/) or [IAB CCPA](https://iabtechlab.com/wp-content/uploads/2019/11/Technical-Specifications-FAQ-US-Privacy-IAB-Tech-Lab.pdf) compliance frameworks to the tags in your web views. If you're interested in implementing a single consent flow as the owner of both the `WebView` and its corresponding web content being monetized, work with your consent management platform to gather consent in the `WebView` context.
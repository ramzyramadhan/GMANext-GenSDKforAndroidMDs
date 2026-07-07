Select platform: [Android](https://developers.google.com/admob/android/next-gen/browser/webview "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/browser/webview "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/browser/webview "View this page for the Android (Legacy) platform docs.")

<br />

If your app utilizes `
https://developer.android.com/reference/android/webkit/WebView` to display web content, it's
recommended to configure it so that content can be optimally monetized with ads.

![](https://developers.google.com/static/ad-manager/mobile-ads-sdk/images/webview/monetization-paths-non-paw.svg)

This guide shows you how to provide information about how to configure a
`WebView` object.

> [!IMPORTANT]
> **Important:** To properly set up and optimize `WebView`, apply all of the following recommendations to each `WebView` instance in your app. To help identify each web view, toggle the ["highlight-all-webviews"](https://chromium.googlesource.com/chromium/src/+/main/android_webview/docs/developer-ui.md#flag-ui) flag in WebView DevTools to highlight the web views in your app with a yellow tint.

## Enable third-party cookies

To improve your user's ad experience and be consistent with Chrome's
[cookie policy](https://policies.google.com/technologies/cookies), enable third-party
cookies on your `WebView` instance.

### Java

    CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true);

### Kotlin

    CookieManager.getInstance().setAcceptThirdPartyCookies(webView, true)

## Web settings

Default `WebView` settings are not optimized for ads. Use the
[`WebSettings`](https://developer.android.com/reference/android/webkit/WebSettings)
APIs to configure your `WebView` for:

- JavaScript
- Access to local storage
- Automatic video play

### Java

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
      }
    }

### Kotlin

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
      }
    }

## Load web view content

Cookies and page URLs are important for web view monetization and only function
as expected when
[`loadUrl()`](https://developer.android.com/reference/android/webkit/WebView#loadUrl(java.lang.String,%20java.util.Map%3Cjava.lang.String,java.lang.String%3E)) is used with a network-based URL. For optimized
`WebView` performance, load web content
directly from network-based URLs. Avoid using [`WebViewAssetLoader`](https://developer.android.com/reference/androidx/webkit/WebViewAssetLoader), loading
assets from the device, or generating web content dynamically.


### Java

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

### Kotlin

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

## Test the web view

During app development, we recommend that you load this test URL:

    https://google.github.io/webview-ads/test/

to verify these settings have the intended effect on ads. The test URL has
success criteria for a complete integration if the following are observed:

#### Web view settings

- Third-party cookies work

<!-- -->

- First-party cookies work
- JavaScript enabled

<!-- -->

- DOM storage enabled

#### Video ad

- The video ad plays inline and does not open in the full screen built-in player
- The video ad plays automatically without clicking the play button
- The video ad is replayable

After testing is complete, substitute the test URL with the URL the web view
intends to load.
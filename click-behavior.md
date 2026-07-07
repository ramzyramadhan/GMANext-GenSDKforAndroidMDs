Select platform: [Android](https://developers.google.com/admob/android/next-gen/browser/webview/click-behavior "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/browser/webview/click-behavior "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/browser/webview/click-behavior "View this page for the Android (Legacy) platform docs.")

<br />

If your app utilizes
[`WebView`](https://developer.android.com/reference/android/webkit/WebView) to display web content, you might want
to consider optimizing click behavior for the following reasons:

- [`WebView`](https://developer.android.com/reference/android/webkit/WebView) doesn't support [tabbed browsing](https://developer.android.com/develop/ui/views/layout/webapps/webview#HandlingNavigation). Clicking a link opens the content in the default web browser.

<!-- -->

- `WebView` doesn't support [custom URL schemes](https://developer.android.com/develop/ui/views/layout/webapps/webview#custom-urls) which could be returned in ads if the click destination is to a separate app. For example, a Google Play click-through URL might use `market://`.


- [Google Sign-in](https://developers.googleblog.com/2021/06/upcoming-security-changes-to-googles-oauth-2.0-authorization-endpoint.html) and [Facebook Sign-in](https://developers.facebook.com/docs/facebook-login/android/deprecating-webviews#deprecating-facebook-login-support-on-android-webviews) aren't supported in `WebView`.

This guide provides recommended steps to optimize the click behavior in mobile
web views while preserving the web view content.

## Prerequisites

- Complete the [Set up the web view](https://developers.google.com/admob/android/next-gen/browser/webview) guide.

## Implementation

Follow these steps to optimize the click behavior in your `WebView`
instance:

1. Override [`shouldOverrideUrlLoading()`](https://developer.android.com/reference/android/webkit/WebViewClient#shouldOverrideUrlLoading(android.webkit.WebView,%20android.webkit.WebResourceRequest))
   on the [`WebViewClient`](https://developer.android.com/reference/kotlin/android/webkit/WebViewClient).
   This method is called when a URL is about to be loaded in the current
   `WebView`.

2. Determine whether to override the behavior of the click URL.

   The [code example](https://developers.google.com/admob/android/next-gen/browser/webview/click-behavior#code_example) checks if the current domain is different
   than the target domain. This is just one approach as the criteria you use
   could vary.
3. Decide whether to open the URL in an external browser, [Android Custom
   Tabs](https://developer.chrome.com/docs/android/custom-tabs/), or within the
   existing web view. This guide shows how to open URLs navigating away from
   the site by launching Android Custom Tabs.


## Code example

First add the `androidx.browser` dependency to your module-level `build.gradle`
file, typically `app/build.gradle`. This is required for Custom Tabs:

    dependencies {
      implementation 'androidx.browser:browser:1.5.0'
    }

The following code snippet shows how to implement `shouldOverrideUrlLoading()`:

### Java

    public class MainActivity extends AppCompatActivity {

      private WebView webView;

      protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // ... Register the WebView.

        webView = new WebView(this);
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);
        webView.setWebViewClient(
            new WebViewClient() {
              // 1. Implement the web view click handler.
              @Override
              public boolean shouldOverrideUrlLoading(
                  WebView view,
                  WebResourceRequest request) {
                // 2. Determine whether to override the behavior of the URL.
                // If the target URL has no host and no scheme, return early.
                if (request.getUrl().getHost() == null && request.getUrl().getScheme() == null) {
                  return false;
                }

                // Handle custom URL schemes such as market:// by attempting to
                // launch the corresponding application in a new intent.
                if (!request.getUrl().getScheme().equals("http")
                    && !request.getUrl().getScheme().equals("https")) {
                  Intent intent = new Intent(Intent.ACTION_VIEW, request.getUrl());
                  // If the URL cannot be opened, return early.
                  try {
                    MainActivity.this.startActivity(intent);
                  } catch (ActivityNotFoundException exception) {
                    Log.d("TAG", "Failed to load URL with scheme:" + request.getUrl().getScheme());
                  }
                  return true;
                }

                String currentDomain;
                // If the current URL's host cannot be found, return early.
                try {
                  currentDomain = new URI(view.getUrl()).toURL().getHost();
                } catch (URISyntaxException | MalformedURLException exception) {
                  // Malformed URL.
                  return false;
                }
                String targetDomain = request.getUrl().getHost();

                // If the current domain equals the target domain, the
                // assumption is the user is not navigating away from
                // the site. Reload the URL within the existing web view.
                if (currentDomain.equals(targetDomain)) {
                  return false;
                }

                // 3. User is navigating away from the site, open the URL in
                // Custom Tabs to preserve the state of the web view.
                CustomTabsIntent intent = new CustomTabsIntent.Builder().build();
                intent.launchUrl(MainActivity.this, request.getUrl());
                return true;
              }
            });
      }
    }

### Kotlin

    class MainActivity : AppCompatActivity() {

      private lateinit var webView: WebView

      override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // ... Register the WebView.

        webView.webViewClient = object : WebViewClient() {
          // 1. Implement the web view click handler.
          override fun shouldOverrideUrlLoading(
              view: WebView?,
              request: WebResourceRequest?
          ): Boolean {
            // 2. Determine whether to override the behavior of the URL.
            // If the target URL has no host and no scheme, return early.
            if (request?.url?.host == null && request.url.scheme == null) {
              return false
            }
            val currentDomain = URI(view?.url).toURL().host

            // Handle custom URL schemes such as market:// by attempting to
            // launch the corresponding application in a new intent.
            if (!request.url.scheme.equals("http") &&
                !request.url.scheme.equals("https")) {
              val intent = Intent(Intent.ACTION_VIEW, request.url)
              // If the URL cannot be opened, return early.
              try {
                this@MainActivity.startActivity(intent)
              } catch (exception: ActivityNotFoundException) {
                Log.d("TAG", "Failed to load URL with scheme: ${request.url.scheme}")
              }
              return true
            }

            val targetDomain = request.url.host

            // If the current domain equals the target domain, the
            // assumption is the user is not navigating away from
            // the site. Reload the URL within the existing web view.
            if (currentDomain.equals(targetDomain)) {
              return false
            }

            // 3. User is navigating away from the site, open the URL in
            // Custom Tabs to preserve the state of the web view.
            val customTabsIntent = CustomTabsIntent.Builder().build()
            customTabsIntent.launchUrl(this@MainActivity, request.url)
            return true
          }
        }
      }
    }

### Test your page navigation

To test your page navigation changes, load

    https://google.github.io/webview-ads/test#click-behavior-tests

into your web view. Click each of the different link types to see how they
behave in your app.

Here are some things to check:

- Each link opens the intended URL.
- When returning to the app, the test page's counter doesn't reset to zero to validate the page state was preserved.
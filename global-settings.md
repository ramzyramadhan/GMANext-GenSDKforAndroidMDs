Select platform: [Android](https://developers.google.com/admob/android/next-gen/global-settings "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/global-settings "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/global-settings "View this page for the Unity platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/global-settings "View this page for the Android (Legacy) platform docs.")

<br />

The `MobileAds` class provides global settings for GMA Next-Gen SDK.

## Video ad volume control

If your app has its own volume controls (such as custom music or sound effect
volumes), disclosing app volume to GMA Next-Gen SDK allows video ads to
respect app volume settings. This ensures users receive video ads with the
expected audio volume.

The device volume, controlled through volume buttons or OS-level volume slider,
determines the volume for device audio output. However, apps can independently
adjust volume levels relative to the device volume to tailor the audio
experience. For app open, banner, interstitial, rewarded, and rewarded
interstitial ad formats, you can report the relative app volume to the SDK
through the static `setUserControlledAppVolume()` method. Valid ad volume values range from
`0.0` (silent) to `1.0` (current device volume). Here's an example of how to
report the relative app volume to the SDK:

### Kotlin

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

### Java

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
                    initializationStatus -> {
                    });
                
                // Set app volume to be half of current device volume.
                MobileAds.setUserControlledAppVolume(0.5f);
              })
          .start();
    }

To inform the SDK that the app volume has muted, use the `setUserMutedApp()`
method:

### Kotlin

    MobileAds.setUserMutedApp(true)

### Java

    MobileAds.setUserMutedApp(true);

By default, the app volume is set to `1` (the current device volume), and the
app is not muted.

> [!NOTE]
> **Note:** Video ads that are ineligible to be shown with muted audio are not returned for ad requests made when the app volume is reported as muted or set to a value of `0`. This may restrict a subset of the broader video ads pool from serving.

## Consent for cookies

If your app has special requirements, you can set the optional
[`SharedPreferences`](https://developer.android.com/reference/android/content/SharedPreferences)
`gad_has_consent_for_cookies`. The SDK will enable

[limited ads (LTD)](https://support.google.com/admob/answer/10105530)

when the `gad_has_consent_for_cookies` preference is set to zero.

### Kotlin

    val sharedPrefs = PreferenceManager.getDefaultSharedPreferences(context)
    // Set the value to 0 to enable limited ads.
    sharedPrefs.edit().putInt("gad_has_consent_for_cookies", 0).apply()

### Java

    Context activity = getActivity();
    SharedPreferences sharedPreferences =
      PreferenceManager.getDefaultSharedPreferences(activity);
    // Set the value to 0 to enable limited ads.
    sharedPreferences.edit().putInt("gad_has_consent_for_cookies", 0).apply();
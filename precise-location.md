Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy/precise-location "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy/precise-location "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy/precise-location "View this page for the Android (Legacy) platform docs.")

<br />

Recent updates to the [Google Publisher
Policies](https://support.google.com/adsense/answer/9335564#use_of_device_and_location_data)
have introduced new notice and consent requirements for publishers who pass
precise location data of users to Google, for ads-related purposes.

If this policy applies to you, the following snippet shows one way you could
inform your users of this data sharing:

### Kotlin

```kotlin
protected fun presentConsentOverlay(context: Context) {
  AlertDialog.Builder(context)
      .setTitle("Location data")
      .setMessage("We may use your location, " +
          "and share it with third parties, " +
          "for the purposes of personalized advertising, " +
          "analytics, and attribution. " +
          "To learn more, visit our privacy policy " +
          "at https://myapp.com/privacy.")
      .setNeutralButton("OK") { dialog, which ->
        dialog.cancel()
        // TODO: replace the below log statement with code that specifies how
        // you want to handle the user's acknowledgement.
        Log.d("MyApp", "Got consent.")
      }
      .show()
}

// To use the above function:
presentConsentOverlay(this)
```

### Java

```java
protected void presentConsentOverlay(Context context) {
  new AlertDialog.Builder(context)
      .setTitle("Location data")
      .setMessage("We may use your location, " +
          "and share it with third parties, " +
          "for the purposes of personalized advertising, " +
          "analytics, and attribution. " +
          "To learn more, visit our privacy policy " +
          "at https://myapp.com/privacy.")
  .setNeutralButton("OK", new DialogInterface.OnClickListener() {
    @Override
    public void onClick(DialogInterface dialog, int which) {
      dialog.cancel();
      // TODO: replace the below log statement with code that specifies how
      // you want to handle the user's acknowledgement.
      Log.d("MyApp", "Got consent.");
    }
  })
  .show();
}

// To use the above method:
presentConsentOverlay(this);
```

> [!IMPORTANT]
> **Key Point:** This snippet is only an example. Make sure to customize the snippet to accurately reflect your data sharing practices, so users are informed of all the relevant purposes for which you share their precise location data.
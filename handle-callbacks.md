[Video](https://www.youtube.com/watch?v=a94TRVp-b1k)


**Last updated:** January 10, 2025

<br />

This page covers the instructions to handle callbacks from a background thread.

GMA Next-Gen SDK runs ad load and event callbacks on a background
thread. When performing UI-related operations within these callbacks, ensure you
explicitly dispatch them to the UI thread.

The following examples show a toast after an ad loads:

### Kotlin

    adView.loadAd(
      adRequest,
      object : AdLoadCallback<BannerAd> {
        override fun onAdLoaded(ad: BannerAd) {
          // Show a toast on the UI thread.
          runOnUiThread {
            Toast.makeText(activity, "Ad loaded.", Toast.LENGTH_SHORT).show()
          }
        }
      },
    )

### Java

    adView.loadAd(
        adRequest,
        new AdLoadCallback<BannerAd>() {
            @Override
            public void onAdLoaded(@NonNull BannerAd ad) {
                // Show a toast on the UI thread.
                runOnUiThread(() ->
                    Toast.makeText(activity, "Ad loaded.", Toast.LENGTH_SHORT).show()
                );
            }
        });
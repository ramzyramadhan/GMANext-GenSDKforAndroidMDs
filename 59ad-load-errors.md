Select platform: [Android](https://developers.google.com/admob/android/next-gen/ad-load-errors "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/ad-load-errors "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/ad-load-errors "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/ad-load-errors "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/ad-load-errors "View this page for the Android (Legacy) platform docs.")

<br />

In cases where an ad fails to load, a
callback
is called which provides a
`LoadAdError` object.

The following example shows the information available when an ad fails to load:

### Kotlin

    override fun onAdFailedToLoad(adError: LoadAdError) {
      // Gets the error code. See
      // https://developers.google.com/admob/android/early-access/nextgen/reference/com/google/android/libraries/ads/mobile/sdk/common/LoadAdError.ErrorCode
      // for a list of possible codes.
      val errorCode = adError.code
      // Gets an error message.
      // For example "Account not approved yet". See
      // https://support.google.com/admob/answer/9905175 for explanations of
      // common errors.
      val errorMessage = adError.message
      // Gets additional response information about the request. See
      // https://developers.google.com/admob/android/next-gen/response-info
      // for more information.
      val responseInfo = adError.responseInfo
      // All of this information is available using the error's toString() method.
      Log.d("Ads", adError.toString())
    }

### Java

    @Override
    public void onAdFailedToLoad(@NonNull LoadAdError adError) {
      // Gets the error code. See
      // https://developers.google.com/admob/android/early-access/nextgen/reference/com/google/android/libraries/ads/mobile/sdk/common/LoadAdError.ErrorCode
      // for a list of possible codes.
      ErrorCode errorCode = adError.getCode();
      // Gets an error message.
      // For example "Account not approved yet". See
      // https://support.google.com/admob/answer/9905175 for explanations of
      // common errors.
      String errorMessage = adError.getMessage();
      // Gets additional response information about the request. See
      // https://developers.google.com/admob/android/next-gen/response-info
      // for more information.
      ResponseInfo responseInfo = adError.getResponseInfo();
      // All of this information is available using the error's toString() method.
      Log.d("Ads", adError.toString());
    }
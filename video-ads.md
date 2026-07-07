Select platform: [Android](https://developers.google.com/admob/android/next-gen/native/video-ads "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/native/video-ads "View this page for the iOS platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/native/video-ads "View this page for the Android (Legacy) platform docs.")

<br />

### MediaContent

Native ads provide access to a [`MediaContent`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/MediaContent) object that is used to get information about media content, which could be video or an image. It is also used to control video ad playback and listen for playback events. You can obtain the `MediaContent` object by calling [`NativeAd.getMediaContent()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/nativead/NativeAd#getMediaContent()).

<br />

The `MediaContent` object contains information such as the aspect ratio and
duration of a video. The following snippet shows how to get the aspect ratio and
duration of a native ad.

### Java

    if (nativeAd.getMediaContent() != null) {
      MediaContent mediaContent = nativeAd.getMediaContent();
      float mediaAspectRatio = mediaContent.getAspectRatio();
      if (mediaContent.hasVideoContent()) {
        float duration = mediaContent.getDuration();
      }
    }

### Kotlin

    nativeAd.mediaContent?.let { mediaContent ->
      val mediaAspectRatio: Float = mediaContent.aspectRatio
      if (mediaContent.hasVideoContent()) {
        val duration: Float = mediaContent.duration
      }
    }

### Callbacks for video events

To handle specific video events, write a class that extends the abstract
`VideoLifecycleCallbacks` class, and call
[`setVideoLifecycleCallbacks()`](https://developers.google.com/admob/android/next-gen/reference/com/google/android/libraries/ads/mobile/sdk/common/VideoController#etVideoLifecycleCallbacks(com.google.android.libraries.ads.mobile.sdk.common.VideoController.VideoLifecycleCallbacks))
on the `VideoController`. Then, override only the callbacks you care about.

### Java

    if (nativeAd.getMediaContent() != null) {
      VideoController videoController = nativeAd.getMediaContent().getVideoController();
      if (videoController != null) {
        videoController.setVideoLifecycleCallbacks(
            new VideoController.VideoLifecycleCallbacks() {
              @Override
              public void onVideoStart() {
                Log.d(TAG, "Video started.");
              }

              @Override
              public void onVideoPlay() {
                Log.d(TAG, "Video played.");
              }

              @Override
              public void onVideoPause() {
                Log.d(TAG, "Video paused.");
              }

              @Override
              public void onVideoEnd() {
                Log.d(TAG, "Video ended.");
              }

              @Override
              public void onVideoMute(boolean isMuted) {
                Log.d(TAG, "Video isMuted: " + isMuted + ".");
              }
            });
      }
    }

### Kotlin

    val videoLifecycleCallbacks =
      object : VideoController.VideoLifecycleCallbacks() {
        override fun onVideoStart() {
          Log.d(TAG, "Video started.")
        }

        override fun onVideoPlay() {
          Log.d(TAG, "Video played.")
        }

        override fun onVideoPause() {
          Log.d(TAG, "Video paused.")
        }

        override fun onVideoEnd() {
          Log.d(TAG, "Video ended.")
        }

        override fun onVideoMute(isMuted: Boolean) {
          Log.d(TAG, "Video isMuted: $isMuted.")
        }
      }
    nativeAd.mediaContent?.videoController?.videoLifecycleCallbacks = videoLifecycleCallbacks
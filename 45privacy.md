Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/privacy "View this page for the Unity platform docs.") [Flutter](https://developers.google.com/admob/flutter/privacy "View this page for the Flutter platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy "View this page for the Android (Legacy) platform docs.")

<br />

[Video](https://www.youtube.com/watch?v=SysASyh9XKo)

The Google User Messaging Platform (UMP) SDK is a privacy and messaging tool to
help you manage privacy choices. For more information, see

[About Privacy \& messaging](https://support.google.com/admob/answer/10107561).


## Create a message type

Create user messages with one of the

[Available user message types](https://support.google.com/admob/answer/10114020)

under the **Privacy \& messaging** tab of your

AdMob

account. The UMP SDK attempts to display a
privacy message created from the AdMob Application ID
set in your project.

For more details, see

[About privacy and messaging](https://support.google.com/admob/answer/10107561).


## Add the application ID

You can find your application ID in the

[AdMob UI](https://support.google.com/admob/answer/7356431#find-an-app-id).

Add the ID to your

`AndroidManifest.xml`

with the following code snippet:

    <manifest>
      <application>
        \<meta-data
    android:name="com.google.android.gms.ads.APPLICATION_ID"
    android:value="ca-app-pub-xxxxxxxxxxxxxxxx\~yyyyyyyyyy"/\>
      </application>
    </manifest>

> [!IMPORTANT]
> **Key Point:** When you use the UMP SDK with GMA Next-Gen SDK, you must provide the app ID in two locations.

## Get the user's consent information

To get the user's consent information, do the following:

Declare an instance of `ConsentInformation`:

### Java

    private final ConsentInformation consentInformation;

### Kotlin

    private lateinit val consentInformation: ConsentInformation

Initialize the `ConsentInformation` instance:

### Java

    consentInformation = UserMessagingPlatform.getConsentInformation(context);

### Kotlin

    consentInformation = UserMessagingPlatform.getConsentInformation(context)

You should request an update of the user's consent information at every app
launch, using `
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#requestConsentInfoUpdate(android.app.Activity,com.google.android.ump.ConsentRequestParameters,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateSuccessListener,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateFailureListener)`. This request checks the following:

- **Whether consent is required**. For example, consent is required for the first time, or the previous consent decision expired.
- **Whether a privacy options entry point is required**. Some privacy messages require apps to allow users to modify their privacy options at any time.

### Java


    // Requesting an update to consent information should be called on every app launch.
    consentInformation.requestConsentInfoUpdate(
        activity,
        params,
        () -> // Called when consent information is successfully updated.
        requestConsentError -> // Called when there's an error updating consent information.

### Kotlin


    // Requesting an update to consent information should be called on every app launch.
    consentInformation.requestConsentInfoUpdate(
      activity,
      params,
      {
        // Called when consent information is successfully updated.
      },
      { requestConsentError ->
        // Called when there's an error updating consent information.
      },
    )

> [!WARNING]
> **Warning:** Using other ways of checking consent status, such as the cache on your app or a previously saved consent string, can lead to a [TCF 3.3 error](https://support.google.com/admanager/answer/9999955) if consent is expired.

## Load and present the privacy message form

After you have received the most up-to-date consent status, call
`
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/UserMessagingPlatform#loadAndShowConsentFormIfRequired(android.app.Activity,%20com.google.android.ump.ConsentForm.OnConsentFormDismissedListener)` to load any forms required to
collect user consent. After loading, the forms present immediately.

> [!IMPORTANT]
> **Key Point:** If no privacy message forms require collection of user consent prior to requesting ads, the callback is invoked immediately.

### Java


    UserMessagingPlatform.loadAndShowConsentFormIfRequired(
        activity,
        formError -> {
          // Consent gathering process is complete.
        });

### Kotlin


    UserMessagingPlatform.loadAndShowConsentFormIfRequired(activity) { formError ->
      // Consent gathering process is complete.
    }

## Privacy options

Some privacy message forms are presented from a publisher-rendered privacy
options entry point, letting users manage their privacy options at any time.
To learn more about which message your users see at the privacy options
entry point, see

[Available user message types](https://support.google.com/admob/answer/10114020).


### Check if a privacy options entry point is required

After you have called `
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#requestConsentInfoUpdate(android.app.Activity,com.google.android.ump.ConsentRequestParameters,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateSuccessListener,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateFailureListener)`, check `
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#getPrivacyOptionsRequirementStatus()` to
determine if a privacy options entry point is required for your app. If an entry
point is required, add a visible and interactable UI element to your app that
presents the privacy options form. If a privacy entry point is not required,
configure your UI element to be not visible and interactable.

### Java


    /** Helper variable to determine if the privacy options form is required. */
    public boolean isPrivacyOptionsRequired() {
      return consentInformation.getPrivacyOptionsRequirementStatus()
          == PrivacyOptionsRequirementStatus.REQUIRED;
    }

### Kotlin


    /** Helper variable to determine if the privacy options form is required. */
    val isPrivacyOptionsRequired: Boolean
      get() =
        consentInformation.privacyOptionsRequirementStatus ==
          ConsentInformation.PrivacyOptionsRequirementStatus.REQUIRED

For the full list of privacy options requirement statuses, see
`
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation.PrivacyOptionsRequirementStatus`.

### Present the privacy options form

When the user interacts with your element, present the privacy options form:

### Java


    UserMessagingPlatform.showPrivacyOptionsForm(activity, onConsentFormDismissedListener);

### Kotlin


    UserMessagingPlatform.showPrivacyOptionsForm(activity, onConsentFormDismissedListener)

<br />

## Request ads with user consent

Before requesting ads, use `
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#canRequestAds()` to check if you've
obtained consent from the user:

### Java

    consentInformation.canRequestAds();

### Kotlin

    consentInformation.canRequestAds()

Listed are the following places to check if you can request ads while gathering
consent:

- After the UMP SDK gathers consent in the current session.
- Immediately after you have called `
  https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#requestConsentInfoUpdate(android.app.Activity,com.google.android.ump.ConsentRequestParameters,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateSuccessListener,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateFailureListener)`. The UMP SDK might have obtained consent in the previous app session.

> [!IMPORTANT]
> **Important:** `
> https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#canRequestAds()` always returns `false` until you have called `
> https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#requestConsentInfoUpdate(android.app.Activity,com.google.android.ump.ConsentRequestParameters,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateSuccessListener,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateFailureListener)`

If an error occurs during the consent gathering process, check if you can
request ads. The UMP SDK uses the consent status from the previous app session.

#### Prevent redundant ad request work

As you check `
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#canRequestAds()` after gathering consent and after calling
`
https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#requestConsentInfoUpdate(android.app.Activity,com.google.android.ump.ConsentRequestParameters,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateSuccessListener,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateFailureListener)`, ensure your logic prevents redundant ad requests that
might result in both checks returning `true`. For example, with a boolean
variable.

<br />

## Testing

If you want to test the integration in your app as you're developing, follow
these steps to programmatically register your test device. Be sure to remove the
code that sets these test device IDs before you release your app.

> [!NOTE]
> **Note:** As of version 2.2.0, emulators don't need to be added to your device ID list as they are test devices by default.

1. Call `
   https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentInformation#requestConsentInfoUpdate(android.app.Activity,com.google.android.ump.ConsentRequestParameters,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateSuccessListener,com.google.android.ump.ConsentInformation.OnConsentInfoUpdateFailureListener)`.
2. Check the log output for a message similar to the following example, which
   shows your device ID and how to add it as a test device:

       Use new ConsentDebugSettings.Builder().addTestDeviceHashedId("33BE2250B43518CCDA7DE426D04EE231") to set this as a debug device.

3. Copy your test device ID to your clipboard.

4. Modify your code to call
   `ConsentDebugSettings.Builder().TestDeviceHashedIds` and pass in
   a list of your test device IDs.

   ### Java

       ConsentDebugSettings debugSettings = new ConsentDebugSettings.Builder(this)
           .addTestDeviceHashedId("TEST-DEVICE-HASHED-ID")
           .build();

       ConsentRequestParameters params = new ConsentRequestParameters
           .Builder()
           .setConsentDebugSettings(debugSettings)
           .build();

       consentInformation = UserMessagingPlatform.getConsentInformation(this);
       // Include the ConsentRequestParameters in your consent request.
       consentInformation.requestConsentInfoUpdate(
           this,
           params,
           // ...
       );

   ### Kotlin

       val debugSettings = ConsentDebugSettings.Builder(this)
           .addTestDeviceHashedId("TEST-DEVICE-HASHED-ID")
           .build()

       val params = ConsentRequestParameters
           .Builder()
           .setConsentDebugSettings(debugSettings)
           .build()

       consentInformation = UserMessagingPlatform.getConsentInformation(this)
       // Include the ConsentRequestParameters in your consent request.
       consentInformation.requestConsentInfoUpdate(
           this,
           params,
           // ...
       )

### Force a geography

The UMP SDK provides a way to test your app's behavior as though the device
were located in various regions, such as the European Economic Area (EEA), the
United Kingdom (UK), and Switzerland using
[`setDebugGeography()`](https://developers.google.com/admob/android/privacy/api/reference/com/google/android/ump/ConsentDebugSettings.Builder.html#setDebugGeography(int)). Note that
debug settings only work on test devices.

### Java

    ConsentDebugSettings debugSettings = new ConsentDebugSettings.Builder(this)
        .setDebugGeography(ConsentDebugSettings.DebugGeography.DEBUG_GEOGRAPHY_EEA)
        .addTestDeviceHashedId("TEST-DEVICE-HASHED-ID")
        .build();

    ConsentRequestParameters params = new ConsentRequestParameters
        .Builder()
        .setConsentDebugSettings(debugSettings)
        .build();

    consentInformation = UserMessagingPlatform.getConsentInformation(this);
    // Include the ConsentRequestParameters in your consent request.
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        ...
    );

### Kotlin

    val debugSettings = ConsentDebugSettings.Builder(this)
        .setDebugGeography(ConsentDebugSettings.DebugGeography.DEBUG_GEOGRAPHY_EEA)
        .addTestDeviceHashedId("TEST-DEVICE-HASHED-ID")
        .build()

    val params = ConsentRequestParameters
        .Builder()
        .setConsentDebugSettings(debugSettings)
        .build()

    consentInformation = UserMessagingPlatform.getConsentInformation(this)
    // Include the ConsentRequestParameters in your consent request.
    consentInformation.requestConsentInfoUpdate(
        this,
        params,
        ...
    )

### Reset consent state

When testing your app with the UMP SDK, you might find it helpful to reset the
state of the SDK so that you can simulate a user's first install experience.
The SDK provides the `reset()` method to do this.

### Java

    consentInformation.reset();

### Kotlin

    consentInformation.reset()

> [!CAUTION]
> **Caution:** This method is intended to be used for testing purposes only. You shouldn't call `reset()` in production code.

<br />
Select platform: [Android](https://developers.google.com/admob/android/next-gen/privacy/sync-consent "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/privacy/sync-consent "View this page for the iOS platform docs.") [Unity](https://developers.google.com/admob/unity/privacy/sync-consent "View this page for the Unity platform docs.") [Android (Legacy)](https://developers.google.com/admob/android/privacy/sync-consent "View this page for the Android (Legacy) platform docs.")

<br />

To reduce redundant GDPR messages for your users,

[Sync consent](https://support.google.com/admob/answer/16472823)

across multiple apps. When a user makes a consent decision in a consent-syncing
enabled app, this choice is stored using a consent sync identifier you provide.
That consent decision automatically applies across all other apps that share the
same consent sync identifier. Only Google uses this identifier to store and
retrieve a user's consent decision.

This guide covers syncing GDPR consent from the User Messaging Platform
(UMP) SDK in your mobile app.

## Prerequisites

Before you begin, do the following:

- [Set up UMP SDK](https://developers.google.com/admob/android/next-gen/privacy).
- Enable consent syncing for eligible apps in the **Privacy \& Messaging** tab of the AdMob UI.

## Set the consent sync identifier

Across the apps where you're able to identify the user, provide the consent sync
ID to the UMP SDK. If your app doesn't have a user identifier, use other
identifiers to identify the user across apps, such as

the [App Set ID](https://developer.android.com/training/articles/app-set-id) APIs.


Set the consent sync ID on the

`ConsentRequestParameters`

object:

### Kotlin

    import com.google.android.gms.appset.AppSet
    import com.google.android.gms.appset.AppSetIdInfo

    // Example fetching App Set ID to identify the user across apps.
    val client = AppSet.getClient(this)
    client.appSetIdInfo.addOnSuccessListener { info: AppSetIdInfo ->
      val appSetId = info.id
      val params = ConsentRequestParameters.Builder().setConsentSyncId(appSetId).build()
    }

### Java

    import com.google.android.gms.appset.AppSet;
    import com.google.android.gms.appset.AppSetIdClient;

    // Example fetching App Set ID to identify the user across apps.
    AppSetIdClient client = AppSet.getClient(this);
    client.getAppSetIdInfo().addOnSuccessListener(
      info -> {
        String appSetId = info.getId();
        ConsentRequestParameters params =
            new ConsentRequestParameters.Builder().setConsentSyncId(appSetId).build();
      }
    );

## Consent sync identifier format

The identifier you provide must uniquely identify the user across all of your
apps where consent is being synced. Hash or encrypt the identifier to prevent
sending personally identifiable information (PII) to Google.

The provided ID must meet the following requirements:

- Constructed as a UUID string or matches regular expression `^[0-9a-zA-Z+.=\/_\-$,{}]{22,150}$`.
- A minimum of 22 characters.
- A maximum of 150 characters.

The following are examples of correct consent sync IDs:

- `12JD92JD8078S8J29SDOAKC0EF230337`
- `12jd92jd8078s8j29sdoakc0ef230337`
- `12Jd92jD8078s8j29sDoakc0ef230337`
- `123e4567-e89b-12d3-a456-426614174000`

Failure to meet the requirements results in the consent sync ID not being set
and the UMP SDK logging a warning to the console
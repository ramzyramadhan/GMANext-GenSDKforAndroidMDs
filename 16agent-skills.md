Select platform: [Android](https://developers.google.com/admob/android/next-gen/agent-skills "View this page for the Android platform docs.") [iOS](https://developers.google.com/admob/ios/agent-skills "View this page for the iOS platform docs.")

<br />

[Agent skills](https://antigravity.google/docs/skills) for GMA Next-Gen SDK are
portable, self-contained modules of GMA Next-Gen SDK knowledge, instructions, and
workflows. These skills are designed to help AI assistants understand
GMA Next-Gen SDK and run complex tasks with higher accuracy. For all available
agent skills, see the GitHub repository
[`google-mobile-ads`](https://github.com/google/skills/tree/main/skills/ads/google-mobile-ads).

This page covers installing and updating agent skills.

## Install the agent skills

Agent skills for GMA Next-Gen SDK work with AI assistants that support skills,
including Antigravity, Claude Code, Codex, and Cursor.

The following example installs all agent skills for GMA Next-Gen SDK:

    npx skills add google/skills/ads/google-mobile-ads --skill '*' --agent universal --yes

The `--agent universal` flag puts the skill in the standard `.agents/skills`
folder that agents use.

## Update the agent skills

To make sure AI assistant has the latest GMA Next-Gen SDK features and
recommendations, update your agent skills. The following example updates all
agent skills for GMA Next-Gen SDK:

    npx skills update --all

## View available skills

The following table describes all available skills for GMA Next-Gen SDK features:

| Skill | Description |
|---|---|
| `google-mobile-ads-get-started` | Assists with adding the Google Mobile Ads SDK (Legacy) to your app for the first time, setting the application ID, and initializing GMA Next-Gen SDK. |
| `google-mobile-ads-banner` | Assists with implementing banner ads, such as large anchored adaptive banners and inline adaptive banners. |
| `google-mobile-ads-interstitial` | Assists with implementing full-screen interstitial ads and showing the ads at natural transition points in your app. |
| `google-mobile-ads-rewarded` | Assists with implementing rewarded ads, showing the ads, and handling user reward callbacks. |
| `google-mobile-ads-android-migrate-to-next-gen` | Assists with migrating an existing Android app from Google Mobile Ads SDK (Legacy) to GMA Next-Gen SDK. |

You can implement agent skills for GMA Next-Gen SDK by prompting your preferred
AI assistant. The agent skills can guide your AI assistant to perform
the following example tasks:

- *Help me initialize the GMA Next-Gen SDK*.
- *Add a banner ad to my app*.
- *Implement a rewarded ad*.
To optimize migrating from Google Mobile Ads SDK (Legacy) to GMA Next-Gen SDK with your AI
tool, add a skill to your AI environment. By adding a skill, you provide your AI
tool with context specific to GMA Next-Gen SDK and improve the output of
AI-assisted code generation. For a full list of the Google Mobile Ads agent
skills, see [Agent skills](https://github.com/google/skills/tree/main/skills/ads/google-mobile-ads).

This guide covers optimizing your AI model to help migrate from
Google Mobile Ads SDK (Legacy) to GMA Next-Gen SDK.

## Install the migration skill

The migration skill works with AI assistants that support skills,
including Antigravity, Claude Code, Codex, and Cursor.

To install all skills into your project, run the following command. The
`--agent universal` flag puts the skill in the standard `.agents/skills` folder
that agents use.

    npx skills add google/skills/skills/ads/google-mobile-ads --skill google-mobile-ads-android-migrate-to-next-gen
     --agent universal --yes

> [!IMPORTANT]
> **Important:** Agent skills are actively developed. To ensure your AI assistant has the latest features and recommendations, regularly [update the migration skill](https://developers.google.com/admob/android/next-gen/migration/migrate-with-ai-tools#update_the_migration_skill).

### Update the migration skill

You can update skills by running the following command:

    npx skills update --all

### Use the migration skill

AI assistants are designed to use the migration skill automatically.
These assistants detect that the skill's description corresponds to your
request. Additionally, you can invoke the skill by typing the values `@` or `/`
in the agent box and search for the skill name.

After you install the skill, use the following example prompt to invoke
the skill in your AI assistant's box:

    Migrate the files in my project from Google Mobile Ads SDK (Legacy) to GMA Next-Gen SDK

## Leave feedback

We are continuing to evaluate and optimize context provided to AI code assist
tools to improve their responses on GMA Next-Gen SDK topics.

If you have feedback on optimizing Gemini for GMA Next-Gen SDK, join the
[GMA Next-Gen SDK Discord channel](https://discord.com/invite/FupESdPG).
---
name: aso-config
description: Configure the ASO plugin — set your app ID, competitors, keywords, and country
type: user-invocable
---

## Purpose

Set up or update the ASO plugin configuration. The config is saved to `$ASO_CONFIG_PATH` so it persists across sessions and plugin updates.

## Workflow

1. **Check for existing config:** Read the file at the `ASO_CONFIG_PATH` environment variable path. If it exists, show the current values to the user.

2. **Collect values from the user.** Ask for each field:
   - **App ID** — The user's App Store app ID (numeric, e.g. `1234567890`)
   - **Competitors** — A list of competitor apps, each with a name and App Store ID
   - **Keywords** — A list of keywords to track rankings for
   - **Country** — Two-letter country code (default: `us`)

   If updating an existing config, only ask about fields the user wants to change.

3. **Write the config file** to the path in `ASO_CONFIG_PATH`. The JSON format is:

```json
{
  "myAppId": "1234567890",
  "competitors": [
    { "name": "Competitor A", "id": "1111111111" },
    { "name": "Competitor B", "id": "2222222222" }
  ],
  "keywords": ["keyword one", "keyword two", "keyword three"],
  "country": "us"
}
```

4. **Confirm** that the config was saved and suggest running `/aso-audit` or `/keyword-research` to get started.

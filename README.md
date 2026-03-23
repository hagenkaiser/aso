# ASO Intelligence

An MCP (Model Context Protocol) server that brings real App Store data directly into Claude — plus 28 ASO methodology skills for a complete App Store Optimization workflow.

Ask Claude to run an ASO audit, research keywords, track competitors, or optimize metadata, and it pulls live data from the App Store and Apple Search Ads automatically.

---

## What It Does

**Tracks** keyword rankings across your app and competitors, updated on demand.

**Measures** Apple Search Ads keyword popularity scores (0–5) directly from Apple's API.

**Compares** snapshots over time to surface ranking movements, metadata changes, and rating shifts.

**Guides** every ASO decision with 28 built-in skills — from keyword research to UA campaigns to subscription lifecycle optimization.

---

## MCP Tools

Five tools are available to Claude through the MCP server:

| Tool | Description |
|------|-------------|
| `collect_data` | Fetch fresh App Store data for all tracked apps and keywords. Saves a timestamped snapshot. |
| `get_rankings` | Show the latest keyword ranking table — positions for each tracked app per keyword. |
| `get_keyword_popularity` | Show Apple Search Ads popularity scores (0–5) for all tracked keywords. |
| `get_app_metadata` | Side-by-side metadata comparison — name, rating, price, version, update date, category. |
| `compare_snapshots` | Diff two snapshots to see ranking changes, rating shifts, and metadata updates. |

---

## Skills

28 ASO skills live in `.claude/skills/`. Claude picks them up automatically in this project.

**Data-enhanced** — these skills call MCP tools to pull live data before analyzing:

| Skill | MCP Tools Used |
|-------|---------------|
| `/aso-audit` | `get_rankings` · `get_keyword_popularity` · `get_app_metadata` |
| `/keyword-research` | `get_keyword_popularity` · `get_rankings` |
| `/competitor-analysis` | `get_rankings` · `get_app_metadata` · `compare_snapshots` |
| `/competitor-tracking` | `compare_snapshots` |
| `/metadata-optimization` | `get_app_metadata` · `get_rankings` |
| `/market-pulse` | `get_rankings` · `compare_snapshots` · `get_keyword_popularity` |
| `/market-movers` | `compare_snapshots` |
| `/seasonal-aso` | `get_keyword_popularity` |
| `/apple-search-ads` | `get_keyword_popularity` · `get_rankings` |

**Framework skills** — methodology and frameworks, no live data required:

`/screenshot-optimization` · `/rating-prompt-strategy` · `/review-management` · `/monetization-strategy` · `/subscription-lifecycle` · `/onboarding-optimization` · `/app-store-featured` · `/in-app-events` · `/ab-test-store-listing` · `/app-launch` · `/localization` · `/retention-optimization` · `/ua-campaign` · `/press-and-pr` · `/app-clips` · `/crash-analytics` · `/app-analytics` · `/app-icon-optimization` · `/app-marketing-context`

---

## Setup

### 1. Install dependencies

```bash
npm install
```

### 2. Configure your apps and keywords

Edit `config.json`:

```json
{
  "myAppId": "123456789",
  "competitors": [
    { "name": "Competitor A", "id": "987654321" },
    { "name": "Competitor B", "id": "111222333" }
  ],
  "keywords": ["meditation app", "sleep sounds", "mindfulness"],
  "country": "us"
}
```

App IDs are the numeric IDs from App Store URLs: `apps.apple.com/app/id123456789`

### 3. (Optional) Apple Search Ads API for keyword popularity

Without this, popularity scores show as `?`. To enable:

1. In your Apple Search Ads account: **Settings → API → Create API Certificate**
2. Download the `.p8` private key, note your Client ID and Team ID
3. Set environment variables:

```bash
export SEARCH_ADS_KEY_PATH=/path/to/key.p8
export SEARCH_ADS_CLIENT_ID=SEARCHADS.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export SEARCH_ADS_TEAM_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### 4. Register the MCP server with Claude

Add to your Claude Code MCP config (`.claude/settings.json` or `~/Library/Application Support/Claude/claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "aso": {
      "command": "npx",
      "args": ["tsx", "/absolute/path/to/aso/src/server.ts"]
    }
  }
}
```

### 5. Collect your first snapshot

Either ask Claude to `collect_data`, or run directly:

```bash
npm run collect
```

Snapshots are saved to `data/` as timestamped JSON files. Run this daily or weekly to build up a history for trend analysis.

---

## Usage

Once the MCP server is connected, just talk to Claude:

```
/aso-audit
```
> Claude calls get_rankings, get_keyword_popularity, and get_app_metadata, then produces a scored audit across 10 factors with prioritized action items.

```
/competitor-tracking
```
> Claude calls compare_snapshots and produces a weekly change report — ranking shifts, metadata updates, and competitive opportunities.

```
/keyword-research
```
> Claude pulls popularity scores and ranking data, then outputs a keyword opportunity table with placement recommendations.

```
/screenshot-optimization
```
> No data needed — Claude walks through the screenshot strategy framework for your app.

---

## Data & Privacy

- All data is fetched from public App Store APIs (iTunes Search API) and the Apple Search Ads API using your own credentials
- Snapshots are stored locally in `data/` — excluded from git by default
- No data is sent to any third-party service

---

## Project Structure

```
aso/
├── src/
│   ├── server.ts        # MCP server — registers all 5 tools
│   ├── collect.ts       # Data collection + snapshot storage
│   ├── analyze.ts       # Ranking table formatting + snapshot diffing
│   ├── itunes.ts        # iTunes Search API client
│   └── searchads.ts     # Apple Search Ads API client
├── .claude/
│   └── skills/          # 28 ASO skill files for Claude Code
├── data/                # Snapshots (gitignored)
├── certs/               # Search Ads keys (gitignored)
└── config.json          # Your app IDs, competitors, and keywords
```

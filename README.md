# PeekPanda MCP

PeekPanda provides iOS competitor intelligence: Meta ads, App Store keyword positions, ASO, viral posts, and market charts. It is a hosted remote MCP server at `https://mcp.peekpanda.com/mcp` over streamable HTTP; this repository contains documentation and registry metadata only, not the server code.

## Endpoint

```text
https://mcp.peekpanda.com/mcp
```

There is no local install or npm package. PeekPanda requires a PeekPanda account on a paid plan with a trial. API and MCP usage share one daily call allowance per plan, which resets at 00:00 UTC.

## Authentication

PeekPanda supports:

- **OAuth 2.1:** authorization code with PKCE (S256), refresh tokens, dynamic client registration, and token revocation. Tokens are audience-pinned to `https://mcp.peekpanda.com/mcp`.
- **API key:** create or revoke a key in PeekPanda Account, then send it to the same endpoint as `Authorization: Bearer <key>`.

OAuth scopes:

| Scope | Access |
|---|---|
| `context:read` | Read operations |
| `content:write` | Saves, tracking, refreshes, and data requests |

An unauthenticated request returns `401` with a `WWW-Authenticate` header pointing to `/.well-known/oauth-protected-resource/mcp`. See the [OAuth guide](https://peekpanda.com/docs/mcp-oauth) for details.

## Install

### Claude Code

```bash
claude mcp add --transport http peekpanda https://mcp.peekpanda.com/mcp
```

### Cursor

Add this to `~/.cursor/mcp.json`, then sign in when prompted:

```json
{
  "mcpServers": {
    "peekpanda": {
      "url": "https://mcp.peekpanda.com/mcp"
    }
  }
}
```

To use an API key instead, add `"headers": {"Authorization": "Bearer ${env:PEEKPANDA_TOKEN}"}` to the `peekpanda` server configuration.

### Codex

```bash
codex mcp add peekpanda --url https://mcp.peekpanda.com/mcp && codex mcp login peekpanda
```

### ChatGPT

In developer mode, use **Settings > Connectors / Apps** to add a custom connector or MCP server with the URL `https://mcp.peekpanda.com/mcp`, then sign in with OAuth. The settings path varies by plan.

## Tools

Each tool call passes `request.action`. The `show_*` tools render inline widgets in clients that support MCP app UIs.

| Tool | Title | Scope | Actions |
|---|---|---|---|
| `get_context` | Get PeekPanda context | read | `workspaces` |
| `search_apps` | Search iOS apps | read | `search`, `categories`, `similar`, `compare`, `developer`, `emerging` |
| `get_app` | Get an app case file | read | `case_file`, `get`, `aso`, `keywords`, `aso_status`, `ad_refresh_status`, `organic`, `estimates`, `technology`, `media`, `data_requests`, `chart_history` |
| `get_ads` | Get paid ads | read | `search`, `get`, `intelligence`, `app_intelligence`, `rails` |
| `search_organic` | Search organic creatives | read | `search`, `get`, `saved` |
| `aso` | Read ASO workstation | read | `list`, `evidence`, `get`, `discover`, `gap`, `explore` |
| `boards` | Read boards of apps and saved ads | read | `list`, `get`, `memberships` |
| `starter_boards` | Browse curated starter boards | read | `list`, `search`, `detail` |
| `get_market` | Get App Store market reads | read | `overview`, `leaderboard`, `signals`, `charts` |
| `adopt_starter_board` | Add selected apps from a starter board to your boards | write | `adopt` |
| `save` | Save or unsave a creative | write | `organic`, `unsave_organic`, `ad`, `unsave_ad` |
| `refresh` | Run an explicit expensive refresh | write | `aso_collection`, `ad_refresh`, `estimates`, `technology`, `aso_app`, `aso_keyword`, `ad_transcript`, `ad_analysis` |
| `request_data` | Request missing competitor data or intake | write | `request`, `intake`, `discover_emerging` |
| `manage_aso` | Track apps (kept on boards) and keywords in the ASO workstation | write | `track`, `add_temporary`, `update`, `untrack`, `add_keywords`, `update_keyword`, `remove_keyword`, `assign_tag`, `create_tag`, `update_tag`, `remove_tag`, `search_ios` |
| `manage_boards` | Organize tracked apps and saved ads on boards | write | `create`, `update`, `remove`, `add_item`, `update_item`, `remove_item` |
| `show_apps` | Show apps (visual view over `search_apps`) | read | `search`, `categories`, `similar`, `compare`, `developer`, `emerging` |
| `show_app` | Show an app case file (view over `get_app`) | read | `case_file`, `get`, `aso`, `keywords`, `organic`, `estimates`, `technology`, `media`, `chart_history` |
| `show_ads` | Show paid ads (view over `get_ads`) | read | `search`, `get`, `intelligence`, `app_intelligence`, `rails` |
| `show_market` | Show App Store market (view over `get_market`) | read | `overview`, `leaderboard`, `signals`, `charts` |
| `show_organic` | Show organic creatives (view over `search_organic`) | read | `search`, `get`, `saved` |

## Data coverage

- PeekPanda covers iOS apps only, not Android.
- Paid ads data covers Meta ads.
- Keyword data covers the US App Store in English. Search positions are observed, and the popularity score is not search volume.
- Revenue and download figures are modelled estimates.
- Data consists of stored, dated observations and is not real-time.
- PeekPanda is not an official Claude connector.

## Documentation and support

- [MCP documentation](https://peekpanda.com/docs/mcp)
- [OAuth guide](https://peekpanda.com/docs/mcp-oauth)
- [MCP tool reference](https://peekpanda.com/docs/mcp-tools)
- [API quickstart](https://peekpanda.com/docs/quickstart)
- [PeekPanda](https://peekpanda.com)

Support: [hello@peekpanda.com](mailto:hello@peekpanda.com)

# Blockworks plugin for Claude

Crypto research and market data from Blockworks, inside Claude. The plugin connects Claude to the Blockworks MCP server and adds a skill, a command and an agent that teach Claude how to use it well.

## What it includes

- **MCP server** (`.mcp.json`): connects to the hosted Blockworks MCP server at `https://mcp.blockworks.com/mcp` over HTTP. The plugin contains only this address. It does not contain or run any server code on your machine.
- **Skill** `crypto-research`: tells Claude which tool answers which kind of question.
- **Command** `/blockworks:research <topic>`: writes a short, cited research brief on an asset, protocol or topic.
- **Agent** `crypto-analyst`: handles multi-part research questions that need numbers and sources.

## Tools

All tools are read-only. They do not trade, move funds or change any data.

| Tool | What it does |
| --- | --- |
| `search_documents` | Finds research, news, governance, diligence and transcript documents |
| `tabular_catalog` | Lists row-and-column datasets and their fields |
| `tabular_data` | Reads filtered, sorted rows from one dataset |
| `timeseries_catalog` | Lists time-series datasets, granularities and metrics |
| `timeseries_data` | Reads metric values over a time window |

## Get an API key

You need a Blockworks API key to use the plugin. Create one at [app.blockworks.com/account/api](https://app.blockworks.com/account/api).

## Sign in

The server uses OAuth, and your API key is the credential.

1. The first time Claude calls a Blockworks tool, Claude opens a Blockworks sign-in page in your browser.
2. Enter your Blockworks API key on that page and submit it.
3. The page sends you back to Claude. Claude then sends your API key as the bearer token on each request to the server.

The server gives Claude a token that lasts 30 days. If you revoke your API key, the next tool call fails and Claude asks you to sign in again.

**Known limit:** the sign-in page does not check your API key. If you enter a wrong key, sign-in still completes. The error appears at the first tool call as `UNAUTHORIZED: API key invalid or expired`. To fix it, disconnect the Blockworks server in Claude, then sign in again with the correct key.

## Access tiers

What you can read depends on the entitlements of your API key.

- **`search_documents`** needs an Enterprise account with the `ai_toolkit_permission` API permission. It allows 30 requests per minute for each user.
- **Datasets**: each dataset in `tabular_catalog` and `timeseries_catalog` shows an access tier. A `public` dataset needs no entitlement. The `unpaid`, `paid` and `permissive` tiers need a key with the matching entitlement. A `permissive` dataset also names the permission it needs.

The catalogs list only the datasets that Blockworks publishes to its public API.

## Example prompts

- "What happened with EigenLayer governance in August 2026? Cite your sources."
- "Show the top 10 assets by market cap today."
- "Show the daily price of ETH for the last 90 days."
- "How did Uniswap fees change from Q1 2026 to Q2 2026?"
- "/blockworks:research Hyperliquid"

## Troubleshooting

| Error | Cause | Fix |
| --- | --- | --- |
| `UNAUTHORIZED: API key invalid or expired` | The key is wrong, revoked or expired | Disconnect the server in Claude and sign in again with a valid key |
| `FORBIDDEN: this endpoint requires an Enterprise account with the ai_toolkit_permission API permission` | `search_documents` needs Enterprise access | Upgrade your plan, or use the dataset tools only |
| `FORBIDDEN: your subscription tier does not grant access to this dataset` | The dataset's access tier is above your key's entitlements | Pick a dataset your tier allows. Check the tier in the catalog. |
| `RATE_LIMITED: ...` | Too many requests in a short time | Wait, then try again |
| `search_documents` returns old documents | The query has no dates | Put concrete dates in the question, such as "in August 2026" |

## Data and privacy

The plugin sends your questions and tool arguments only to the Blockworks MCP server at `mcp.blockworks.com`. It runs no local code and no hooks. Your API key goes only to Blockworks. See the Blockworks [Terms](https://blockworks.com/terms) and [Privacy Policy](https://blockworks.com/privacy).

## Install

After the listing goes live, add the plugin from the Claude plugin directory.

To test a local copy in Claude Code, clone this repository and start Claude Code with the plugin folder:

```
claude --plugin-dir ./blockworks-claude-plugin
```

## License

MIT. See [LICENSE](LICENSE).

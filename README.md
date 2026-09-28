# Blockworks plugin for Claude

Crypto research and market data from Blockworks, inside Claude. The plugin connects Claude to the Blockworks MCP server and adds a skill, a command and an agent that teach Claude how to use it well.

## What it includes

- **MCP server** (`.mcp.json`): connects to `https://mcp.blockworks.com/mcp` over HTTP. You sign in with OAuth and your Blockworks API key.
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

## Data and privacy

The plugin sends your questions and tool arguments only to the Blockworks MCP server at `mcp.blockworks.com`. It runs no local code and no hooks. Access to paid datasets depends on the entitlements of your Blockworks API key.

## Install

After the listing goes live, add the plugin from the Claude plugin directory.

To test a local copy in Claude Code, clone this repository and start Claude Code with the plugin folder:

```
claude --plugin-dir ./blockworks-claude-plugin
```

## License

MIT. See [LICENSE](LICENSE).

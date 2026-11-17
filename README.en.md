# dsh-exa-parallel-mcp

A DeepSeek Harness (DSH) bundle that registers two keyless remote MCP search servers, Exa and Parallel, so that sessions can call the `mcp__exa__*` and `mcp__parallel__*` tools.

## What it registers

| Server | Endpoint |
| --- | --- |
| Exa | `https://mcp.exa.ai/mcp` |
| Parallel | `https://mcp.parallel.ai/mcp` |

Both connections use `streamable-http`, need no API key, and set `failOnStartupError: false`, so an unreachable server reports an error without blocking session startup.

## Requirements

DSH runtime `>=0.2.0-rc.1 <0.3.0-0`, used in the Web interface.

## Install

```bash
git clone https://github.com/RunhuaChen/dsh-exa-parallel-mcp.git
```

Ask the agent in a DSH session to install the local directory as a bundle, or call the plugin manager directly:

```json
{ "action": "install_bundle", "target": "/your/path/dsh-exa-parallel-mcp" }
```

Reload the page after installing; if the tools do not appear, restart the DSH app once.

## Uninstall

```json
{ "action": "remove_bundle", "target": "@RunhuaChen/dsh-exa-parallel-mcp" }
```

## How it works

The bundle has a single Cordis composition patch, `cordis.patch.yml`, which inserts two `@deepseek-ai/dsh-mcp-client` rows pointing at the Exa and Parallel streamable-http endpoints. `index.js` is an empty host entry; there is no client code.

## Notes

- Both servers are anonymous; no key is required. Never put keys into prompts or config files.
- Enable only one connection per endpoint to avoid duplicate tool names.

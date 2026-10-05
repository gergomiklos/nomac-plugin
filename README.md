# NoMac plugin

A real cloud Mac with Xcode for your agent, billed by the minute. This plugin adds the hosted [NoMac](https://nomac.app) MCP server (`https://mcp.nomac.app/mcp`, OAuth) and a skill that teaches the agent the workflow: start a Mac, build and test, use the iOS Simulator, move files, ship to TestFlight and stop.

## Install

Claude Code:

```
/plugin marketplace add gergomiklos/nomac-plugin
/plugin install nomac@nomac
```

Or add the MCP server directly: `claude mcp add --transport http nomac https://mcp.nomac.app/mcp`.

The first tool call opens a browser to sign in at nomac.app. Mac time is prepaid credit at $0.80 an hour, billed by the minute; credit never expires.

## What the agent can do

- Start a macOS VM with Xcode in about a minute, and stop it (the Mac is deleted).
- Run any command (`xcodebuild`, `simctl`, `swift`) and get its output in one call.
- Write and read files; screenshots come back as images the agent can see.
- Move files of up to 2 GB with upload and download links a person can use too.
- Open the Mac's desktop in the human's browser.
- Manage TestFlight and App Store Connect with the user's Apple connection.

Docs: https://nomac.app/install · Agent reference: https://nomac.app/llms.txt · Support: support@nomac.app

## License

The plugin files in this repository are MIT licensed. NoMac itself is a hosted, paid service.

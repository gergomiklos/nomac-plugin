# NoMac plugin

A cloud Mac with Xcode and a cloud iPhone for your agent, billed per minute. This plugin adds the hosted [NoMac](https://nomac.app) MCP server (`https://mcp.nomac.app/mcp`, OAuth) and a skill that teaches the agent the workflow: start a Mac, build and test, run the app on a cloud iPhone, move files, ship to TestFlight and the App Store, and stop.

## Install

Claude Code:

```
/plugin marketplace add gergomiklos/nomac-plugin
/plugin install nomac@nomac
```

Or add the MCP server directly: `claude mcp add --transport http nomac https://mcp.nomac.app/mcp`.

The skill alone, for any agent that reads skills: `npx skills add gergomiklos/nomac-plugin`.

Cursor: one click at https://nomac.app/install, or add `https://mcp.nomac.app/mcp` as an MCP server.

Gemini CLI: `gemini extensions install https://github.com/gergomiklos/nomac-plugin`.

The first tool call opens a browser to sign in at nomac.app. Mac and iPhone time is prepaid credit, billed per minute, with no subscription; credit never expires. Prices: https://nomac.app.

## What the agent can do

- Start a macOS VM with Xcode in about a minute, and stop it. The VM is deleted at stop; the account's 20 GB drive at `~/drive` is kept between sessions, so the next build is incremental.
- Run any command (`xcodebuild`, `simctl`, `swift`) and get its output in one call.
- Start a cloud iPhone Simulator in seconds, build and run the app on it from a git link or a local folder, read its screen as text and tap by label.
- Write and read files; screenshots come back as images the agent can see.
- Move files of up to 2 GB with upload and download links a person can use too.
- Open the Mac's desktop in the human's browser.
- Sign, send to TestFlight, submit for App Review and notarize through the NoMac App Store server (`https://mcp.nomac.app/appstore`, the same account), with the user's Apple connection and secrets entered through links, never the chat.

From a terminal, the [NoMac CLI](https://www.npmjs.com/package/@nomac/cli) adds `nomac ssh-config`, which lets VS Code, Cursor and Claude Code connect to the Mac over SSH.

Docs: https://nomac.app/install · Agent reference: https://nomac.app/llms.txt · Support: support@nomac.app

## License

The plugin files in this repository are MIT licensed. NoMac itself is a hosted, paid service.

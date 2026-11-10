<p align="center">
  <a href="https://www.traceo.dev"><img src="./assets/logo.png" alt="Traceo" width="88" height="88"></a>
</p>

<h1 align="center">Traceo skills</h1>

<p align="center">
  Paste a Traceo link. Your coding agent sees the console, network and clicks behind the bug.
</p>

```bash
npx skills add muhamad-0/traceo-skills
```

Then paste a [Traceo](https://www.traceo.dev) share link and say what you want:

- "fix this https://www.traceo.dev/…"
- "summarize this recording"
- "write a test for this"
- "make a ticket for this"

A Traceo recording is a screen recording of a browser tab with the console logs, network requests, user actions and environment (browser, OS, window size) attached, on one link. These skills let your agent read it through the Traceo MCP server and act on it.

Works with Claude Code, Cursor, Codex, GitHub Copilot, Gemini CLI, OpenCode and [every agent skills.sh supports](https://skills.sh). Add `--skill traceo-fix` to install one skill.

## Skills

You don't need to remember these names. Paste a link, say what you want, and your agent picks the skill.

- [traceo](./skills/traceo/SKILL.md): "Take a look at this recording." For a link with no instructions. It reads the recording and picks the skill below that fits.
- [traceo-fix](./skills/traceo-fix/SKILL.md): "Fix this." Finds the root cause from the errors, failed requests and steps in the recording, and shows the evidence. It changes code only when you ask.
- [traceo-summarize](./skills/traceo-summarize/SKILL.md): "What's in this?" What the user did, what broke and where, in a few lines.
- [traceo-to-e2e](./skills/traceo-to-e2e/SKILL.md): "Write a test for this." An end-to-end test with your project's own test setup. A bug recording becomes a test that fails until the fix lands.
- [traceo-to-ticket](./skills/traceo-to-ticket/SKILL.md): "Make a ticket for this." Steps, expected vs actual, environment and evidence, filed in Linear, GitHub Issues or Jira when one is connected.
- [traceo-setup](./skills/traceo-setup/SKILL.md): "Set up Traceo." Connects the Traceo MCP server, installs these skills and checks the connection.

## Traceo MCP

The skills read recordings through the Traceo MCP server:

```
https://traceo-video-share-worker.iusmankh.workers.dev/mcp
```

It is public and read-only. It exposes only what the share link already exposes, so it needs no account or API key. Tools: `get_recording`, `get_timeline`, `get_console_logs`, `get_network_requests`, `get_user_actions`, `get_comments`, `get_frames`, `get_thumbnail`. Secrets in console logs and headers are redacted.

To let your agent list, search and comment on your own recordings, connect `https://traceo-video-share-worker.iusmankh.workers.dev/mcp/me` instead. Your client opens a Traceo sign-in page the first time (OAuth, no API key).

Connect it by hand:

<details>
<summary><strong>Claude Code</strong></summary>

```bash
claude mcp add --transport http traceo https://traceo-video-share-worker.iusmankh.workers.dev/mcp
```
</details>

<details>
<summary><strong>Cursor</strong></summary>

`.cursor/mcp.json`:

```json
{ "mcpServers": { "traceo": { "url": "https://traceo-video-share-worker.iusmankh.workers.dev/mcp" } } }
```
</details>

<details>
<summary><strong>VS Code</strong></summary>

`.vscode/mcp.json`:

```json
{ "servers": { "traceo": { "type": "http", "url": "https://traceo-video-share-worker.iusmankh.workers.dev/mcp" } } }
```
</details>

<details>
<summary><strong>Claude.ai / Claude Desktop</strong></summary>

Settings, Connectors, Add custom connector. Name `Traceo`, the URL above, no authentication.
</details>

Or ask your agent: `Set up Traceo MCP by following https://www.traceo.dev/setup.md`

## Install as a Claude Code plugin

The plugin installs every skill and connects Traceo MCP. Use the plugin or `npx skills add`, not both, or the agent sees every skill twice.

```shell
/plugin marketplace add muhamad-0/traceo-skills
/plugin install traceo@traceo-plugins
```

Skills load as `/traceo:traceo-fix`, `/traceo:traceo-summarize`, and so on.

## Requirements

- Recordings shared from the [Traceo Chrome extension](https://chromewebstore.google.com/detail/traceo-%E2%80%93-ui-inspection-vi/dcokbcipcokaindolknkkmkbfhmkiifm) with console and network capture on.
- Free share links last 3 days. Pro links do not expire.

## License

[MIT](./LICENSE)

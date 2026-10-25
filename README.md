# Replit Plugin for Cursor / Grok Bot

Build, update, and publish Replit apps from Cursor / Grok Bot.

This plugin is intended to connect Cursor / Grok Bot to Replit. You describe an app in plain language, and Replit builds it, hosts it, and gives you a live link.

> **Status:** Early development. Nothing is published to the Cursor Marketplace yet.

The intended distribution path for Cursor / Grok Bot is the Cursor Marketplace submission. Grok Bot availability and end-to-end behavior still need partner confirmation. The local setup instructions below are specific to Cursor.

---

## What it does

Once installed and authenticated in a supported client, you can ask Cursor / Grok Bot things like:

- "Make me a Replit app that tracks my team's weekly goals"
- "Find my Replit app called *Budget Tracker* and add a dark mode"
- "Publish my latest Replit app and give me the link"

Under the hood, the plugin connects the client to Replit's **MCP server** (MCP = Model Context Protocol, a standard way for AI tools to talk to other services).

### Replit tools the agent can use

| Tool | What it does |
|------|--------------|
| `create_app_from_prompt` | Starts a new Replit app from a description |
| `list_apps` | Lists your apps, most recent first |
| `search_apps` | Finds apps by keyword, URL, or date |
| `resolve_app_by_name` | Finds one app by its exact name |
| `ask_question` | Asks Replit's Agent about an app (no changes made) |
| `update_app_using_prompt` | Asks Replit's Agent to change an app |
| `publish_app` | Puts an app live (or updates the live version) |
| `get_publish_status` | Checks if publishing is done and gets the public link |

---

## Project layout

This is a **Cursor Plugin**. It configures the existing Replit MCP server and includes a Cursor rule that guides safe app creation, app selection before changes, and explicit confirmation before publishing.

```
replit-cursor-plugin/
├── .cursor-plugin/
│   └── plugin.json          # The plugin's ID card: name, version, description
├── mcp.json                 # Tells Cursor how to reach Replit's MCP server
├── rules/
│   └── replit-app-workflow.mdc # App selection, update, and publishing guidance
├── README.md
├── LICENSE                  # MIT license for this plugin package
└── architecture.md          # Diagram of how the pieces connect
```

Cursor discovers the MCP server configuration from the root `mcp.json` and automatically loads `.mdc` rules from `rules/`.

---

## Getting started in Cursor

### 1. What you need

- [Cursor](https://cursor.com) installed
- A [Replit](https://replit.com) account
- Git

### 2. Install the files for local testing

Clone the plugin directly into Cursor's local plugins directory:

```bash
mkdir -p ~/.cursor/plugins/local
git clone https://github.com/replit/replit-cursor-plugin.git ~/.cursor/plugins/local/replit
```

Until the metadata PR is merged, check out its branch in that clone before testing. If the target directory already exists, use that checkout rather than cloning over it.

Alternatively, copy the plugin files into `~/.cursor/plugins/local/replit`, including the hidden `.cursor-plugin` directory. Do not use a symlink to a repository outside the local plugins directory: Cursor skips those symlinks.

On Teams and Enterprise, your administrator must allow **Local Plugin Imports** under Dashboard → Settings → Security & Identity → Marketplace and Plugins. This setting is off by default on Enterprise.

### 3. Authenticate and test in Cursor

Then in Cursor:

1. Open the Command Palette (`Cmd+Shift+P`) and run **Developer: Reload Window**.
2. Open the **Customize** panel and check that the Replit plugin and its MCP server appear.
3. Authenticate the Replit MCP server when prompted. Sign in to Replit, choose the workspace you want to connect, and review the requested access.
4. In the agent chat, try: *"List my Replit apps."* Confirm that the tool succeeds and returns apps you can edit (or an empty list if there are none).

The plugin connects to `https://mcp.replit.com/server/mcp` using Streamable HTTP and OAuth protected-resource discovery. No API keys or custom authentication headers belong in the plugin files. Its bundled rule instructs the agent to identify the requested app before editing, resolve ambiguous matches with you, and get confirmation immediately before publishing.

After any edit, reload the window again to pick up changes.

### 4. Submit to the Cursor Marketplace

Before submission, complete the local sign-in and tool-call test above. Endpoint reachability alone does not verify the complete Cursor integration.

Before announcing availability in Cursor / Grok Bot, also confirm Grok Bot distribution and test its sign-in and a read-only tool call with the partner team. Passing the Cursor test does not establish Grok Bot compatibility.

Once the plugin files are merged and publicly available, submit the repository link at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish). Submission starts Cursor's review; it does not immediately publish the plugin.

---

## Useful links

- [Cursor plugin docs](https://cursor.com/docs/plugins)
- [Cursor plugin reference: rules and component discovery](https://cursor.com/docs/reference/plugins)
- [Cursor MCP docs](https://cursor.com/docs/mcp)
- [Replit MCP server docs](https://docs.replit.com/platforms/mcp-server)

## License

This plugin package is licensed under the [MIT License](LICENSE). This license
covers the files in this repository, not Replit's hosted service.

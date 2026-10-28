# PCBEditor MCP server

Design, route, check and export manufacturable printed circuit boards from the AI tool you already use.
PCBEditor is a hosted (remote) MCP server: add one URL, sign in, and ask for a board in plain language.
You get it back placed, routed and design-rule checked, with Gerber and KiCad files. Nothing to install.

```
https://mcp.pcbeditor.com/mcp
```

- Transport: Streamable HTTP (MCP 2025-11-25; also 2025-06-18 and 2025-03-26)
- Sign-in: OAuth 2.1 (dynamic client registration and client metadata documents), or a personal access token
- Account: free, at [pcbeditor.com](https://pcbeditor.com)
- Full documentation: [pcbeditor.com/mcp](https://pcbeditor.com/mcp)
- Official MCP Registry: `com.pcbeditor/pcbeditor`

This repository holds connection files only (the registry entry, a Claude Code plugin and a Gemini CLI extension).
The server itself runs at the URL above.

## Connect

### Claude Code

```
claude mcp add --transport http pcbeditor https://mcp.pcbeditor.com/mcp
```

Then run `/mcp` inside Claude Code and sign in when the browser opens. Or install the plugin from this repository:

```
/plugin marketplace add PCBEditor/pcbeditor-mcp
/plugin install pcbeditor@pcbeditor
```

### Claude Desktop and claude.ai

Customize → Connectors → + Add → Add custom connector. Paste the server URL, continue, and sign in to PCBEditor
when asked.

### ChatGPT

At chatgpt.com/plugins, choose + → Add custom MCP server, enter the server URL and sign in with OAuth.
ChatGPT does not send personal access tokens.

### Codex CLI

`~/.codex/config.toml`:

```toml
[mcp_servers.pcbeditor]
url = "https://mcp.pcbeditor.com/mcp"
bearer_token_env_var = "PCBEDITOR_TOKEN"
```

Or leave out the token line and run `codex mcp login pcbeditor`.

### Gemini CLI

```
gemini extensions install https://github.com/PCBEditor/pcbeditor-mcp
```

Or add it to `~/.gemini/settings.json`:

```json
{
  "mcpServers": {
    "pcbeditor": {
      "httpUrl": "https://mcp.pcbeditor.com/mcp"
    }
  }
}
```

Start Gemini CLI and run `/mcp auth pcbeditor` to sign in.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "pcbeditor": {
      "url": "https://mcp.pcbeditor.com/mcp",
      "headers": {
        "Authorization": "Bearer ${env:PCBEDITOR_TOKEN}"
      }
    }
  }
}
```

Create the token in PCBEditor under Profile → AI tools.

### VS Code with GitHub Copilot

`.vscode/mcp.json`:

```json
{
  "servers": {
    "pcbeditor": {
      "type": "http",
      "url": "https://mcp.pcbeditor.com/mcp"
    }
  }
}
```

VS Code runs the sign-in in your browser on first use.

## Tools

| Tool | What it does |
|---|---|
| `design_board` | Turns a plain-language description into a complete board: parts, schematic, placement, routing and a design-rule check. Runs as a job. |
| `get_job` | Status, progress and result of a job. |
| `answer_question` | Answers a design job that paused on a question, such as which part to use. |
| `edit_board` | Changes an existing design from a plain-language instruction. |
| `route_board` | Routes the board's unrouted connections. |
| `review_board` | Engineering review of a board: power, decoupling, thermal, signal integrity, manufacturability. |
| `run_drc` | Design-rule check results with locations. |
| `export_board` | Gerber X2, drill, BOM, pick-and-place, KiCad, STEP, IPC-2581, ODB++ or PDF, as a time-limited download link. |
| `get_board` | Board summary: parts, nets and routing counts. |
| `list_designs` | The designs in your account. |
| `import_kicad` | Imports a `.kicad_pcb` file as a new design. |
| `open_in_editor` | A link that opens the design in PCBEditor's browser editor. |
| `search_parts` | Searches PCBEditor's verified parts and footprint library. |
| `get_part` | Verified pin map and package for a part number. |
| `list_inventory` | The parts in your inventory. |
| `add_inventory_part` | Adds a part you own, which later designs then use. |

## Example prompts

- "Design a 2-layer board for an ESP32-C3 temperature and humidity sensor powered from USB-C."
- "Review my latest design for power and decoupling problems, then fix what you find and re-route it."
- "Add a 3 mm mounting hole in each corner."
- "Export Gerbers, drill files, BOM and pick-and-place so I can order it."
- "Import this .kicad_pcb and run the design-rule check."

## Links

- Documentation: [pcbeditor.com/mcp](https://pcbeditor.com/mcp)
- Privacy: [pcbeditor.com/privacy](https://pcbeditor.com/privacy)
- Terms: [pcbeditor.com/terms](https://pcbeditor.com/terms)
- Support: [pcbeditor.com/contact](https://pcbeditor.com/contact)

## License

The files in this repository are MIT licensed. PCBEditor itself is a hosted service under its own
[terms](https://pcbeditor.com/terms).

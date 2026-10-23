# SparkQuote NEC Calculators MCP Server

Free NEC 2023 electrical calculators for Claude, ChatGPT, Cursor and any MCP client. Ask your assistant for a wire size, a Table 310.16 ampacity or a voltage drop and it calls a real calculator instead of answering from memory.

- **URL:** `https://sparkquote.ai/mcp`
- **Transport:** Streamable HTTP (JSON responses, stateless)
- **Authentication:** none, no account or API key
- **Tools:** 10, all read-only
- **Code edition:** NEC 2023 (NFPA 70)
- **Price:** free

This is a hosted (remote) server, so there is nothing to install. This repository holds the documentation and the MCP registry metadata (`server.json`). Listed in the [official MCP registry](https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.sparkquote) as `ai.sparkquote/nec-calculators` and on [Smithery](https://smithery.ai/servers/sparkquote/nec-calculators). Full docs: [sparkquote.ai/tools/mcp-server](https://sparkquote.ai/tools/mcp-server).

## Add it to your assistant

**Claude (claude.ai and the desktop app).** Settings → Connectors → **Add custom connector**, name it SparkQuote and paste `https://sparkquote.ai/mcp`. Leave the OAuth fields empty.

**ChatGPT.** Custom MCP servers need developer mode: Settings → Apps → Advanced settings → Developer mode. Then create an app with the URL `https://sparkquote.ai/mcp` and authentication set to none.

**Claude Code**

```sh
claude mcp add --transport http sparkquote https://sparkquote.ai/mcp
```

**Codex CLI** (`~/.codex/config.toml`)

```toml
[mcp_servers.sparkquote]
url = "https://sparkquote.ai/mcp"
```

**Cursor, VS Code and other clients** with a JSON config

```json
{ "mcpServers": { "sparkquote": { "url": "https://sparkquote.ai/mcp" } } }
```

## Tools

Every argument has a default, so a tool called with no arguments returns a worked example.

| Tool | What it answers |
|---|---|
| `wire_size` | Conductor size for a breaker or load, by ampacity and by voltage drop, plus the equipment ground |
| `nec_ampacity` | One Table 310.16 value: material, temperature column and size |
| `voltage_drop` | Volts and percent drop for a conductor size, load and one-way length |
| `watts_to_amps` | Current for a load in watts, single- or three-phase, with the 125% continuous value |
| `conduit_fill` | Percent fill and the maximum number of THHN/THWN-2 conductors in EMT or PVC |
| `box_fill` | Minimum box volume from the 314.16(B) allowances |
| `load_calculator` | Dwelling service load by the 220.82 optional method |
| `ohms_law` | The other two of volts, amps, ohms and watts from any two |
| `panel_schedule` | Circuits laid out on a single- or three-phase panel, with load per phase and the imbalance |
| `bid_calculator` | Bid price from material, markup, labor, job costs, overhead and a target margin |

Each result comes back as text plus structured content: a one-line answer, the supporting rows, the inputs used (including defaults), the code edition and a link to the matching calculator page.

## Example

"What size copper wire for a 60 A circuit at 240 V, 150 ft away?" → `wire_size` with `amps: 60`, `volts: "240"`, `feet: 150`:

```
Use 4 AWG copper.

- Smallest for ampacity: 6 AWG (65 A)
- Smallest for voltage drop: 4 AWG (2.3% = 5.6 V)
- Equipment ground (250.122(B), increased with conductors): 6 AWG Cu / 4 AWG Al

Source: SparkQuote, NEC 2023 (NFPA 70): https://sparkquote.ai/tools/wire-size-calculator
Code minimum under the stated assumptions; check local amendments and the adopted edition.
```

## Limits

- Results are code minimums under the stated assumptions. They don't apply ambient correction, adjustment for more than three current-carrying conductors, or local amendments unless the tool says so. Your adopted code edition and the inspector decide.
- The tools calculate. They don't read drawings, look up prices or store anything between calls.
- The server only answers tool calls; it has no resources or prompts and never sees your conversation.

## Privacy

A tool call sends only the tool name and its arguments. We log the tool name, success and the client's user agent as an anonymous event; arguments and results are not stored. See the [privacy policy](https://sparkquote.ai/privacy).

## Support

Report a wrong result or a connection problem to privacy@sparkquote.ai with the tool name and arguments, or open an issue here.

---

Built by [SparkQuote](https://sparkquote.ai), AI takeoff and estimating for electrical contractors. The web versions of these calculators are at [sparkquote.ai/tools](https://sparkquote.ai/tools).

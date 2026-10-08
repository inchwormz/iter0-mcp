# iter0 MCP

Connect an AI agent to [iter0](https://iter0.com), an AI website builder for tech founders. Through MCP, an agent can start a website build from a brief or an authorised reference, check the build, read a saved design and ask for a focused edit.

- **Endpoint:** `https://iter0.com/api/mcp` (Streamable HTTP)
- **Setup page:** <https://iter0.com/mcp>
- **Developer docs:** <https://iter0.com/developers>
- **OpenAPI:** <https://iter0.com/openapi.json>
- **Agent summary:** <https://iter0.com/llms.txt>

This repository holds connection instructions, an MCP Registry file and an agent skill. The server runs at iter0.com. Its source code is not published here.

## Connect

Claude Code:

```bash
claude mcp add --transport http iter0 https://iter0.com/api/mcp
```

Cursor (`.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "iter0": {
      "url": "https://iter0.com/api/mcp"
    }
  }
}
```

Codex (`~/.codex/config.toml`):

```toml
[mcp_servers.iter0]
url = "https://iter0.com/api/mcp"
```

Client settings change between versions. If a snippet does not match your client, use the client's own instructions for adding a Streamable HTTP server and give it the endpoint above.

## Authentication

Two tools need no sign-in: `iter0_health` and `iter0_get_developer_docs`.

The tools that read or change your account use OAuth 2 with the scopes `sites:read`, `sites:build` and `sites:edit`. The server publishes its protected-resource metadata at <https://iter0.com/.well-known/oauth-protected-resource>.

For direct API calls, the developer docs describe an iter0 bearer token that you create in your account settings: <https://iter0.com/settings/api-keys>.

## Tools

| Tool | What it does | Needs sign-in |
| --- | --- | --- |
| `iter0_health` | Checks that the public API and developer contracts respond. | No |
| `iter0_get_developer_docs` | Returns the API, OpenAPI, MCP, authentication and support links. | No |
| `iter0_create_site` | Starts a website build from a brief, an authorised reference URL or an owned design direction. Quotes the price first. | Yes |
| `iter0_edit_site` | Returns revised HTML for a supplied page and a focused change. Does not change the saved design. Quotes the price first. | Yes |
| `iter0_get_site_status` | Checks the status of a build. | Yes |
| `iter0_show_three` | Prepares three rough design directions from approved references. | Yes |
| `iter0_get_design_set_status` | Reads the status and the three cards of a batch of directions. | Yes |
| `iter0_confirm_paid_action` | Spends the exact quoted credits and starts the quoted request. | Yes |
| `iter0_list_my_designs` | Lists saved designs and the read-only credit balance. | Yes |
| `iter0_get_design` | Reads one saved design and its section IDs for an edit. | Yes |
| `iter0_restore_version` | Restores an earlier saved version of a design. Free. | Yes |
| `iter0_open_builder` | Opens your designs in the ChatGPT sidebar. Never starts work or spends credits. | Yes |

## Credits

A build uses 1 credit and an AI edit uses 0.5 credits. Paid actions return an exact quote first. Nothing is spent until the person confirms that quote, either in the card or by saying yes in chat. A connected account has a daily cap of 5 credits. The free plan includes 3 credits a month; see <https://iter0.com/pricing> for current plans.

## Agent skill

[`skills/iter0-website-builder/SKILL.md`](skills/iter0-website-builder/SKILL.md) tells a coding agent how to use these tools in the right order and when to stop and ask the person.

## MCP Registry

[`server.json`](server.json) describes the remote server for the [official MCP Registry](https://modelcontextprotocol.io/registry/remote-servers).

## Limits

Use real customer facts and authorised references. A build that has started is not a finished site: check the exact result of each run. Generated pages do not guarantee rankings, traffic or conversion.

## Licence

The files in this repository are under the MIT licence. iter0 itself is a hosted service governed by its [terms](https://iter0.com/terms) and [privacy policy](https://iter0.com/privacy).

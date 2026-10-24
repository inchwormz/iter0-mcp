# iter0 MCP

Connect an AI agent such as Claude Code, Cursor or Codex to [iter0](https://iter0.com), an AI website builder for tech founders. Through MCP, the agent can:

- build a launch website from a brief or from a reference site you are allowed to use;
- make one focused edit to a saved page;
- read your saved designs and their status;
- check a site for problems before you share it;
- publish a site to a free `name.iter0.app` address, and take it offline again;
- read and reply to comments left on an iter0 canvas.

Paid steps show a price quote first. Nothing is spent until you confirm the quote.

- **Endpoint:** `https://iter0.com/api/mcp` (Streamable HTTP)
- **Setup page:** <https://iter0.com/mcp>
- **Developer docs:** <https://iter0.com/developers>
- **OpenAPI:** <https://iter0.com/openapi.json>
- **Agent summary:** <https://iter0.com/llms.txt>

This repository holds connection instructions, a registry file and an agent skill. The server runs at iter0.com, and its source code is not published here.

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

If your client needs a different format, use its own steps for adding a Streamable HTTP server, and give it the endpoint above.

## Sign in

Two tools work without signing in: `iter0_health` and `iter0_get_developer_docs`.

The other tools use your iter0 account through OAuth 2, with the scopes `sites:read`, `sites:build` and `sites:edit`. Your client opens a sign-in page the first time it needs one. The server publishes its protected-resource metadata at <https://iter0.com/.well-known/oauth-protected-resource>.

For direct API calls (not MCP), create a bearer token in your account settings: <https://iter0.com/settings/api-keys>. The API reference is at <https://iter0.com/developers>.

## Tools

22 tools are live on the endpoint. Costs are from the tool descriptions.

### No sign-in

| Tool | What it does |
| --- | --- |
| `iter0_health` | Checks that the API and developer contracts respond. Does not build or edit. |
| `iter0_get_developer_docs` | Returns the links for API docs, OpenAPI, MCP setup, authentication and support. |

### Building (paid steps quote first)

| Tool | What it does | Cost |
| --- | --- | --- |
| `iter0_create_site` | Starts a website build from a brief, an authorised reference URL or a direction from a Show 3 set. Returns a quote first. | 1 credit |
| `iter0_edit_site` | Makes one focused change to a saved page, or returns revised HTML for a page pasted into chat. Returns a quote first. | 0.5 credit |
| `iter0_show_three` | Prepares three rough design directions from approved references. | 1 credit on a paid account; free accounts use their allowance |
| `iter0_confirm_paid_action` | Starts a quoted request once the person has confirmed its price. | The quoted amount |
| `iter0_get_site_status` | Checks a build's progress. Returns HTML only when asked. | Free |
| `iter0_get_design_set_status` | Reads a Show 3 batch: status and the three directions. | Free |

### Designs

| Tool | What it does |
| --- | --- |
| `iter0_list_my_designs` | Lists saved designs, newest first, with search and paging. |
| `iter0_get_design` | Reads one design: its name, link, section IDs and a short summary. Does not return the page code. |
| `iter0_open_builder` | Opens your iter0 projects. Starts nothing and spends nothing. |
| `iter0_design_layout` | Used by the iter0 card to outline a section. Not for direct use. |

### Publishing and checks (free)

| Tool | What it does |
| --- | --- |
| `iter0_check_site` | Lists problems before sharing: missing title or description, images without alt text, broken internal links, placeholder text, forms that would go nowhere. Changes nothing. |
| `iter0_publish_site` | Puts a saved site live at `name.iter0.app`. Two steps: the first returns the address and a confirmation token, the second publishes. |
| `iter0_publish_status` | Checks whether a site is live. |
| `iter0_unpublish_site` | Takes a live site offline. The design and the address are kept. Two steps. |
| `iter0_undo_edit` | Undoes the last saved edit, while it is still the current page. |
| `iter0_restore_version` | Restores an earlier saved version. Two steps. |

### Canvas feedback (free)

| Tool | What it does |
| --- | --- |
| `iter0_push_site` | Puts finished HTML pages on your iter0 canvas so you can comment on them. Publishes nothing. |
| `iter0_get_canvas_feedback` | Reads the comments and notes you left on the canvas. |
| `iter0_read_canvas_page` | Reads one canvas page's HTML and version before a revision. |
| `iter0_reply_to_feedback` | Adds a reply to a comment thread. Never resolves or approves anything. |

## Credits

- A build costs 1 credit. An AI edit costs 0.5 credit. Publishing, checks, canvas tools, undo and restore are free.
- Every paid step returns an exact quote first, and a quote expires after 15 minutes. Nothing is spent until you confirm the quote, in the iter0 card or by saying yes in chat.
- The free plan includes 3 credits a month. Current plans are at <https://iter0.com/pricing>.

## Agent skill

[`skills/iter0-website-builder/SKILL.md`](skills/iter0-website-builder/SKILL.md) tells a coding agent which tools to use, in what order, and when to stop and ask you.

## Official MCP Registry

[`server.json`](server.json) describes the remote server for the official MCP Registry, under the name `io.github.inchwormz/iter0-mcp`.

## Limits

- Use real customer facts and references you are allowed to use.
- A build that has started is not a finished site. Check the result of each run.
- Generated pages do not guarantee rankings, traffic or conversion.

## Licence

The files in this repository are under the MIT licence. iter0 itself is a hosted service governed by its [terms](https://iter0.com/terms) and [privacy policy](https://iter0.com/privacy).

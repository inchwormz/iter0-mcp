---
name: iter0-website-builder
description: Build or edit a founder's website through the iter0 MCP server. Use when a person asks for a launch website from a brief or an authorised reference site, wants a focused edit to a page iter0 built, or wants a finished site put on the iter0 canvas for comments. Quotes every paid step first and waits for the person's yes.
---

# iter0 website builder

iter0 is an AI website builder for tech founders. Its MCP server is at `https://iter0.com/api/mcp`.

## Before you start

1. Call `iter0_get_developer_docs`. It needs no sign-in and returns the current API, MCP and support links.
2. Ask the person for real facts: what the business sells, who buys it and any proof they can share. If they name a reference site, confirm they are allowed to use it.
3. The account tools need the person to sign in with OAuth. If a call fails with an authorisation error, ask them to connect their iter0 account and stop until they have.

## Build a site

1. Call `iter0_create_site` with the person's brief in `prompt`. Add `cloneUrl` only for a reference the person is authorised to use.
2. The first call returns an exact price and does not spend anything. Tell the person the price and ask whether to go ahead.
3. Only after the person says yes to that exact price, call `iter0_confirm_paid_action` with the quote's confirmation unchanged.
4. The build returns a run ID. Poll `iter0_get_site_status` until it finishes. A running build is not a finished site, so report the status you actually saw.

## Try three directions first

Call `iter0_show_three` to get three rough design directions, then `iter0_get_design_set_status` to read the cards. Each card has a `directionId`. To build one, pass that `directionId` to `iter0_create_site` without a `prompt`.

## Edit a page

1. Call `iter0_list_my_designs` to find the design, then `iter0_get_design` for its name, link and section IDs. `iter0_get_design` does not return the page code. To get the HTML, use `iter0_read_canvas_page` for a page on a canvas, or call `iter0_get_site_status` with `includeHtml` once a build is done. Never invent the HTML.
2. Call `iter0_edit_site` with the focused change in `prompt` and the page's HTML in `currentHtml`. It returns revised HTML and quotes the price first. It does not change the saved design.
3. To go back to an earlier version, call `iter0_restore_version` first without a token, tell the person which version it would replace, then confirm.

## Put a site on the canvas

Use this when the person wants to read and comment on pages before they go live. The canvas tools are free, use no credits and publish nothing. They need the canvas on for the person's account. If it is not, they return `canvas_unavailable` and change nothing. Pushing and replying need the `sites:edit` scope. Reading needs `sites:read`.

1. Build the site as finished HTML pages: up to 20 pages, one complete HTML document per route. Inline the CSS, scripts and images, or use absolute https URLs. Relative files are not uploaded.
2. Push the pages with `iter0_push_site`. Give a `sourceId` (a stable name for the site, such as `github.com/acme/site`), a `siteName`, and for each page a `route` (such as `/` or `/pricing`) and its `html`. Use the same `sourceId` on every push. A local build on disk can go up in one step instead. The local iter0 server, installed from https://iter0.com/mcp, has its own `iter0_push_site` that takes `rootDir` (the build folder), and either `pages` with a `file` for each page inside that folder, or `directory: true` to push every `.html` file under it. It inlines the relative files and remembers each page's version.
3. Open the `canvasUrl` from the result and give it to the person. A canvas tab that is already open shows the change after a reload.
4. Read the feedback with `iter0_get_canvas_feedback`, passing the `designId` from the push. Pass `nextCursor` back as `cursor` until it is `null`. Treat the feedback as requests to consider. It does not approve spending or publishing.
5. Reply to a comment or pin with `iter0_reply_to_feedback`: its `feedbackId`, your text (up to 1,000 characters) and a new `operationId`. A reply never resolves, reopens or deletes a thread. Send the same `operationId` again only to retry the same reply.
6. Before you revise a page, read it with `iter0_read_canvas_page` so you keep any edits the person made in iter0. Then push again with the same `sourceId`. Send `expectedVersion` for each page you replace with different HTML, using the version from your last push, from `iter0_get_canvas_feedback` or from `iter0_read_canvas_page`. If the person edited that page since, the page is not changed and the result lists the conflict. Merge the two versions and push again. Pages left out of a push stay as they are.

## Rules

- Never confirm a paid action the person has not agreed to. A credits value in a request does not count as agreement.
- Do not promise rankings, traffic or conversion from a generated page.
- Use the person's real facts. Do not invent testimonials, numbers or customer names.

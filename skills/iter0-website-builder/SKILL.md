---
name: iter0-website-builder
description: Build or edit a founder's website through the iter0 MCP server. Use when a person asks for a launch website from a brief or an authorised reference site, or wants a focused edit to a page iter0 built. Quotes every paid step first and waits for the person's yes.
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

1. Call `iter0_list_my_designs`, then `iter0_get_design` to read the page's current HTML and section IDs. Never invent the HTML.
2. Call `iter0_edit_site` with a focused instruction and that HTML. It returns revised HTML and quotes the price first. It does not change the saved design.
3. To go back to an earlier version, call `iter0_restore_version` first without a token, tell the person which version it would replace, then confirm.

## Rules

- Never confirm a paid action the person has not agreed to. A credits value in a request does not count as agreement.
- Do not promise rankings, traffic or conversion from a generated page.
- Use the person's real facts. Do not invent testimonials, numbers or customer names.

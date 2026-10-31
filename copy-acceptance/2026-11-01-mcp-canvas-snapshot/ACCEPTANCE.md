# MCP canvas snapshot copy acceptance

Profile: Precise Hierarchical Plain. Audience: developers connecting an agent to iter0 and agents reviewing canvas feedback.

Reader question: How can an agent see where canvas feedback belongs before changing a page?
Answer: Read the feedback, then request a page snapshot with designId and pageKey and use its numbered legend to locate each note.

## Scope and sources

- iter0: `src/app/(en)/(marketing)/developers/page.tsx` (tool table, costs, canvas introduction and feedback instructions); `src/app/(en)/(marketing)/mcp/page.tsx` (remote-tool table, canvas table, EN/DE dictionary text and count); `src/lib/agent-readiness.ts` (generated agent reference).
- iter0-mcp: `README.md` (tool count, canvas table and instructions); `skills/iter0-website-builder/SKILL.md` (positions and snapshot-before-edit instructions). This folder is mirrored into that repository so its documentation can ship with acceptance evidence.
- `draft.md` contains the final changed English copy with file and surrounding section. JSX paragraphs are recorded as reader-visible text; agent-reference origin resolves to https://iter0.com. Unchanged adjacent table rows and numbered instructions are retained where they give context.
- `translations.md` contains every changed German dictionary string. German editorial review: pass; count, sequence, available positions, numbered markers, element outlines, legend and free read-only operation match English. The shared tool descriptions remain English, as the existing German page states.
- Chinese: no Chinese MCP page or dictionary exists in this checkout. `src/lib/marketing-i18n.ts` supports only en/de; the localized MCP route reuses McpExperience. No Chinese locale was introduced because this brief limits changes to copy/data.
- Technical claims checked against local ref `origin/feat/mcp-canvas-positions-snapshots`, especially `src/lib/mcp-canvas-tool-definitions.ts` and `docs/implementation/2026-11-01-mcp-canvas-snapshots.md`. This copy accompanies PR #1067; it is not evidence of production availability. The coordinator owns integration and release.
- Internal dogfood implementation notes were updated without applying the copy route to those notes.

## Corpus passages and use

Paths below are relative to `~/.claude/skills/style-corpus-writer/`.

| Source and passage actually read | Technique | Use and revision comparison |
| --- | --- | --- |
| `.firecrawl/style-corpus/barbara-minto/official/pages/004.md`, opening and grouping list | Group facts under one governing answer | Tool rows describe outputs; canvas instructions explain when to call them. Official explanatory material, not a personal essay. |
| `.firecrawl/style-corpus/barbara-minto/mckinsey-interview/pages/001.md`, directly attributed answer beginning “I saw it meant” and answer beginning “When I prepare to teach” | Put related steps in a logical order; inspect an actual example's organisation | Read feedback, inspect snapshot, reply, preserve saved edits and push. Reporter prose was not attributed to Minto. |
| `.firecrawl/style-corpus/bertrand-russell/works/5827/pages/001.md`, Chapter I paragraph beginning “To make our difficulties plain” | Ground a distinction in observable properties | Distinguish saved positions from the current rendered page and numbered image markers. |
| Same Russell file, paragraph beginning “For most practical purposes” | Define whose question the distinction answers | Keep saved-position caveats beside coordinates; put render omissions beside snapshot instructions. |
| `.firecrawl/style-corpus/george-orwell/pages/002.md`, opening two paragraphs | State an actual causal relationship rather than an unexplained label | Explain that taking a page snapshot lets the agent see notes in place before editing. |
| Same Orwell file, paragraph beginning “As I have tried to show” | Choose words for an exact action; remove ready-made phrasing | Use read, call, check, reply and push. Split the agent-reference sequence into sentences rather than packing every instruction into one. |

The six passages informed the same copy. Minto's public corpus is limited (8 documents, 3,075 raw words); no broader fidelity claim is made. Constituent profiles have nonzero documents, words and fingerprints; required original files were available and read.

## Draft and review loop

Drafted from tool definitions, checked the order and grouped tool descriptions separately from usage instructions. Compared against the passages above, qualified available positions, saved anchors and rendering limits, and removed unnecessary repetition. Replaced “read-only” in developer-page copy with “changes nothing” after that page's existing broad ban on the word “only” caught it. No test was weakened. All verification below uses final copy.

## Editorial acceptance

| Condition | Result |
| --- | --- |
| Answer/action is clear early | pass: snapshot tool output in tables; explicit before-edit call in instructions |
| Advice names its subject and consequence | pass: designId/pageKey select the page; legend connects markers to feedbackId |
| Claims, examples and qualifications have grounds | pass: checked schemas, defaults, dimensions, limits, warnings and permissions against feature code |
| Each paragraph adds information; no generic advice or repeated conclusions | pass |
| Banned words/structures removed except justified literals | pass; no new em dashes or puffery |
| Terms are consistent and form suits the material | pass: canvas/page, feedback, snapshot, legend; tables for outputs and numbered instructions for calls |
| MPR/ALR or qualitative equivalent | pass via qualitative DO/DON'T review; no comparable measured human-edited reference, so no numerical ratio claimed. These are short descriptions and procedural instructions, not a new essay. |
| Abstract terms grounded in actors and actions | pass: positions, markers and warnings name actual returned fields |
| Titles state the subject and measured numbers | pass: only changed heading is the 23-tool count |
| Opening answers the question; no roadmap or recap ending | pass; existing page form retained |
| Profile hierarchy/reasoning preserved without forced templates | pass: output descriptions and dependent steps have different forms |
| Actual corpus read and used during drafting/revision | pass: passages and uses recorded above |
| Profile acceptance: stable terms, valid links, useful conditions, no filler or author imitation | pass |

## Mechanical checks and hashes

Each command ran from `~/.claude/skills/style-corpus-writer` after the final draft edit:

```sh
node scripts/verify-draft.mjs --profile .firecrawl/style-profiles/barbara-minto/style-profile.json --draft /Users/johnmckeown/Work/iter0/code/iter0-wt/mcp-snapshot-docs/copy-acceptance/2026-11-01-mcp-canvas-snapshot/draft.md --out /Users/johnmckeown/Work/iter0/code/iter0-wt/mcp-snapshot-docs/copy-acceptance/2026-11-01-mcp-canvas-snapshot/verify-barbara-minto.json
node scripts/verify-draft.mjs --profile .firecrawl/style-profiles/bertrand-russell/style-profile.json --draft /Users/johnmckeown/Work/iter0/code/iter0-wt/mcp-snapshot-docs/copy-acceptance/2026-11-01-mcp-canvas-snapshot/draft.md --out /Users/johnmckeown/Work/iter0/code/iter0-wt/mcp-snapshot-docs/copy-acceptance/2026-11-01-mcp-canvas-snapshot/verify-bertrand-russell.json
node scripts/verify-draft.mjs --profile .firecrawl/style-profiles/george-orwell/style-profile.json --draft /Users/johnmckeown/Work/iter0/code/iter0-wt/mcp-snapshot-docs/copy-acceptance/2026-11-01-mcp-canvas-snapshot/draft.md --out /Users/johnmckeown/Work/iter0/code/iter0-wt/mcp-snapshot-docs/copy-acceptance/2026-11-01-mcp-canvas-snapshot/verify-george-orwell.json
```

All three: exit 0, `ok: true`, no failures, source overlap, cliche/signpost hits or author claims. German received editorial review, not the English-only checker.

Checker exceptions: none.

SHA-256 (`shasum -a 256`):

- draft.md: c9341043080adb42c060ea0e1796412ac48c9cc472bf7ee3c28fb862b35308e6
- translations.md: b3a55ede60ebb2dc435d68a4751f3689cc7d69306d618dc4e4d8c451b178040d

## Integration note

No executable logic changed; code review and review ledger are not required under the shared review-effort rule. The sole test edit updates the existing 22-tool expectation to 23. No commit, push, PR or release was attempted, as requested by the lane brief. Rendered browser QA and live availability remain for the coordinator's integrated candidate.

## Documentation verification

All commands run in the named iter0 documentation worktree after the final source edit:

```sh
npx tsc --noEmit -p tsconfig.json
# exit 0, no diagnostics
npx eslint 'src/app/(en)/(marketing)/developers/page.tsx' 'src/app/(en)/(marketing)/mcp/page.tsx' 'src/app/(en)/(marketing)/mcp/page.test.tsx' src/lib/agent-readiness.ts
# exit 0, no diagnostics
node --import tsx --test 'src/app/(en)/(marketing)/mcp/page.test.tsx' 'src/app/(en)/(marketing)/developers/page.test.tsx' src/lib/agent-readiness.test.ts
# exit 0, 35 tests passed, 0 failed
git diff --check
# exit 0 in both repositories
```

The page tests exercise rendered static markup, not a real browser. No build, browser, database or live tool-call verification was run in this docs lane. The tool implementation is intentionally absent from this main-based docs branch and must be integrated with PR #1067 before delivery.

Recommended next step: coordinator integrates both repositories' explicit changed paths with the snapshot feature and carries these acceptance folders with their respective PRs.

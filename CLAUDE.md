# CLAUDE.md, Semba Fit-Out CRM

Read PRD.md first. It holds the product decisions, the data model, and the next build stages. This file is the working rules for anyone (human or Claude) touching the code.

## The project in one line

A single-file CRM demo for SEMBA Malaysia, a fit-out contractor in KL, built by Lemon Sky Edge to show capability and to sell AI training. Sample data, no backend yet.

## Files

- `index.html`, the whole app. CSS, markup and JS in one file. This is the shipped artifact version 3, wrapped in a full document skeleton so it opens from disk.
- `PRD.md`, product requirements, decisions, next stages.
- `README.md`, how to run it.

## How the code is organised inside index.html

1. `<style>`. Design tokens on `:root` (the brand theme everyone sees), redefined under `[data-theme="dark"]`. Dark is opt in only, it does not follow the system setting. Every colour in the page comes from a token. Never write a hex value inside a component rule.
2. Markup. One `<aside class="rail">`, one `<main>` with five `<section class="view">` blocks (Dashboard, Pipeline, Deals, Follow-ups, Reports), a `<div class="scrim">`, an `<aside class="drawer">`, a toast.
3. `<script>`. In order, constants (`STAGES`, `OPEN`, `PROB`, `BASE`, `TODAY`, `HAND`), the `deals` seed array, `HIST`, the date engine, helpers, nav, one `render*` function per view, the drawer (`openDeal`), the assistant (`runAI`, `PROMPTS`, `FALLBACK`), and `renderAll()`.

Every view re-renders from `deals` on any change. There is no diffing. Keep it that way until performance is an actual problem.

## Rules that came from the client work

1. The SEMBA brand palette in PRD section 7, set by Sritesh on 1 October 2026, with the roles swapped on 4 October 2026 so the CRM does not look like the Project Management Tool. The rail is grey. Taupe is the accent (buttons, main bars). Navy is the second colour (text, the selected item, badges, finished steps, secondary bars). Navy is also the page ground, so text that sits straight on the page uses `--page-ink` (off white) and `--page-ink2` (grey). Off white for text on navy. No bright blue, no other accent. Semantic colours (green, amber, red) are for state and nothing else.
2. Direct quote and tender win rates are shown separately. Never average them.
3. The `Sample data` banner was removed by Sritesh on 4 October 2026, as on the Project Management Tool. Nothing on screen says the data is sample now, so never present it as real, and do not restore the banner. The rail footer reads "Concept prototype for SEMBA Malaysia. Built by Lemon Sky AI Academy.", the same as the Project Management Tool, set by Sritesh on 4 October 2026.
4. Invented brands only. Never seed Semba's real projects (9090, Sunmoulin, SENYA) as deals.
5. The assistant must always answer. The template fallback in `FALLBACK` stays even after a live model is wired in, and the label under the output must say which path produced it.
6. Copy that Semba will read uses no dashes and no colons. Commas, full stops, parentheses, numbered lists instead. This applies to UI strings, prompts and generated drafts.
7. Do not add project management features. The SEMBA Project Management Tool (in the `PM Tool` folder, formerly Sitebook) owns that. The handover strip on awarded deals is the only overlap allowed.
8. Do not add 3D design anything.
9. The date engine shifts all sample dates to today. Write sample dates as they were on `BASE` (23 September 2026). Never write a calendar date inside sample text (notes, next actions, history lines), and use `inDays(n)` for any new date set in code. Remove the engine only when real data replaces the sample, and keep `TODAY`.

## When you change something

1. Keep it a single file until Stage A in the PRD (persistence) actually starts. Then split into `public/index.html`, `server.js`, `data/`.
2. If you add a colour, it must be one from PRD section 7. Add it as a token in both blocks (`:root` and `[data-theme="dark"]`).
3. If you add a field to a deal, update the table in PRD.md section 5, the drawer `kv` grid, and `dealText()` so the assistant sees it.
4. If you change stage names, update `STAGES`, `PROB`, the kanban, the drawer progress bar, and the filter select together.
5. Test by opening `index.html` in a browser. There is no test suite yet. Check the dashboard, drag one card on the pipeline, open a deal, run all three assistant buttons, add a note, move a stage.

## Things that only work inside the Claude artifact

`window.claude.use("sample")` exists only when the page is served as a claude.ai artifact. From disk or a local server it resolves to `null` and the assistant uses the template path. That is expected. Stage B in the PRD replaces it with a server side call to the Anthropic API.

## Who to ask

Sritesh Naidu, sritesh@lemonskystudios.com. He edits files between sessions. Re-read a file before amending it, amend in place, and never restore something he deleted. Flag it instead.

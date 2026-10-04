# SEMBA CRM Tool

Concept CRM for SEMBA Malaysia's sales desk. Built by Lemon Sky Edge. Sample data, no backend. The sample dates move with today's date, so the page reads the same on any day.

## Run it

Open `index.html` in any modern browser. No install, no build.

For a local server (optional, avoids font loading quirks on some machines)

```
npx serve .
```

then open the address it prints.

## Where things are

- `index.html`, the app
- `PRD.md`, what it is, what was decided, what to build next
- `CLAUDE.md`, working rules for Claude Code and for anyone editing

## Live artifact

The same page is published as a private Claude artifact (version 3, 25 September 2026). Inside the artifact the assistant panel can call Claude live. From disk it uses a built-in template. Both are intended.

## Start in Claude Code

Open this folder in Claude Code and say

```
Read CLAUDE.md and PRD.md, then tell me where the project stands and what Stage A needs.
```

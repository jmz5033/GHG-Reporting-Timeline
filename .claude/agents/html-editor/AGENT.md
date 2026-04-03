---
name: html-editor
description: Efficient agent for editing index.html. Use this agent for ALL edits to index.html to avoid loading the full 32k-token file into the main session context. Invoke when the user asks to change anything in index.html — CSS, HTML structure, JavaScript, colors, layout, data, etc.
allowed-tools: Read, Edit, Grep, Glob, Bash
---

# HTML Editor Agent — GHG Reporting Timeline

You are a focused, context-efficient agent for editing `index.html` in this repo. The file is ~32,000 tokens. You must never read it in full. Always work surgically.

## Mandatory workflow — follow every time

1. **Grep first** — find the relevant line numbers before reading anything
2. **Read only the range you need** — use `offset` and `limit` to fetch 20–60 lines around the target
3. **Edit surgically** — use the `Edit` tool with the smallest possible `old_string` that is still unique
4. **Verify** — Grep for a key string in your change to confirm it landed correctly

## File structure reference (memorize — don't read the file to find these)

| Section | Approx lines |
|---|---|
| `<style>` CSS block | 8 – 318 |
| `<body>` + sticky header + tab buttons | 320 – 340 |
| Timeline tab controls (legend, pills, year axis) | 341 – 370 |
| Scan tab HTML | 371 – 400 |
| Methodology tab HTML | 401 – 435 |
| `<script>` data section — `DATA` array | 440 – 570 |
| `METHODOLOGY_ENTRIES` array | 575 – 665 |
| Core JS functions (analyzeEntry, addPendingEntry) | 700 – 820 |
| Regulatory Scan JS (runRegScan, helpers) | 820 – 1100 |
| switchTab, setFilter, render | 1100 – 1180 |
| Timeline rendering (render, buildLegend) | 1180 – 1340 |
| DOMContentLoaded init | 1340 – 1360 |

## Rules

- **Never** call `Read` on `index.html` without `offset` and `limit`
- If you need to find something, use `Grep` with a pattern, then `Read` the returned line range ± 10 lines
- Keep edits atomic — one logical change per Edit call
- If a CSS change is needed, Grep for the class name first (e.g., `.pill-s3`) to get its line number
- If a JS function change is needed, Grep for `function functionName` first
- If an HTML section change is needed, Grep for a nearby unique string (e.g., `id="tab-scan"`)

## Color palette reference (avoids needing to read the file)

| Element | Color |
|---|---|
| Scope 1&2 (blue) | `#1E6BA8` (full), `#3579A8` (muted), `#90C2E7` (light), `#EBF4FD` (bg) |
| Scope 3 (orange) | `#B45309` (full), `#AC4A0C` (muted), `#F4A068` (light), `#FFF3ED` (bg) |
| All Scopes (purple) | `#7C3AED` (full), `#7B6CA5` (muted), `#C4B5F0` (light), `#F2EFFA` (bg) |
| Brand dark navy | `#1E2D3D` |
| Muted text | `#64748B` |
| Border | `#E2E8F0` |

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A single-page English vocabulary learning website for CET-4/CET-6 (Chinese college English tests). 2,243 words with exam frequency data, Chinese translations, and TTS audio links. Hosted on GitHub Pages at `https://3214796734.github.io/english-vocab`.

## Build & deploy

No build step. Static files served directly.

```bash
# Deploy: commit and push to GitHub Pages
git add . && git commit -m "..." && git push origin main
# Site: https://3214796734.github.io/english-vocab
```

## File architecture

- `index.html` — Complete app: HTML + CSS + JS in one file. List view with search/filter/pagination + flashcard study mode overlay.
- `data.js` — Vocabulary data: `cet4Words` and `cet6Words` arrays. Each entry: `{en, zh, simple, freq, examCount}`. Loaded via `<script>` before the inline app JS.
- `gen_words.js` — Generator script to rebuild `data.js`. Run with `node gen_words.js`.
- `audio/` — Optional local MP3 files for TTS (not currently used; TTS uses direct links to Youdao API).
- `config/` — mcporter config (for agent-reach skill, not related to the website).

## Key design decisions

- **No frameworks or dependencies.** Single HTML file with inline everything for zero-install deployment.
- **Audio: direct `<a>` links to Youdao TTS** (`https://dict.youdao.com/dictvoice?audio=WORD&type=0`). JavaScript `Audio()` API failed on Chinese mobile browsers (QQ Browser, Quark) due to cross-origin blocking. Direct links work because the browser handles playback natively.
- **Sorting:** Words sorted by `examCount` descending (most frequent first).
- **Pagination:** 50 words per page to keep DOM light with 2,243 entries.
- **Study mode:** Fullscreen overlay card with ←/→ navigation and keyboard shortcuts.

## Editing vocabulary

To add or change words, edit `gen_words.js` and run:

```bash
node gen_words.js
```

This regenerates `data.js` with the updated word lists. Word format in gen_words.js is compact: `["english","中文释义","freq"]`.

## Adding exam frequency data

The `examCount` field is separate from `freq` (high/mid/low category). To update counts, modify the Python script in the push history or edit `data.js` directly. Known real counts: `solve:30, decline:27, unique:25, environment:22`.

# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A small static site — the **NYOS Parent PSA Series**, a parent-led series of public service announcements for families of NYOS (a charter school). It's hosted on GitHub Pages (`wavecentral/nyos-psa`); there is no build step, no package manager, and no test suite. Editing an HTML file and pushing to `main` is the deploy.

`.nojekyll` is intentional — it disables Jekyll on GitHub Pages so files are served as-is.

## Tone & content guardrails

The series is deliberately framed as factual, calm, and parent-to-parent — not adversarial. When editing copy, preserve that voice: no inflammatory language, no speculation presented as fact, no naming individuals beyond what's already in the contributor list. The mission letter in `index.html` (the "Why this series exists" section) is the canonical statement of voice — match it.

`Public Service Announcements.docx` is the source content the parent group is drafting from; treat it as the source-of-truth for unpublished PSA copy.

## Architecture

Each page is a **fully self-contained HTML file** with its CSS embedded in a `<style>` block. There is no shared stylesheet, no JS framework, no templating. The pages:

- `index.html` — landing page, mission letter, contributor list, PSA index, CTA
- `what-makes-nyos-nyos.html` — PSA #01 (published)
- `what-makes-nyos-not-ordinary.html` — PSA #02 (linked as "Coming Soon" on the index but the file exists)
- `nyos-behind-the-scenes-timeline.html` — PSA #03 (linked as "Coming Soon" on the index but the file exists)

The "Coming Soon" status on the index is set by the `.status coming` span in each `psa-card`. Flip to `.status live` + label "Published" when promoting a PSA.

### Shared design system (duplicated across files)

Every page redeclares the same `:root` CSS variables and re-imports the same Google Fonts (Fraunces + Inter). Because there is no shared CSS file, **a design change must be applied to every HTML file** — search for the variable name across all pages before editing.

Current brand palette (matches nyos.org, last updated in `fbaecce`):

```
--cream: #eef2f7;       --paper: #f7f9fb;
--ink: #0d1b2a;         --ink-soft: #3d5a80;
--moss: #013468;        --moss-deep: #012148;
--ochre: #2a8fc2;       --rust: #1a6fb5;
--gold: #7bc7e1;        --rule: #c0cdd9;
```

Typography: **Fraunces** (serif, used for headlines, deck, letter body, contributor names) and **Inter** (sans, used for masthead, kickers, buttons, footer). Italic Fraunces in `--ochre` is the standard accent treatment for emphasized words inside headlines (e.g. `<em>Transparency.</em>`).

Recurring page chrome shared across all pages: `.masthead`, `.hero`, the `.ornament` divider (line/dot/line), `.cta` block, and the footer with the `school` tagline. Mobile breakpoint is `@media (max-width: 720px)` everywhere.

### Adding a new PSA

1. Copy an existing PSA HTML file (`what-makes-nyos-nyos.html` is the most complete example) as the starting template — keep its embedded `<style>` block intact so the design stays consistent.
2. Add a `psa-card` link to the listing in `index.html` with the next number, title, subtitle, description, and a `.status coming` or `.status live` span.
3. Don't extract shared CSS into a separate file unless the user explicitly asks — the duplication is a deliberate trade-off for a zero-build static site.

## Common tasks

- **Preview locally**: `python3 -m http.server 8000` (or any static server) from the repo root, then open `http://localhost:8000/`.
- **Deploy**: push to `main`. GitHub Pages serves it.
- **No linters, formatters, or tests are configured.** Don't add them unless asked.

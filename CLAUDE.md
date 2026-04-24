# CLAUDE.md — Brain Talents Explorer

## What this is

Single-file interactive web app (`index.html` + `talents/*.svg`) for Six Seconds' 18 Brain Talents. No build step, no dependencies. Embedded as an iframe at `www.6seconds.org/talents`.

GitHub repo: `6seconds-org/brain-talents-explorer` (GitHub Pages deployment).

---

## Where everything lives

All editable content is in the `<script>` block near the top of `index.html`:

- **`TALENTS`** array (~line 476) — ordered list of 18 talent IDs and their scale assignment. Order mirrors `brain-talents.svg`.
- **`I18N`** object (~line 500) — all translatable strings, keyed by language code (`en`, `it`, `zh`, `es`, `vi`). Each language block has the same shape:
  - `title`, `subtitle`, `closeHint`, `footer`
  - `scale` / `cluster` — short scale names (e.g. "Focus", "Decisions", "Drive")
  - `scales.focus/decisions/drive` — full scale modal content: `name`, `question`, `leftLabel`, `rightLabel`, `leftName`, `rightName`, `leftBody`, `rightBody`, `note`
  - `talents["talent-id"]` — `name` and `def` for each of the 18 talents

---

## Languages

| Code | Language | Native-speaker reviewed? |
|------|----------|--------------------------|
| `en` | English | Yes — source of truth |
| `it` | Italiano | Yes |
| `zh` | 中文 (Simplified) | Partially — scale modal copy AI-translated, flag for review |
| `es` | Español | Yes |
| `vi` | Tiếng Việt | No — AI-translated throughout, needs native review before wide release |

### Adding a language

1. Copy the `I18N.en` block, translate all strings
2. Add `<button data-lang="xx">XX</button>` inside `<nav class="lang">` in the HTML

---

## Translation sources

| Language | Names source | Definitions source |
|----------|--------------|--------------------|
| EN | `talent-definitions.png` | `talent-definitions.png` + Brain Talent Interpretation Guide |
| IT | English names (per Joshua) | `Brain-Talent-Interpretation-Guide-IT.pdf` p.4 |
| ZH | `BTP(Chinese).pdf` | `Brain-Talent-Interpretation-Guide-4.0-CN.pdf` pp.4–5 |
| ES | Updated team graphic (Apr 2026) | `Guía+de+Brain+Talent+ESP.pdf` pp.4–6 |
| VI | `Talent translation - Vietnamese - Josh.xlsx` | Same xlsx; UI + scale copy AI-translated |

Scale modal copy source: `BBP Guide Final - for end users v1 2026.pdf` page 4 (EN verbatim).

Reference files (not deployed): PDFs, `brain-talents.svg`, `talent-definitions.png`, `3 Scales-CN.pptx`, `Talent translation - Vietnamese - Josh.xlsx` — all in the project root.

---

## Workflow: applying translation corrections

Colleagues sometimes send corrections as `.pptx` files (e.g. `3 Scales-CN.pptx`). To read them without converting:

```bash
python3 -m venv /tmp/pptx-venv
/tmp/pptx-venv/bin/pip install "markitdown[pptx]" -q
/tmp/pptx-venv/bin/python -m markitdown "path/to/file.pptx"
```

The pptx will contain text like `Change to "连接"` — map these to the relevant `I18N.zh.talents` or `I18N.zh.scales` fields in `index.html`.

---

## Scale color coding

- **Focus** — blue `#1d8cc4`
- **Decisions** — orange `#e25c2c`
- **Drive** — green `#6fb43d`

---

## Build progress

| Date | What was done |
|------|---------------|
| Apr 2026 | Initial app: 18 talents, EN/IT/ZH/ES |
| Apr 2026 | Spanish talent names updated per new team graphic |
| Apr 2026 | Clickable scale bars with wider scale modals added |
| Apr 2026 | Vietnamese (VI) added as 5th language |
| Apr 23 2026 | ZH corrections from colleague's pptx: connection name → 连接, updated defs for connection + emotional-insight, Focus scale axis labels → 理性的/感性的, Drive cluster labels updated |

---

## Todos

- [ ] Native-speaker review of Vietnamese (VI) — all strings AI-translated
- [ ] Native-speaker review of ZH scale modal copy (questions, blurbs, notes)
- [ ] Consider responsive iframe height for mobile (`~1100px` works but isn't automatic)

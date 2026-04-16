# 18 Brain Talents — Interactive Explorer

A self-contained, no-build, single-page web app that lets visitors click any of the
18 Six Seconds Brain Talents to read its definition. Includes a language selector
for **English / Italiano / 中文 / Español**.

Designed to be embedded as an `<iframe>` inside the Divi page at
`www.6seconds.org/talents`.

## Files

```
brain-talents/
├── index.html          # the entire app (HTML + CSS + JS, no build step)
├── talents/            # 18 talent icons (.svg) referenced by index.html
└── README.md
```

Source PDFs and the layout reference (`brain-talents.svg`, `talent-definitions.png`,
the four PDFs) live alongside but are **not** required at runtime — only
`index.html` and the `talents/` folder need to be deployed.

## Local preview

Just open `index.html` in a browser, or run any static server, e.g.:

```bash
cd brain-talents
python3 -m http.server 8000
# then visit http://localhost:8000/
```

## Deploy to GitHub Pages

1. Create a new public repo in the `6seconds-org` organisation, e.g.
   `brain-talents-explorer`.
2. From the project folder:
   ```bash
   cd /Users/joshuafreedman/Desktop/CLAUDE/brain-talents
   git init
   git add index.html talents README.md
   git commit -m "Initial Brain Talents explorer"
   git branch -M main
   git remote add origin git@github.com:6seconds-org/brain-talents-explorer.git
   git push -u origin main
   ```
   (Use the HTTPS URL instead of SSH if you prefer.)
3. In GitHub → repo **Settings → Pages**, set:
   - **Source**: *Deploy from a branch*
   - **Branch**: `main`, folder `/ (root)`
4. After ~1 minute the page will be live at:
   ```
   https://6seconds-org.github.io/brain-talents-explorer/
   ```

## Embed in Divi (iframe)

Add a **Code Module** to the `/talents` page and paste:

```html
<div style="position:relative;width:100%;max-width:1100px;margin:0 auto;">
  <iframe
    src="https://6seconds-org.github.io/brain-talents-explorer/"
    title="The 18 Brain Talents"
    loading="lazy"
    style="width:100%;height:760px;border:0;display:block;"
    allow="clipboard-write"></iframe>
</div>
```

Tweak `height` to taste. On narrow screens the layout reflows to 3 columns,
so on phones an iframe height of `~1100px` looks best — if you want it perfectly
responsive, replace the inline style with a CSS aspect-ratio container.

## Editing content

All copy lives at the top of the `<script>` block in `index.html`:

- `TALENTS` — order of the 18 talents (mirrors `brain-talents.svg`).
- `I18N` — every translatable string per language. Each language has:
  - `title`, `subtitle`, `closeHint`, `footer`
  - `scale` and `cluster` headers (Focus / Decisions / Drive)
  - `talents[id].name` and `talents[id].def` for all 18 talents.

To add a 5th language, copy the `I18N.en` block, translate the strings, then
add a `<button data-lang="xx">XX</button>` inside `<nav class="lang">`.

## Translation sources

| Language | Names                              | Definitions                                              |
|----------|------------------------------------|----------------------------------------------------------|
| EN       | `talent-definitions.png` (English) | `talent-definitions.png` + Brain Talent Interpretation Guide |
| IT       | English (per Joshua)               | `Brain-Talent-Interpretation-Guide-IT.pdf` p.4           |
| ZH       | `BTP(Chinese).pdf`                 | `Brain-Talent-Interpretation-Guide-4.0-CN.pdf` pp.4–5    |
| ES       | `BTP(Spanish).pdf`                 | `Guía+de+Brain+Talent+ESP.pdf` pp.4–6                    |

## Browser support

Vanilla HTML/CSS/JS, no dependencies. Works in all evergreen browsers
(Chrome, Safari, Firefox, Edge). Selected language is persisted via
`localStorage` so returning visitors keep their preference.

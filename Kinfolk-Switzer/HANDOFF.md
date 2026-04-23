# SakeYō Kinfolk rebuild — handoff

**To the new session:** read this end-to-end before touching anything. The design direction is locked. Your job is to rebuild 37 screens in this folder using [UI-Kit.html](UI-Kit.html) as the single source of truth.

---

## 1. Folder structure

```
Daniel Feedback - Test/
├── Original/           # Original prototype (reference only, don't touch)
├── Daniel/             # 4 reference screens we ported patterns from (reference only)
├── Apple/              # Previous rebuild (Inter-only, Type C). Current live on GitHub Pages.
│   └── Images/         # All photo/bottle assets
└── Kinfolk/            # ← YOU WORK HERE
    ├── HANDOFF.md       # this file
    ├── UI-Kit.html      # visual spec for every component
    ├── SakeYo_Home.html # canonical reference — the only screen already rebuilt
    └── Images/          # symlink → ../Apple/Images (reuse all existing assets)
```

**Workflow per screen:**
1. Open `Apple/<source-screen>.html` to see current structure, wiring, and content.
2. Rebuild as `Kinfolk/<new-screen>.html` matching the UI kit's tokens and patterns.
3. Image paths stay `Images/xxx.png` — the symlink handles it.
4. Internal links point to other Kinfolk files (drop the `_TypeC` suffix — folder is now the qualifier).

**Filenames in Kinfolk/** (drop `_TypeC`):
- `SakeYo_Home.html` (done)
- `sakeyo_Explore.html`, `sakeyo_Japan.html`, `sakeyo_Local.html`, `sakeyo_Pairings.html`, `sakeyo_Craft.html`, `sakeyo_Breweries.html`
- `sakeyo_101_Hub.html`, `sakeyo_101_Styles.html`, `sakeyo_101_Daiginjo.html`, `sakeyo_101_Glossary.html`
- `sakeyo_Brewery_Hakugakusen.html`, `sakeyo_Brewery_Shinrai.html`, `sakeyo_Brewery_Kinmon.html`, `sakeyo_Brewery_Daijiro.html`, `sakeyo_Brewery_Natsuta.html`
- `sakeyo_Cellar.html`, `sakeyo_Cellar_Detail.html`, `sakeyo_Diary.html`, `sakeyo_Profile.html`, `sakeyo_Alerts.html`
- `sakeyo_Calendar.html`, `sakeyo_Calendar_Month.html`, `sakeyo_Event_Detail.html`
- `sakeyo_Counter.html`, `sakeyo_Counter_Note.html`
- `sakeyo_Wishlist.html`, `sakeyo_Passport.html`
- `sakeyo_Article.html`, `SakeYo_Bottle_Detail.html`, `sakeyo_Search.html`
- `sakeyo_Scan_Capture.html`, `sakeyo_Scan_Result.html`, `sakeyo_Scan_NotFound.html`, `sakeyo_Scan_BackLabel.html`, `sakeyo_Scan_Thanks.html`

**First housekeeping task:** once a few Kinfolk screens exist, update `SakeYo_Home.html` internal links (currently still pointing at `sakeyo_Explore_TypeC.html` etc. from when Home lived in `Apple/`).

---

## 2. Design direction — "Kinfolk-for-sake"

Move the app from "tech product using Inter" to "editorial Japanese brand" — Kinfolk-adjacent, quietly confident, no marketing gloss.

**Canonical reference:** `Kinfolk/SakeYo_Home.html` — the only screen already rebuilt. Every other screen matches its tokens, patterns, and rules.

**Visual spec:** open `Kinfolk/UI-Kit.html` in a browser alongside the target screen as you work.

---

## 3. Design tokens (copy-paste into every screen)

### Font loading (`<head>`)

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preconnect" href="https://api.fontshare.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&display=swap" rel="stylesheet">
<link href="https://api.fontshare.com/v2/css?f[]=switzer@400,500,600,700&display=swap" rel="stylesheet">
```

### CSS variables (`:root`)

```css
:root{
  --black:#0d131a; --black-deep:#05080c; --charcoal:#1a2028;
  --crimson:#9B1B1B; --crimson-deep:#7A1515;
  --cream:#F5F0EB; --cream-deep:#EDE6DD; --cream-warm:#E8DFD2;
  --cream-lit:#F5EBD8; --cream-shadow:#D4C5AA;   /* NEW: bottle stage */
  --ink:#101820; --ink-mute:#5F5E5A; --ink-faint:#8A8880;
  --hair:rgba(16,24,32,0.10); --hair-soft:rgba(16,24,32,0.06); --hair-strong:rgba(16,24,32,0.14);
  --sage:#2D6B4F;
  --sans:'Switzer','Inter',-apple-system,BlinkMacSystemFont,'SF Pro Display',system-ui,sans-serif;
  --serif:'Instrument Serif', Georgia, 'Iowan Old Style', serif;
  --r-sm:6px; --r-md:12px;
}
```

### Phone frame

```css
.device{width:436px;background:#0a0a0a;border-radius:52px;padding:10px;box-shadow:0 30px 80px rgba(0,0,0,.5),0 0 0 2px #1a1a1a inset;height:fit-content}
.phone{width:416px;height:896px;background:var(--cream);overflow-y:auto;overflow-x:hidden;border-radius:42px;scrollbar-width:none;position:relative;box-shadow:inset 0 0 0 1px #0a0a0a}
.phone::-webkit-scrollbar{display:none}
```

42px radius + 1px black inset rim are required — they eliminate rounded-corner edge artifacts.

### Typography scale

| Element | Font | Size | Weight | Letter-spacing |
|---|---|---|---|---|
| Greeting name | serif | 36px | 400 | -0.018em |
| Hero H1 | serif | 44px | 400 | -0.018em |
| Feature article title | serif | 26px | 400 | -0.012em |
| Pullquote | serif italic | 18px | 500 | -0.005em |
| Italic bot-cat, event-meta, byline-date | serif italic | 12px | 400 | 0 |
| Section title | sans | 24px | 600 | -0.022em |
| Overline | sans | 10px | 600 | 0.20em uppercase |
| Body / deks | sans | 14.5px | 400 | -0.008em |
| Bottle name | sans | 14.5px | 600 | -0.018em |
| Event title | sans | 14.5px | 600 | -0.018em |
| Chip | sans | 13px | 500 | -0.005em |
| Meta / faint | sans | 11–12px | 400–500 | -0.005em |

---

## 4. Design patterns (7 — full specs in UI-Kit.html)

1. **White mini-menu tiles** — 44px flat cream-deep discs with hairline border. Home + 101 Hub.
2. **Section header** — overline + trailing hairline + sans title + crimson "All →" link. Every section, every screen.
3. **Lit-stage bottle background** — warm radial (cream-lit → cream-warm → cream-shadow) with blurred contact shadow and upper-right vignette. Every bottle that sits on cream. Skip bottles on dark chiaroscuro heroes.
4. **Filled crimson hero pill** — featured sections on dark backgrounds.
5. **Sage match ring with serif digit** — no white disc. Track: `rgba(255,255,255,0.5)` on warm stage, `var(--hair)` on cream.
6. **Crimson "All →"** — flat crimson text link.
7. **Numbered 01/02/03 contents list** — italic crimson numerals. Home "Stories & reading" + Sake 101 pieces.

CSS snippets and visual examples are in `UI-Kit.html` sections 01–10 + live in `SakeYo_Home.html`.

---

## 5. Global rules (apply to every screen, no exceptions)

1. **No "X min read" anywhere.** Strip every read-time. Bylines collapse to author name.
2. **No photo captions anywhere.** Strip italic caption lines beneath hero images.
3. **Phone frame:** `border-radius:42px; box-shadow:inset 0 0 0 1px #0a0a0a`. Not 44px, not 46px.
4. **"Community" never "Counter"** as a visible label. (Filename `sakeyo_Counter.html` is fine.)
5. **Sentence case everywhere** except `SAKEYŌ` wordmark.
6. **代 footer on every screen** — 72px kanji, `rgba(16,24,32,0.10)`, + "One rung at a time." in `var(--ink-mute)`, centered.
7. **Filled crimson reserved** for: hero pill, scan FAB, "Yes, this is it" scan primary, Thank-you page primary. Every other accent is flat crimson text.

---

## 6. Screen-by-screen execution plan

Work in batches. After each batch, show the user the first screen for approval before continuing. **Do not push anything until the user explicitly says "push."**

### Batch A — Hubs (13 screens)

- [ ] `sakeyo_Explore.html` ← source: `Apple/sakeyo_Explore_TypeC.html` **(first — show user)**
- [ ] `sakeyo_Japan.html`
- [ ] `sakeyo_Local.html`
- [ ] `sakeyo_Pairings.html`
- [ ] `sakeyo_Craft.html`
- [ ] `sakeyo_Breweries.html`
- [ ] `sakeyo_101_Hub.html`
- [ ] `sakeyo_Counter.html`
- [ ] `sakeyo_Wishlist.html`
- [ ] `sakeyo_Passport.html`
- [ ] `sakeyo_Calendar.html`
- [ ] `sakeyo_Calendar_Month.html`
- [ ] `sakeyo_Search.html`

### Batch B — Editorial (9 screens)

- [ ] `sakeyo_Article.html` ← Shu Shu **(first — show user)**
- [ ] `sakeyo_101_Styles.html`
- [ ] `sakeyo_101_Daiginjo.html`
- [ ] `sakeyo_101_Glossary.html`
- [ ] `sakeyo_Brewery_Hakugakusen.html`
- [ ] `sakeyo_Brewery_Shinrai.html`
- [ ] `sakeyo_Brewery_Kinmon.html`
- [ ] `sakeyo_Brewery_Daijiro.html`
- [ ] `sakeyo_Brewery_Natsuta.html`

### Batch C — Data (8 screens)

- [ ] `sakeyo_Cellar.html` **(first — show user)**
- [ ] `sakeyo_Cellar_Detail.html`
- [ ] `sakeyo_Diary.html`
- [ ] `sakeyo_Profile.html`
- [ ] `sakeyo_Alerts.html`
- [ ] `sakeyo_Event_Detail.html`
- [ ] `sakeyo_Counter_Note.html`
- [ ] `SakeYo_Bottle_Detail.html`

### Batch D — Scan flow (5 screens)

- [ ] `sakeyo_Scan_Capture.html`
- [ ] `sakeyo_Scan_Result.html`
- [ ] `sakeyo_Scan_NotFound.html`
- [ ] `sakeyo_Scan_BackLabel.html`
- [ ] `sakeyo_Scan_Thanks.html`

### Batch E — Polish pass

- Update `SakeYo_Home.html` internal links to point to new Kinfolk filenames
- Walk every screen — Switzer loading, no read times or captions, section headers have overline + hairline, bottles have lit stages where appropriate
- Verify FAB + bnav + back links across all screens

### Batch F — Commit + push

GitHub: `https://github.com/Sakesho/SakeYo`. Pages deploys from `main` → live on `https://sakesho.github.io/SakeYo/Kinfolk/<file>`.

Clone fresh to `/tmp/SakeYo-push` if the prior clone is stale. Copy `Kinfolk/` into the clone, commit, push. After push, verify:

- `https://sakesho.github.io/SakeYo/Kinfolk/SakeYo_Home.html`
- `https://sakesho.github.io/SakeYo/Kinfolk/sakeyo_Explore.html`
- One screen from each batch

---

## 7. Wiring that must stay correct

- FABs → `sakeyo_Scan_Capture.html`
- bnav Search tile → `sakeyo_Search.html`
- Back links → previous screen in flow
- Explore hub tile links → `sakeyo_Japan.html`, `sakeyo_Local.html`, etc.
- Alerts forum card → `sakeyo_Counter.html` (file name OK; visible text says "Community")
- Japan tabs → Featured (Japan hub), Wishlist, Passport
- Breweries 01–05 → individual brewery profiles
- Cellar bottles → Cellar Detail (Kubota)
- Community post clicks → Counter Note
- 101 Hub tiles → Styles (Glossary util → Glossary)
- Styles pieces → Daiginjo
- Scan Result primary "Yes, this is it" → Bottle Detail
- Scan Result secondary "Not my sake" → Scan NotFound
- Scan NotFound "Help us add it" → Scan BackLabel
- Scan BackLabel capture → Scan Thanks

---

## 8. First action for the new session

1. Read this file end-to-end.
2. Open `Kinfolk/UI-Kit.html` in a browser — scroll sections 01–11.
3. Open `Kinfolk/SakeYo_Home.html` — internalize the reference screen.
4. Start Batch A: open `Apple/sakeyo_Explore_TypeC.html` (source), build `Kinfolk/sakeyo_Explore.html` (new).
5. Show the user the result before moving on.
6. **Do not push until the user says "push."**

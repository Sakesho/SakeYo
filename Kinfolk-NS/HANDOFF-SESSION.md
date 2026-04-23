# SakeYō Kinfolk Rebuild — Session Handoff

## Where we are

Rebuilding 37 screens of the SakeYō sake-discovery app in a "Kinfolk-for-sake" editorial direction. Working folder: `/Users/bradbrown/Desktop/Daniel Feedback - Test/Kinfolk/`.

**Batch A progress (8 of ~14 screens done):**
- ✅ `SakeYo_Home.html` — hero toned down, match ring refined, bottle cards unified cream-warm, tile SVGs audited
- ✅ `sakeyo_Explore.html` — 6 editorial shelves, story cards, synced images with Craft
- ✅ `sakeyo_Japan.html` — masthead *"Japan, the sake, and where we drink it."*, map dropped, hero + feat cards
- ✅ `sakeyo_Local.html` — masthead *"The places we love and the people who make sake great in Singapore."*, Featured + Wishlist tabs
- ✅ `sakeyo_Pairings.html` — masthead *"Sake, food, and the matches you'd never guess."*, chip rows + 3 feats
- ✅ `sakeyo_Craft.html` — masthead *"Beyond the bottle. The people building a culture."*, 3 sections (Craft/Food/Seasons)
- ✅ `sakeyo_Breweries.html` — masthead *"The brewers, the kura, and their sake."*, 5 numbered brewery cards
- ✅ `sakeyo_101_Hub.html` — masthead *"Start here. Rice, water, regions, and labels."*, 7 topic tiles + featured article + start cards + editor's note

**Still pending in Batch A:** Counter/Community, Wishlist, Passport, Calendar, Calendar_Month, Search
**Later batches:** B (Editorial articles), C (Data screens), D (Scan flow), E (Polish), F (Commit + push)

## Open items from last turn (unresolved)

The user's last message raised three things and requested **feedback only, not execution**:

1. **Oysters hero photo** — user sent 6 new oyster/seafood photos and asked if any work better as "the oysters on the home page." Home doesn't currently have an oyster hero — the user likely means the **Pairings page oyster hero** (the current hero card for "Why a cold junmai is the only thing an oyster actually wants"). User says the existing crop cuts off oysters and doesn't feel like a hero. **Needs clarification + a recommended pick.**
2. **101 Hub feels light** — user asked "Should we add examples of each section below?" Needs a proposal (e.g., a short "On the reading list" section below the start cards with 3–4 example articles, or example styles/regions under each topic tile).
3. **"i can't delete"** — cryptic. Unclear what the user is trying to delete. **Ask for clarification.**

## Files to read for background

**Read first (in order):**
1. `Kinfolk/HANDOFF.md` — source of truth for design tokens, patterns, global rules
2. `Kinfolk/UI-Kit.html` — visual spec for components
3. `Kinfolk/SakeYo_Home.html` — canonical reference screen; most patterns originate here
4. One or two of the completed screens above to see how patterns are applied (e.g., `sakeyo_Craft.html` for editorial sections, `sakeyo_Breweries.html` for numbered contents)

## Design system (quick reference)

**Frame:** 416×896, 42px radius, 1px black inset
**Type:** Switzer (sans) + Instrument Serif
**Tokens:** crimson `#9B1B1B`, cream `#F5F0EB`, cream-deep `#EDE6DD`, cream-warm `#E8DFD2`, ink `#101820`, sage `#2D6B4F`
**Kinfolk photo filter:** `brightness(1.04) contrast(0.88) saturate(0.82) sepia(0.06)`
**Per-image crop:** tune via `object-position`

## Pattern vocabulary

- **Story cards** — many items, for scanning (Explore, Home FOR YOU)
- **Feat cards** — fewer items, editorial read (Japan, Local, Pairings, Craft)
- **Brewery cards** — numbered contents-list style, italic-serif crimson numbers (Breweries)
- **Bottle cards** — product tiles, unified cream-warm, centered sake name + brewery (Home)
- **Topic tiles** — 101 Hub style, flat and quiet
- **Masthead** — uppercase label + italic serif tagline, thin rule below
- **Overlines** — crimson 10/500 tracking .16em uppercase, only on cream backgrounds (muted white on dark)

## Process rules (must follow)

1. **Propose changes before executing** — user pays per token. For any edit with multiple possible interpretations, draft the change in text, wait for go-ahead, then execute.
2. **Never push to GitHub** until the user explicitly says "push".
3. **Banned words:** "table", "counter", "pour", "glass" (in copy, especially mastheads).
4. **Crimson overlines only on cream.** On dark/black backgrounds (like the Explore masthead area) use muted white 12/500 so crimson doesn't compete with the crimson YŌ in the wordmark.
5. **Binary image writes are impossible** — I cannot save attached photos. User must drop files into `Kinfolk/Images/` with the filename I specify, then I reference it.
6. **Photo workflow:** user exports clean from Apple Photos at native resolution; the Kinfolk CSS filter does the treatment. No in-Photos editing needed.

## User profile

- Product owner + design collaborator
- Wants an editorial, restrained Japanese-adjacent aesthetic (Kinfolk, NYT Cooking, Monocle — **not Vivino**)
- Terse communicator; expects me to flag ambiguities, suggest improvements, enforce consistency
- Will push back on cliché, fluff, and any repeated banned vocabulary

## Suggested opening move for new session

1. Read `HANDOFF.md`, `UI-Kit.html`, `SakeYo_Home.html`, and this file.
2. Answer the three open items above with proposals (not edits).
3. Ask the user to clarify "i can't delete" and whether "oysters on the home page" means Pairings.
4. Once those are resolved, continue Batch A with whichever screen the user picks next.

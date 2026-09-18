# Imaginary Football

Static site for the Sleeper league **Imaginary Football** (`1385283286355423232`).
Same engine as the Sigma Pi B site, reconfigured for this league.

Live at `https://flanpow123.github.io/imaginary-football/` once Pages is switched on.

## First-time setup

1. On GitHub, create a new repository named exactly **`imaginary-football`**, public.
2. Upload these files, keeping the folder structure:
   ```
   index.html
   news.json
   og.png
   archive/index.json
   ```
   Drag `index.html`, `news.json` and `og.png` in first. Then use **Add file → Create new file**,
   type `archive/index.json` as the name (the slash makes the folder), and paste in the
   contents of the `archive/index.json` from this batch.
3. Go to **Settings → Pages**, set Source to **Deploy from a branch**, branch **main**, folder **/ (root)**, Save.
4. Wait a minute or two, then open the URL.

If the repo name is ever changed, the `og:url` and `og:image` tags near the top of
`index.html` have to be changed to match, or link previews break.

## Weekly workflow

Same as the other site:

1. Open `https://flanpow123.github.io/imaginary-football/?desk`
2. Click **Copy this week's briefing**
3. Paste it to Claude
4. Claude returns three files: a new `news.json`, a new `archive/2026-week-NN.json`,
   and an updated `archive/index.json`
5. Upload all three

The `?desk` parameter is the only thing that reveals the briefing buttons. Regular
visitors see no sign a human is involved.

## What is different from the Sigma Pi B site

- **Five analysts, not four.** Fat McCafferty (`wildcard`) is the fifth — AM overnight radio,
  total conviction, no supporting logic. He gets a championship pick and the Title Picks
  tab tracks it against reality like everyone else's.
- **Real names.** The `MANAGERS` map near the top of the script turns Sleeper handles into
  first names under each team. Edit that block if someone joins or leaves.
- **Projections are scored from this league's own settings.** Sleeper publishes `pts_ppr`
  with 4-point passing touchdowns baked in; this league pays 6, so every quarterback was
  being undersold by three or four points a week. Defenses and kickers still use Sleeper's
  number, because their scoring is bucketed and cannot be recomputed that way.
- **Missing projections are filled once, globally.** A player Sleeper has no projection for
  gets a replacement-level number at his position instead of a zero, and every part of the
  page now shares that one corrected map.
- **The briefing lists the desk.** A `THE DESK` section names every analyst key and persona,
  so the first edition knows the cast before any archive exists.

## Title Picks

Each analyst has one team picked to win the championship. The picks live in the
`TITLE_PICKS` block near the top of the script in `index.html`, not in `news.json`,
because they are locked for the season — one team, one quote, no edits. The tab
tracks each pick's record, points and power ranking and re-sorts itself every week.

To change a pick (a new analyst, a mid-season reset) edit that block. Team names are
matched the same way everywhere else on the site, so a manager renaming his team on
Sleeper does not break anything.

## Tabs

Two rows. The top row — Newsroom, The Desk, Power, Rivalries, Title Picks — is what this site
does that Sleeper does not. The bottom row is the reference material. The nav wraps instead of
scrolling, so there is no scrollbar at any screen width.

**The Book is shelved.** It was a private against-the-spread card saved in one browser, which
meant nobody could compare theirs to anybody else's. The tab and its `<section>` are gone but
`renderBook()` and the grading code are still in the file and still work — restoring it needs a
nav button and a `<section id="tab-book">`. The grading machinery is wanted for the analyst
pick'em, which is why it was shelved rather than deleted.

## The draft report card

The card grades against week 1 projections, which never change once week 1 is over.
To pin them permanently, open the site with `?desk`, click **Copy draft snapshot**, and
save the result as `draft-projections.json` in the repo root. Until that file exists the
card recomputes from the week 1 feed, which is stable but not frozen.

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

## Two kinds of story

The Newsroom is for what happened in the games. **Petty Cash** is for what people paid, and it
replaced the old Moves tab rather than sitting beside it.

A story routes itself. Give it `"section": "wire"` in `news.json` and it renders on Petty Cash;
leave the key off and it stays in the Newsroom. The share card only ever leads with a Newsroom
story, so a waiver receipt can never end up as the week's headline by accident.

Petty Cash also computes **the ledger** with no writing involved: every winning bid, what it cost,
and what the player has produced *for the team that paid*. Two columns matter — **Started** is the
part that counted, **Total** includes bench scoring, and the gap between them is usually the story.
A player scoring 20 a week on somebody's bench has not been bought, he has been stored. Underneath
it the page names the week's Steal (a $0–$1 claim that produced) and its Reach (a bid of $5 or more
that did not).

Then the FAAB budgets, the full move feed, and "the one that got away" — every losing bid and what
that player has scored since.

## The share card

`?desk` reveals a **Share this week** button that draws the week onto a 1080x1350 PNG and hands it
to the phone's share sheet, or downloads it on desktop.

The card leads with one story. Add `"share": true` to a story in `news.json` and the card uses its
headline and pulls its quote from that story's takes. With no flag set it uses the top story. Keep
the headline short — the card gives it two lines and truncates after that.

## Injuries

The briefing carries an **INJURY REPORT** section and tags every starter line with Sleeper's
designation, e.g. `DJ Moore (WR · BUF) 13.1 -> -0.10  [final]  [Out · Shoulder]`. A starter who
finished under 40% of his projection while carrying a designation is flagged as having likely
left the game.

This costs nothing extra: the page already downloads Sleeper's player file for names, and the
injury fields are on the same objects. Nothing has to be looked up by hand at writing time, and
because the injury is *in* the briefing, the rule that every claim must come from the briefing
covers it.

Two limits worth knowing. Sleeper updates designations on its own schedule, so a player hurt in
a game that has just ended may not be listed yet. And a designation says a player is hurt, not
that an injury caused a specific bad score — the briefing says so explicitly and tells the desk
to report the number rather than invent the reason.

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

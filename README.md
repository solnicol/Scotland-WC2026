# Scotland — FIFA World Cup 2026

A single-page tribute to Scotland's first World Cup since 1998.

**Live:** [scotland-wc2026.vercel.app](https://scotland-wc2026.vercel.app)

## What this is

A static HTML page covering Scotland's Group C campaign: the three group-stage fixtures, the 26-player squad, the venues, and a few details a Scottish fan might want to take with them — calendar files for each kickoff, Google Maps satellite views of the stadiums, kickoff times rendered in the visitor's local timezone.

No framework, no build step. One HTML file, three `.ics` files, one `vercel.json` header rule. `git clone` and open in a browser.

## Design notes

A few deliberate choices that I think are interesting:

**OKLCH palette discipline.** The entire page works in a single brand hue (Scotland navy, H=264) with one warm accent (heraldic gold, H=85). Every flag, every club crest wash, every UI element is hue-shifted from those two points. No `#hex` values, no exception colours. Opponent flag colours are also OKLCH, hue-true to each nation.

**Timezone-aware kickoffs.** Match times are stored as UTC ISO timestamps in `data-utc` attributes and re-rendered on the client via `Intl.DateTimeFormat`. The visitor sees their device's local time and timezone abbreviation, with the date adjusted if the kickoff crosses midnight in their region.

**iOS-safe calendar handoff.** Each match has a pre-generated static `.ics` file served with a `Content-Type: text/calendar` header (via `vercel.json`). Tapping "Add to calendar" navigates directly to that URL — iOS Safari then hands off to the Calendar app and opens a pre-filled new-event sheet. The blob-URL approach common in tutorials fails on iOS because the `download` attribute is ignored; a real URL with the right MIME type works.

**Animated gold sheen on the wordmark.** The "SCOTLAND" heading has a continuously sweeping highlight, implemented as a tiled linear-gradient animated via `background-position`. The travel distance is exactly one tile width, so the loop is seamless — no visible jump when the animation restarts. The hue stays inside the gold range (H=78–90) so the palette discipline isn't broken for the sake of an effect.

**iOS-safe long-press gestures.** The page uses Pointer Events with `touchAction: 'none'` and `contextmenu` suppression so iOS's long-press selection menu and image-save callout don't kick in mid-gesture. This lets the page own custom long-press behaviour cleanly on touch devices.

**Single file, no build.** Deliberate. The whole site is one HTML document. It runs identically from a CDN, a local filesystem, or an email attachment. There's nothing to install, nothing to break in CI, and the project will still work in ten years.

## Stack

- HTML, CSS, vanilla JS — no framework
- OKLCH colour space
- Hosted on Vercel (static project, no functions)
- One `vercel.json` rule to set the calendar MIME type

## Project layout

```
index.html              # the page
haiti-v-scotland.ics    # calendar files, served with Content-Type: text/calendar
scotland-v-morocco.ics
scotland-v-brazil.ics
vercel.json             # MIME type header for *.ics
```

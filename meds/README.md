# Medication Tracker

A single-page app for logging when you took your medication. One HTML file, no
build step, no dependencies, no server, no account. Everything is stored in your
own browser.

## Run it locally

The app is one file, so any static server works. From the repository root:

```sh
# Option 1 — Python (already on macOS and most Linux systems)
python3 -m http.server 8000

# Option 2 — Node
npx serve .
```

Then open <http://localhost:8000/meds/> (adjust the port if your server prints a
different one).

> **Prefer a `localhost` URL over double-clicking the file.** Opening
> `index.html` directly gives a `file://` page, where some browsers block local
> storage — the app will show a warning banner and your entries won't survive a
> reload. Serving over `http://localhost` avoids this entirely.

## What it starts with

| Medication | Dose per take | Spacing | Max per day | Notes |
|---|---|---|---|---|
| Optalgin | 40 drops | at least 6 h | 3 | |
| Rokacet | 1 tablet | — | 3 | double dose allowed (`×2` button) |
| Taragin | 20 mg | — | 1 | tagged *Before sleep* |

Every one of those figures is editable in **Settings**, and none of them is a
suggestion from the app — they are the numbers you supplied.

**The daily maximum counts doses, not button presses.** One `×2` on Rokacet
counts as 2 of the 3, so `×2` then a single tablet reaches the limit exactly,
and a second `×2` would overshoot it.

## Using it

**Today** — one card per medication. `Take now` stamps the current time; `×2`
(Rokacet only) logs a double. `Other time` opens a date/time picker for a dose
you took earlier and forgot to log. Each card shows how long ago you last took
it and where you are against the daily maximum.

A few guards, because this is easy to mis-tap:

- Every log shows an **Undo** for a few seconds, and deletes are undoable too.
- Logging inside a medication's spacing window asks first, and says when the
  window is up.
- Logging past the daily maximum asks first, and says what the running total
  would become.
- Where no spacing is set, a second tap within two minutes asks — so a fumbled
  double-tap isn't silently recorded as two doses.

Each of these is a **confirm, never a refusal**. If you really did take an extra
dose, say yes and the log records what actually happened. A tracker that argues
with you is a tracker you stop trusting.

**History** — every entry, newest first, grouped by day, with `×2` marked. `×`
deletes one.

**Settings** — edit any medication's name, dose, spacing, daily maximum, whether
doubles are allowed, and an optional *when* label. Export or import your data.
Edits save as you type.

This app is a log. It is not medical advice, and it does not check your
medications against each other.

## Where the data lives

In this browser's `localStorage`, under the key `medtracker.v1`. It is never
uploaded anywhere — there is no backend to upload it to.

The practical consequence: **the log is per-browser and per-device.** Your phone
and your laptop keep separate logs, and clearing site data wipes it. Use
`Export JSON` in Settings for a backup you can re-`Import` later, or `Export CSV`
to open the log in a spreadsheet.

## Changing the medications

Easiest is Settings, in the app. To change what a brand-new install starts with,
edit `defaultState()` near the top of the `<script>` block in `index.html`.

If you opened an earlier build that started with `Medication 1/2/3`, it upgrades
itself to the regimen above on next load — but only if you never renamed
anything and never logged a dose. Anything you customised is left untouched.

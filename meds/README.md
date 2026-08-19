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

## Using it

**Today** — one card per medication. `Take now` stamps the current time.
`Other time` opens a date/time picker for a dose you took earlier and forgot to
log. Each card shows how long ago you last took it and how many doses you've had
today.

A few guards, because this is easy to mis-tap:

- Every log shows an **Undo** for a few seconds.
- Tapping `Take now` twice within two minutes asks whether you really meant a
  second dose.
- Deleting an entry is also undoable.

**History** — every entry, newest first, grouped by day. `×` deletes one.

**Settings** — rename the three medications, set an optional dose label, and
export or import your data. Edits save as you type.

### The "min hours between doses" field

Optional, and blank by default. If you enter a number, the card shows the
earliest time your own spacing allows for the next dose. It is a reminder of a
figure *you* enter from your prescription — the app never suggests a dose or an
interval, and it never blocks you from logging one.

This app is a log. It is not medical advice.

## Where the data lives

In this browser's `localStorage`, under the key `medtracker.v1`. It is never
uploaded anywhere — there is no backend to upload it to.

The practical consequence: **the log is per-browser and per-device.** Your phone
and your laptop keep separate logs, and clearing site data wipes it. Use
`Export JSON` in Settings for a backup you can re-`Import` later, or `Export CSV`
to open the log in a spreadsheet.

## Changing the medications

Easiest is Settings, in the app. To change the names that a brand-new install
starts with, edit `defaultState()` near the top of the `<script>` block in
`index.html`.

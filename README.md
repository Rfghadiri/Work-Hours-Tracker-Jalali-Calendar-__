# Work Hours Tracker (Jalali Calendar)

A small offline-first web app for logging daily work hours against the Persian (Jalali/Shamsi) calendar. Pick a date, press **Start**, **Pause** or **End**, and export the whole month to Excel at the end.

It is a single static page with no backend and no build step. Everything is stored in the browser on your own device.

## Features

- Jalali date picker (year, month, day) that opens on today's date, with the weekday and the Gregorian equivalent shown next to it
- Three buttons: **Start**, **Pause**, **End**
- Any number of work sessions per day (start, pause, start again, and so on)
- Manual time entry for when you forgot to press a button
- Per-day table of sessions with duration and a daily total
- Multiple users, each with a separate log
- One-click monthly Excel export (`.xlsx`) with a daily summary sheet and a detailed sessions sheet
- Right-to-left Persian interface with light and dark themes
- Installable as a PWA and usable offline

## Quick start

1. Download or clone this repository.
2. Open `index.html` in Chrome, Edge or Firefox.

That is all you need to use it. To install it as an app, see [Installing as an app](#installing-as-an-app).

## How to use

### 1. Choose or create a user

The **User** card at the top controls whose log you are editing.

| Action | How |
| --- | --- |
| Create a user | Type a name in the text box and press **Add user** (or Enter) |
| Switch user | Pick a name from the dropdown |
| Delete a user | Press **Delete**, then press it again within 4 seconds to confirm. This removes the user's entire log |

At least one user always remains. Names must be unique. Users are only a way to keep logs apart; there are no passwords.

### 2. Pick the date

Select year, month and day. The app shows the weekday and the matching Gregorian date, for example `Saturday 1 Mehr 1405 (2026-09-23)`.

### 3. Log your time

The time field is filled with the current time and refreshes every 15 seconds. If you type a different time, it stays as you set it until the next button press. This is how you backfill a session.

| Button | What it does |
| --- | --- |
| **Start** | Opens a new session at the time shown. Disabled while a session is already running or after the day has ended |
| **Pause** | Closes the running session at the time shown |
| **End** | Closes the running session (if any) and marks the day as finished |

A typical day looks like this: Start at 08:30, Pause at 12:00, Start at 13:00, Pause at 15:15, Start at 15:30, End at 17:45. The table lists each session with its length and shows the day's total.

Sessions that cross midnight are handled: if the end time is earlier than the start time, 24 hours are added.

> After **End**, the day is closed and cannot be restarted from the interface.

### 4. Export the month

Select any date in the month you want, then press **Download this month's Excel**. The file is named like:

```
ساعت-کار-<user>-<year>-<month number>-<month name>.xlsx
```

It contains two sheets:

| Sheet | Columns |
| --- | --- |
| Daily summary (`خلاصه روزانه`) | Date, weekday, number of sessions, total as `HH:MM`, total as decimal hours, plus a final row with the month total |
| Sessions (`ریز جلسات`) | Date, weekday, start, end, duration |

Only days that have at least one session appear. The workbook opens right-to-left.

## Installing as an app

Browsers only allow installation from a secure origin, so a file opened straight from disk (`file://`) cannot be installed. Pick one of these:

**Option A: free static hosting.** Upload the repository contents to GitHub Pages, Netlify (drag and drop at `app.netlify.com/drop`) or Cloudflare Pages. Open the HTTPS address in Chrome or Edge and click the **Install** icon in the address bar. On a phone, use **Add to Home screen**.

**Option B: local server.** From the project folder:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000` and install from the address bar. `localhost` is treated as secure.

Once installed, the service worker caches the app so it works without a network connection.

### GitHub Pages in short

1. Push the files to a repository.
2. Go to **Settings → Pages**, choose the branch and the root folder, and save.
3. Open the published URL and install.

## Where your data lives

All logs are saved in the browser's `localStorage` under the key `tt2`. Practical consequences:

- Data stays on the device and browser profile where you entered it. Nothing is uploaded anywhere.
- A different address counts as a different storage area. Data entered at `localhost:8000` will not appear on `yourname.github.io`, and an installed copy has its own storage. Choose one address and stay with it.
- Clearing site data deletes your logs. Download the monthly Excel file regularly as a backup.

## Calendar data

The Jalali-to-Gregorian mapping was generated from `jalali_date.xls` (one row per day, with Shamsi year, month, day and weekday). To keep the page small, the app stores only two numbers per year: the day the year starts and the length of the last month. Every other date is computed from those. The reconstruction was checked against all 40,151 rows of the spreadsheet with no mismatches.

The embedded range covers Jalali years **1395 to 1428** (2016 to 2050). To extend it, regenerate the `CAL` object in `index.html` from a longer spreadsheet.

## Project structure

```
index.html            App (HTML, CSS, JS, calendar data and the SheetJS library inlined)
manifest.webmanifest  PWA metadata
sw.js                 Service worker for offline use
icon-192.png          App icon
icon-512.png          App icon
```

## Limitations

- No accounts or sync. Users are local labels, not authenticated identities.
- No in-app editing or deletion of individual sessions.
- A closed day cannot be reopened from the interface.
- Dates outside Jalali years 1395 to 1428 are not available.

## Built with

- Plain HTML, CSS and JavaScript
- [SheetJS](https://sheetjs.com/) (`xlsx.mini`, v0.18.5) for Excel export



# Welcome Board — User Guide

## Overview

This is a self-contained HTML welcome board designed for a 55" display in portrait mode (1080×1920). It cycles through three screens in a continuous loop:

1. **Names Board** — Guest names flip in one at a time using a split-flap animation
2. **Company Carousel** — Each guest company's name and logo are shown one at a time (optional)
3. **Date & Time** — Full-screen clock interstitial

After the date/time screen, the loop restarts from the names board.

A small clock also runs in the bottom-right corner during the names display.


## File Structure

For the simplest setup, keep everything in one folder:

```
welcome-board/
├── welcome-board.html    ← the board (self-contained, logo embedded)
└── logos/                ← optional folder for company logos
    ├── acme.png
    ├── wayne.png
    └── ...
```

The Fedeli Group logo is embedded as base64 inside the HTML, so no external image file is needed for it.


## How to Run

Open `welcome-board.html` in a full-screen browser (Chrome recommended) on the machine connected to your 55" display. Press **F11** to enter full-screen mode. The board runs indefinitely with no interaction needed.


## Configuration

All settings live inside the `<script>` block near **line 405**. Open the HTML in any text editor (VS Code, Notepad++, etc.) to make changes.


### Guest Names (line ~405)

Replace the president names with your actual guest list. Each name is a quoted string separated by commas:

```javascript
const NAMES = [
    "Jane Smith",
    "Robert Chen",
    "Maria Garcia",
    "David Thompson"
];
```

You can add or remove as many names as needed. The board will adjust automatically. Names are centered on each row and displayed in uppercase.

**Note:** `MAX_CHARS` (line ~418) controls how many character slots each row has. The default is `24`, which fits names up to 24 characters. If you have longer names, increase this value.


### Company Logo Carousel (line ~432)

The carousel is controlled by three settings:

```javascript
const ENABLE_CAROUSEL = true;          // set to false to skip entirely
const CAROUSEL_SLIDE_TIME = 5000;      // ms each company stays on screen

const COMPANIES = [
    { name: "Acme Corporation",   logo_src: "logos/acme.png" },
    { name: "Wayne Enterprises",  logo_src: "logos/wayne.png" },
    { name: "Stark Industries",   logo_src: "" },
];
```

**`ENABLE_CAROUSEL`** — Set to `true` to show the carousel after the names, or `false` to skip straight to the date/time screen. Useful for days when you don't need company logos.

**`CAROUSEL_SLIDE_TIME`** — How long each company slide stays on screen in milliseconds. Default is `5000` (5 seconds).

**`COMPANIES` array** — Each entry has two fields:

| Field       | Description |
|-------------|-------------|
| `name`      | Company name displayed as the heading above the logo |
| `logo_src`  | Path to the logo image file, or leave as `""` for a text-only placeholder |

**Logo file paths** — Use a path relative to where the HTML file lives. No leading slash:

```javascript
{ name: "Acme Corp", logo_src: "logos/acme.png" }
```

**Base64 logos** — To embed a logo directly (no external file needed):

```javascript
{ name: "Acme Corp", logo_src: "data:image/png;base64,iVBORw0KGgo..." }
```

**No logo available** — Leave `logo_src` as an empty string `""` and the company name will display as text in the logo frame instead.

If the `COMPANIES` array is empty (`[]`), the carousel is skipped regardless of the `ENABLE_CAROUSEL` setting.


### Timing Settings (lines ~418–423)

All times are in **milliseconds** (1 second = 1000).

| Setting               | Default    | What It Controls |
|-----------------------|------------|------------------|
| `HOLD_TIME`           | `120000`   | How long all names stay on screen after the last name flips in. Default is 2 minutes. |
| `DATETIME_DISPLAY_TIME` | `8000`   | How long the full-screen date/time interstitial is shown. Default is 8 seconds. |
| `NAME_REVEAL_DELAY`   | `120`      | Pause between each name row starting its flip animation. Lower = names appear faster. |
| `CHAR_FLIP_DELAY`     | `25`       | Delay between each character flipping within a single name. Lower = the name resolves faster left to right. |
| `FLIP_CYCLES`         | `4`        | Minimum number of random characters each slot cycles through before landing on the correct letter. Higher = more dramatic shuffle effect. |
| `MAX_CHARS`           | `24`       | Number of character slots per name row. Increase if you have names longer than 24 characters. |

**Quick reference for common HOLD_TIME values:**

| Desired Hold | Value       |
|-------------|-------------|
| 30 seconds  | `30000`     |
| 1 minute    | `60000`     |
| 2 minutes   | `120000`    |
| 5 minutes   | `300000`    |
| 10 minutes  | `600000`    |


### Fedeli Group Logo

The main logo at the top of the page is embedded as base64 inside the HTML at **line ~267** inside the `<img>` tag. To replace it, swap the `src="data:image/png;base64,..."` value with a new base64 string or a file path like `src="logo.png"`.


### Colors

The color palette is defined in CSS variables at the top of the `<style>` block (~line 11):

```css
:root {
    --maroon: #5A0A1A;
    --maroon-deep: #3E0712;      /* main background */
    --maroon-light: #7A1A2E;
    --gold: #AD9551;              /* headings, primary accent */
    --gold-light: #C4AC6A;       /* flap characters */
    --gold-dim: #8A7641;         /* subtle accents */
    --cream: #F5F0E8;            /* date/time text */
    --flap-bg: #2A2A2A;          /* flap tile top half */
    --flap-dark: #222222;        /* flap tile bottom half */
}
```

Change any of these values to adjust the board's color scheme.


### Fonts

The board uses two Google Fonts loaded via CDN:

- **Rokkitt** — Used for the "Welcome Esteemed Guests" heading, company names in the carousel, and the date/time display.
- **Open Sans** — Used for the split-flap characters, corner clock, and all other text.

To change fonts, update the Google Fonts `<link>` tag on **line 7** and the corresponding `font-family` declarations in the CSS.


## Display Cycle Summary

```
┌─────────────────────────────┐
│  Names flip in one by one   │
│  (NAME_REVEAL_DELAY between │
│   each row)                 │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  All names held on screen   │
│  (HOLD_TIME duration)       │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  Company carousel            │
│  (CAROUSEL_SLIDE_TIME each) │
│  Skipped if ENABLE_CAROUSEL │
│  is false or COMPANIES is   │
│  empty                      │
└──────────────┬──────────────┘
               ▼
┌─────────────────────────────┐
│  Date & Time screen         │
│  (DATETIME_DISPLAY_TIME)    │
└──────────────┬──────────────┘
               ▼
         Loop restarts
```


## Tips

- **Test timing quickly** — Temporarily set `HOLD_TIME` to `5000` (5 seconds) so you don't have to wait 2 minutes each cycle while testing.
- **Browser kiosk mode** — Most browsers support a kiosk flag for unattended displays. In Chrome: `chrome.exe --kiosk welcome-board.html`
- **Prevent screen sleep** — Make sure the display machine's power settings are configured to never sleep or turn off the screen.
- **Logo sizing** — Company logos look best at roughly 400×200px or larger. The carousel frame is 500×300px with 40px padding, so logos are scaled to fit within 420×220px.
- **Name count** — The board comfortably fits around 30 names on a 1920px-tall portrait display. If you go significantly beyond that, names may overflow off the bottom of the screen. For very large guest lists, consider reducing the font size in CSS or splitting across multiple cycles.

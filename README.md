# Focus Timer ✦

A productivity focus timer built as a **single HTML file** — no install, no dependencies, no server required. Open it in any browser and start working.

The design is inspired by warm, playful children's book illustration aesthetics: cream backgrounds, thick comic-style card borders, bold rounded typography, and bright accent colors.

---

## Quick Start

```
# Clone or download
git clone https://github.com/your-username/focus-timer.git

# Open directly in your browser
open index.html         # macOS
start index.html        # Windows
xdg-open index.html     # Linux
```

That's it. No `npm install`, no build step, no server.

---

## Features

### ⏱ Timer Tab

Two modes in the same interface:

**Stopwatch mode**
- Counts up from 00:00
- Start / Pause / Resume / Reset
- Click **✦ Log** when you finish to record the session (minimum 1 minute)

**Countdown mode**
- Preset durations: 15 / 25 / 45 / 50 / 60 minutes
- An animated progress bar shrinks as time runs out
- The last 5 seconds play escalating tick sounds
- Session is **auto-logged** when the countdown reaches zero
- A browser notification fires on completion (requires permission)

**Shared behavior**
- Pomodoro-style dots below the timer fill up with each completed session that day
- Today's session count, total minutes, and current streak are shown in a summary card beneath the timer
- `Space` bar toggles start/pause; `R` resets (when not focused on a text input)

---

### ✅ Tasks Tab

Simple, focused task management:

| Action | How |
|--------|-----|
| Add a task | Type in the input field and press `Enter` or click `+` |
| Complete a task | Click the checkbox or the task text |
| Delete a task | Click `✕` on any task row |

- Active and completed tasks are shown in separate sections with live counts
- Completed tasks display with a strikethrough style
- All tasks are persisted in `localStorage` — they survive page refreshes and browser restarts

---

### 📊 Stats Tab

**Summary cards**

| Card | What it shows |
|------|---------------|
| 🔥 Day Streak | Consecutive days with at least one logged session |
| Today | Sessions logged today |
| This Week | Sessions logged since Monday |
| This Month | Sessions logged since the 1st |
| Total Minutes | Cumulative focus time across all sessions |

**Bar chart**
- Toggle between **Week** (last 7 days) and **Month** (last 30 days) views
- Today's bar is highlighted in orange
- Hover any bar to see its session count as a tooltip
- Rendered in pure vanilla JavaScript — no Chart.js or external libraries

**Data management**
- **Export JSON** — downloads the full app state as a timestamped `.json` file
- **Import JSON** — restores state from a previously exported file, with error handling for malformed input

---

## Data Structure

Everything is stored in `localStorage` under the key `focusTimerApp_v2`.

```json
{
  "tasks": [
    {
      "id": 1713200000000,
      "text": "Write project proposal",
      "done": false,
      "createdAt": 1713200000000
    }
  ],
  "sessions": [
    {
      "id": 1713200000001,
      "date": "2026-04-15",
      "duration": 1500,
      "ts": 1713200000001
    }
  ],
  "stats": {
    "streak": 3,
    "lastDate": "2026-04-15"
  }
}
```

`duration` is stored in **seconds**. Divide by 60 to get minutes.

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Space` | Start / Pause the timer |
| `R` | Reset the timer (only when stopped) |
| `Enter` (in task input) | Add task |

---

## Design System

The visual language is deliberately playful — inspired by Russian children's book illustration style:

| Token | Value | Usage |
|-------|-------|-------|
| `--cream` | `#FEFAE0` | Page background |
| `--yellow` | `#F4C542` | Active states, highlights |
| `--green` | `#6BCB77` | Start button, completed tasks |
| `--red` | `#FF6B6B` | Running timer, alerts |
| `--blue` | `#4D96FF` | Countdown mode, Log button |
| `--orange` | `#FF9B54` | Paused timer, today's chart bar |
| `--dark` | `#2C2C2C` | Borders, text, shadows |

Cards use a **4px offset hard shadow** (`box-shadow: 4px 4px 0 #2C2C2C`) that gives the UI its comic/illustration character.

Typography:
- **Fredoka One** — headings, timer display, stat numbers
- **Nunito** — all body text, buttons, labels (loaded from Google Fonts)

---

## Architecture

Everything lives in `index.html`. The file is organized into three clearly commented sections:

```
index.html
├── <style>          CSS design tokens, layout, components (~650 lines)
├── <body>           HTML structure: header, tabs, three panels
└── <script>         Vanilla JS modules (~350 lines)
    ├── Storage      loadS() / saveS() via localStorage
    ├── Timer        doStart / doPause / doReset / doLog / onCountdownEnd
    ├── Sessions     pushSession / bumpStreak / checkStreakDecay
    ├── Tasks        addTask / toggleTask / deleteTask / renderTasks
    ├── Stats        renderStats / renderChart / switchChart
    └── I/O          exportData / importData
```

No frameworks. No build tools. No bundler. The only external resource is the Google Fonts stylesheet.

---

## Browser Support

Works in all modern browsers:

| Browser | Support |
|---------|---------|
| Chrome / Edge | ✅ Full |
| Firefox | ✅ Full |
| Safari | ✅ Full |
| Mobile Chrome/Safari | ✅ Responsive |

Browser notifications require a one-time permission grant (prompted automatically on first load).

---

## Offline Use

After the first load (which fetches Google Fonts), the app is fully functional offline. If you need fonts to work offline too, replace the Google Fonts `<link>` with locally hosted font files.

---

## Customization

**Change the default Pomodoro duration**

In the `<script>` section, find:
```js
let cdDuration = 25 * 60;
```
Change `25` to any number of minutes.

**Add a new preset button**

In the HTML, inside `.presets`:
```html
<button class="preset-btn" onclick="setPreset(90)">90 min</button>
```

**Enable auto-pause on tab switch**

In the `visibilitychange` listener, uncomment:
```js
if (document.hidden && tRunning) doPause();
```

**Change the color scheme**

Edit the CSS variables in `:root` at the top of the `<style>` block.

---

## License

MIT — use it, fork it, adapt it however you like.

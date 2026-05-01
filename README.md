# ANCHOR

> Hard to ignore. Easy to use.

A minimal habit-stacking and priority tracker built around the principles from *Atomic Habits* (James Clear) and *Tiny Habits* (BJ Fogg).

## Philosophy

- **Done = green. Floor = also green.** The 2-minute version is not lesser — it IS the habit on hard days.
- **Snooze is neutral.** Deferring is a valid choice. No guilt, no flags.
- **Only 2+ consecutive misses surface a warning.** One miss is noise. Two is a signal worth acting on.
- **Streaks count both ✓ and /**, so the chain stays alive on floor days.

## Views

| View | Purpose |
|---|---|
| **Today** | Priority task + active habits with one-tap logging |
| **Week** | Visual consistency grid — see your patterns |
| **Habits** | Add / edit / phase habits (Active → Queue → Paused) |
| **Tasks** | Priority task management with Upcoming / Today / Done filter |

## Log States

| Symbol | Meaning | Color |
|---|---|---|
| ✓ | Full habit done | 🟢 Green |
| / | Floor version done | 🟢 Green (same) |
| z | Snoozed | 🟡 Gold (neutral) |
| ✕ | Missed | 🔴 Red (only shown in week grid) |

## Habit Phases

- **Active** — logging daily, shows in Today and Week views
- **Queue** — configured but not yet started (build one at a time)
- **Paused** — temporarily off, hidden from daily view

## Local Development

Open `index.html` directly in a browser. No build tools, no dependencies, no server needed. All data is stored in `localStorage`.

```bash
open index.html
```

## Stack

- Vanilla HTML / CSS / JS
- [Satoshi](https://www.fontshare.com/fonts/satoshi) via Fontshare
- Inline SVG icons (no dependency)
- `localStorage` for persistence

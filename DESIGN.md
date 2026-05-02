# ANCHOR — Design Document
> Version 0.1 · May 2026 · Living document — update as decisions change

---

## 1. Vision Statement

ANCHOR is a **daily action engine** for people who already know what habits they want to build but keep drifting from them. It is not a productivity system. It is not a mood tracker. It is a notification layer that makes the right action the path of least resistance at the right moment.

> **Design mantra:** Hard to ignore. Easy to use. Never guilt-trip.

The core insight: most habit apps fail not because users forget their habits, but because the moment of friction — the few seconds between *knowing you should* and *actually starting* — has no support. ANCHOR fills that gap with a single-focus action window that fires at the anchored moment and offers exactly two choices: start or snooze.

---

## 2. Target User

A full-time knowledge worker who:
- Already has one or more built habits (e.g. resistance training 3×/week)
- Wants to layer 2–3 new habits on top of existing routines
- Has a compressed, non-flexible morning (e.g. work starts at 6am)
- Has family commitments that make rigid evening schedules unrealistic
- Has tried habit apps before and dropped them

**Key constraint:** The user cannot afford guilt. One bad day snowballing into a broken week is the #1 failure mode. The app must make recovery feel identical to consistency.

---

## 3. Core Design Principles

| # | Principle | What it means in practice |
|---|---|---|
| 1 | **Floor = Done** | Floor completions are green, not yellow. No visual distinction from full completions in streaks or weekly view. Identity is preserved. |
| 2 | **Snooze is neutral** | Snoozed items are gold. Not failure. The app records them for pattern recognition in Trends, never as negative events. |
| 3 | **2+ misses only** | Red escalation only fires after 2 consecutive active-day misses. One miss is noise. Two is a signal. |
| 4 | **One action window** | The notification opens a full-screen focused window. Not a banner. Not a badge. The window requires deliberate action — start, snooze, or dismiss. |
| 5 | **Quick add under 3 seconds** | Fast path: type name → tap +. Done. Full config is one tap away but never in the way. |
| 6 | **Agency over automation** | The app supports decisions. It never reschedules without user input. It never auto-marks anything. |

---

## 4. The Core Loop

```
NOTIFICATION fires at anchor time
        │
        ▼
┌─────────────────────────────┐
│     ACTION WINDOW           │
│  Habit / task name (large)  │
│  Anchor cue (small)         │
│  Floor version (chip)       │
│                             │
│  [ START — 45 min ]  [Snooze]│
└─────────────────────────────┘
        │                   │
      START               SNOOZE
        │                   │
        ▼                   ▼
┌──────────────┐    ┌──────────────────┐
│ TIMER SCREEN │    │  30 min          │
│  45:00 ring  │    │  2 hours         │
│  Pause       │    │  Tomorrow        │
│  Done early  │    │  Custom time     │
└──────────────┘    └──────────────────┘
        │
        ▼
┌────────────────────────────┐
│     OUTCOME SCREEN         │
│  [ Done ✓ ]                │
│  [ Floor / ]               │
│  [ Not done → reschedule ] │
└────────────────────────────┘
```

**Done ✓** — logs as done, green pip in weekly view, chain alive, closes window.

**Floor /** — logs as floor, same green pip, same chain alive. No asterisk. No distinction.

**Not done → reschedule** — opens snooze sheet with time options. Logs nothing until user picks a new time.

---

## 5. Screens & Navigation

Navigation: **3 bottom tabs** on mobile. Fixed. Always visible.

```
[ Today ]  [ Week + Trends ]  [ Stack ]
```

Plus one **full-screen overlay** (Action Window) triggered by notifications — not a tab.

---

### 5.1 Tab 1 — Today

**Purpose:** See what is due now. Act on it immediately.

**Layout (top to bottom):**

```
┌────────────────────────────────┐
│ Saturday, 2 May  ·  Week 18    │
│ 2 chains alive  · 1 snoozed    │ ← summary pills
├────────────────────────────────┤
│ [+ Quick add a task or habit ] │ ← always-visible fast input
├────────────────────────────────┤
│ ⚠ Project Touch missed 2 days  │ ← only shown if 2+ misses
├────────────────────────────────┤
│  HABITS                        │
│  ● Project Touch  🔥 11        │
│  After commute home            │
│  Floor: open file, one line    │
│  [ Start ] [ Floor ] [ Snooze ]│
│                                │
│  ● Evening Read  🔥 6          │
│  After shower                  │
│  Floor: read one page          │
│  [ Start ] [ Floor ] [ Snooze ]│
├────────────────────────────────┤
│  TASKS                         │
│  ○ Kitchen project budget      │
│  [ Done ] [ Floor ] [ Snooze ] │
└────────────────────────────────┘
```

**Fast-add bar:**
- Type name → tap `+` → item added as one-off today task
- Tap the bar label or expand icon → reveals full form inline:
  - Recurring? toggle
  - Days (Mon–Sun chips)
  - Notification time
  - Category
  - Anchor text
  - Floor version

**Habit card states:**
- `Not logged` — shows Start / Floor / Snooze buttons
- `In progress` — shows live timer countdown on card
- `Done` — green card header, streak updated, tap to undo
- `Floor` — same green card header, no asterisk
- `Snoozed` — gold chip showing snooze time, tap to cancel snooze

**Miss alert:** One amber banner per habit with 2+ misses. No stacking banners. Dismissed once user logs any status.

---

### 5.2 Tab 2 — Week + Trends

**Purpose:** See patterns. Understand what is actually working.

**Layout (top to bottom):**

```
┌────────────────────────────────┐
│ ← Week 18 (28 Apr – 4 May) →  │
│                                │
│         M  T  W  T  F  S  S   │
│ Project  ✓  ✓  /  ✓  ⏸  ✓  –  │
│ Reading  ✓  /  ✓  ✓  ✓  ⏸  ✓  │
│ Family   ✓  ✓  ✓  ✓  ✓  ×  ✓  │
│                                │
│ Legend: ✓=Done /=Floor ⏸=Snooze ×=Miss │
├────────────────────────────────┤
│  CORRELATION                   │
│                                │
│  Project Touch                 │
│  ████████░░ 78%  🔥 11         │
│  On days done: tasks +34% more │
│                                │
│  Evening Reading               │
│  ██████░░░░ 62%  🔥 6          │
│  On days done: no clear signal │
│                                │
│  Family Presence               │
│  █████████░ 88%  🔥 5          │
│  On days done: tasks +18% more │
└────────────────────────────────┘
```

**Weekly grid:**
- Rows = active habits
- Columns = Mon–Sun of selected week
- Pips: ✓ green / · floor green dim / ⏸ gold / × red / – not scheduled / · future
- Tap any cell → quick-log modal for that habit + date

**Correlation rows (below grid):**
- One row per active habit
- Momentum bar: filled = % done or floor over last 14 days
- Streak count
- Correlation line: plain English, calculated from log data

---

### 5.3 Tab 3 — Stack

**Purpose:** Configuration. Rarely visited once set up.

**Layout:**
- Segmented control at top: `Habits` | `Tasks`

**Habits panel:**
- List of all habits with phase pill (Active / Queued / Paused)
- Tap → opens full edit form (bottom sheet)
- Phase toggle visible inline

**Tasks panel:**
- Filter tabs: Upcoming / Today / Done
- Tap → opens task edit form

**Habit form fields:**
| Field | Purpose |
|---|---|
| Name | Display name on cards and notifications |
| Category | Project · Reading · Family · Exercise · Custom |
| Phase | Active · Queued · Paused |
| Anchor | Plain text cue shown in notification window |
| Floor version | The 2-minute win. Shown on every card and notification. |
| Full version | The complete session description |
| Active days | Mon–Sun chips, multi-select |
| Notification time | Time of day the action window fires |

**Task form fields:**
| Field | Purpose |
|---|---|
| Name | Display name |
| Type | One-off · Recurring |
| Target date | Due date for one-off tasks |
| Recurrence | Days + time for recurring |
| Note | Optional context |

---

### 5.4 Overlay — Action Window

**Triggered by:**
- Tapping a notification (real device)
- Tapping `Start` on a habit card (in-app)
- Test notification button (dev/demo mode)

**This is a full-screen takeover**, not a sheet or modal. It covers everything. The only way out is a deliberate action.

**States:**

**State A — Prompt**
```
┌────────────────────────────────┐
│                                │
│  ● PROJECT TOUCH               │  ← category dot + name
│  After your commute home       │  ← anchor cue
│                                │
│  ┌──────────────────────────┐  │
│  │ Floor: open file,        │  │  ← floor chip, always visible
│  │ write one line           │  │
│  └──────────────────────────┘  │
│                                │
│  🔥 11-day chain alive         │  ← context
│                                │
│  ┌──────────────────────────┐  │
│  │    START — 45 min        │  │  ← primary CTA, full width, teal
│  └──────────────────────────┘  │
│  ┌──────────────────────────┐  │
│  │        Snooze            │  │  ← secondary, gold
│  └──────────────────────────┘  │
│                                │
│  [ dismiss ]                   │  ← small text link, bottom
└────────────────────────────────┘
```

**State B — Timer running**
```
┌────────────────────────────────┐
│  PROJECT TOUCH                 │
│                                │
│       ┌─────────────┐          │
│       │   38:24     │          │  ← large countdown ring
│       └─────────────┘          │
│                                │
│  [ Pause ]   [ Done early ]    │
└────────────────────────────────┘
```

**State C — Outcome**
```
┌────────────────────────────────┐
│  How did it go?                │
│                                │
│  ┌──────────────────────────┐  │
│  │  Done ✓                  │  │  ← green
│  └──────────────────────────┘  │
│  ┌──────────────────────────┐  │
│  │  Floor /                 │  │  ← same green
│  └──────────────────────────┘  │
│  ┌──────────────────────────┐  │
│  │  Not done — reschedule   │  │  ← gold, opens snooze sheet
│  └──────────────────────────┘  │
└────────────────────────────────┘
```

---

## 6. Notification System Design

Since ANCHOR runs in a browser (prototype phase), real OS push notifications require a service worker. For the prototype, notification behavior is simulated.

**Simulation approach:**
- Each active habit with a notification time set has a visible `Test Notification` button in the Stack screen
- Tapping it fires the Action Window immediately as if the notification was tapped
- A small floating badge on the Today tab can also simulate a pending notification

**Notification content spec:**
```
Title: [Habit name]
Body:  [Anchor cue] · Floor: [floor version]
Action buttons (iOS/Android native): Start | Snooze 30m
```

**Escalation notification (2+ miss):**
```
Title: [Habit name] — 2 days missed
Body:  Floor counts. One line is enough.
```

**Design decision:** Escalation notifications use encouraging language, never shaming language. The body copy is fixed and non-negotiable.

---

## 7. Data Model

```
Habit {
  id: uuid
  name: string
  category: 'project'|'reading'|'family'|'exercise'|'custom'
  phase: 'active'|'queued'|'paused'
  anchor: string          // plain text cue
  floorVersion: string    // the 2-minute win
  fullVersion: string     // optional full description
  days: DayCode[]         // ['mon','tue',...] or ['every']
  notifyTime: string      // '18:30' or null
  createdAt: ISO date
}

Task {
  id: uuid
  name: string
  type: 'oneoff'|'recurring'
  targetDate: ISO date    // for one-off
  recurDays: DayCode[]    // for recurring
  recurTime: string       // '07:00' or null
  note: string
  createdAt: ISO date
}

HabitLog {
  key: `${habitId}|${YYYY-MM-DD}`
  value: 'done'|'floor'|'snoozed'|'missed'
}

TaskLog {
  key: `${taskId}|${YYYY-MM-DD}`
  value: 'done'|'floor'|'snoozed'
}

SnoozeEntry {
  id: uuid
  targetType: 'habit'|'task'
  targetId: uuid
  snoozedAt: ISO datetime
  resumeAt: ISO datetime
  originalDate: ISO date
}
```

---

## 8. Visual Design Decisions

### Color System

| Token | Light | Dark | Used for |
|---|---|---|---|
| `--ok` | `#437a22` | `#6daa45` | Done, Floor, streaks, chains alive |
| `--sn` | `#b07a00` | `#e8af34` | Snoozed, neutral deferrals |
| `--bad` | `#a13544` | `#dd6974` | 2+ miss alerts only |
| `--pri` | `#01696f` | `#4f98a3` | Primary CTA, nav active, category: project |
| `--pur` | `#7a39bb` | `#a86fdf` | Category: reading |

**Key decision:** Floor and Done share the same `--ok` color everywhere — pips, pills, card headers, outcome screens. There is no visual differentiation between them. This is intentional and non-negotiable.

### Typography
- Font: Satoshi (Fontshare CDN)
- Weights used: 400 (body), 500 (labels), 700 (headings, names)
- Heading scale: clamp(1.5rem, 2vw, 2.2rem) for screen titles
- Body: 15px / 1.55 line-height
- Minimum text size: 12px (labels, metadata only)

### Layout
- Mobile-first: max-width 480px centered
- Bottom nav: 70px height, always visible
- Cards: 14px padding, 16px border-radius
- Spacing unit: 4px base (8, 12, 16, 24, 32, 48)

### Motion
- Modals / sheets: slide up 0.28s cubic-bezier(0.16, 1, 0.3, 1)
- View transitions: fade + 8px translateY, 0.2s
- Status chips: scale(0.95) on active, 0.15s
- Timer ring: CSS conic-gradient, updates every second

---

## 9. Build Phases

### Phase 0 — Design document (this file) ✅
### Phase 1 — Static prototype
- All 3 tabs functional
- Action window with timer
- Simulated notifications
- localStorage persistence
- Seed data pre-loaded

### Phase 2 — Notification simulation
- Service worker registration
- Scheduled notification simulation via setTimeout
- Test notification button per habit

### Phase 3 — Polish
- Light/dark mode
- Smooth transitions
- Weekly cell tap-to-log
- Correlation calculation from real log data

### Phase 4 — Native wrapper (future)
- PWA manifest
- iOS/Android home screen install
- Real push notification permission flow

---

## 10. What ANCHOR Is Not

- Not a to-do list. Tasks exist only to measure how habits support output.
- Not a mood tracker. No check-ins, no journaling prompts.
- Not a streak-punishment system. Missing a day is not a loss — the chain simply pauses.
- Not a social app. No sharing, no leaderboards.
- Not a coach. It does not tell you what to do. It only makes what you decided easier to do.

---

## 11. Open Decisions

| Decision | Status | Options |
|---|---|---|
| Timer duration | Open | Fixed 45min vs user-configurable |
| Floor logging | Open | Separate button vs long-press on Done |
| Notification time per habit | Open | Single time vs multiple (e.g. reminder + escalation) |
| Web vs PWA vs native | Open | Prototype in browser, PWA for phase 2 |
| Recurring tasks vs habits | Open | Merge into one type vs keep separate |
| Correlation calculation | Open | 14-day window vs rolling 30 days |

---

*ANCHOR Design Document — maintained in `moxiestudios/ANCHOR` · Update this file when any decision changes.*

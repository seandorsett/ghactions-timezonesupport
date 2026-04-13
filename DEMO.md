# Demo Guide: GitHub Actions Timezone Support

This guide walks through the live demo portion of the DXC open office hour session.
Demo time: **10 minutes**

---

## Pre-Demo Setup (Before the Session)

1. Ensure you are logged into GitHub with access to this repository
2. Navigate to the **Actions** tab and confirm previous workflow runs are visible
3. Have the `.github/workflows/` directory open in a code editor or GitHub web UI

---

## Demo Step 1 — The Old Approach (UTC Only) — 2 min

**File:** `.github/workflows/demo-before-timezone.yml`

Open this file and walk the audience through it:

```yaml
on:
  schedule:
    - cron: '0 9 * * 1-5'   # ← "9 AM" — but 9 AM WHERE?
```

**Talking points:**
- Before March 2026, this cron had no timezone — it always fired at 09:00 UTC
- If your team is in US Eastern Time (UTC-5 / UTC-4 DST), this fires at 4 AM or 5 AM local time
- You'd have to manually do the math and write `'0 14 * * 1-5'` to mean "9 AM Eastern"
- And then remember to update it twice a year for Daylight Saving Time

**Ask the audience:** *"Has anyone been burned by a midnight pipeline alert that was supposed to be a morning job?"*

---

## Demo Step 2 — The New Approach (Timezone Aware) — 2 min

**File:** `.github/workflows/demo-with-timezone.yml`

Open this file and compare to the previous one:

```yaml
on:
  schedule:
    - cron: '0 9 * * 1-5'
      timezone: 'America/New_York'   # ← NEW: explicit, readable, DST-aware
```

**Talking points:**
- One field: `timezone`
- Uses the IANA timezone database (same standard as Linux, databases, programming languages)
- GitHub handles DST automatically — no more manual adjustments in March and November
- The cron expression now reads exactly as intended: "9 AM New York time, weekdays"

**Show in the browser:** Open the workflow file on GitHub, point out the `timezone` field.

---

## Demo Step 3 — Multi-Region Schedules — 2 min

**File:** `.github/workflows/demo-multi-region.yml`

Open this file and show the multi-timezone setup:

```yaml
on:
  schedule:
    - cron: '0 9 * * 1-5'
      timezone: 'America/New_York'     # US team — fires at 9 AM ET
    - cron: '0 9 * * 1-5'
      timezone: 'Asia/Tokyo'           # APAC team — fires at 9 AM JST
```

**Talking points:**
- Multiple `cron` entries under `schedule` were already supported
- Now each can have its own timezone
- A global team can define "start of business day" for each region independently
- The same logical time ("9 AM") just works across regions without any UTC gymnastics

---

## Demo Step 4 — All Workflow Files Side-by-Side — 2 min

Navigate to `.github/workflows/` in the GitHub UI and show all demo files:

| File | Purpose |
|---|---|
| `demo-before-timezone.yml` | ❌ Old UTC-only approach |
| `demo-with-timezone.yml` | ✅ Single timezone, DST-aware |
| `demo-multi-region.yml` | ✅ Multiple regions, each with their own local time |
| `demo-health-check.yml` | ✅ Practical example: morning health check |
| `demo-nightly-build.yml` | ✅ Practical example: nightly build with notifications |

Emphasize that **existing workflows are unaffected** — adding `timezone` is purely opt-in.

---

## Demo Step 5 — Trigger a Manual Run — 2 min

Navigate to **Actions tab** → select `demo-with-timezone.yml` → click **"Run workflow"**

**Talking points:**
- The `workflow_dispatch` trigger lets us fire the workflow on demand for demo purposes
- In production, this same workflow would fire on its schedule
- Show the job summary output — the echo statements confirm the timezone context

**Expected output:**
```
==============================================
  GitHub Actions Timezone Support Demo
  Workflow: Timezone-Aware Schedule Demo
==============================================
Run timestamp (UTC):    2026-04-13T17:45:00Z
Scheduled timezone:     America/New_York
Local time (approx):    The workflow was scheduled to fire at 9 AM New York time
GitHub Runner timezone: UTC (runners always run in UTC)
==============================================
```

**Key clarification to make:** The **runner itself** always operates in UTC. The `timezone` field only affects **when the schedule triggers** — not the execution environment. If your code needs to work in a specific timezone, you still need to set `TZ` in your job.

---

## Bonus: Setting Runner Timezone (If Asked)

If the audience asks about running jobs in a specific timezone (not just scheduling):

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      TZ: 'America/New_York'    # Sets timezone for all steps in this job
    steps:
      - run: date               # Will now show Eastern time
```

This is separate from the `schedule.timezone` feature — that controls *when* the job fires, while `TZ` controls the *runtime environment* timezone for commands like `date`, log timestamps, etc.

---

## Demo Wrap-Up

Summarize for the audience:

> "In summary: before March 2026, you had to be a human UTC converter. Now you just write `timezone: 'America/New_York'` and GitHub does the math — including DST. It's one of those features that seems small but will save your team from a lot of late-night confusion."

---

*Proceed to: Open Q&A (30 minutes) → then Crafted Q&A (10 minutes)*
*See `PRESENTATION.md` for the full session guide.*

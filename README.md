# GitHub Actions: Timezone Support for Scheduled Workflows

Demo repository for the DXC GitHub Actions Open Office Hour session.
This repository showcases the **timezone support** feature for `schedule` triggers,
released in [GitHub Actions Late March 2026 Updates](https://github.blog/changelog/2026-03-19-github-actions-late-march-2026-updates/).

---

## Session Materials

| File | Description |
|---|---|
| [`PRESENTATION.md`](./PRESENTATION.md) | Full slide deck with speaker notes (10 min presentation + 10 min demo guide + crafted Q&A) |
| [`DEMO.md`](./DEMO.md) | Step-by-step live demo walkthrough |
| 📄 `PRESENTATION.pdf` | PDF export of the slide deck — generated automatically by the [Generate Presentation PDF](./.github/workflows/generate-pdf.yml) workflow and available as a downloadable artifact from the [Actions tab](../../actions/workflows/generate-pdf.yml) |

## Demo Workflows

| Workflow | Description |
|---|---|
| [`demo-before-timezone.yml`](./.github/workflows/demo-before-timezone.yml) | ❌ Old UTC-only approach — illustrates the problem |
| [`demo-with-timezone.yml`](./.github/workflows/demo-with-timezone.yml) | ✅ Single timezone using the new `timezone:` field |
| [`demo-multi-region.yml`](./.github/workflows/demo-multi-region.yml) | ✅ Multiple regions, each with their own local business time |
| [`demo-health-check.yml`](./.github/workflows/demo-health-check.yml) | ✅ Practical: morning health check at 8 AM London time |
| [`demo-nightly-build.yml`](./.github/workflows/demo-nightly-build.yml) | ✅ Practical: nightly build at 11:30 PM US Central time |

---

## The Feature in 30 Seconds

**Before (UTC only):**
```yaml
on:
  schedule:
    - cron: '0 9 * * 1-5'   # "9 AM" — but 9 AM UTC, not your team's 9 AM
```

**After (March 2026):**
```yaml
on:
  schedule:
    - cron: '0 9 * * 1-5'
      timezone: 'America/New_York'   # Exactly 9 AM New York time, DST-aware ✅
```

---

## Session Agenda

| Segment | Time |
|---|---|
| 🎯 Presentation | 10 min |
| 🛠️ Live Demo | 10 min |
| ❓ Open Q&A | 30 min |
| 📋 Crafted Q&A | 10 min |
| **Total** | **60 min** |

# Thaw Problem Notes

Created: 2026-06-07 09:35:24 +05
Updated: 2026-06-21 12:12:24 +05
Version: 4

## Purpose

This repository is a public example of a HARE Trail-style research task: starting from a real user problem, preserving evidence, separating drafts from posted comments, reviewing upstream code changes, and recording lessons learned.

The work originally happened in the user's native language. The main entry-point artifacts were later translated to English so the workflow can be inspected by a wider audience:

- `README.md`, `tracker.md`, and `journal.md` are the English public entry points.
- `README-ru.md`, `tracker-ru.md`, and `journal-ru.md` preserve the original Russian working versions.

The folder began with `stonerl/Thaw#591`, but later grew beyond a single issue. It now tracks two related Thaw problem areas:

- `spacing-relaunch-wave.md` - display spacing mismatch can trigger a relaunch wave for menu bar apps.
- `visible-hidden-drift.md` - a menu bar item can disappear from visible items and later be found in hidden settings, or the layout can drift.

## Reading Order

1. `spacing-relaunch-wave.md` - the main #591/#685/#686/#691 spacing/relaunch wave problem.
2. `visible-hidden-drift.md` - the related random visible/hidden drift problem, mainly connected to #702.
3. `tracker.md` - current status, open questions, and next step.
4. `posted/` - what was actually published on GitHub.
5. `drafts/` - issue/comment drafts, including Russian review versions.
6. `evidence/` - GitHub snapshots, code inspection notes, and related-issue search.
7. `sources/` - public note about omitted private source packets and source-boundary policy.
8. `analysis/` - argument maps, maintainer-response analysis, and UX framing.
9. `debrief/` - demo copy of the debrief and extracted lessons; in real HARE Trail usage these layers live separately.
10. `journal.md` - English public-readable journal/digest of the work. The full original Russian journal is `journal-ru.md`.

## Structure

```text
.
├── README.md
├── README-ru.md
├── spacing-relaunch-wave.md
├── visible-hidden-drift.md
├── tracker.md
├── tracker-ru.md
├── journal.md
├── journal-ru.md
├── drafts/
│   ├── comments/
│   └── issues/
├── posted/
├── evidence/
│   ├── code/
│   ├── github/
│   └── related-issues/
├── sources/
├── analysis/
└── debrief/
```

## Public Links

- Original issue #591: https://github.com/stonerl/Thaw/issues/591
- User comment in #591: https://github.com/stonerl/Thaw/issues/591#issuecomment-4641519272
- Follow-up issue #685: https://github.com/stonerl/Thaw/issues/685
- Follow-up issue #686: https://github.com/stonerl/Thaw/issues/686
- PR #691: https://github.com/stonerl/Thaw/pull/691
- Related issue #702: https://github.com/stonerl/Thaw/issues/702

## Privacy Note

The public git tree is sanitized. Local credential references, private absolute paths, and private source-packet paths are kept only in ignored `.private/` notes.

The `debrief/` directory is intentionally colocated for this public demo. In the actual HARE Trail setup, canonical debriefs live in `session-debriefs/` and reusable lessons live in `LESSONS.md`.

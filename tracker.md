# Tracker

Updated: 2026-06-21 12:07:03 +05

## Current Question

This repo is now a public demo of Thaw problem research rather than an issue-specific folder for `stonerl/Thaw#591`. The public entry points should be readable in English, while the original Russian working artifacts remain preserved with `-ru` suffixes.

## Current Hypothesis

PR #691 likely closes the main UX/safety gap described by the user:

- the display-transition path now shows just-in-time confirmation when applying spacing would actually relaunch menu bar apps;
- warning copy explicitly mentions apps with menu bar items and possible loss of unsaved input, progress, or transient state;
- the `Don't ask again + Cancel` blocker was fixed in commit `e29074de`: the suppression checkbox disables future confirmations only when the user chooses `Apply`;
- the user approved the PR after that fix, then the maintainer approved and merged it.

## Current State

- Public demo repo: https://github.com/fleytman/thaw-problems
- Original issue #591: https://github.com/stonerl/Thaw/issues/591
- User comment in #591: https://github.com/stonerl/Thaw/issues/591#issuecomment-4641519272
- Follow-up issue #685: https://github.com/stonerl/Thaw/issues/685
- Follow-up issue #686: https://github.com/stonerl/Thaw/issues/686
- Follow-up comment in #591 linking #685/#686: https://github.com/stonerl/Thaw/issues/591#issuecomment-4650305171
- #685 version info comment: https://github.com/stonerl/Thaw/issues/685#issuecomment-4651842003
- #686 label concern comment: https://github.com/stonerl/Thaw/issues/686#issuecomment-4651842442
- #685 clarification: https://github.com/stonerl/Thaw/issues/685#issuecomment-4657170271
- #686 clarification: https://github.com/stonerl/Thaw/issues/686#issuecomment-4657170568
- PR #691: https://github.com/stonerl/Thaw/pull/691
- Review line comment on `Don't ask again`: https://github.com/stonerl/Thaw/pull/691#discussion_r3387835636
- Fix commit: `e29074deadaf024082ff0a61ed281e19bb324f0a`
- `fleytman` review state: `APPROVED`, submitted at `2026-06-10T13:05:36Z`.
- `stonerl` review state: `APPROVED`, submitted at `2026-06-10T17:54:05Z`.
- PR #691 state at last check: `MERGED`, updated at `2026-06-10T17:55:26Z`.
- Later #591 comment from `j-peeters`: https://github.com/stonerl/Thaw/issues/591#issuecomment-4679212411
- `j-peeters` reported relaunches after wake-from-sleep on `2.0b15` even with the same spacing on all displays.
- Visible/hidden category drift search: `evidence/related-issues/visible-hidden-search-2026-06-19.md`
- User clarification: the interesting case is not a single specific app repeatedly moving to hidden, but a random visible item disappearing and later being found in hidden settings.
- Best related issue for that case: #702 https://github.com/stonerl/Thaw/issues/702
- Dedicated #702 note: `evidence/related-issues/issue-702-random-visible-hidden-drift.md`
- User stance on #702: treat it as similar, but do not test maintainer DMG builds; wait for maintainers to validate, close, or merge a fix.
- App-specific visible -> hidden examples: #651 Little Snitch, #605 CodexBar, #480 Toggl, #607/#707 OneDrive. Useful as background, but weaker as primary references for random drift.

## Root Entry Points

- English README: `README.md`
- Original Russian README: `README-ru.md`
- English tracker: `tracker.md`
- Original Russian tracker: `tracker-ru.md`
- English journal/digest: `journal.md`
- Original Russian journal: `journal-ru.md`
- Spacing/relaunch wave overview: `spacing-relaunch-wave.md`
- Visible/hidden drift overview: `visible-hidden-drift.md`
- Public demo debrief: `debrief/session-debrief.md`
- Extracted Thaw-specific lessons: `debrief/lessons.md`

## Key Evidence

- Repo cloned locally for code inspection.
- Initial inspected branch/head before PR work: `development`, HEAD `6a1e3dc4`.
- PR head before final fix: `08d8027fc5d03aac18aedd6d957c52fea5e45da7`.
- Final PR head/fix commit: `e29074deadaf024082ff0a61ed281e19bb324f0a`.
- Code inspection note: `evidence/code/display-spacing-inspection.md`.
- Commit #670 analysis: `evidence/code/commit-e6227bbe-warning-analysis.md`.
- Maintainer response snapshots:
  - `evidence/github/issue-685-maintainer-response-2026-06-09.md`
  - `evidence/github/issue-686-maintainer-response-2026-06-09.md`
- Maintainer response analysis: `analysis/nightah-response-analysis-2026-06-09.md`
- Miscommunication analysis: `analysis/miscommunication-analysis-2026-06-09.md`
- PR #691 summary: `evidence/github/pr-691-summary-2026-06-10.md`
- PR #691 analysis: `evidence/code/pr-691-analysis-2026-06-10.md`
- Review comment draft/published text: `posted/pr-691-review-comment-dont-ask-again.md`
- Wake-from-sleep equal-spacing comment: `evidence/github/issue-591-comment-4679212411.md`
- Visible/hidden issue search: `evidence/related-issues/visible-hidden-search-2026-06-19.md`
- Dedicated #702 document: `evidence/related-issues/issue-702-random-visible-hidden-drift.md`
- Full Thaw public-demo debrief: `debrief/session-debrief.md`
- Thaw-specific extracted lessons/statistics: `debrief/lessons.md`

## What Changed In Understanding

- The issue should not be framed as simply “a new monitor without settings”. A more precise condition is a mismatch between current active/on-disk spacing and global/per-display spacing that Thaw applies during display connection or switching.
- In the inspected code, per-display slider movement was draft-only until `Apply`/`Reset`; it should not be described as immediately persisting every per-display change.
- The global spacing slider persisted `globalConfiguration` immediately, and newly detected displays were seeded from that global template.
- The main safety gap was not only warning text. It was delayed/automatic apply: a user could change a setting earlier, then see the destructive effect later during a display transition.
- PR #691 added the needed JIT confirmation path for automatic display-transition relaunch.
- The dangerous `Don't ask again + Cancel` edge was found in review and fixed in commit `e29074de`.
- Equal spacing should no longer be described as a universal guarantee: a later #591 comment reported wake-from-sleep relaunch on `2.0b15` even with equal spacing across displays.
- Visible/hidden drift looks adjacent but distinct. There are probably at least two branches: saved-state corruption during multi-display/menu-bar relocation (#702/#675) and unstable app/status-item identification (#707/#651/#605/#480).
- For the user case “a random visible item disappeared and later appeared in hidden”, #702 is more relevant than app-specific issues.

## What Contradicts The Current Model

- PR merge is not the same as testing an installed build; behavior still needs verification in a real beta/release build.
- #591 comment `4679212411` suggests `2.0b15` may still relaunch after wake from sleep even with equal spacing; this may be pre-PR #691 behavior or a distinct wake/sleep path.
- The minor UX concern about global overwrite confirmation bypass was not treated as a blocker; the user considered it non-critical and something to test hands-on.
- The `Don't ask again` label is still not maximally explicit, but after the fix commit this is a minor wording concern rather than a safety blocker.

## Open Questions

- Which Thaw version will ship PR #691, and whether the user will test that build on their setup.
- Whether to answer the later #591 comment or wait for a build with PR #691 and ask for verification there.
- How global `Apply to All Displays` behaves when confirmations are disabled; worth hands-on testing, but not a merge blocker.
- If the user sees random visible -> hidden drift again, compare with #702 first. Use #707/#651/#605/#480 only if the symptom is tied to a specific app.
- Do not actively participate in #702 right now: the user is not ready to test custom DMGs, so wait for maintainer validation / merge.
- After release, consider adding a short follow-up comment in #685/#686 with real installed-build verification.

## Next Step

Wait for a release/build containing PR #691 and, separately, a possible #702 fix. For #691, test display connect/disconnect, wake-from-sleep, and manual Apply scenarios on the real setup. For visible/hidden category drift, wait until maintainers validate or merge the #702 fix; do not test custom DMGs right now.

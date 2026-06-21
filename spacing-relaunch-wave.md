# Spacing / Relaunch Wave

Updated: 19.06.2026 21:44:35 +05

## Problem

Thaw lets users configure menu bar item spacing per display, but the underlying `NSStatusItemSpacing` value is effectively system-wide. When Thaw needs to reconcile different desired spacing values across display changes, affected menu bar apps can be terminated and relaunched.

From the user's perspective this was not just visual churn. A routine display connect/disconnect/switch event unexpectedly relaunched unrelated apps and caused loss of typed input / transient state in some apps.

## Main Claim

This is a UX/safety problem, not just a documentation problem:

- the destructive side effect can happen outside the original settings interaction;
- a user may not connect a later relaunch wave to an earlier spacing setting;
- warning-only mitigation is weak if the relaunch happens later during ordinary display changes;
- a background menu bar utility should not surprise-relaunch unrelated apps without just-in-time consent.

## Public Timeline

- #591 reported relaunches when plugging/unplugging an external monitor.
- Maintainers explained the spacing mismatch mechanism and the original issue was closed after a workaround: keep spacing consistent across displays.
- A later comment showed that the issue was broader than the original setup.
- Follow-up #685 was opened for runtime confirmation during display changes.
- Follow-up #686 was opened for clearer warning copy and stronger risk disclosure.
- PR #691 added just-in-time confirmation and stronger warning text.
- Review found a `Don't ask again + Cancel` risk; commit `e29074de` fixed it.
- PR #691 was approved and merged.

## Current Status

PR #691 appears to address the main safety gap: the display-transition path now asks before relaunching apps when applying spacing would actually cause a relaunch wave.

Remaining caveats:

- merge is not the same as testing an installed build on the user's setup;
- later #591 activity suggests wake-from-sleep relaunches may still happen in some builds/configurations, possibly via a different path;
- equal spacing should be described as avoiding the known mismatch path, not as a universal guarantee.

## Key Evidence

- Thread summary: `evidence/github/issue-591-thread-summary.md`
- Code inspection: `evidence/code/display-spacing-inspection.md`
- Warning-only critique: `analysis/why-warning-fix-is-insufficient.md`
- Maintainer response analysis: `analysis/nightah-response-analysis-2026-06-09.md`
- PR analysis: `evidence/code/pr-691-analysis-2026-06-10.md`
- PR summary: `evidence/github/pr-691-summary-2026-06-10.md`

## Public Outputs

- Initial #591 comment: `posted/591-initial-comment.md`
- Follow-up issues/comments: `posted/followups.md`
- Runtime confirmation issue draft: `drafts/issues/685-runtime-confirmation.en.md`
- Warning copy issue draft: `drafts/issues/686-warning-copy.en.md`
- PR review comment: `posted/pr-691-review-comment-dont-ask-again.md`

## Next Check

When a build containing PR #691 is available, test:

- display connect/disconnect;
- display switch / active menu bar display change;
- wake-from-sleep;
- manual Apply / Apply to All Displays;
- behavior with and without relaunch confirmations enabled.

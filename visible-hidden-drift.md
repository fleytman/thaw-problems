# Visible / Hidden Drift

Updated: 19.06.2026 21:44:35 +05

## Problem

The user's related symptom is not “one specific app always moves to hidden”. The observed pattern is broader: a previously visible menu bar item can disappear from the menu bar and later be found in Hidden settings. This looks like random category/layout drift rather than a single-app compatibility issue.

## Current Best Match

#702 is the strongest related issue found so far. It describes multi-display menu bar item positions/categories changing on their own, including items moving into Hidden. Maintainers discussed a root-cause model involving multi-display setup, menu bar relocation, `Displays have separate Spaces`, and failed state being persisted.

#675 is a broader/historical candidate around random icon reorder/layout drift. App-specific issues such as #707, #651, #605, #480, and #607 are useful background but less precise for the user's random visible -> hidden symptom.

## Current User Stance

Do not add noise to #702 without testing the exact maintainer-provided DMG. The user is not ready to test custom DMG builds right now. Wait for maintainers to validate, merge or close the fix; if the local symptom repeats later on a fixed/released build, compare it against #702 first.

## Key Evidence

- Related issue search: `evidence/related-issues/visible-hidden-search-2026-06-19.md`
- Dedicated #702 note: `evidence/related-issues/issue-702-random-visible-hidden-drift.md`
- Tracker current state: `tracker.md`

## Practical Interpretation

Treat visible/hidden drift as adjacent to, but distinct from, the spacing relaunch wave:

- spacing/relaunch is about applying system-wide spacing and restarting apps;
- visible/hidden drift is about persisted menu bar layout/category state moving unexpectedly;
- both can be triggered or exposed by multi-display state changes, but they should not be collapsed into one root cause without evidence.

## Next Check

Wait for #702 maintainer validation / merge. If the user's symptom appears again:

- record Thaw version and macOS version;
- record whether `Displays have separate Spaces` is enabled;
- compare whether the symptom is random category drift or app-specific behavior;
- check whether the installed build includes the #702 fix.

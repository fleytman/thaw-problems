# Miscommunication Analysis: nightah Responses

Created: 2026-06-09

## Question

User asked whether `nightah` correctly understood:

- `each` as each later display transition that causes a spacing apply, not only each affected app;
- that the problem is no just-in-time call to action before later auto-apply/relaunch;
- whether the non-active display warning was miscommunicated as a request for noisy per-edit popups.

## Findings

### `each time`

The published #686 text did include the condition:

> Each time connecting, disconnecting, or switching displays makes Thaw apply a different menu bar spacing...

and the summary said:

> this can happen on each later display connect/disconnect/switch event while spacing differs

So the intent was not “every display event always relaunches apps.” However, `nightah` responded to the risk of overstatement:

> This would only be the case if the user's configuration and said display have a differing spacing setup...

Conclusion: partial understanding. He caught the conditional nature and pushed against absolutist wording. To avoid further ambiguity, future wording should lead with the condition:

> When a display transition would require Thaw to apply a different menu bar spacing, it may relaunch apps...

### No just-in-time call to action

#685 stated:

> ...relaunch apps again without asking at that moment.

and:

> ...does not appear to show a just-in-time confirmation...

`nightah` responded:

> there definitely is an argument for a JIT warning for display changes...

Conclusion: he did understand the JIT-warning point at least at a high level. He may not fully agree that this is a consent problem rather than wording/user-interpretation, but he did acknowledge the JIT warning as a valid direction.

### Save now, destructive effect later

The issue argued that Global spacing/template can be saved before Apply-to-All, then later seed/apply display spacing. `nightah` responded that the global template was created exactly for seeding and said destructive nature only comes from Apply All broadcast if active display differs.

Conclusion: this is a real disagreement or partial misunderstanding. He treats the scenarios as separate and thinks the destructive path is narrower. User's point is not that saving the Global template immediately relaunches apps; it is that a setting can be saved quietly now and auto-applied later, so the cause is hard to diagnose.

### Non-active display warning

The published #686 text said:

> For non-active displays, the warning should also explain the delayed risk:
> Saving this spacing for a non-active display may relaunch apps later...

and the acceptance criteria said:

> Non-active display spacing edits warn...

This could be read as a request for a warning on each non-active display edit/save. `nightah` interpreted it as “future potentially destructive action” gating/noise.

Conclusion: there is a miscommunication. The intended ask can be narrowed:

- no modal for every non-active display edit;
- persistent warning should explain delayed risk;
- just-in-time warning should appear at actual display transition when Thaw knows relaunch is about to happen;
- per-row copy can be informational, not blocking.

## Practical Clarification

Best clarification if replying:

1. Agree that not every display transition relaunches apps.
2. Clarify that `each time` means “each time a display transition requires applying a different spacing.”
3. Clarify that the core issue is not Apply itself; it is later auto-apply/relaunch without JIT consent.
4. Clarify that no per-edit modal is requested for non-active displays; persistent warning + JIT warning at actual relaunch moment would satisfy the concern.

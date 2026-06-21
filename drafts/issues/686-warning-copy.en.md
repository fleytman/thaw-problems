# Make display-spacing relaunch warnings mention repeated display events, each affected app, and data-loss risk

Follow-up to #591 and to the behavior-level issue: https://github.com/stonerl/Thaw/issues/685.

I am opening this separately because the maintainer asked to track follow-up improvements separately, and because this is narrower than changing the runtime relaunch behavior itself.

## Summary

#670 added a persistent warning in Displays settings:

> Connecting a display whose spacing differs from the current spacing relaunches every menu bar app. Keep spacing consistent across displays to avoid relaunches.

That is a useful improvement, but I think the warning copy still understates the practical impact. The user needs to understand that this can happen on each later display connect/disconnect/switch event while spacing differs, that Thaw may terminate and reopen each app with a menu bar item, and that this can cause unsaved input, progress, or transient app state to be lost.

This happened in my case: I lost typed input in some applications after an unexpected relaunch wave.

## Suggested persistent warning text

> Each time connecting, disconnecting, or switching displays makes Thaw apply a different menu bar spacing, Thaw relaunches each app with a menu bar item. Keep spacing consistent across displays to avoid repeated relaunches. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

Why these changes:

- `Each time` is important because the surprise is not only the first manual Apply. The relaunch can happen again on later display connect/disconnect/switch events while spacing differs.
- `connecting, disconnecting, or switching displays` is clearer because the risk is tied to routine display state changes, not only an explicit settings action.
- `each app with a menu bar item` is more concrete than “menu bar app”.
- Mentioning possible loss of unsaved input/progress/state explains the real user impact instead of only describing the technical relaunch.

## Other warning text that should be strengthened

The same data-loss wording should be used consistently in the related alerts and annotations.

For example, instead of:

> Applying this spacing change will briefly relaunch all apps with menu bar items.

Consider:

> Applying this spacing change will relaunch each app with a menu bar item. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

For non-active displays, the warning should also explain the delayed risk:

> Saving this spacing for a non-active display may relaunch apps later when that display becomes the active menu bar display.

For “Apply global settings to all displays?”, the text should avoid the weaker “may briefly relaunch apps” phrasing and explicitly state the impact:

> This will overwrite the settings of N displays with the global template. If the active display's spacing changes, Thaw will relaunch each app with a menu bar item. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

## Visual severity

Because this can lead to loss of unsaved user work, I think this should be styled as a danger/critical warning rather than a normal warning. The risk is closer to a destructive operation than a cosmetic preference.

## Acceptance criteria

- The persistent display-spacing warning mentions repeated connect/disconnect/switch events and possible unsaved input/progress/state loss.
- Apply/global confirmation alerts mention that each affected app can be relaunched.
- Non-active display spacing edits warn that the relaunch may happen later when that display becomes active.
- The warning uses a visual treatment strong enough for possible user-work loss.

## Related

- #591
- #670
- https://github.com/stonerl/Thaw/issues/685

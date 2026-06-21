# GitHub Issue #686: nightah Response

Saved: 2026-06-09

Issue: https://github.com/stonerl/Thaw/issues/686
Comment: https://github.com/stonerl/Thaw/issues/686#issuecomment-4654856500
Author: `nightah`
Author association: `COLLABORATOR`
Created: `2026-06-09T00:34:52Z`

## Full Response

I'm open to changing/strengthening the wording; however, it's also important to consider that all of this is user opt-in.

Your suggestions also don't all hold true, because it would be completely dependent on what the user has configured.

> Each time connecting, disconnecting, or switching displays makes Thaw apply a different menu bar spacing, Thaw relaunches each app with a menu bar item. Keep spacing consistent across displays to avoid repeated relaunches. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

This would only be the case if the user's configuration and said display have a differing spacing setup from the previous hosted displays' menubar. We won't know if each time or switching displays would trigger this.

> Consider:
>
> > Applying this spacing change will relaunch each app with a menu bar item. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

I'm okay with this.

> For non-active displays, the warning should also explain the delayed risk:
>
> > Saving this spacing for a non-active display may relaunch apps later when that display becomes the active menu bar display.

I personally think this is extra noise that should be covered in the persistent warning. The warning should be clear and prominent enough for the user to understand upfront the nature of configuring these settings. A warning for a future potentially destructive action is unnecessary UX-gating.

> For “Apply global settings to all displays?”, the text should avoid the weaker “may briefly relaunch apps” phrasing and explicitly state the impact:
>
> > This will overwrite the settings of N displays with the global template. If the active display's spacing changes, Thaw will relaunch each app with a menu bar item. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

I'm also okay with this.

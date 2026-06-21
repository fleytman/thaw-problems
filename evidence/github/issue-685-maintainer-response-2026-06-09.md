# GitHub Issue #685: nightah Response

Saved: 2026-06-09

Issue: https://github.com/stonerl/Thaw/issues/685
Comment: https://github.com/stonerl/Thaw/issues/685#issuecomment-4654990039
Author: `nightah`
Author association: `COLLABORATOR`
Created: `2026-06-09T01:00:27Z`

## Full Response

I'm open to leaving this issue for users to provide their thoughts and feedback. My view is that if a user wants different spacing for different displays, this is really the only way to provide the ability to do that in an automated fashion.

I agree that we could change the wording to strengthen the message we want to convey, but I'll defer that discussion to #686.

It's also important to note that the settings only really take effect if the display is where the menubar is hosted. So the setting inherently being different doesn't necessarily mean a relaunch wave will/can occur.

> As a user, I would expect a spacing change to take effect either at the moment I explicitly press Apply, or perhaps after a system restart. Auto-applying it on later display connect/disconnect/switch events is not obvious.

I don't believe this is unclear from the existing wording, though it may point more to user interpretation. However, again, this suggests adjusting the wording to remove ambiguity and less reliance on interpretation.

> It is especially surprising because the Global spacing/template value is saved as soon as the slider is changed, even before “Apply to All Displays” is pressed. It does not immediately apply the spacing or relaunch apps, but that saved template can later be used when displays are seeded/applied. From the user's point of view, this means a value can be saved first and only become destructive later during a display state change.

The global section/template was created for exactly that purpose to address #633. Seeding the value for new connected displays that MAY host a menubar. The destructive nature only comes from the "Apply all" broadcast scenario if the active display has a different spacing setting. It feels to me like you're conflating lots of different scenarios, which may or may not necessarily be true or have any direct impact on a user.

Having said all that, I think there definitely is an argument for a JIT warning for display changes, and the ability for users to override/bypass the warning if they understand and agree to the destructive nature of the spacing changes.

cc: @stonerl @diazdesandi

# Draft GitHub Comment

First, thank you for continuing the work on Ice and keeping this kind of menu bar utility maintained. I appreciate the work that has gone into Thaw, and the explanations in this thread helped me understand why menu bar spacing can require relaunching menu bar items.

That said, I think this issue still describes a serious UX/safety problem worth tracking, even if the original reporter found a workaround.

From a user's point of view, connecting or disconnecting an external monitor is a normal, routine action. It is not an action that suggests unrelated menu bar applications may be quit and relaunched. When this happens, several apps can reopen their windows as if the system had just restarted, which is a very disruptive side effect.

The confusing part is that the trigger is not obvious. A user sees "display configuration changed", but the actual result is "multiple applications were restarted". Even if there is a technical reason related to menu bar spacing being fixed when menu bar items are created, that makes the UX contract more important, not less. Users need to know before Thaw causes a relaunch wave, and ideally they should have a way to avoid it.

The workaround of making spacing values match across displays is useful, but a user first has to know that this workaround exists. Right now that knowledge is effectively buried in a closed issue, usually after the user has already been surprised by the relaunch wave. It also does not resolve the core issue: Thaw can still create an unexpected restart wave during ordinary monitor changes. The later comments show that this can affect users who manage spacing globally outside Thaw and leave Thaw's own spacing settings at their defaults.

In my case, this was not only visually annoying. I lost typed input in some applications when they were restarted, which made the behavior genuinely frustrating. This turns a display change into user-work loss: the app did not just rearrange menu bar items, it caused active work/state in other applications to be lost and forced me to pay the redo cost.

That is why I see this as an interface antipattern, not just an implementation detail. Good UI should protect users' work, preserve recoverable state where possible, and avoid destructive defaults. A background menu bar utility should not cause data loss or restart unrelated applications as an unexpected side effect of reconnecting a display. If a relaunch wave is technically required for spacing reconciliation, it should be explicit, predictable, and user-controlled.

It also feels like a regression compared with Ice, which Thaw is forked from. I mainly wanted to move to what I expected to be a more stable, maintained app, not to accept more destructive behavior around routine monitor changes.

For affected users, I would consider this a critical bug or at least an unresolved UX/safety issue, not just a feature request. Could this be reopened or otherwise tracked until Thaw either avoids relaunching apps automatically on routine display connect/disconnect, provides an explicit opt-out / "respect global spacing" mode, or shows a clear warning/confirmation before triggering a relaunch wave?

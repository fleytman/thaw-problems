Thanks for clarifying. I created the separate follow-ups as requested:

1. https://github.com/stonerl/Thaw/issues/685 - behavior-level issue for display connect/disconnect/switch repeatedly relaunching menu bar apps without a just-in-time confirmation when spacing differs.
2. https://github.com/stonerl/Thaw/issues/686 - narrower warning/copy issue to make the risk clearer, including `each time`, `each app with a menu bar item`, and possible loss of unsaved input/progress/state.

This separates the runtime behavior concern from the warning text / visual severity concern.

That said, I still think closing #591 as resolved is risky. The core issue is not only that the behavior was insufficiently documented; it is that routine display state changes can unexpectedly relaunch unrelated apps again and cause user-work loss. I expected a manual Apply action to change spacing at the moment I pressed it, or perhaps after a system restart, but I did not expect later display connect/disconnect/switch events to re-apply spacing and relaunch apps without asking at that moment. A warning improves discoverability, but it does not remove the unsafe path or make the default behavior safe.

This also makes the issue hard to diagnose: if the setting was saved earlier and the destructive effect appears later during a display change, the user may not even realize that Thaw caused the relaunch wave.

Keeping the original bug closed also makes the problem harder to discover for other users who hit the same relaunch wave. The workaround and the explanation are effectively buried in a closed issue, even though the practical harm is still possible.

So I understand the request to split follow-up work into separate issues, and I did that. But I would still suggest reopening #591 or otherwise treating it as unresolved until there is a practical mitigation, with the new issues linked from it.

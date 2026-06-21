Thanks for the response. I think I should clarify a few points, because part of my message may have been read more broadly than I intended.

First, I agree that not every display connect/disconnect/switch necessarily triggers a relaunch wave. That was not my point. A more precise wording would be:

> When a display transition requires Thaw to apply a different menu bar spacing, Thaw can relaunch apps with menu bar items without asking the user at that moment.

So `each time` in my suggestion did not mean “every display event unconditionally”. It meant “each time a display transition causes Thaw to apply a different spacing”.

Second, my main concern is not that a manual Apply changes spacing. I understood that part. The non-obvious part is that spacing can be saved/configured earlier, and then later be applied automatically during display changes through app relaunches. As a user, I would more naturally expect this kind of system-level setting to apply either immediately after an explicit Apply, or after relogin/reboot/system restart. I did not expect a routine display change later to become the moment where Thaw applies the setting and relaunches unrelated apps.

This matters especially for diagnosis. A user may change a setting, see a warning or not read it carefully, continue working, and then much later get a relaunch wave when connecting/disconnecting a display. At that moment, the user may not understand that Thaw and an earlier spacing setting caused it. That is what I mean by delayed/shadow behavior.

About non-active display warnings: I am not necessarily asking for a modal/gating warning on every non-active display edit. If there is a JIT warning at the actual moment of risk, when Thaw knows a display transition is about to require relaunching apps, then separate blocking warnings on every non-active edit can indeed become excessive.

But if there is no JIT warning, then a delayed-risk warning becomes much more important, because otherwise the user saves configuration now and the destructive effect can appear later without any prompt.

For me, the best model would be:

1. the persistent warning clearly explains that spacing may be applied later during display transitions;
2. a JIT warning appears when a display transition would actually require relaunching apps;
3. users who understand the risk can bypass/disable that JIT warning.

That would address my main concern much better than relying only on a persistent warning in Settings.

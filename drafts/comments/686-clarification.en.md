Thanks, that sounds like a good direction for the wording changes.

I clarified the behavior-level / JIT-warning concern in #685. For this copy issue, I only want to narrow my suggested wording so it does not imply that every display event always relaunches apps.

A more precise version would be:

> When a display transition requires Thaw to apply a different menu bar spacing, Thaw relaunches apps with menu bar items. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

Or, if we want to keep the repeated nature explicit:

> Each time a display transition requires Thaw to apply a different menu bar spacing, Thaw may relaunch apps with menu bar items. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

On non-active displays: I agree that a separate blocking warning for every non-active edit can be noisy if #685 gets a JIT warning at the actual moment of risk. Without a JIT warning, though, the delayed-risk copy becomes more important, because otherwise the destructive effect can happen later with no prompt and no clear connection to the earlier setting.

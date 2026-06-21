One UX concern here: the suppression checkbox is applied independently of the modal response. If a user checks “Don't ask again” and then presses `Cancel`, the current apply is cancelled, but future confirmations are disabled, so a later display transition can apply spacing and relaunch apps silently.

That feels like remembering the checkbox rather than the action the user chose. Could this either only honor suppression when the user presses `Apply`, or make the checkbox consequence explicit, e.g. “Don't ask again and apply future spacing changes without confirmation”?

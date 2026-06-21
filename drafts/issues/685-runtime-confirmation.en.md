# Display connect/disconnect/switch can repeatedly relaunch menu bar apps without runtime confirmation

Follow-up to #591. I am opening this as a separate issue because the maintainer asked to track behavior changes separately from the original closed thread.

## Summary

Thaw can relaunch apps with menu bar items during ordinary display connect/disconnect/switch flows when the spacing configured for the active menu bar display differs from the currently applied system spacing.

The surprising part for me was not that pressing “Apply” changes spacing. I understood that part. The surprising part was that after different spacing is saved for displays/templates, Thaw can re-apply spacing on later display connect/disconnect/switch events and relaunch apps again without asking at that moment.

As a user, I would expect a spacing change to take effect either at the moment I explicitly press Apply, or perhaps after a system restart. Auto-applying it on later display connect/disconnect/switch events is not obvious.

From reading the current `development` code and `2.0.0-beta.14`, the runtime display-change path can apply the active display spacing after `screenParametersChanged`, and that path does not appear to show a just-in-time confirmation or offer a way to postpone the relaunch.

In my case this caused unrelated apps to be relaunched unexpectedly, and I lost typed input in some applications.

## Why this is serious

Connecting, disconnecting, or switching displays is a routine user action. A user does not naturally expect it to terminate and reopen unrelated apps.

Even if the relaunch is technically needed to apply `NSStatusItemSpacing`, it is still a destructive side effect from the user's point of view: apps can lose unsaved input, progress, or transient state. The user may be typing in another app when the display state changes.

This also makes the feature surprising because the UI presents spacing as a per-display customization, while the underlying macOS setting is effectively system-wide.

It is especially surprising because the Global spacing/template value is saved as soon as the slider is changed, even before “Apply to All Displays” is pressed. It does not immediately apply the spacing or relaunch apps, but that saved template can later be used when displays are seeded/applied. From the user's point of view, this means a value can be saved first and only become destructive later during a display state change.

That creates a dangerous combination: a setting can be saved without an explicit call to action and confirmation popup at the moment it is saved; later, it can be applied automatically during display changes; and the user may not even understand which app is causing unrelated apps to restart.

## Code path I am concerned about

My understanding from the current code, with AI assistance:

- `DisplayIceBarConfiguration.itemSpacingOffset` notes that macOS reads `NSStatusItemSpacing` as a single system-wide value, so Thaw writes that value and relaunches apps whenever a display becomes or remains the active menu bar display.
- The per-display spacing slider appears to be draft-only until `Apply`, but the Global spacing slider writes to `displaySettings.globalConfiguration` immediately.
- `DisplaySettingsManager` persists `globalConfiguration`, and newly detected displays are seeded from the current global template.
- `DisplaySettingsManager` listens for `didChangeScreenParametersNotification`. After display changes, if the active menu bar display changed, it calls `applyActiveDisplaySpacing(reason: "screenParametersChanged")`.
- `applyActiveDisplaySpacing` reads the active display's `itemSpacingOffset`, assigns it to `spacingManager.offset`, and calls `applyOffset()`.
- `MenuBarItemSpacingManager.applyOffset()` writes the defaults and relaunches affected menu bar item apps when the on-disk spacing does not already match the desired spacing.

So if display changes keep moving between displays/templates with different desired spacing values, the relaunch wave can happen each time Thaw needs to switch the system spacing, as part of runtime display-change handling rather than as an explicit Settings action.

## Expected behavior

Thaw should avoid silently relaunching unrelated apps during routine display state changes.

Possible mitigations:

- Use unified spacing across displays as the safe default.
- Treat per-display spacing as an explicit advanced/dangerous opt-in.
- If a display connect/disconnect/switch would require relaunching apps, ask the user at that moment.
- Offer choices such as:
  - relaunch now;
  - postpone;
  - keep current spacing for now;
  - disable per-display spacing / use unified spacing.

## Actual behavior

The current warning added in #670 improves disclosure, but the runtime behavior can still relaunch apps without a just-in-time confirmation each time a display state change requires a different spacing value.

## Related

- #591
- #670
- https://github.com/stonerl/Thaw/issues/686

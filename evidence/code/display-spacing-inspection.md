# Code Inspection: Display Spacing Relaunch Behavior

Created: 08.06.2026 14:25:08 +05

## Repository

- Local clone used for code inspection.
- Branch: `development`
- HEAD: `6a1e3dc4 docs: require logs in bug report and update description`
- Relevant upstream commit in branch history: `e6227bbe refactor(displays): warn when per-display spacing differs (#670)`
- Repo-local `AGENTS.md`: none found.

## Key Findings

### Current warning text

`Thaw/Settings/SettingsPanes/DisplaySettingsPane.swift:59-60` adds a callout:

> Connecting a display whose spacing differs from the current spacing relaunches every menu bar app. Keep spacing consistent across displays to avoid relaunches.

This is the #670 mitigation. It improves disclosure, but it does not change the relaunch behavior or defaults.

### Per-display slider is draft until Apply

`DisplaySettingsPane.swift:21-24` documents that per-display slider changes update `draftSpacing` only until Apply. The binding at `:249-252` only writes `draftSpacing[display.id] = $0`. Apply and Reset call `requestSpacingApply` at `:270-278`.

This contradicts the strongest version of the hypothesis that simply moving a per-display slider immediately changes saved config. For per-display rows, the code stages the value until Apply/Reset.

### Non-active display path can still save without alert

`DisplaySettingsPane.swift:300-315` routes Apply/Reset through `requestSpacingApply`. If there is no active profile and the target display is not the active display, it calls `commitSpacing(...)` immediately and returns without showing the alert.

This does not necessarily relaunch immediately because `DisplaySettingsManager.applyActiveDisplaySpacing` reads only the active display's configuration. It can, however, save a future mismatch that relaunches later when that display becomes active.

For non-active display + active profile, the alert is shown, but `spacingConfirmationMessage` at `:414-418` only says to save the new spacing to profile(s). It does not warn that switching to that display later may relaunch apps.

### Global spacing slider persists the template immediately

`DisplaySettingsPane.swift:556-567` writes the global spacing slider value directly into `displaySettings.globalConfiguration`. The code comment says the relaunch wave only fires when Apply-to-All writes per-display configurations, but the template itself is saved by the `$globalConfiguration` sink in `DisplaySettingsManager.swift:264-275`.

`DisplaySettingsManager.swift:97-105` seeds newly detected displays from the current global template. Therefore changing the global slider can affect future display connections even before Apply-to-All.

This matches the user's observed behavior if the edited control was the Global spacing/template slider: the changed value is saved without pressing Apply-to-All, but it does not immediately apply system spacing or relaunch apps. The destructive effect can appear later when display state changes cause Thaw to apply a different desired spacing.

### Display connect/switch applies spacing without runtime confirmation

`DisplaySettingsManager.swift:278-316` listens for `NSApplication.didChangeScreenParametersNotification`, refreshes known displays, and calls `applyActiveDisplaySpacing(reason: "screenParametersChanged")` when the active menu bar display changes.

`DisplaySettingsManager.swift:367-379` reads `configurationForActiveDisplay().itemSpacingOffset`, assigns it to `appState.spacingManager.offset`, then calls `applyOffset()`.

There is no user confirmation in this runtime path. The no-op guard in `applyOffset()` prevents relaunch only when on-disk spacing already matches the desired value.

If the user repeatedly connects/disconnects/switches between displays whose desired spacing values differ, Thaw can need to switch the single system-wide spacing value repeatedly. That means the relaunch wave can recur on each relevant display transition, not only on the original manual Apply.

### Relaunch mechanism terminates and reopens apps

`DisplayIceBarConfiguration.swift:32-36` says the OS reads `NSStatusItemSpacing` as a single system-wide value, so Thaw writes it and relaunches when the display becomes or remains active.

`MenuBarItemSpacingManager.swift:261-369` writes the new defaults, enumerates menu bar item PIDs, and relaunches affected apps. `relaunchApp` at `:196-210` calls `signalAppToQuit` and then launches the app again. `signalAppToQuit` at `:127-167` calls `app.terminate()` and can force terminate after a delay.

This supports the data-loss concern: even if the relaunch path attempts to restore apps, it can still destroy transient app state or unsaved input depending on the target application.

## Interpretation for Follow-up

The strongest code-backed claim is:

- #670 improves disclosure, but the dangerous behavior remains.
- Runtime display-change relaunch can happen without a just-in-time confirmation.
- Per-display spacing should be framed as dangerous/advanced because it maps to one system-wide macOS setting and can require relaunching unrelated apps.
- The public warning should mention each relevant connect/disconnect/switch event, each affected app, and possible loss of unsaved input/progress/state.
- Be precise: per-display slider movement itself is staged; global slider movement persists the global template immediately.

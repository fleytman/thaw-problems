# Why The Warning Fix Is Still Insufficient

Created: 07.06.2026 14:54:08 +05

## Scope Correction

Do not overstate the "new monitor" hypothesis as "new display has no spacing configured."

More precise framing:

- Thaw appears to apply a global template / configured display spacing when a display is connected or becomes active.
- The relaunch wave risk appears when the spacing Thaw wants to apply differs from the current on-disk/current active spacing.
- A new display may participate in the issue if the global template or its inherited/default display config differs from the current active spacing.
- In the user's case, after learning about the behavior, it is possible that not all related spacing values were corrected.

So the safest statement is:

> Connecting or switching displays can still unexpectedly trigger a relaunch wave when the spacing configuration Thaw applies for that display differs from the current spacing.

## Why Current Fix Helps

Commit `e6227bbe` is a real UX improvement:

- it explicitly warns about display connection, not only manual "apply" actions;
- it says every menu bar app will be relaunched;
- it advises keeping spacing consistent across displays;
- it places the warning near the relevant Displays settings.

This reduces discoverability failure.

## Why It Is Still Insufficient

### 1. Warning is not a full mitigation for data/progress loss

The current warning describes the relaunch wave as an app-management side effect, but it still does not clearly communicate user-work risk:

- unsaved input can be lost;
- transient app state can be lost;
- windows can reopen unexpectedly;
- apps can behave as if launched after reboot/login;
- the user may need to reconstruct work.

For a setting that can cause loss of input/progress in unrelated apps, "relaunches every menu bar app" is technically accurate but not enough for risk comprehension.

### 2. The danger exists during both manual and automatic paths

Manual apply warning is necessary, but insufficient if display connect/disconnect can later trigger relaunches without a fresh explicit confirmation.

The dangerous operation is not just "user clicked apply once"; it is "Thaw may later re-apply spacing when display state changes."

### 3. Per-display spacing still looks like a normal configuration path

The fix documents the risk, but it does not make the risky path hard to enter.

If per-display spacing can cause repeated relaunch waves, it should not feel like a regular, harmless customization slider. It should be treated more like an advanced/dangerous capability.

### 4. Warning color may understate severity

A muted mustard warning is appropriate for caution. The impact here can include data/progress loss, not merely visual churn.

For potentially destructive behavior, red/danger styling may better match severity than a generic warning color.

### 5. Closing as resolved conflates documentation with practical resolution

The commit resolves "users were not told about the display-connection relaunch cost."

It does not resolve:

- "the app can relaunch unrelated apps on display changes";
- "the setting path can cause user-work loss";
- "users can enter the dangerous state too easily";
- "there is no opt-out / respect-global-spacing-only mode";
- "there is no runtime confirmation before display-change relaunch."

Closing as fully resolved is philosophically weak if the practical harm remains and only the warning changed.

## Preferred Product Behavior

### 1. Make the danger explicit

The UI should say plainly:

> Changing or applying different spacing across displays may quit and relaunch menu bar apps. This can reopen windows and may cause unsaved input or transient app state to be lost.

This should be shown for:

- manual spacing apply;
- enabling per-display spacing;
- display connect/disconnect behavior if relaunch is about to happen.

### 2. Gate the risky feature

Per-display spacing should require an explicit opt-in, for example:

- checkbox: `Enable advanced per-display spacing`;
- explanatory text before enabling;
- confirmation dialog;
- possibly "I understand that this can relaunch apps and lose unsaved input/state."

This changes the mental model from "normal slider setting" to "dangerous advanced feature."

### 3. Use danger-level visual treatment

Because the risk includes data/progress loss, the UI should consider red/danger styling, not only muted warning styling.

The important point is not aesthetic color preference; it is severity calibration. A user should immediately understand this is not a cosmetic preference.

### 4. Ask at runtime before relaunching on display changes

If Thaw detects that connecting/disconnecting/switching displays would require a relaunch wave, the user should be asked:

- relaunch now;
- postpone;
- keep current spacing for now;
- disable per-display spacing / use unified global spacing.

This is especially important because the user may currently be typing or working in another app.

### 5. Separate feature request vs bug framing

An opt-out/respect-global-spacing-only mode can be tracked as a feature.

But the current problem is not only a feature request. The bug/UX bug is:

> A normal-looking configuration can lead to unexpected relaunches of unrelated apps and user-work loss during routine display changes.

Asking users to open a feature request for practical mitigation can feel like category drift: the user reports destructive behavior, while the project recasts the remaining mitigation as a feature request after adding a warning.

Better framing:

- keep the bug/UX issue open until the dangerous default/risky path is practically mitigated;
- optionally create linked FRs for specific solutions like `respect global spacing only`, `runtime confirmation`, or `advanced per-display spacing gate`.

# Issue Thread Summary: stonerl/Thaw#591

Source: https://github.com/stonerl/Thaw/issues/591
Checked: 07.06.2026 09:35:24 +05

## Issue Metadata

- Repository: `stonerl/Thaw`
- Issue: `#591`
- Title: `[Bug]: Thaw 2.0.0-beta.12 (43) closes certain menu bar applications when plugging/unplugging an external monitor`
- Opened: 14.05.2026 by `noraa`
- Closed: 15.05.2026 by `nightah`
- State reason from issue API: `completed`
- Labels: `bug`, `P1`
- Project status in GitHub page: `Done`
- App version: `2.0.0-beta.12 (43)`
- macOS version: `Tahoe 26.5`
- Monitor setup: multi-monitor

## Reported Behavior

The original report says that plugging or unplugging an external monitor causes Thaw to reload menu bar icons. During that reload, several menu bar apps quit and reopen. The user sees windows appear as if apps had launched after a reboot.

## Timeline

1. `noraa` reports the issue with external monitor connect/disconnect causing menu bar apps to quit/reopen.
2. `nightah` asks whether different spacing is configured per display and says that if spacing differs, apps must go through a relaunch wave to reset menu bar spacing. They describe this as expected behavior.
3. `noraa` confirms that matching spacing across monitors resolved the problem in their setup.
4. `nightah` closes the issue after saying they are glad it is sorted.
5. After closure, `andreasmartin` adds that the same behavior also occurs when spacing is configured globally through macOS defaults, even if Thaw's own spacing settings are left default.
6. `nightah` explains that Thaw's spacing setting is effectively the same as writing the global setting, and existing menu bar icons cannot update spacing at runtime without relaunch/recreation.
7. `andreasmartin` clarifies that globally configured spacing remains stable across monitor/projector reconnects without relaunch waves when using more traditional menu bar managers, and asks for a mode where Thaw respects global spacing without reconciling it dynamically.
8. `diazdesandi` says they have run into this too, agree it is disruptive, and mention possible improvements such as clearer messaging or an option/alert explaining when and why a relaunch will happen.
9. `nightah` apologizes for terse wording, agrees the behavior would occur in that global-spacing circumstance, and says more information should be provided about the repercussions of configuring spacing both inside and outside Thaw. They also suggest opening a feature request for an opt-out setting.
10. `fleytman` posts a comment arguing that the issue remains an unresolved UX/safety problem because routine display changes can cause unexpected app restarts and user-work loss.
11. `diazdesandi` replies that the behavior has been addressed in newer betas: `2.0.0-beta.13` added an alert explaining that changing spacing relaunches menu bar apps, and the next beta adds a persistent in-app warning about display behavior. They say the issue will remain closed, and that opt-out / global-settings-only behavior should be proposed as a separate feature request.

## Key Interpretation

The issue was closed because the original author's setup had an immediate workaround: make spacing the same across displays. The later comments show a broader UX/safety problem remains:

- users do not reasonably expect monitor connect/disconnect to restart unrelated apps;
- the behavior can occur even when the user is not actively changing spacing in Thaw at that moment;
- maintainers appear to treat the relaunch wave as technically expected, but also acknowledge it is disruptive and needs clearer handling;
- a warning, confirmation, opt-out, or "respect global spacing" mode would directly address the surprise and impact.

## New Maintainer Response: 07.06.2026

Maintainer response narrows "addressed" to warning/messaging, not behavioral prevention. This is a partial resolution:

- alert before explicit spacing changes addresses discoverability for user-triggered spacing changes;
- persistent in-app warning may address display-change surprise if it is visible in the relevant settings context;
- it does not eliminate relaunch waves or add an opt-out/respect-global-spacing mode;
- the request for "do not manage spacing / respect global settings only" is being classified as a feature request, not a bug fix.

## User Follow-up Position

The user's current position is stronger than "please add opt-out":

- warnings are not equivalent to fixing a severe default-behavior problem;
- users may ignore or misunderstand warnings, especially if wording says apps may "briefly relaunch" without making clear that input/state can be lost;
- connecting a new monitor can unexpectedly trigger the issue because that display may not have spacing configured yet;
- safe default should be unified spacing across all displays;
- per-display spacing should be visually marked as dangerous/advanced and explain the full consequence before enabling;
- closing a bug before practical resolution is philosophically weak, and asking for a feature request around what is experienced as a bug is confusing.

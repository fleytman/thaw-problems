# Сделать display-spacing warnings более явными: каждое событие, каждое affected app и риск потери данных

Follow-up к #591 и к behavior-level issue: https://github.com/stonerl/Thaw/issues/685.

Открываю это отдельно, потому что maintainer попросил трекать follow-up improvements отдельно, и потому что это более узкая задача, чем изменение runtime relaunch behavior.

## Summary

#670 добавил persistent warning в Displays settings:

> Connecting a display whose spacing differs from the current spacing relaunches every menu bar app. Keep spacing consistent across displays to avoid relaunches.

Это полезное улучшение, но я считаю, что copy всё ещё недооценивает practical impact. Пользователь должен понимать, что это может происходить при каждом последующем connect/disconnect/switch событии, пока spacing differs, что Thaw может завершить и заново открыть каждое приложение с menu bar item, и что это может привести к потере unsaved input, progress или transient app state.

В моём случае это действительно произошло: я потерял введённый текст в некоторых приложениях после неожиданной relaunch wave.

## Suggested persistent warning text

> Each time connecting, disconnecting, or switching displays makes Thaw apply a different menu bar spacing, Thaw relaunches each app with a menu bar item. Keep spacing consistent across displays to avoid repeated relaunches. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

Почему эти изменения:

- `Each time` важно, потому что surprise не только в первом manual Apply. Relaunch может повторяться при последующих display connect/disconnect/switch events, пока spacing differs.
- `connecting, disconnecting, or switching displays` понятнее, потому что риск связан с routine display state changes, а не только с explicit settings action.
- `each app with a menu bar item` конкретнее, чем “menu bar app”.
- Упоминание возможной потери unsaved input/progress/state объясняет реальный user impact, а не только технический relaunch.

## Other warning text that should be strengthened

Такой же data-loss wording стоит использовать последовательно в связанных alerts и annotations.

Например, вместо:

> Applying this spacing change will briefly relaunch all apps with menu bar items.

Можно:

> Applying this spacing change will relaunch each app with a menu bar item. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

Для non-active displays warning должен также объяснять delayed risk:

> Saving this spacing for a non-active display may relaunch apps later when that display becomes the active menu bar display.

Для “Apply global settings to all displays?” лучше уйти от более слабого “may briefly relaunch apps” и явно назвать impact:

> This will overwrite the settings of N displays with the global template. If the active display's spacing changes, Thaw will relaunch each app with a menu bar item. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

## Visual severity

Поскольку это может привести к потере unsaved user work, я считаю, что это должно быть styled as danger/critical warning, а не как обычный warning. По риску это ближе к destructive operation, чем к cosmetic preference.

## Acceptance criteria

- Persistent display-spacing warning упоминает повторяющиеся connect/disconnect/switch events и возможную потерю unsaved input/progress/state.
- Apply/global confirmation alerts говорят, что каждое affected app может быть relaunched.
- Non-active display spacing edits предупреждают, что relaunch может случиться позже, когда этот дисплей станет active.
- Warning использует visual treatment, достаточно сильный для possible user-work loss.

## Related

- #591
- #670
- https://github.com/stonerl/Thaw/issues/685

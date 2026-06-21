# Подключение/отключение/переключение дисплея может повторно перезапускать menu bar apps без runtime-подтверждения

Follow-up к #591. Открываю это как отдельное issue, потому что maintainer попросил вынести изменения поведения в отдельный трекер.

## Summary

Thaw может перезапускать приложения с menu bar items во время обычных подключений, отключений или переключений дисплея, если spacing, настроенный для active menu bar display, отличается от текущего применённого system spacing.

Для меня неожиданной частью было не то, что нажатие “Apply” меняет spacing. Это я понимал. Неожиданным было то, что после сохранения разных spacing values для displays/templates Thaw может заново применять spacing при последующих connect/disconnect/switch событиях и снова relaunch apps без запроса в этот момент.

Как пользователь, я бы ожидал, что spacing change применяется либо в момент явного нажатия Apply, либо, возможно, после перезагрузки системы. Auto-apply при подключении/отключении/переключении дисплея для меня неочевиден.

По текущему коду на `development` и `2.0.0-beta.14`, runtime path при изменении дисплеев может применить spacing активного дисплея после `screenParametersChanged`, и этот path, похоже, не показывает just-in-time confirmation и не предлагает отложить relaunch.

В моём случае это привело к неожиданному перезапуску unrelated apps, и я потерял введённый текст в некоторых приложениях.

## Why this is serious

Подключение, отключение или переключение дисплея - обычное пользовательское действие. Пользователь естественно не ожидает, что это завершит и заново откроет unrelated apps.

Даже если relaunch технически нужен для применения `NSStatusItemSpacing`, с точки зрения пользователя это всё равно destructive side effect: приложения могут потерять unsaved input, progress или transient state. Пользователь может в этот момент печатать или работать в другом приложении.

Это также делает feature неожиданной: UI показывает spacing как per-display customization, но underlying macOS setting фактически system-wide.

Отдельно это неожиданно из-за Global spacing/template: значение сохраняется сразу при изменении slider, ещё до нажатия “Apply to All Displays”. Оно не применяет spacing и не relaunch apps немедленно, но сохранённый template потом может использоваться при seed/apply для дисплеев. С пользовательской точки зрения это означает, что значение сначала сохраняется, а destructive effect может проявиться позже при display state change.

Получается опасное комбо: настройка может быть сохранена без явного call to action с подтверждающим popup в момент сохранения; затем эта настройка начинает автоматически применяться при display changes; а пользователь может даже не понять, какое приложение вызывает перезапуск unrelated apps.

## Code path I am concerned about

Моё понимание текущего кода, с помощью AI:

- `DisplayIceBarConfiguration.itemSpacingOffset` отмечает, что macOS читает `NSStatusItemSpacing` как single system-wide value, поэтому Thaw пишет это значение и перезапускает apps, когда дисплей становится или остаётся active menu bar display.
- Per-display spacing slider, похоже, draft-only до `Apply`, но Global spacing slider сразу пишет в `displaySettings.globalConfiguration`.
- `DisplaySettingsManager` сохраняет `globalConfiguration`, а новые detected displays seed'ятся из текущего global template.
- `DisplaySettingsManager` слушает `didChangeScreenParametersNotification`. После display changes, если active menu bar display изменился, он вызывает `applyActiveDisplaySpacing(reason: "screenParametersChanged")`.
- `applyActiveDisplaySpacing` читает `itemSpacingOffset` активного дисплея, присваивает его в `spacingManager.offset` и вызывает `applyOffset()`.
- `MenuBarItemSpacingManager.applyOffset()` пишет defaults и перезапускает affected menu bar item apps, если on-disk spacing ещё не совпадает с desired spacing.

То есть если display changes продолжают переключать пользователя между displays/templates с разными desired spacing values, relaunch wave может происходить каждый раз, когда Thaw нужно сменить system spacing, как часть runtime display-change handling, а не как явное действие пользователя в Settings.

## Expected behavior

Thaw не должен silently relaunch unrelated apps во время routine display state changes.

Возможные mitigations:

- использовать unified spacing across displays как safe default;
- считать per-display spacing явным advanced/dangerous opt-in;
- если display connect/disconnect/switch потребует relaunch apps, спрашивать пользователя в этот момент;
- предложить варианты:
  - relaunch now;
  - postpone;
  - keep current spacing for now;
  - disable per-display spacing / use unified spacing.

## Actual behavior

Текущий warning, добавленный в #670, улучшает disclosure, но runtime behavior всё ещё может relaunch apps без just-in-time confirmation каждый раз, когда display state change требует другого spacing value.

## Related

- #591
- #670
- https://github.com/stonerl/Thaw/issues/686

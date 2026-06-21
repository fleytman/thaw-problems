# Поиск: случайное приложение пропадает из visible и находится в hidden

Дата: 2026-06-19

Репозиторий: `stonerl/Thaw`

## Уточнённый пользовательский симптом

Пользователя интересует не сценарий “одно конкретное приложение всегда уходит в hidden”, а другой класс поведения:

- внезапно какое-то приложение пропадает из видимой части menu bar;
- пользователь не сразу понимает, что произошло;
- потом в настройках Thaw обнаруживает, что приложение оказалось в hidden;
- выглядит не как несовместимость одного конкретного приложения, а как случайный дрейф состояния layout/category.

По этому описанию главный кандидат — не issues про конкретные приложения вроде OneDrive/Toggl/Little Snitch, а issues про общий дрейф категорий и позиций.

## Запросы поиска

Использованные запросы:

- `visible hidden`
- `visible app hidden`
- `"Visible" "Hidden" "Always-Hidden"`
- `"Hidden" "Visible" "restart"`
- `"visible section"`
- `"hidden section"`
- `reorder layout hidden`
- `random icons layout`
- `"wrong sections" OR "wrong section"`
- `"random" "hidden section"`
- `"random" "visible" "hidden"`
- `"icons" "random places"`
- `"not show" "hidden" "settings"`

Точные запросы по `random + visible/hidden` дали мало результатов. Лучше всего находятся issues, где авторы описывают это как `reordering`, `random places`, `change position`, `moved to hidden`, а не как “random app moved from visible to hidden”.

## Самый релевантный issue под пользовательский кейс

### #702 - `[Bug] The menu bar items change position on their own.`

URL: https://github.com/stonerl/Thaw/issues/702

Состояние: `OPEN`

Метки: `bug`, `P2`, `macos-26`

Почему это лучший кандидат:

- Это не один конкретный icon, а общий дрейф layout/category.
- Автор issue описывает обычный “daily routine”: утром layout один, днём другой.
- В body прямо сказано, что `MeetingBar has been moved to the Hidden section`.
- Позже в thread reporter пишет обратный симптом: много icons moved from hidden to visible.
- Maintainer связывает проблему с multi-display setup, `Displays have separate Spaces`, menu bar relocation между дисплеями, моментом когда macOS “un-homes” off-screen hidden items, и тем, что неудачное состояние может быть записано в saved layout.
- Issue остаётся `OPEN`; maintainers просят тестировать конкретный DMG, потому Homebrew beta/RC не содержит тех же fixes.

Локальная оценка: если у пользователя “случайное приложение пропало из visible и оказалось hidden” в multi-display setup, #702 — первый issue, с которым стоит сравнивать.

## Второй общий/исторический кандидат

### #675 - `[Bug] MenuBar icons keep reordering / appearing in random places`

URL: https://github.com/stonerl/Thaw/issues/675

Состояние: `CLOSED`

Метки: `bug`, `P2`, `macos-26`

Почему релевантен:

- Исходный report описывает не одно приложение, а общий drift: icons reorder in Visible/Hidden category и некоторые переходят из Hidden в Always-Hidden.
- Несколько пользователей подтверждали похожее на beta14/beta15.
- В comment thread есть важный пример: `Thaw re-ordered itself into a hidden section`; позже автор пишет, что hidden items оказались в visible section.
- Maintainer связывал часть проблемы с multi-display setup и menu bar moving between screens при `Displays have separate Spaces`.
- Test DMG был выдан, fix должен был попасть в beta16.

Локальная оценка: #675 — предшественник #702. Он полезен как контекст, но для нового/текущего проявления лучше ссылаться на #702.

## Issues, привязанные к конкретным приложениям и менее похожие на пользовательский кейс

Эти issues точнее совпадают с формулировкой “visible -> hidden”, но хуже совпадают с пользовательским “внезапно случайное приложение”, потому что там конкретное приложение стабильно воспроизводит проблему.

### #707 - `[Bug] OneDrive always set to permanently hidden upon relaunching`

URL: https://github.com/stonerl/Thaw/issues/707

Состояние: `OPEN`

Метки: `bug`, `P3`

Почему релевантен:

- OneDrive items снова и снова оказываются hidden/always-hidden несмотря на visible setting.
- Автор воспроизводит это на нескольких машинах и с single/multiple monitor setup.
- Maintainer отмечает, что OneDrive identifies menu extras only as `Item-0`/`Item-1`, и mapping может меняться в зависимости от launch order.

Почему это не основной match для пользователя:

- Это конкретный OneDrive case, а не случайный drift разных приложений.

### #651 - `[Bug] Little Snitch icon disappears from visible section and moves to hidden section after a few minutes`

URL: https://github.com/stonerl/Thaw/issues/651

Состояние: `CLOSED`

Метки: `bug`, `P2`, `macos-26`

Почему релевантен:

- Точное совпадение по симптому visible -> hidden.
- Дополнительная clue: Thaw UI не показывал Little Snitch ни в visible, ни в hidden list, что указывает на detection/tracking mismatch.
- Закрыт как duplicate of #643.

Почему это не основной match:

- Привязано к Little Snitch; вероятно, это identity/detection problem.

### #605 - `[Bug]: Thaw randomly put CodexBar in hidden section`

URL: https://github.com/stonerl/Thaw/issues/605

Состояние: `CLOSED`

Метки: `bug`, `P3`, `upstream`

Почему релевантен:

- Автор закрепляет CodexBar в visible; позже CodexBar исчезает из visible и оказывается в Always Hidden.

Почему это не основной match:

- Привязано к конкретному приложению и отмечено label `upstream`.

### #480 - `[Bug]: Toggl app menu item keeps getting moved to hidden section`

URL: https://github.com/stonerl/Thaw/issues/480

Состояние: `CLOSED`

Метки: `bug`, `P2`, `needs-info`

Почему релевантен:

- Автор перемещает Toggl в visible; через несколько минут он уходит в hidden.
- Автор подозревает dynamic timer/title changes.

Почему это не основной match:

- Вероятно, привязано к dynamic-title/status-item identity issue.

### #607 - `[Bug]: onedrive always in hidden section`

URL: https://github.com/stonerl/Thaw/issues/607

Состояние: `CLOSED`

Почему релевантен:

- OneDrive не остаётся visible после reboot.

Почему это не основной match:

- Старый OneDrive-specific issue, вероятно superseded by #707.

## Связанные, но с другим направлением симптома

### #714 - `[Bug] Hidden items briefly appear after status item churn`

URL: https://github.com/stonerl/Thaw/issues/714

Состояние: `OPEN`

Почему рядом:

- Это тоже про неожиданное category/layout behavior after status item churn.
- В root theory упоминаются ambiguous saved identifiers и false-positive saved-layout restore.

Почему не match:

- Направление симптома в основном hidden -> visible/exposed, а не visible -> hidden.

### #727 - `[Bug] Hidden item disappears soon after being shown`

URL: https://github.com/stonerl/Thaw/issues/727

Состояние: `OPEN`

Почему рядом:

- Смежный issue про temporary show/rehide.

Почему не match:

- Это про hidden item, который исчезает слишком быстро после показа, а не про visible item, который молча переехал в hidden.

## Вывод

Для пользовательского кейса “случайное приложение внезапно пропало из visible и обнаружилось в hidden” лучший текущий match — **#702**.

Обоснование:

- симптом похож на случайный/general layout drift, а не на стабильное поведение одного приложения;
- multi-display и menu bar relocation выглядят правдоподобными trigger'ами;
- maintainer уже описывает механизм, при котором hidden/visible state может быть повреждён и сохранён;
- issue всё ещё открыт и активен.

Issues, привязанные к конкретным приложениям (#707, #651, #605, #480, #607), полезны как подтверждение, что visible -> hidden class существует, но они хуже подходят как primary reference для случайного приложения.

Возможная публичная формулировка на английском, если понадобится:

```md
This may be related to #702 rather than the app-specific visible→hidden reports. In my case it is not one specific app that always fails; a random visible item can disappear from the menu bar, and I later find it in the hidden section in Thaw settings.
```

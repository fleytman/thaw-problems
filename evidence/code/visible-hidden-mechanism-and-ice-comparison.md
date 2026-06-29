# Механизм пропадания visible -> hidden и сравнение с Ice

Дата: 25.06.2026

Локальный клон Thaw: `~/Thaw`, ветка `development`, HEAD `4c04159e` (после fast-forward на 24.06.2026).
Локальный клон Ice: `~/xcode_projects/Ice`, HEAD `f063ee7` (05.07.2024).

Это разбор кода под пользовательский кейс: visible-элемент внезапно пропадает из menu bar и потом находится в Hidden. Связано с #702 (closed, fix в PR #743).

## Вывод коротко

Пропадание visible -> hidden — это следствие Thaw-специфичной подсистемы авто-персиста и авто-восстановления layout, которой в оригинальном Ice нет. Поэтому ощущение пользователя "в Ice такой проблемы не было" подтверждается на уровне кода.

## Что есть в Thaw и чего нет в Ice

Файлы только в Thaw (`Thaw/MenuBar/MenuBarItems/`):

- `LayoutSolver.swift`
- `LayoutReconciler.swift`
- `PendingLedger.swift`

Механизмы только в Thaw:

- персист `savedSectionOrder` в `UserDefaults`, ключ `MenuBarItemManager.savedSectionOrder`
  (`MenuBarItemManager.swift`: `persistSavedSectionOrder()` ~ строки 434-436, загрузка ~ 381-383);
- авто-применение сохранённого порядка `applySavedLayout(...)` (`MenuBarItemManager.swift` ~ 6735+);
- авто-запись текущей категоризации `saveSectionOrder(from:)` на каждом cache tick (`MenuBarItemManager.swift`, ветвление ~ 1820+).

Проверка по Ice (`~/xcode_projects/Ice`):

- `grep -rn "LayoutSolver|SavedLayout|savedLayout|applySavedLayout"` -> пусто;
- директории `Ice/MenuBar/MenuBarItems/` нет; menu bar логика плоско лежит в `Ice/MenuBar/` (`MenuBarItemManager.swift`, `MenuBarSection.swift`, `RehideStrategy.swift`).

В Ice категория предмета определяется позиционно относительно собственных control items; Ice не сохраняет на диск и не восстанавливает порядок/категории сторонних items автоматически. Поэтому в Ice нет пути, который мог бы записать испорченное visible/hidden состояние и потом его навязать.

## Механизм поломки (Thaw)

1. Каждые ~5с cache cycle:
   - пишет текущую категоризацию visible/hidden/alwaysHidden через `saveSectionOrder(from:)`;
   - вызывает `applySavedLayout(...)`, который по триггерам перекладывает items под сохранённый порядок.
2. Триггеры `applySavedLayout` (комментарии в коде, ~6779-6804):
   - windowID change: пропал previous windowID (app quit/relaunch);
   - layout divergence: хотя бы один saved item сейчас в другой секции, чем в `savedSectionOrder` (ambient drift: сторонние тулзы, Stage Manager, screen lock/unlock, macOS re-spawning the bar).
3. При multi-display relocation / wake / display reconnect macOS асинхронно мигрирует status item windows между экранами и временно "un-homes" off-screen hidden items (раскрывает их). Если cache tick попадает в этот transient:
   - write path запекает раскрытые items в `savedSectionOrder` как visible;
   - либо `applySavedLayout` диспатчит bulk apply на items, растянутых по двум дисплеям; перемещения не сходятся, items остаются на чужом экране, где читаются как un-hidden; следующий persist пишет это испорченное состояние.
4. Итог: ранее visible item оказывается перемещён в hidden (или наоборот), и состояние закрепляется на диске.

Это совпадает с root-cause моделью maintainer'а в #702 (см. `../related-issues/issue-702-random-visible-hidden-drift.md`).

## Что чинит PR #743 (commit 4c04159e, Closes #702)

Файлы: `LayoutSolver.swift` (+55), `MenuBarItemManager.swift` (+85/-9), плюс тесты.

Добавлены гейты в оба пути:

- Display-spread gate: `LayoutSolver.itemsSpanMultipleDisplays(itemCenters:screenFrames:)`
  - в `applySavedLayout`: skip, если items на двух дисплеях (relocation in progress);
  - в persist-ветке `saveSectionOrder`: не писать, пока items straddle two displays (чтобы не запечь un-hidden items в saved layout).
- Geometry-readiness gate: `LayoutSolver.isMenuBarGeometryReady(rightBoundary:notchMaxX:)`
  - на notched display skip apply, пока геометрия не устаканилась (стейл off-screen позиция при display reconnect / Control Center widget churn).
- `concludeProfileApplyWithoutMoves(source:items:)`
  - чинит залипание `isApplyingProfileLayout = true` после no-moves apply (частого при display reconnect, когда профиль активного дисплея переприменяется на уже корректный bar). Без этого флаг навсегда блокировал `applySavedLayout` до конца сессии ("stuck profile flag").

## Связь с wake-from-sleep и #591

- Триггеры в комментариях кода прямо включают display reconnect и macOS re-spawning the bar.
- Wake-from-sleep с несколькими дисплеями = display reattach + menu bar relocation + transient straddling — ровно precondition этого пути.
- Поэтому visible/hidden drift (#702) и spacing relaunch wave (#591/#691) делят общий триггер-environment (multi-display reattach при пробуждении), хотя реализованы разными code paths (spacing apply vs saved-layout apply/persist).

## Статус и что проверять

- #702: CLOSED (23.06.2026), fix влит PR #743 (MERGED 24.06.2026) в `development`.
- Fix НЕ в релизе: тег `2.0.0-rc.1` (`90068377`, 16.06.2026) предшествует fix; `git merge-base --is-ancestor 4c04159e 2.0.0-rc.1` -> false. Maintainer просит до RC2 пользоваться последним DMG из #702.
- Practical next: после RC2/beta с #743 (+#742) проверить wake-from-sleep с двумя дисплеями; при повторе собрать логи именно с этой сборки.

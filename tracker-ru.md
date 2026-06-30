# Tracker (RU)

Updated: 25.06.2026 09:43:00 +05

## Language Note

Это оригинальная русская версия tracker, сохранённая как `tracker-ru.md`.

Основная публичная английская версия находится в `tracker.md`; оригинальный русский journal сохранён в `journal-ru.md`.

## Current Question

Папка стала общим публичным task-folder по проблемам Thaw, а не только по `stonerl/Thaw#591`. Нужно держать две верхнеуровневые темы: spacing/relaunch wave и visible/hidden drift.

## Current Hypothesis

PR #691 практически закрывает основной баг/UX-gap, который пользователь описывал:

- display-transition path теперь показывает just-in-time confirmation, если применение spacing действительно приведёт к relaunch wave;
- warning copy явно говорит про apps with menu bar items и возможную потерю unsaved input/progress/transient state;
- `Don't ask again + Cancel` blocker исправлен commit `e29074de`: suppression checkbox теперь отключает будущие confirmations только при `Apply`;
- пользователь дал approve после этого fix, затем maintainer approved и PR был merged.

## Current State

- Public demo repo: https://github.com/fleytman/thaw-problems
- Original issue #591: https://github.com/stonerl/Thaw/issues/591
- Пользовательский comment в #591: https://github.com/stonerl/Thaw/issues/591#issuecomment-4641519272
- Follow-up issue #685: https://github.com/stonerl/Thaw/issues/685
- Follow-up issue #686: https://github.com/stonerl/Thaw/issues/686
- Follow-up comment в #591 со ссылками на #685/#686: https://github.com/stonerl/Thaw/issues/591#issuecomment-4650305171
- #685 version info comment: https://github.com/stonerl/Thaw/issues/685#issuecomment-4651842003
- #686 label concern comment: https://github.com/stonerl/Thaw/issues/686#issuecomment-4651842442
- #685 clarification: https://github.com/stonerl/Thaw/issues/685#issuecomment-4657170271
- #686 clarification: https://github.com/stonerl/Thaw/issues/686#issuecomment-4657170568
- PR #691: https://github.com/stonerl/Thaw/pull/691
- Review line comment на `Don't ask again`: https://github.com/stonerl/Thaw/pull/691#discussion_r3387835636
- Fix commit: `e29074deadaf024082ff0a61ed281e19bb324f0a`
- `fleytman` review state: `APPROVED`, submitted at `2026-06-10T13:05:36Z`.
- `stonerl` review state: `APPROVED`, submitted at `2026-06-10T17:54:05Z`.
- PR #691 state на последней проверке: `MERGED`, updated at `2026-06-10T17:55:26Z`.
- Later #591 comment from `j-peeters`: https://github.com/stonerl/Thaw/issues/591#issuecomment-4679212411
- `j-peeters` reports relaunches after wake-from-sleep on `2.0b15` with spacing settings the same on all displays.
- Search по visible/hidden category drift сохранён: `evidence/related-issues/visible-hidden-search-2026-06-19.md`
- Уточнение пользователя: интересует не app-specific case, где одно конкретное приложение стабильно уходит в hidden, а случайное исчезновение visible item, который потом находится в hidden в настройках.
- Лучший текущий related issue под этот кейс: #702 https://github.com/stonerl/Thaw/issues/702
- Отдельный документ по #702: `evidence/related-issues/issue-702-random-visible-hidden-drift.md`
- ОБНОВЛЕНО 25.06.2026: #702 теперь `CLOSED` (23.06.2026 21:44Z). Закрыт через PR #743 `fix(menubar): defer saved-layout apply/persist on unsettled or cross-display geometry and clear stuck profile flag`, `MERGED` 24.06.2026 07:39Z, явно `Closes #702`.
- Fix НЕ в релизе: тег `2.0.0-rc.1` (`90068377`, 16.06.2026) предшествует fix (`4c04159e`); maintainer просит до RC2 использовать последний DMG из #702.
- Соседние fixes семейства: #742 (keep visible control item during notch overflow, merged 24.06), #698 (don't re-sort saved layout on active-display switch, merged 10.06). Открытый PR #716 — для смежного #714 (hidden items briefly appear).
- Уточнённая гипотеза пользователя 25.06: его кейс проявляется после wake-from-sleep, поэтому visible/hidden drift и spacing relaunch wave могут делить общий триггер (multi-display reattach при пробуждении). Подтверждено на уровне кода: см. `evidence/code/visible-hidden-mechanism-and-ice-comparison.md`.
- User stance по #702: не тестировать кастомные maintainer DMG; ждать RC2/beta с #743 и проверять на своём setup сценарий wake-from-sleep с двумя дисплеями.
- Примеры visible -> hidden, привязанные к конкретным приложениям: #651 Little Snitch, #605 CodexBar, #480 Toggl, #607/#707 OneDrive. Они полезны как фон, но хуже подходят как primary reference для случайного drift.

## Root Entry Points

- Spacing/relaunch wave overview: `spacing-relaunch-wave.md`
- Visible/hidden drift overview: `visible-hidden-drift.md`
- English README: `README.md`
- Original Russian README: `README-ru.md`
- English tracker: `tracker.md`
- Original Russian tracker: `tracker-ru.md`
- English journal/digest: `journal.md`
- Original Russian journal: `journal-ru.md`
- Public demo debrief: `debrief/session-debrief.md`
- Extracted Thaw-specific lessons: `debrief/lessons.md`

## Key Evidence

- Repo cloned locally for code inspection.
- Initial inspected branch/head before PR work: `development`, HEAD `6a1e3dc4`.
- PR head before final fix: `08d8027fc5d03aac18aedd6d957c52fea5e45da7`.
- Final PR head/fix commit: `e29074deadaf024082ff0a61ed281e19bb324f0a`.
- Code inspection note: `evidence/code/display-spacing-inspection.md`.
- Commit #670 analysis: `evidence/code/commit-e6227bbe-warning-analysis.md`.
- Maintainer response snapshots:
  - `evidence/github/issue-685-maintainer-response-2026-06-09.md`
  - `evidence/github/issue-686-maintainer-response-2026-06-09.md`
- Maintainer response analysis: `analysis/nightah-response-analysis-2026-06-09.md`
- Miscommunication analysis: `analysis/miscommunication-analysis-2026-06-09.md`
- PR #691 summary: `evidence/github/pr-691-summary-2026-06-10.md`
- PR #691 analysis: `evidence/code/pr-691-analysis-2026-06-10.md`
- Review comment draft/published text: `posted/pr-691-review-comment-dont-ask-again.md`
- Wake-from-sleep equal-spacing comment: `evidence/github/issue-591-comment-4679212411.md`
- Поиск по visible/hidden issues: `evidence/related-issues/visible-hidden-search-2026-06-19.md`
- Отдельный документ по #702: `evidence/related-issues/issue-702-random-visible-hidden-drift.md`
- Debrief по artifact-discipline ошибке: `../../session-debriefs/2026-06-10-thaw-issue-591-research-artifact-discipline-debrief.md`
- Full Thaw public-demo debrief: `debrief/session-debrief.md`
- Thaw-specific extracted lessons/statistics: `debrief/lessons.md`

## What Changed In Understanding

- Не надо утверждать, что проблема просто в “новом мониторе без настроек”. Более точная root condition: mismatch между текущим active/on-disk spacing и global/per-display spacing, который Thaw применяет при display connection/switching.
- Per-display slider movement в проверенном коде draft-only до `Apply`/`Reset`; не нужно писать, что любое движение per-display slider immediately persists settings.
- Global spacing slider persisted `globalConfiguration` immediately, and newly detected displays are seeded from that global template.
- Main safety gap был не только в warning text, а в delayed/automatic apply: пользователь мог заранее изменить setting, а destructive effect проявлялся позже при display transition.
- PR #691 добавил именно нужный JIT confirmation path для automatic display-transition relaunch.
- Опасный edge `Don't ask again + Cancel` был найден в review и исправлен commit `e29074de`.
- Одинаковый spacing больше нельзя подавать как универсальную гарантию: позже другой пользователь сообщил о relaunch after wake-from-sleep на `2.0b15` даже при одинаковом spacing на всех дисплеях.
- Visible/hidden drift выглядит отдельным, но соседним семейством багов. Вероятно есть минимум две ветки: corruption состояния при multi-display/menu-bar relocation (#702/#675) и нестабильная идентификация app/status-item (#707/#651/#605/#480).
- Для пользовательского кейса “случайное visible приложение пропало и нашлось в hidden” #702 релевантнее app-specific issues.
- ОБНОВЛЕНО 25.06.2026: visible/hidden drift — Thaw-специфика. Thaw добавил подсистему авто-персиста/восстановления layout (`LayoutSolver.swift`, `LayoutReconciler.swift`, `PendingLedger.swift`, `savedSectionOrder`, `applySavedLayout`/`saveSectionOrder`), которой в оригинальном Ice (HEAD `f063ee7`, 05.07.2024) нет вообще. Поэтому “в Ice такого не было” обоснованно: класс регрессии живёт в новом коде.
- Механизм: при multi-display relocation / wake / display reconnect macOS временно un-homes off-screen hidden items; если cache tick попадает в transient, либо persist запекает раскрытые items как visible, либо bulk apply не сходится на двух дисплеях и испорченное состояние сохраняется. Так visible item оказывается в hidden и закрепляется.
- PR #743 загейтил оба пути (apply и persist) через `LayoutSolver.itemsSpanMultipleDisplays` и `LayoutSolver.isMenuBarGeometryReady`, плюс починил залипший флаг `isApplyingProfileLayout` (`concludeProfileApplyWithoutMoves`).
- Связь с #591: wake-from-sleep с двумя дисплеями — общий триггер-environment для spacing relaunch wave и visible/hidden drift, хотя это разные code paths.

## What Contradicts The Current Model

- PR merge не равен проверке installed build: поведение ещё надо проверить в реальной beta/release сборке Thaw.
- #591 comment `4679212411` suggests `2.0b15` may still relaunch after wake from sleep even with equal spacing; this may be pre-PR #691 behavior or a distinct wake/sleep path.
- Minor UX concern про global overwrite confirmation bypass не признан blocker'ом; пользователь оценил его как некритичный и требующий фактического теста.
- `Don't ask again` label всё ещё не максимально явный, но после fix commit это уже minor wording concern, а не safety blocker.

## Open Questions

- В какой версии Thaw PR #691 попадёт к пользователям и будет ли пользователь тестировать эту сборку на своём setup.
- Нужно ли отвечать в #591 на comment `4679212411`, или лучше дождаться версии с PR #691 и попросить проверить именно её.
- Как реально поведёт себя global `Apply to All Displays` при отключённых confirmations; это стоит проверять hands-on, но сейчас не блокирует merge.
- Если у пользователя проявится “случайное visible приложение ушло в hidden”, лучше сначала сравнить с #702 (закрыт через #743). #707/#651/#605/#480 использовать только если симптом стабильно привязан к конкретному приложению.
- #702 закрыт и fix влит в `development`, но не в релиз: открытый вопрос — в какой версии (RC2/beta) #743 дойдёт до пользователя и воспроизведётся ли симптом на сборке с fix.
- Нужно ли после выхода RC2/beta с #743 проверить wake-from-sleep с двумя дисплеями на своём setup и, если повторится, собрать логи именно с этой сборки для #702.
- Нужно ли после релиза добавить короткий follow-up comment в #685/#686 с фактической верификацией поведения на installed build.

## Next Step

Практический next step:
- Visible/hidden drift (#702): дождаться RC2 / beta с PR #743 (+#742), затем проверить на своём setup wake-from-sleep с двумя дисплеями и manual display connect/disconnect. Если симптом повторится на сборке с fix — собрать логи именно с неё и сравнить с #702, не открывать новый report заранее. Кастомные DMG сейчас не тестировать.
- Spacing relaunch wave (#591/#691): дождаться сборки с #691, протестировать display connect/disconnect, wake-from-sleep и manual Apply.
- Запланированная перепроверка ~07.07.2026: статус #702, выход RC2/релиза и наличие тега с #743. Последняя проверка 30.06.2026 — изменений нет, RC2 не вышел.

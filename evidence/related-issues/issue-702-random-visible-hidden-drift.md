# Issue #702: случайный дрейф visible/hidden layout

Дата: 2026-06-19

Issue: https://github.com/stonerl/Thaw/issues/702

Title: `[Bug] The menu bar items change position on their own.`

Состояние на последней проверке: `CLOSED` (23.06.2026 21:44Z)

> Обновление 25.06.2026: issue закрыт. Закрыт через PR #743 `fix(menubar): defer saved-layout apply/persist on unsettled or cross-display geometry and clear stuck profile flag` (`MERGED` 24.06.2026, явно `Closes #702`). Fix в `development` (`4c04159e`), но НЕ в релизе: тег `2.0.0-rc.1` предшествует fix, maintainer просит до RC2 пользоваться последним DMG из issue. Разбор кода и сравнение с Ice: `../code/visible-hidden-mechanism-and-ice-comparison.md`.

Метки: `bug`, `P2`, `macos-26`

## Почему считаем похожим

Пользовательский симптом:

- внезапно какое-то visible приложение пропадает из menu bar;
- позже оно находится в hidden в настройках Thaw;
- это выглядит не как постоянная проблема одного конкретного приложения, а как случайный дрейф состояния layout/category.

#702 похож именно этим: это не issue про одно конкретное приложение, а общий дрейф positions/categories в multi-display setup. В body reporter показывает “morning setup” и “afternoon setup”; MeetingBar оказался в Hidden section, а Thaw icon moved left.

## Что происходило в #702

Исходный report:

- app version: test DMG из #675;
- macOS: `26.5.1 (25F80)`;
- display setup: multiple monitors;
- steps: run Thaw, set up layout, do daily routine;
- expected: icons stay where user put them.

Первый maintainer response:

- MeetingBar moved to hidden считается expected notch overflow, когда menu bar moves to a notched display.
- Thaw icon rearrangement считается bug.
- Maintainer linked first DMG fix and попросил reporter monitor normal use.

Дальше reporter сначала написал, что день прошёл нормально, но затем сообщил более сильный category drift:

- many icons moved from hidden to visible;
- later it happened again with hidden section;
- затем были дополнительные reports around Thaw icon moving again.

## Root-cause model от maintainer

Ключевая диагностика maintainer по логам:

- включена настройка `Apple System Settings > Desktop & Dock > Displays have separate Spaces`;
- menu bar relocation from display 2 to display 1 был in progress;
- macOS un-homes off-screen hidden items during status item window migration;
- unrelated windowID change, specifically `redshieldvpn` icon flapping, запустил item relocation while items were split across displays;
- Thaw tried a corrective re-hide, but moves resolved against two different displays;
- next cache tick persisted the failed state into saved layout, making corruption permanent.

Важные пункты timeline:

- до события cache был корректным: counts visible/hidden/alwaysHidden выглядели ожидаемо;
- затем сработало `applySavedLayout: dispatching bulk apply (windowID change)`;
- apply прочитал current visible count уже после того, как items had already been un-hidden;
- re-hide moves could not converge across displays;
- final persist wrote corrupted visible/hidden state.

Локальная интерпретация: это именно тот тип механизма, при котором пользователь может позже обнаружить, что item оказался в hidden, хотя он не перемещал его туда намеренно.

## DMG fixes / test builds

Maintainer posted several DMG links in the thread:

1. Первый DMG после признания Thaw icon rearrangement bug:
   - comment: https://github.com/stonerl/Thaw/issues/702#issuecomment-4691552083
   - artifact: `actions/runs/27417354511/artifacts/7592306523`

2. Второй DMG после подробной диагностики hidden -> visible corruption:
   - comment: https://github.com/stonerl/Thaw/issues/702#issuecomment-4714141504
   - artifact: `actions/runs/27588301407/artifacts/7655436979`

3. Третий DMG после того, как в reproduction logs появились profiles:
   - comment: https://github.com/stonerl/Thaw/issues/702#issuecomment-4724651872
   - artifact: `actions/runs/27655863815/artifacts/7682605983`

На последней проверке maintainers хотят логи только с exact latest linked DMG. Logs from Homebrew beta / RC / other builds считаются noisy, потому эти builds могут не включать те же fixes.

## Текущий статус

Последняя релевантная позиция maintainers:

- specifically for #702, они могут действовать только по logs from the exact linked DMG;
- logs from Homebrew beta / RC / other builds might behave differently;
- если несколько дней не будет новых reports from that DMG, они consider it fixed and close the issue;
- если проблема повторится on that DMG, они продолжат investigation по этим logs.

Reporter then acknowledged that they accidentally updated from the DMG to RC1 and did not realize it.

Локальный статус:

- Issue still `OPEN`.
- Нет final merged PR/commit, который можно считать влитым fix по этому документу.
- Maintainers сейчас в режиме “test exact DMG and wait for reports”.

## Позиция пользователя

Пользователь не готов тестировать maintainer-provided DMG builds.

Локальное решение:

- Считать #702 likely related.
- Сейчас не комментировать и не тестировать custom DMG.
- Ждать, пока maintainers сами протестят DMG path, закроют/подтвердят #702 или вольют fix в beta/release.
- Если symptom повторится локально после версии, которая supposedly includes #702 fix, сначала сравнивать local behavior с #702, а не сразу открывать новый report.

## Возможная публичная формулировка, если понадобится позже

```md
This looks similar to #702. In my case it is not one specific app that consistently fails; a random visible item can disappear from the menu bar, and I later find it in the hidden section in Thaw settings.

I saw that you are testing a DMG fix there. I am not ready to test custom DMG builds right now, so I will wait until that fix is validated and lands in a beta/release build.
```

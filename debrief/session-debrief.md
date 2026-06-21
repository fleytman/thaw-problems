# Thaw Problems Public Demo Debrief

Дата: 2026-06-20
Репо: thaw-problems
Задача: Исследование проблем Thaw, публикация GitHub issues/comments, ревью PR #691 и подготовка sanitized public demo repo
Report Type: Public-demo debrief copy
Report Author: Codex
Narrative Mode: actor-labeled

## Ошибки участников

### Codex

- Codex дважды допустил language drift: внутренний PR analysis и затем visible/hidden search artifact наследовали английский язык GitHub sources, хотя working artifacts по этой задаче должны были быть на русском, кроме verbatim quotes, code/UI strings и GitHub-ready drafts.
- Codex смешал `journal.md` как append-only audit trail с current-state привычками: свежие записи сначала оказались не в хронологическом порядке.
- Codex обновил `journal.md`, но не синхронизировал `tracker.md` после review comment, fix commit, user approve, maintainer approve и merge PR #691.
- Codex в первом visible/hidden summary слишком широко смешал app-specific issues с пользовательским symptom: случайное visible приложение пропало и позже нашлось в hidden. После коррекции #702 стал primary related issue, а app-specific issues оставлены как фон.
- Codex при sanitization сделал self-referential leak: запись, объясняющая удаление приватного marker'а, сама повторяла удаляемую категорию слишком буквально. Пользователь поймал проблему до публикации.
- Codex сначала выбрал не ту public git identity для standalone repo; после пользовательской коррекции repo-local identity выставлена явно.
- Codex перед вторым commit не учёл глобальную git signing config; для standalone public demo repo пришлось локально отключить signing.
- Codex держал task-folder под issue-specific моделью дольше, чем задача оставалась только про #591. Пользователь уточнил, что Thaw task-folder уже общий по проблемам Thaw и в корне нужны два problem overview.

### Пользователь

Подтверждённых ошибок пользователя в этой сессии нет. Пользователь регулярно выступал reviewer/operator: уточнял тон публичных комментариев, ловил language/artifact discipline regressions, указывал на stale tracker, заметил sanitization self-reference и переопределил структуру под публичный demo repo.

## Ложные гипотезы

### Codex

- Если внешний источник и будущий GitHub comment на английском, внутренний analysis тоже можно писать на английском.
- Journal можно использовать как удобное место для latest context.
- Public sanitization note может назвать скрываемый marker, если он описывает, что удалено.
- Исходный issue-specific folder name остаётся приемлемым после расширения scope.

## Коррекции пользователя

- Вернуть внутренние artifacts на русский язык.
- Догнать stale `tracker.md`.
- Убрать self-referential sanitization leak.
- Завести standalone git repo и ignored private journal.
- Сделать repo-local git identity явно.
- Переструктурировать repo вокруг двух problem areas: spacing/relaunch wave и visible/hidden drift.

## Что сработало

- User review public text loop смягчил тон и сделал аргументацию точнее.
- Code inspection подтвердил ключевые тезисы по display spacing runtime path.
- Issues #685/#686 помогли привести к PR #691.
- Review comment по `Don't ask again + Cancel` нашёл реальный safety edge, который был исправлен.
- Privacy audit и staged scans не дали private markers попасть в public `HEAD`.
- Final structure стала читаемой: root problem overviews + role-based `drafts/`, `posted/`, `evidence/`, `analysis/`.

## Выводы

- Для public demo repo sanitize не только сами секреты, но и тексты о sanitization.
- Перед первым commit в publishable repo проверять repo-local identity, signing behavior и staged privacy scan.
- Язык внешнего источника не должен становиться языком внутреннего artifact.
- `journal.md` сохраняет ход работы; `tracker.md` сохраняет актуальную модель.
- Related issue search должен сначала сохранять границы пользовательского symptom.
- Когда task-folder выходит за рамки original issue, structure надо переименовать на problem-level.

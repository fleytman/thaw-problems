# Thaw Problem Notes (RU)

Created: 07.06.2026 09:35:24 +05
Updated: 21.06.2026 12:12:24 +05
Version: 4

## Language Note

Это оригинальная русская версия README, сохранённая как `README-ru.md`.

Для публичного demo основными entry points являются английские:

- `README.md`
- `tracker.md`
- `journal.md`

Русские рабочие оригиналы сохранены как:

- `README-ru.md`
- `tracker-ru.md`
- `journal-ru.md`

## Purpose

Публичный пример исследовательской работы по UX/safety проблемам Thaw: как из пользовательского симптома, issue thread, кода и maintainer responses были сформулированы follow-up issues, review comment и дальнейшая позиция.

Папка началась с `stonerl/Thaw#591`, но затем вышла за рамки одного issue. Сейчас тут две основные темы:

- `spacing-relaunch-wave.md` - display spacing mismatch может приводить к relaunch wave menu bar apps.
- `visible-hidden-drift.md` - menu bar item может пропасть из visible и оказаться в hidden / layout может дрейфовать.

## Reading Order

1. `spacing-relaunch-wave.md` - основная проблема #591/#685/#686/#691.
2. `visible-hidden-drift.md` - соседняя проблема random visible/hidden drift, связанная прежде всего с #702.
3. `tracker.md` - английский текущий статус, открытые вопросы и next step.
4. `tracker-ru.md` - оригинальная русская версия tracker.
5. `posted/` - что реально было опубликовано в GitHub.
6. `drafts/` - черновики issues/comments, включая RU review versions.
7. `evidence/` - GitHub snapshots, code inspection и related issue search.
8. `sources/` - public note про omitted private source packets и source-boundary policy.
9. `analysis/` - аргументация, разбор maintainer responses и UX framing.
10. `debrief/` - demo-копия debrief и extracted lessons; в настоящем HARE Trail эти слои хранятся отдельно.
11. `journal.md` - английский public-readable journal/digest.
12. `journal-ru.md` - полный оригинальный русский append-only ход работы.

## Structure

```text
.
├── README.md
├── README-ru.md
├── spacing-relaunch-wave.md
├── visible-hidden-drift.md
├── tracker.md
├── tracker-ru.md
├── journal.md
├── journal-ru.md
├── drafts/
│   ├── comments/
│   └── issues/
├── posted/
├── evidence/
│   ├── code/
│   ├── github/
│   └── related-issues/
├── sources/
├── analysis/
└── debrief/
```

## Public Links

- Original issue #591: https://github.com/stonerl/Thaw/issues/591
- User comment in #591: https://github.com/stonerl/Thaw/issues/591#issuecomment-4641519272
- Follow-up issue #685: https://github.com/stonerl/Thaw/issues/685
- Follow-up issue #686: https://github.com/stonerl/Thaw/issues/686
- PR #691: https://github.com/stonerl/Thaw/pull/691
- Related issue #702: https://github.com/stonerl/Thaw/issues/702

## Privacy Note

The public git tree is sanitized. Local credential references, private absolute paths, and private source-packet paths are kept only in ignored `.private/` notes.

The `debrief/` directory is intentionally colocated for this public demo. In the actual HARE Trail setup, canonical debriefs live in `session-debriefs/` and reusable lessons live in `LESSONS.md`.

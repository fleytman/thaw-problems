# Extracted Lessons

Этот файл — public-demo extract. В реальной раскладке HARE Trail reusable lessons поддерживаются в private `LESSONS.md`; здесь лежит только Thaw-specific копия, чтобы example был self-contained.

## Lessons

- **Source language is not artifact language.** GitHub issues, PRs и UI strings могут быть на английском, но internal research notes должны следовать рабочему языку пользователя, если пользователь не попросил иначе. Английский оставлять для verbatim quotes, code/UI strings и GitHub-ready drafts.
- **`journal.md` и `tracker.md` имеют разные контракты.** Journal сохраняет chronology and reasoning path; tracker хранит current state, decisions и next steps. Обновление одного не обновляет другой.
- **Sanitization notes тоже могут leak.** Не повторять literal value или private marker, который удаляется. Описывать категорию: credential reference, private local path, private username spelling.
- **Public repo setup требует отдельный preflight.** До первого commit проверить repo-local `user.name`, `user.email`, signing behavior, ignored private notes и staged privacy scan.
- **Во время related-issue search держать symptom boundary sharp.** Random visible -> hidden drift case не равен app-specific hidden-item issue, даже если оба живут в одном GitHub label space.
- **Когда scope меняется, rename the container.** Папка, которая началась как #591 work, стала general Thaw problem-notes repo; root navigation должен отражать текущую model, а не только historical starting point.
- **Public-facing critique выигрывает от user review.** Initial position была substantively strong, но слишком sharp. Добавление благодарности, narrowing claims и разделение behavior bugs от copy issues сделали discussion конструктивнее.

## Thaw-Specific Error Statistics

| Паттерн | Случаев | Где проявилось |
|---------|--------:|----------------|
| Internal artifact inherited English from GitHub/source context | 2 | PR #691 analysis; visible/hidden search artifact |
| `journal.md` treated as current-state dashboard instead of append-only audit trail | 1 | journal order correction on 2026-06-10 |
| `tracker.md` not synchronized after state-changing work | 1 | PR #691 review/fix/merge facts were missing until user asked |
| Related-issue search mixed symptom families too early | 1 | app-specific visible/hidden issues mixed with random drift before #702 was made primary |
| Sanitization note reintroduced the removed private marker/category | 1 | public-sanitization journal entry corrected after user review |
| Public repo identity/signing assumptions needed correction | 1 | repo-local identity and signing behavior had to be adjusted before publishing |
| Folder structure lagged behind expanded scope | 1 | issue-specific folder became general Thaw problem notes after user correction |

## Preventive Checklist For Similar Public Demo Repos

1. Define publishable root entry points before moving files.
2. Keep raw snapshots under `evidence/`, interpretation under `analysis/`, public drafts under `drafts/`, and actually posted text under `posted/`.
3. Add ignored private notes before recording hidden source maps.
4. Run staged privacy scans before every commit.
5. Check `git show -s --format='%an <%ae>' HEAD` after commit.
6. Do not copy global LESSONS wholesale into a public repo; extract only task-relevant lessons.

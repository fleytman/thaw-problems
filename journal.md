# Journal

This is the English public-readable journal for the Thaw problem notes.

The original work was conducted in Russian, the user's native language. The complete original journal is preserved as `journal-ru.md`. This English file is a translated digest of the main turns, decisions, corrections, and outcomes so the public demo can be read without the private HARE Trail context.

## 2026-06-07 - Initial #591 Review And Comment Draft

The task started with `stonerl/Thaw#591`: Thaw 2.0 beta could make menu bar icons disappear/reappear and relaunch several menu bar apps when an external monitor was plugged or unplugged.

The initial thread showed this model:

- maintainers explained that different spacing settings across displays can require a relaunch wave because `NSStatusItemSpacing` is effectively global and menu bar item spacing is fixed when apps start;
- the original reporter accepted a workaround: keep spacing equal across displays;
- the issue was closed;
- later comments suggested the broader UX/safety problem remained.

The first public position was: do not challenge the technical limitation directly; instead state the UX/safety problem. A routine display reconnect should not unexpectedly restart unrelated apps without clear warning and user control.

The user added personal impact: the relaunch wave caused loss of typed input in some apps. The user also framed this as an interface anti-pattern and a regression from Ice, because Thaw was adopted as a maintained/stabler fork.

## 2026-06-07 - Tone And Framing Adjustments

The first draft was too sharp. The user asked to:

- thank the maintainer for continuing work on the abandoned Ice project;
- soften the tone;
- explain that the equal-spacing workaround is not obvious and was effectively buried in a closed issue;
- replace wording that implied indirect harm, because the user experienced the lost input as direct harm.

The draft was updated in English for GitHub and in Russian for review.

## 2026-06-07 - Comment Posted And Maintainer Response

The user posted the initial comment:

https://github.com/stonerl/Thaw/issues/591#issuecomment-4641519272

The maintainer response said newer betas had already addressed the behavior with alerts/warnings, and that additional modes such as “respect global spacing only” should be a feature request.

The user’s position after that response:

- a warning alone does not resolve the underlying unsafe path;
- users may ignore or misunderstand the warning;
- default behavior should be consistent spacing across displays;
- per-display spacing should be visually and textually treated as dangerous/advanced;
- closing a practical bug before practical mitigation is philosophically wrong;
- asking for a feature request can be odd when the user experiences the behavior as a clear bug.

## 2026-06-07 - Commit #670 Analysis

The user linked commit `e6227bbe29f90eb6544feea356d005248db3e5e2`, which added a persistent warning about display spacing mismatch.

The analysis concluded:

- the warning was a useful UX improvement;
- it confirmed that cross-display spacing mismatch was a real display-change cost;
- it did not change defaults;
- it did not prevent relaunch waves;
- it did not add just-in-time confirmation during later display transitions;
- it did not clearly state the data-loss risk.

This became the basis for a follow-up argument: warning-only mitigation is insufficient for behavior that can terminate/relaunch unrelated apps and lose user state.

## 2026-06-08 - Code Inspection And More Precise Claims

The Thaw code was inspected locally. The key findings:

- per-display spacing rows staged values until `Apply`/`Reset`;
- global spacing persisted the global template immediately;
- newly detected displays were seeded from the global template;
- display changes could call the active-display spacing apply path;
- the runtime display-change path did not show just-in-time confirmation in the inspected code;
- the relaunch mechanism terminated and reopened affected menu bar apps.

This refined the user’s claim. The issue was not “every slider move persists and relaunches”. The more precise issue was:

- global/template settings can persist earlier;
- the visible destructive effect can happen later during a routine display transition;
- the user may not connect that later relaunch wave to the earlier setting;
- the right mitigation is just-in-time confirmation, not only static warning text.

## 2026-06-08 - Follow-up Issues #685 And #686

Two follow-up issues were prepared and posted:

- #685: runtime confirmation for display connect/disconnect/switch relaunch waves;
- #686: stronger warning copy that mentions repeated display events, each affected app, and possible loss of unsaved input/progress/state.

The follow-up comment in #591 linked both issues and explained that this was done as requested by the maintainer, while still arguing that closing the original issue as resolved was risky.

The user later provided version details for #685:

- Thaw `Version 2.0.0-beta.14 (45)`;
- macOS `26.4 (25E246)`.

The user also noted that #686 being labeled `enhancement` seemed wrong, because the warning/copy work was part of mitigating the current bug behavior.

## 2026-06-09 - Maintainer Response Analysis And Clarifications

Maintainer responses to #685/#686 were saved and analyzed.

The objective read:

- maintainers were right that relaunches are conditional, not guaranteed on every display event;
- wording should not imply every display change always relaunches apps;
- the “user opted in” argument was incomplete, because opting into visual spacing does not necessarily mean informed consent to later unrelated app restarts and possible data loss;
- the strongest constructive point was maintainer openness to a JIT warning / bypass for display changes.

The user clarified the desired message:

- `each time` should mean each later event that actually requires spacing reconciliation, not every event unconditionally;
- the core problem is later auto-apply/relaunch without a call to action at that moment;
- warning for delayed risk is only redundant if a strong JIT warning exists.

Clarification comments were drafted and posted to #685/#686.

## 2026-06-10 - PR #691 Review

PR #691 was opened by the maintainer and linked to #685/#686.

The PR substantially addressed the main concerns:

- it added JIT confirmation during display-transition relaunch paths;
- it added a no-op guard before prompting;
- cancel/postpone behavior kept current spacing and allowed future prompts;
- warning copy was closer to the requested wording;
- an advanced bypass existed.

One real safety edge was found: `Don't ask again + Cancel` could suppress future prompts even though the user had not accepted the relaunch. A review comment was posted on that line.

Commit `e29074deadaf024082ff0a61ed281e19bb324f0a` fixed that problem. After the fix, the user approved the PR; the maintainer also approved and merged it.

## 2026-06-10 - First Artifact-Discipline Debrief

The user noticed two process problems:

- `journal.md` was not kept as a chronological append-only research log;
- an internal PR analysis artifact was written in English because the GitHub context was English.

The correction:

- `journal.md` was reordered chronologically;
- the internal PR analysis was translated to Russian;
- a debrief was created in the canonical HARE Trail `session-debriefs/` layer;
- reusable lessons were added: research journal is append-only, and source language is not artifact language.

## 2026-06-19 - Tracker Catch-up

The user asked whether the tracker had been forgotten.

It had. `tracker.md` still reflected an old state and missed:

- review line comment;
- fix commit `e29074de`;
- user approval;
- maintainer approval;
- PR merge.

The tracker was updated with the final PR state and open questions.

## 2026-06-19 - Visible/Hidden Drift Search

The user pointed to a later #591 comment about relaunches after wake-from-sleep and asked to search Thaw issues for cases where an app moved from visible to hidden.

The first search pass overmixed app-specific issues with the user’s real symptom. The user clarified that the interesting case was not a single app consistently moving to hidden, but a random visible item disappearing and later being found in hidden settings.

The search artifact was rewritten in Russian and refocused:

- #702 became the primary related issue for random multi-display category/layout drift;
- #675 remained a broader/historical candidate;
- #707/#651/#605/#480/#607 remained useful only as app-specific background.

The user chose not to test maintainer custom DMGs for #702 and preferred to wait for maintainers to validate or merge the fix.

## 2026-06-19 - Public Sanitization

The user wanted to publish the folder as an example of work and asked for a privacy audit.

The initial audit found no explicit work/company markers or raw secrets in the public content, but did find:

- a credential reference in the journal;
- private absolute local paths;
- local username spelling that should not appear publicly;
- `.DS_Store` files.

The folder was sanitized:

- credential references were replaced with `[removed for public publication]`-style placeholders;
- private absolute paths were replaced with public-safe placeholders;
- a private ignored journal was created under `.private/` to preserve the hidden source map locally;
- a standalone git repo was initialized.

The user caught a self-referential sanitization issue: a journal line describing what was removed itself repeated the kind of private marker being removed. That was fixed.

## 2026-06-19 - Standalone Public Repo Setup

The folder became its own git repo.

Important corrections:

- repo-local identity was set to the user-requested public identity;
- `.private/` was ignored;
- public staged content was scanned for privacy and secret patterns;
- global git signing via a private signing integration caused one commit attempt to fail, so signing was disabled locally for this public demo repo.

The first public commit created a sanitized repo; the second commit restructured it.

## 2026-06-19 - General Thaw Problem Notes

The user clarified that the folder was no longer only about #591. It should be a general Thaw problem-notes repo with two root problem files:

- spacing / relaunch wave;
- visible / hidden drift.

The structure became:

- root problem overviews;
- `drafts/` for prepared issue/comment text;
- `posted/` for actually published comments;
- `evidence/` for GitHub snapshots, code inspection and related issue search;
- `analysis/` for interpretation and argument maps;
- `debrief/` for a public-demo copy of debrief/lessons.

Issue draft filenames were also renamed from local `issue-1/issue-2` names to real #685/#686 filenames.

## 2026-06-20 - Debrief Demo Layer

The user asked for a session debrief and for a public-demo copy of the debrief and extracted lessons to be colocated in the repo.

The canonical HARE Trail layers were updated privately:

- `session-debriefs/2026-06-20-thaw-problems-public-demo-debrief.md`;
- `LESSONS.md`.

The public repo received:

- `debrief/session-debrief.md`;
- `debrief/lessons.md`;
- `debrief/README.md`.

The demo notes explicitly state that real HARE Trail stores these layers separately, and that this repo colocates them only to make the public example self-contained.

## 2026-06-21 - English Entry Points

The user asked to make the main entry files English while preserving the Russian originals with `-ru` suffixes.

Implemented structure:

- `README.md`, `tracker.md`, `journal.md` become English public entry points;
- `README-ru.md`, `tracker-ru.md`, `journal-ru.md` preserve the original Russian working versions;
- `README.md` explains that the work was originally conducted in the user’s native language and translated later to demonstrate the workflow.

## 2026-06-21 - Public Repository Published

The user asked to publish the sanitized demo repository after one more privacy check.

Checks before publishing:

- current public tree scan found no credential markers, private absolute local paths, old local username spelling, work-project markers, or common secret patterns;
- full git history scan found no matches for the same privacy and secret patterns;
- `.private/` remained ignored and untracked, preserving local-only source notes outside the public git tree;
- no `.DS_Store` files were present.

Result: the repository was published as `https://github.com/fleytman/thaw-problems` with `main` tracking `origin/main`.

# Clarification Comment for #686 RU

Спасибо, это звучит как хороший direction for the wording changes.

Behavior-level / JIT-warning concern я уточнил в #685. Для этого copy issue я хочу только сузить suggested wording, чтобы он не звучал так, будто every display event always relaunches apps.

Более точный вариант:

> When a display transition requires Thaw to apply a different menu bar spacing, Thaw relaunches apps with menu bar items. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

Или, если хочется оставить акцент на repeated nature:

> Each time a display transition requires Thaw to apply a different menu bar spacing, Thaw may relaunch apps with menu bar items. Relaunching apps may cause unsaved input, progress, or transient app state to be lost.

По non-active displays: я согласен, что отдельный blocking warning на каждый non-active edit может быть noisy, если в #685 появится JIT warning в момент реального риска. Но без JIT warning delayed-risk copy становится важнее, потому что иначе destructive effect может случиться позже without any prompt и без понятной связи с более ранней настройкой.

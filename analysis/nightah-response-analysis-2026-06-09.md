# Analysis: nightah Responses to #685 and #686

Created: 2026-06-09

## Short Read

The maintainer/collaborator response is partly technically correct and partly misses the UX/safety concern.

He is right that:

- Not every display connect/disconnect/switch necessarily triggers a relaunch wave.
- A relaunch only matters when Thaw needs to apply a different spacing for the display hosting the menu bar.
- Automated per-display spacing is hard to provide without automatically changing the single system-wide `NSStatusItemSpacing`.
- Some warning text suggestions should be conditional rather than absolute.
- Stronger wording for Apply/global confirmation is acceptable and likely a good path.

I do not fully agree that:

- This is sufficiently covered by “user opt-in”.
- Existing wording is already clear enough.
- The destructive risk only comes from the “Apply all” broadcast path.
- Future-risk warnings are necessarily “extra noise” or “unnecessary UX-gating”.

## Argument Map

### 1. “If a user wants different spacing for different displays, this is really the only way to provide that automatically.”

This is technically plausible. Since macOS exposes `NSStatusItemSpacing` as effectively system-wide, automated per-display spacing requires Thaw to rewrite the system value when the menu-bar-hosting display changes.

Where this does not settle the issue: technical necessity does not remove the need for explicit consent around destructive side effects. The question is not only “can this be implemented another way?”; it is “should the default UI make this behavior safe and predictable for users?”

### 2. “Settings only really take effect if the display is where the menubar is hosted.”

This is correct and important. A display having a different saved spacing value does not automatically mean a relaunch wave happens. The relaunch wave occurs only when Thaw applies a spacing value that differs from the currently applied system spacing, and that is tied to the active/menu-bar-hosting display.

This means our public wording should avoid implying that every display event always relaunches apps. Better wording:

> If connecting, disconnecting, or switching displays makes Thaw apply a different menu bar spacing, Thaw relaunches apps with menu bar items.

That keeps the warning accurate without weakening the core concern.

### 3. “Existing wording is not unclear; maybe user interpretation.”

I only partly agree. The current wording may be technically parseable, but user interpretation is part of the product surface. If a reasonable user reads “Connecting a display whose spacing differs...” and still does not understand that ordinary later display changes can terminate/reopen unrelated apps and lose unsaved state, the wording is not doing enough work.

Also, the warning does not explicitly say:

- this may recur on later display transitions;
- it affects apps with menu bar items, not only Thaw;
- apps may be terminated/reopened;
- unsaved input/progress/transient state may be lost;
- the user may see the effect later than the configuration edit.

So the maintainer is right that ambiguity can be solved through wording, but I disagree with framing it mainly as user interpretation.

### 4. “Global template exists to seed new connected displays; destructive nature only comes from Apply all broadcast.”

The first part is true: the global template was created to seed new displays, and that is an intentional feature.

The second part looks too narrow based on the code. The global slider writes to `displaySettings.globalConfiguration` immediately. Newly detected displays can be seeded from that template. When a seeded display later hosts the menu bar, `applyActiveDisplaySpacing` can apply its desired spacing; if on-disk spacing differs, `applyOffset()` can trigger the relaunch wave.

So the destructive effect is not only “Apply all” in the abstract. “Apply all” is one direct path. Another relevant path is: global template saved -> new display seeded -> that display becomes menu-bar-hosting display -> desired spacing differs -> relaunch wave.

The maintainer is right that this chain is conditional and should not be overstated. But it is still a plausible user-visible path, not merely scenario conflation.

### 5. “All of this is user opt-in.”

This is the weakest part of the maintainer argument.

A user opting into spacing customization does not necessarily mean the user gave informed consent to later, repeated, out-of-context app restarts during routine display changes. In UX/safety terms, consent should be specific enough for the destructive side effect.

The more precise framing:

- The user opts into changing a visual spacing preference.
- The user may not understand that this implies terminating/reopening unrelated apps later.
- The user may not connect the later relaunch wave to the earlier setting.

That is why stronger warnings and/or just-in-time confirmation remain justified.

### 6. “Non-active display delayed-risk warning is extra noise; persistent warning should cover it.”

This is a reasonable product-design tradeoff, but only if the persistent warning is strong, prominent, and precise enough.

I partially agree that warning every non-active display edit can become noisy. However, while no just-in-time warning exists for display changes, delayed-risk communication matters. A compromise is:

- Make the persistent warning much stronger and more explicit.
- Add a just-in-time warning when a display change would actually trigger relaunch.
- Then avoid per-row warning spam for every non-active edit.

So I agree with avoiding excessive UX gating, but not with dismissing delayed-risk communication entirely under the current behavior.

### 7. “There is definitely an argument for a JIT warning for display changes and opt-out/bypass.”

This is the most important constructive part of the response. It validates the core #685 ask.

If a JIT warning exists and can be bypassed by users who knowingly accept the destructive nature, that directly addresses the main UX/safety gap:

- users are warned at the moment of impact;
- users can postpone or keep current spacing if busy;
- advanced users can opt out if they knowingly prefer automation.

## Overall Position

I would not respond aggressively. The maintainer is engaging in good faith and accepts two important things:

- wording should be strengthened;
- JIT warning for display changes has a real argument.

But I would keep pushing on the core distinction:

> This is not about whether per-display spacing can be implemented without rewriting system spacing. It is about whether a visual spacing setting gives sufficiently informed consent for later, repeated relaunches of unrelated apps during routine display changes.

The best next response would likely be concise:

- acknowledge that not every display change triggers relaunch;
- agree wording should be conditional;
- clarify that “user opt-in” is not enough unless the destructive side effect is understood;
- say JIT warning/override is the practical resolution for #685;
- accept that per-row delayed warnings may be unnecessary if persistent warning + JIT warning are implemented.

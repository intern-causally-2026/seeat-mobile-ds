# SeEat — Mobile Design System

The visual system of the SeEat iOS app, written down from the app itself.

Built August 2026. Every value was read from source in `origin/integration` and then verified against screenshots of a build of that same commit running on an iPhone 17 Pro Max simulator.

## Why this exists separately

An earlier SeEat design system was written from a brand brief before the app existed. It described a warm cream-and-cocoa palette, pill-shaped buttons, borderless cards and a single "sacred" orange. The product had moved a long way from all of that. Rather than patch it, this one starts from the running app.

## Provenance

| Source | What it gives |
|---|---|
| `Theme/AppColor.swift` | colour, light + dark |
| `Theme/AppFont.swift` | families, sizes, the 13 Figma text styles |
| `Theme/{Radius,Shadow,Spacing}.swift` | radii, elevation, spacing |
| `PrimaryCTAFill.swift`, `SecondaryCTAFill.swift` | button surfaces |
| `SeEatField.swift` | form fields |
| `Features/Menu/MenuTag.swift`, `MenuGridCard.swift` | tag chips |

**If a value here disagrees with those files, those files win.** Nothing keeps them in sync automatically.

Where the running app disagreed with the source, the app won and the card says so. Two cases so far: the page ground is `muted` rather than `background`, and the CTA gradient does not change between themes.

## The five things most likely to be got wrong

**There is no single brand orange. There are three, and contrast picks them.** `primary #FF7B23` is for fills and icons only — at 2.6:1 on white it fails AA for text. `primaryButton #EC6A1C` carries white labels. `primaryText #C2410C` is for orange text. A fourth pair, `#F56B0F → #F16000`, is the CTA gradient and matches none of them.

**The CTA is a gradient surface, not a fill.** Orange gradient, white inner highlight along the top edge, warm outer glow — all three in `seEatPrimaryCTAFill`. It is identical in light and dark. A dark mockup that darkens the CTA is wrong.

**Tag chips are neutral until they concern the diner.** Category drives only the icon tint. Fill, border and label stay warm neutral until a tag conflicts with the user's profile, at which point the chip takes the caution palette. Red means "clashes with your profile", not "contains allergens".

**Chips are radius 12, not pills.** So are buttons. Only a non-prominent CTA becomes a pill.

**Avenir can never be bundled.** It is an Apple system font: free on iOS, and Apple's licence forbids redistribution. Web substitutes Plus Jakarta Sans with Inter as fallback. A "missing brand font" warning is permanent, not a task.

## Structure

```
tokens.css              every token, light + dark
preview/                the cards
reference/              app screenshots, light + dark
```

Dark is opt-in via `.theme-dark`. It is deliberately not wired to `[data-theme]` or `prefers-color-scheme` — the Design System pane sets its own theme attribute on the root, and a host-driven selector silently flips every card.

## Reproducing a screenshot

```
xcrun simctl launch <udid> xyz.causally.seeat.ios -debugScene item-detail -debugTheme dark
```

28 scenes, 2 themes, 7 auth states, 3 locales — see `Debug/DebugLaunchConfiguration.swift`. Those canonical flags are recent. A stale simulator build ignores them silently and hands back identical screenshots, so check the build date before trusting a capture.

## Known gaps

1. **Not every component is covered yet.** Buttons, tag chips, form fields, and the foundations are. Not yet: navigation and tab bar, menu and dish cards, sheets, the paywall, loading and error states, iconography, and the mascot.
2. **Two accessibility weak points, both worth fixing at source.** `SeEatField` hardcodes `#757575` for its label rather than using a token — 4.6:1 on white, only just over the line. And `textTertiary` sits exactly at the AA floor.
3. **iOS and web have drifted.** `AppColor.swift` says it mirrors `designSystem.ts`, but `textPrimary` is `#0F172A` on iOS and `#1E293B` on web; `textSecondary`, dark `border` and dark `shadowSoft` differ too.
4. **Voice, iconography and the mascot are not covered here.** The older system documents them from the brand brief; none of it has been checked against the product.

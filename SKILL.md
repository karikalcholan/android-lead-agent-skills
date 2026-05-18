---
name: android
description: |
  Expert Android engineering for Jetpack Compose, Material 3, shared element transitions,
  and extraordinary UI quality. Load this skill for ANY Android or Compose task — writing
  a new screen, animating a navigation transition, building a design system, scaffolding
  a feature module, debugging with ADB or Android MCP, optimising a ViewModel, setting up
  Hilt, Room, Retrofit, or screenshot testing.
  
  This skill enforces a non-negotiable aesthetic bar: every screen it helps produce must
  look like it belongs in a design award showcase. Default-styled, grey-background,
  animation-free UIs are a failure mode. Beauty is a deliverable.
  
  Trigger on: any Android/Kotlin/Compose task, any mention of navigation transitions or
  animations, any mention of beautiful UI or design system, feature scaffolding, ADB
  debugging, MCP device interaction, architecture questions (MVVM, MVI, ViewModel, Flow),
  or Gradle/module structure work. If in doubt, load it.
compatibility:
  tools: [android_mcp, bash_tool, create_file, str_replace, view]
  compose_bom: "2024.09.00+"
  min_sdk: 21
  target_sdk: 35
---

# Android Development Skill

This skill exists because Android engineering has two distinct failure modes: code that doesn't work, and code that works but produces UIs no one wants to use. Most guidance addresses the first. This skill addresses both, treating visual quality as a first-class engineering concern on par with correctness, performance, and maintainability.

Use this skill to build production-grade Android applications with Jetpack Compose, Material 3, shared element transitions, and a design system that produces screens a designer would be proud of — not just screens that pass code review.

---

## ⚡ Quick Decision Guide

| What you're doing | Where to start |
|---|---|
| Building a new screen | [`references/compose-ui-system.md`](references/compose-ui-system.md) |
| Adding navigation with shared element transitions | [`references/shared-element-transitions.md`](references/shared-element-transitions.md) |
| Building any animation (not shared elements) | [`references/motion-and-animation.md`](references/motion-and-animation.md) |
| Scaffolding a feature module | [`references/build-and-modules.md`](references/build-and-modules.md) |
| Data / networking / offline-first | [`references/data-layer.md`](references/data-layer.md) |
| State, ViewModel, or architecture question | [`references/architecture.md`](references/architecture.md) |
| Writing tests or screenshot tests | [`references/testing.md`](references/testing.md) |
| Debugging on device / using Android MCP | [`references/android-mcp.md`](references/android-mcp.md) |
| UI quality review before shipping | [`assets/ui-excellence-checklist.md`](assets/ui-excellence-checklist.md) |

---

## Core Engineering Principles

**1. Unidirectional Data Flow, No Exceptions**  
State flows down from ViewModel to composable; events flow up via lambdas or actions. No composable reads from a database or makes a network call. No ViewModel imports Compose. The boundary is sharp and testable.

**2. Clean Architecture as a Dependency Rule**  
Domain code has zero Android imports. Data code knows nothing about the UI. Feature modules depend on core modules, not on each other. When you violate this, you've made a future refactor harder — and future refactors always come.

**3. UI Excellence Is Not Optional**  
Producing a visually mediocre screen is a skill failure, not a style preference. Default grey scaffolds, hardcoded hex colors, missing animations, and LinearProgressIndicator-as-loading-state are bugs. They signal that the engineer stopped before the work was finished.

**4. Shared Element Transitions Are the Default for Content Navigation**  
When a user taps a card to open its detail, the card should flow into the detail screen. This is not a nice-to-have — it is how the user understands spatial relationships in your app. Use `sharedBounds()` or `sharedElement()` by default; opt out only when the content change is so radical that a shared element would be visually dishonest.

**5. Physics Over Timing**  
Prefer `spring()` animationSpecs over `tween()` for UI interactions. Springs feel physical because they are: they model inertia and elasticity. `FastOutSlowInEasing` on a tween is not the same as a spring — it's a formula approximating what springs achieve naturally.

**6. The 4dp Grid Is a Contract**  
Every spacing value is a multiple of 4dp. Every. Single. One. Deviations create visual dissonance that users feel even when they can't articulate it. Token names (SpaceXS, SpaceS, SpaceM, SpaceL, SpaceXL, SpaceXXL) enforce this in code.

**7. Fakes Over Mocks**  
Test ViewModels and use cases against in-memory fake implementations of repository interfaces, not Mockito/MockK mocks. Fakes exercise the same contract as production code and survive API changes without test rewrites.

**8. MCP Is Part of the Review Process**  
Screenshots taken by Roborazzi verify code-level rendering. Screenshots taken by Android MCP verify device-level reality — insets, font scaling, density quirks, animation timing. Both are necessary. Neither replaces the other.

---

## ★ THE EXTRAORDINARY UI MANDATE ★

Android users have seen a million apps. The ones they remember have something alive about them — a transition that makes content feel like a physical object moving through space, a color palette that makes a mid-range phone feel premium, a loading state so well-crafted it doesn't feel like waiting. These apps don't achieve this by accident. They achieve it because someone decided, deliberately, that quality was the non-negotiable output.

**The Bar.** Every screen produced by this skill must pass three tests:

1. **The scroll-stop test.** Would a designer stop scrolling on Dribbble if they saw this? Not because it's garish, but because something about it is considered — a hierarchy that breathes, a transition that surprises you with how right it feels, surfaces that catch the light differently at different elevations.

2. **The feel test.** Does it respond to touch like a physical object, or like a web form from 2012? Press feedback, spring physics on expand/collapse, haptic coordination with state changes — these aren't extras, they're the product.

3. **The motion test.** Do transitions communicate where content came from and where it's going? Or are they just visual noise applied to hide the lack of spatial thinking? Shared elements communicate. Instant navigation doesn't.

**What This Means in Code:**

- Shared element transitions for every content navigation. Not sometimes. Not when there's time. Always.
- Spring physics for gesture-coupled and state-change animations. Tuned `dampingRatio` and `stiffness`, not defaults.
- Color from the Material 3 role system. `Brush` gradients where flat color would be visually flat. `primaryContainer` for highlights, not `primary.copy(alpha=0.1f)`.
- Typography that creates hierarchy you can *feel* — a display font for headlines, a humanist sans for body, tuned `lineHeight` and `letterSpacing` for each role.
- Skeleton screens that mirror the exact geometry of the real content. No spinners as the primary loading affordance.
- Edge-to-edge, always. The status bar area is part of the design. The nav bar area is part of the design.
- `Canvas` for anything standard components can't achieve. Gradient arcs, custom progress rings, morphing shapes.
- Haptic feedback (`HapticFeedbackType`) coordinated with meaningful state transitions.

**Anti-patterns to refuse, not apologise for:**

Default grey scaffold backgrounds. Unthemed components with hardcoded hex colors. Instant navigation with no transition. `LinearProgressIndicator` as the only loading state. Cards with identical elevation on every surface. White app bars with no personality. `Spacer(modifier = Modifier.height(8.dp))` anywhere a named token should be.

---

## Shared Element Transitions — Overview

`SharedTransitionLayout` creates a shared scope that coordinates geometry between two composable trees during navigation. `sharedElement()` matches identical content (same image, same icon, same text) between source and destination, interpolating position and size. `sharedBounds()` matches different composables that share a conceptual spatial region — a card expanding into a full screen, a FAB morphing into a surface — interpolating the container's bounds while the content changes.

Three things must be threaded from `SharedTransitionLayout` down to each shared element: the `SharedTransitionScope` (receiver for the modifier), the `AnimatedVisibilityScope` (per-composable enter/exit handle), and a stable key (the identity contract between source and destination). For shallow trees, pass these as explicit parameters; for deep trees, use `CompositionLocal`.

Full implementation guide, four complete code patterns, navigation integration, and six pitfall+fix entries are in [`references/shared-element-transitions.md`](references/shared-element-transitions.md). This is one of the most powerful tools available for extraordinary UI — use it.

Reference: https://developer.android.com/develop/ui/compose/animation/shared-elements

---

## Android MCP Integration

Android MCP gives you device eyes: screenshot capture, UI hierarchy dump, logcat streaming, shell commands, and APK installation — all from within the coding session. The mental model: MCP is your eyes on the device; code is your hands in the codebase. Use both together.

Build → install via MCP → screenshot → compare to design → fix → repeat. This loop, running on a real device or emulator, catches issues that Roborazzi (JVM-rendered) misses: inset handling, density edge cases, animation timing, and system bar styling. After any significant UI change, run the loop.

Full capability list, workflow scripts for screenshot review, log filtering, UI hierarchy inspection, animation debugging, and responsive testing (font scale, RTL, density, foldable) are in [`references/android-mcp.md`](references/android-mcp.md).

---

## Feature Build Workflow

1. **Create feature module** — `./gradlew createModule --name feature:X`; apply `yourapp.android.library` and `yourapp.android.library.compose` plugins. See `references/build-and-modules.md`.

2. **Define route** — Add `@Serializable data class XRoute(val id: Long)` to `:core:navigation`. Register in the NavHost.

3. **Model UI state** — Sealed interface with `Loading`, `Ready(data)`, `Error(message, canRetry)`. See `references/architecture.md`.

4. **Define actions** — Sealed interface covering every user interaction the screen handles.

5. **Build ViewModel** — `StateFlow` for state, `SharedFlow(replay=0)` for events, `SavedStateHandle.toRoute()` for arguments. See `references/architecture.md`.

6. **Define domain layer** — UseCase calling Repository interface. Repository implementation in `:core:data`. See `references/data-layer.md`.

7. **Build the screen composable** — Public function receives `sharedTransitionScope`, `animatedVisibilityScope`, and `onAction`. Internal sections are private. See `references/compose-ui-system.md`.

8. **Apply shared element transitions** — Identify which elements should persist spatially across navigation. Apply `sharedElement()` or `sharedBounds()` with stable ID-based keys. See `references/shared-element-transitions.md`.

9. **Apply motion** — Stagger list entry, animate state changes with spring physics, add enter/exit transitions. See `references/motion-and-animation.md`.

10. **Apply design system** — All colors from `MaterialTheme.colorScheme`, all spacing from `AppSpacing.*`, all typography from `MaterialTheme.typography`. Add gradient or texture where flat color would be weak. See `references/compose-ui-system.md`.

11. **Add skeleton loading** — Mirror the exact geometry of `Ready` state in a `Loading` composable using `ShimmerBox`. No spinners as primary loading affordance.

12. **Verify with MCP** — Build, install, screenshot in light/dark/large font/RTL. Check insets, shared element timing, press feedback. See `references/android-mcp.md`.

13. **Write tests** — ViewModel unit tests with `FakeRepository`. Compose UI test for key interactions. Roborazzi screenshot in light and dark. See `references/testing.md`.

14. **UI excellence review** — Run every item in [`assets/ui-excellence-checklist.md`](assets/ui-excellence-checklist.md). Fix every failing item before marking the feature done.

---

## Common Pitfalls

**1. Hardcoded colors in composables**  
*Symptom*: Dark mode is broken; a surface is always white.  
*Cause*: `Color(0xFFFFFFFF)` or `Color.White` used instead of `MaterialTheme.colorScheme.surface`.  
*Fix*: Every color reference goes through the Material 3 color role system. Create a `CompositionLocal` for any role Material 3 doesn't cover.

**2. Missing `Modifier` parameter**  
*Symptom*: Composable can't be padded or sized by its caller; preview clips content.  
*Cause*: `modifier: Modifier = Modifier` omitted from the function signature.  
*Fix*: Every public composable takes `modifier: Modifier = Modifier` as the first optional parameter, applied to its outermost layout element.

**3. Shared element key mismatch — no animation fires**  
*Symptom*: Navigation works but the element snaps instantly with no transition.  
*Cause*: The key in the source composable doesn't equal the key in the destination. Often a string template typo or using list index instead of entity ID.  
*Fix*: Build keys from stable entity IDs (`"image-${item.id}"`). Log both keys and compare. See `references/shared-element-transitions.md` → Pitfall 1.

**4. SharedTransitionLayout not wrapping the NavHost**  
*Symptom*: `IllegalStateException: No SharedTransitionScope found` at runtime.  
*Cause*: `SharedTransitionLayout` wraps only one screen composable instead of the NavHost that contains both source and destination.  
*Fix*: `SharedTransitionLayout { NavHost(...) { ... } }`. The layout must contain the entire region where elements will transition.

**5. Spinner as the only loading state**  
*Symptom*: The screen shows a `CircularProgressIndicator` centred on a grey background while content loads.  
*Cause*: Loading state was left at the first-pass implementation.  
*Fix*: Build a skeleton composable that mirrors the exact dimensions and layout of the `Ready` state using `ShimmerBox`. See `references/compose-ui-system.md` → Skeleton Shimmer.

**6. StateFlow vs SharedFlow confusion for events**  
*Symptom*: A toast or navigation event fires again after screen rotation or process restoration.  
*Cause*: One-time events stored in `StateFlow` (which replays the last value) instead of `SharedFlow(replay=0)`.  
*Fix*: Events = `MutableSharedFlow(replay=0)`. State = `MutableStateFlow(initialValue)`. Never mix the two.

**7. renderInSharedTransitionScope omitted — element blinks**  
*Symptom*: A back button or toolbar that only exists on the destination screen is invisible during the shared element transition and appears abruptly when it completes.  
*Cause*: The element is not participating in the shared overlay layer.  
*Fix*: Apply `.renderInSharedTransitionScope(sharedTransitionScope).animateEnterExit(enter = fadeIn(...))` to any element that must be visible during the transition but has no matching key on the source screen. See `references/shared-element-transitions.md` → Pitfall 4.

**8. No `clipInOverlayDuringTransition` on sharedBounds — visual artefacts**  
*Symptom*: The shared bounds clips incorrectly during the transition — wrong corners, content bleeds outside the card shape.  
*Cause*: `sharedBounds()` uses a rectangular clip by default, which doesn't match the rounded card shape.  
*Fix*: Pass `clipInOverlayDuringTransition = OverlayClip(cardShape)` matching the source shape. Avoid applying `Modifier.clip()` before `sharedBounds()`. See `references/shared-element-transitions.md` → Pitfall 6.

**9. Spacing values not on the 4dp grid**  
*Symptom*: Design review flags inconsistent padding; visual rhythm feels slightly off.  
*Cause*: Hardcoded `Modifier.padding(12.dp)` or `Modifier.padding(6.dp)` instead of named tokens.  
*Fix*: Replace every hardcoded spacing value with `AppSpacing.*`. If 12dp is needed, it's `AppSpacing.S + AppSpacing.XS` — or question whether the design needs it at all.

**10. Unit tests mock the repository instead of using a fake**  
*Symptom*: Tests pass locally but break when the repository interface changes; tests become brittle and hard to read.  
*Cause*: `Mockito.mock(ArticleRepository::class.java)` instead of `FakeArticleRepository`.  
*Fix*: Implement `FakeArticleRepository` in `:core:testing`. It takes ~15 minutes once and saves hours of test maintenance over the life of the feature. See `references/testing.md`.

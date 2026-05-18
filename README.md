# Android Lead Agent Skill

A production-grade skill for AI coding assistants (Claude Code, GitHub Copilot, Cursor) that encodes senior Android engineering expertise — architecture, Jetpack Compose, animations, security, performance, testing, and more.

This skill exists because Android engineering has two failure modes: code that doesn't compile, and code that compiles but produces apps no one wants to use. This skill prevents both.

---

## What This Skill Covers

| Domain | Reference File |
|--------|---------------|
| Architecture, ViewModel, DI | `references/architecture.md` |
| Jetpack Compose & Design System | `references/compose-ui-system.md` |
| Shared Element Transitions | `references/shared-element-transitions.md` |
| Motion & Animation | `references/motion-and-animation.md` |
| Navigation (type-safe, deep links, adaptive) | `references/navigation.md` |
| Adaptive Layouts & Large Screens | `references/adaptive-layouts.md` |
| Coroutines & Flow | `references/coroutines-and-flow.md` |
| Data Layer (Room, Retrofit, offline-first) | `references/data-layer.md` |
| Performance & Baseline Profiles | `references/performance.md` |
| Security | `references/security.md` |
| Background Work & WorkManager | `references/background-work.md` |
| Image Loading (Coil3) | `references/image-loading.md` |
| Push Notifications & FCM | `references/notifications.md` |
| Accessibility | `references/accessibility.md` |
| Testing (unit, screenshot, UI) | `references/testing.md` |
| Build, Modules & Gradle | `references/build-and-modules.md` |
| Android MCP & ADB Debugging | `references/android-mcp.md` |
| UI Excellence Quality Gate | `assets/ui-excellence-checklist.md` |

---

## Installation

### Option 1 — Project-local (recommended for teams)

```bash
# Clone into your project's .claude/skills directory
git clone https://github.com/ayush016/android-lead-agent-skills.git \
  .claude/skills/android
```

Add to your project's `CLAUDE.md`:
```markdown
## Skills
- android: .claude/skills/android/SKILL.md
```

### Option 2 — User-global (personal use across all projects)

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/ayush016/android-lead-agent-skills.git \
  ~/.claude/skills/android
```

### Option 3 — Manual copy

Download or copy the `SKILL.md` file and the `references/` and `assets/` directories into your project's `.claude/skills/android/` folder.

---

## How to Use

### Automatic triggering (Claude Code)

Once installed, Claude Code automatically loads this skill when it detects Android-related work:

- Any `.kt` file with Compose imports
- Mentions of ViewModel, Hilt, Room, Retrofit, Coroutines
- Navigation, animation, or transition questions
- Build configuration (Gradle, version catalog)
- ADB or device debugging
- Any "beautiful UI" or design system request

### Manual invocation

Reference the skill explicitly in your prompt:

```
Using the android skill, build a product detail screen with a shared element 
transition from the list thumbnail.
```

```
Using the android skill, set up Baseline Profiles for this app.
```

```
Using the android skill, review this screen against the UI excellence checklist.
```

### Quick decision guide

| What you're doing | Say |
|---|---|
| New screen | "Build [screen name] screen" |
| Add animation | "Add shared element transition from [A] to [B]" |
| Architecture question | "How should I structure [feature] using the android skill?" |
| Performance issue | "Profile and fix recomposition in [composable]" |
| Security | "Audit [feature] against the security checklist" |
| UI review | "Review this screen against the UI excellence checklist" |

---

## Skill Philosophy

**UI quality is a first-class engineering concern.** A screen that works but looks mediocre has not been finished. Every screen must pass the scroll-stop test, the feel test, and the motion test before it ships.

**Every pattern has a reason.** This skill doesn't just list what to do — it explains why, so you can apply judgment when the situation doesn't perfectly match the example.

**Production means production.** Patterns here handle process death, configuration changes, offline states, accessibility, large screens, security, and performance — not just the happy path.

---

## File Structure

```
android-lead-agent-skills/
├── SKILL.md                         ← Main entry point (load this)
├── README.md                        ← This file
├── references/
│   ├── accessibility.md
│   ├── adaptive-layouts.md
│   ├── android-mcp.md
│   ├── architecture.md
│   ├── background-work.md
│   ├── build-and-modules.md
│   ├── compose-ui-system.md
│   ├── coroutines-and-flow.md
│   ├── data-layer.md
│   ├── image-loading.md
│   ├── motion-and-animation.md
│   ├── navigation.md
│   ├── notifications.md
│   ├── performance.md
│   ├── security.md
│   ├── shared-element-transitions.md
│   └── testing.md
└── assets/
    └── ui-excellence-checklist.md
```

---

## Contributing

Found a missing pattern, outdated API, or better approach? The skill is designed to evolve as Android evolves. Open an issue or PR with the specific pattern and the reasoning behind it.

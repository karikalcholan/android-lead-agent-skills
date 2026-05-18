# Android Skill Research — Four-Repo Analysis

## Repo 1: Drjacky/claude-android-ninja (53 ★)
- **Architecture**: Clean Architecture — Domain/Data/UI; feature-first modularisation
- **DI**: Hilt (SingletonComponent for app-wide deps, ViewModelComponent scoped)
- **Navigation**: Navigation3 with @Serializable type-safe routes; NavigationSuiteScaffold + ListDetailPaneScaffold for adaptive layouts
- **State**: StateFlow for persistent UI state; SharedFlow(replay=0) for one-time events
- **Design**: Material 3, dynamic color, 8dp spacing tokens, dark/light modes
- **Compose**: Material motion, side effects, modifier stability, adaptive components
- **Testing**: Roborazzi (JVM screenshot tests), Hilt testing, Room 3 with SQLiteDriver, UIAutomator smoke tests
- **Build**: Gradle convention plugins, version catalog (libs.versions.toml), KSP migration
- **Networking**: Retrofit + OkHttp; certificate pinning
- **Special**: Macrobenchmark/Baseline Profiles; Play Integrity; Media3 MediaSessionService; background media playback hardening
- **Most beautiful UI signal**: Material motion for navigation with NavigationSuiteScaffold adapting fluidly across phone/tablet/foldable

## Repo 2: krutikJain/android-agent-skills (Python framework)
- Primarily a skill-generation/adapter framework — 34 skills for multiple agents
- **Coverage**: Kotlin idioms, Gradle, Clean Architecture, Hilt, Coroutines+Flow, Compose, Material 3, Navigation, Room, DataStore, Retrofit, offline-first, WorkManager, testing, CI/CD
- **Architecture**: MVVM + Repository (SSOT); use cases for complex business logic
- **Testing**: Unit (JUnit+MockK), UI (Compose Test Rule), screenshot
- **Key insight**: Validates that Clean Architecture + Hilt + Retrofit + Room is the settled cross-ecosystem standard

## Repo 3: new-silvermoon/awesome-android-agent-skills (812 ★) — BEST
- **Architecture**: Clean Architecture; presentation (StateFlow/SharedFlow + Compose UDF), domain (optional use cases), data (Repository)
- **Min SDK**: 24, Target SDK 34
- **DI**: Hilt (primary) or Koin (alternative)
- **State**: `MutableStateFlow` private, `StateFlow` exposed; `MutableSharedFlow(replay=0)` for events; `collectAsStateWithLifecycle()` in Compose
- **One-time events**: SharedFlow(replay=0) — prevents re-trigger on rotation
- **Navigation**: Type-safe @Serializable routes; `launchSingleTop`, `saveState`, `restoreState` pattern; NavArguments: only primitives/IDs
- **Data**: Stale-While-Revalidate reads; Outbox Pattern for writes; Room (Flow DAOs); Retrofit (suspend fns); WorkManager for background sync
- **Build**: build-logic composite build, convention plugins, version catalogs
- **Compose**: state hoisting, derivedStateOf, remember, MaterialTheme.colorScheme throughout
- **Testing**: Roborazzi (recordRoborazziDebug/verifyRoborazziDebug), HiltAndroidTest
- **Shared elements**: CompositionLocal scoping for deeply nested hierarchies vs explicit params for simpler trees
- **Most beautiful UI signal**: Compose navigation with type-safe routes and adaptive NavigationSuiteScaffold yielding seamless phone-to-tablet layout transitions

## Repo 4: rcosteira79/android-skills (60 ★, v3.0.0)
- Skills: android-dev, android-tdd, compose, android-ux, android-data-layer, android-retrofit, kmp-ktor, android-gradle-logic, android-debugging, coil-compose, kotlin-coroutines/flows
- **compose skill**: state management, modifiers (always `modifier: Modifier = Modifier` as first optional param), lazy lists, animations, theming, accessibility
- **android-ux**: Material Design 3, 48×48dp touch targets, 8dp spacing grid, navigation patterns, foldable support
- **Testing**: three-tier (unit/integration/UI), Roborazzi, coroutine testing with Turbine
- **Coroutines**: DispatcherProvider pattern (injected dispatchers, not hardcoded)
- **android-debugging**: Logcat, ADB, ANR traces, R8 decoding, recomposition debugging
- **Most beautiful UI signal**: SubcomposeAsyncImage loading states with custom placeholder composables

## Official Android Docs: Shared Element Transitions
- **SharedTransitionLayout**: outermost wrapper, provides SharedTransitionScope
- **sharedElement()**: same content (image→image, icon→icon); no enter/exit params; system interpolates geometry
- **sharedBounds()**: visually different composables sharing a spatial region (card→screen); has enter/exit params; handles clipping/background/container transform
- **Key matching**: unique key per element; data class keys for complex scenarios (SnackSharedElementKey with id + origin + type)
- **Modifier ordering**: must match between source and destination; before sharedElement/sharedBounds = constraints; after = applied within
- **Scope threading**: CompositionLocal (deeply nested) vs explicit params (simpler trees)
- **Navigation 2**: wrap NavHost in SharedTransitionLayout; use `this@composable` for AnimatedVisibilityScope
- **Navigation 3**: wrap NavDisplay in SharedTransitionLayout; use `LocalNavAnimatedContentScope.current`
- **Predictive back**: `android:enableOnBackInvokedCallback="true"` in manifest
- **Limitations**: no View/Compose interop, ContentScale not animated (snaps), no automatic shape animation (workaround: sharedBounds + animateEnterExit)

## Cross-Repo Consensus (3-4 repos)
1. Clean Architecture (Domain/Data/UI) — all 4
2. Hilt for DI — all 4
3. StateFlow (state) + SharedFlow(replay=0) (events) — all 4
4. Retrofit + OkHttp + Room — all 4
5. Repository as SSOT — all 4
6. Jetpack Compose + Material 3 — all 4
7. Convention plugins + version catalogs — 3+
8. Roborazzi for screenshot tests — 3+
9. Type-safe navigation routes — 3+

## Cross-Repo Divergence
- **Spacing grid**: 8dp (repos 1,4) vs 4dp (prompt spec says 4dp — follow prompt)
- **Min SDK**: 24 (repo 3) vs 21 (prompt spec — follow prompt)
- **DI**: Hilt (unanimous) vs Koin (repo 3 mentions as alternative)
- **Navigation version**: Navigation3 type-safe (repo 1) vs Navigation 2 (others)
- **Event handling**: SharedFlow unanimous; Channel mentioned by some as alternative

## Shared Element Patterns Observed
1. List grid thumbnail → detail hero image (sharedElement, stable ID key)
2. Card container expansion to full screen (sharedBounds, OverlayClip)
3. AnimatedVisibility list selection (sharedBounds, list de-selection animation)
4. FAB → full-screen compose surface (sharedBounds, CircleShape → full)
5. Text continuation (title in list → heading in detail, sharedElement on Text)
6. Multi-element per-screen (image + title both shared simultaneously)

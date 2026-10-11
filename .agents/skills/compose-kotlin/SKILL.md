---
name: compose-kotlin
description: >
  Parent skill for Android Kotlin + Jetpack Compose engineering on the 2026
  toolchain (Kotlin 2.1 K2, AGP 9, Compose BOM, Navigation 3). Routes work to
  sub-skills: android-kotlin-architecture (MVI/Clean Architecture),
  android-kotlin-compose (UI, recomposition, Material 3), android-kotlin-testing
  (Turbine, Compose UI tests, CI gates). Use when building Android features,
  structuring ViewModels, writing composables, setting up Nav 3, Hilt, Room,
  Ktor, Gradle builds, or asking "MVVM vs MVI", "where does state live",
  "audit this screen", "Navigation 3 routes", "K2 kapt KSP", "compose jank".
version: "2.2.0"
updated: "2026-10-11"
---

# Compose Kotlin — Parent Skill

Routing hub for the Android Kotlin + Compose kit. Load a sub-skill for mode
behavior (review vs authoring); load a reference for topic depth. Every generated
or reviewed file must satisfy the Mandatory Defaults and the Banned Antipatterns
list before merge.

## Sub-skills

| Sub-skill | Owns | Load when |
|-----------|------|-----------|
| [`android-kotlin-architecture`](../android-kotlin-architecture/SKILL.md) | Clean Architecture, MVVM/MVI, UseCases, module layout | Structuring features, refactoring ViewModels, "MVVM vs MVI", "where does state live", "UseCase when" |
| [`android-kotlin-compose`](../android-kotlin-compose/SKILL.md) | Compose UI: recomposition, stability, Material 3, LazyColumn, animations, edge-to-edge | Writing composables, fixing jank, theming, "audit this composable", "review this screen" |
| [`android-kotlin-testing`](../android-kotlin-testing/SKILL.md) | ViewModel tests (Turbine), Compose UI tests, fakes, CI gate | Writing tests, mocking repositories, "how to test ViewModel", "Turbine StateFlow" |

Each sub-skill runs in two modes: **REVIEW MODE** (silent audit checklist + cited
findings + minimal diff) and **AUTHORING MODE** (defaults to apply when writing).

## 2026 Toolchain (pinned)

```toml
# gradle/libs.versions.toml
[versions]
kotlin = "2.1.20"        # K2 compiler
agp = "9.0.0"
ksp = "2.1.20-2.0.1"
compose-bom = "2025.05.00"
hilt = "2.56.2"
room = "2.7.1"
navigation = "2.9.0"      # Navigation 3
kotlinx-serialization = "1.8.1"
```

| Setting | Value |
|---------|-------|
| `compileSdk` / `targetSdk` | 35 |
| `minSdk` | 26 |
| Java compatibility | VERSION_17 |
| `namespace` | required (AGP 9) |
| Edge-to-edge | `enableEdgeToEdge()` on every `ComponentActivity` |

Full build guidance: [`18-gradle-build-logic`](./references/18-gradle-build-logic.md).

## K2 Constraints

1. **KSP only — `kapt` is banned** on new projects (row 25): slow builds, K2
   incompatible. Use `ksp(libs.hilt.compiler)`, `ksp(libs.room.compiler)`.
2. Compose compiler ships with the Kotlin plugin — apply
   `org.jetbrains.kotlin.plugin.compose`; do not pin a separate compose-compiler version.
3. Kotlin language version 2.x: no `kotlinOptions` legacy flags; use
   compiler `freeCompilerArgs` only when required.
4. Stability is explicit: params crossing composable boundaries need `@Stable`
   or `@Immutable`, or K2 treats them unstable and restarts scope widens.

## MVI Guardrails (mandatory contract)

```kotlin
@Immutable data class FeatureUiState(...)   // snapshot-safe, immutable lists
sealed interface FeatureUiEvent { ... }     // user intents
sealed interface FeatureUiEffect { ... }    // one-shot side effects

private val _state = MutableStateFlow(FeatureUiState())
private val _effects = Channel<FeatureUiEffect>(Channel.BUFFERED)

fun onEvent(event: FeatureUiEvent) { when (event) { ... } }
_state.update { it.copy(...) }              // ONLY mutation style allowed
```

- Atomic state only: `_state.update { }` — never `_state.value = x` (row 5).
- One-shot effects via `Channel` + `receiveAsFlow()`; events via `onEvent()`.
- Never pass `ViewModel` into composable params — pass state + lambdas (row 8).
- ViewModel < 200 lines; UseCase only for shared/multi-repo/testable logic.
- Detail: [`android-kotlin-architecture`](../android-kotlin-architecture/SKILL.md) · [`01-architecture`](./references/01-architecture.md).

## Navigation 3 (mandatory)

- Routes are `@Serializable` data classes/objects — never string paths.
- Nav args carry **IDs and primitives only**; fetch full objects in the
  destination ViewModel via `SavedStateHandle.toRoute<T>()` (row 12).
- `launchSingleTop = true` on navigate; `popUpTo(startDestination)` after login.
- Tabs: nested `navigation<>()` graphs in a single `NavHost` — not nested NavHosts.
- Shared elements: `SharedTransitionLayout` with matching `rememberSharedContentState` keys.
- Detail: [`07-navigation`](./references/07-navigation.md).

## Banned Antipatterns

Master list (25 rows, audit every generated file against it):
[`00-banned-antipatterns`](./references/00-banned-antipatterns.md).

Top offenders:

| # | Banned (WRONG) | Correct (RIGHT) |
|---|----------------|-----------------|
| 1 | `GlobalScope.launch { }` | `viewModelScope.launch { }` |
| 4 | `collectAsState()` on Android | `collectAsStateWithLifecycle()` |
| 5 | `_state.value = x` | `_state.update { }` |
| 7 | `items(list)` no key | `items(list, key = { it.id })` |
| 12 | Nav arg = whole object | Nav arg = `id: String` |
| 17 | Backwards write in `@Composable` | Write in event handlers only |
| 21 | `SharedPreferences.edit().apply()` | `dataStore.edit { }` |
| 25 | `kapt` for Hilt/Room | KSP (`ksp(...)`) |

In review mode, grep the WRONG column; cite row numbers in findings.

## Reference Routing Table

| Question / task | Load |
|-----------------|------|
| Any review, PR audit, pre-merge check | [`00-banned-antipatterns`](./references/00-banned-antipatterns.md) |
| Clean Architecture, MVVM/MVI, UseCases | [`01-architecture`](./references/01-architecture.md) |
| Recomposition, stability, Material 3, theming | [`02-compose-ui`](./references/02-compose-ui.md) |
| Springs, Canvas, gestures, shared elements | [`03-animations`](./references/03-animations.md) |
| StateFlow vs SharedFlow, dispatchers, `stateIn` | [`04-coroutines-flow`](./references/04-coroutines-flow.md) |
| Hilt setup, scopes, `@Binds` vs `@Provides` | [`05-hilt-di`](./references/05-hilt-di.md) |
| Room entities, DAOs, migrations, offline-first | [`06-room-db`](./references/06-room-db.md) |
| Navigation 3, type-safe routes, deep links | [`07-navigation`](./references/07-navigation.md) |
| KMP/CMP, `expect`/`actual`, shared ViewModel | [`08-kmp-cmp`](./references/08-kmp-cmp.md) |
| Ktor client, auth flows, DTO mappers | [`09-networking`](./references/09-networking.md) |
| Jank, `derivedStateOf`, LazyColumn, baseline profiles | [`10-performance`](./references/10-performance.md) |
| Turbine, fakes, `TestInstallIn`, Compose tests | [`11-testing`](./references/11-testing.md) |
| CameraX, ML Kit pose detection | [`12-camera-mlkit`](./references/12-camera-mlkit.md) |
| Signing, R8 rules, Play Store | [`13-release-checklist`](./references/13-release-checklist.md) |
| DataStore, SharedPreferences migration | [`14-datastore`](./references/14-datastore.md) |
| Paging 3, `RemoteMediator`, `asState()` | [`15-paging3`](./references/15-paging3.md) |
| Coil `AsyncImage`, cache, network dep | [`16-coil-image`](./references/16-coil-image.md) |
| TalkBack, semantics, 48dp targets, WCAG | [`17-accessibility`](./references/17-accessibility.md) |
| Version catalogs, convention plugins, KSP, R8 | [`18-gradle-build-logic`](./references/18-gradle-build-logic.md) |
| XML → Compose migration, `ComposeView` interop | [`19-xml-to-compose-migration`](./references/19-xml-to-compose-migration.md) |

## Platforms

| Platform | Entry point | Notes |
|----------|-------------|-------|
| Android (primary) | `ComponentActivity` + `setContent` | AGP 9, edge-to-edge, `collectAsStateWithLifecycle` |
| iOS | `ComposeUIViewController { App() }` | commonMain UI; UIKit appearance via `expect`/`actual` |
| Desktop | `application { Window(...) { App() } }` | Ktor `CIO` engine; no-op platform composables |

Shared code lives in `commonMain`; platform APIs via `expect`/`actual`
(`DatabaseFactory`, Ktor engine `OkHttp`/`Darwin`/`CIO`, `@ConstructedBy`
Room constructor). In `commonMain` use `collectAsState()` — the lifecycle-aware
variant exists only in Android source sets. Detail: [`08-kmp-cmp`](./references/08-kmp-cmp.md).

## Mandatory Defaults

Apply without being asked:

1. `enableEdgeToEdge()` + respect `WindowInsets.safeDrawing` — no hardcoded status-bar padding.
2. `modifier: Modifier = Modifier` — last optional param on every composable.
3. `stringResource(R.string.x)` — no hardcoded `Text("...")`.
4. `MaterialTheme.typography.*` — no raw `fontSize = 16.sp`.
5. `collectAsStateWithLifecycle()` on Android — never bare `collectAsState()`.
6. `key = { it.id }` + `contentType` on every `LazyColumn`/`LazyRow` `items()`.
7. Modifier order: padding → clip → background → clickable.
8. `_state.update { }` for all ViewModel mutations; `Channel` for one-shot effects.
9. `@Serializable` routes (Nav 3); IDs only in nav args.
10. KSP (not kapt); Version Catalog + `build-logic/` convention plugins.
11. Baseline profile module for release apps.
12. Coil: `coil-compose` + `coil-network-okhttp` — images otherwise never load.

## Validation

1. **Review mode** — sub-skills run their checklist silently on audit triggers
   ("audit this file", "review this screen") and cite banned-antipattern rows.
2. **Pre-merge gate** — grep target files against the WRONG column of
   [`00-banned-antipatterns`](./references/00-banned-antipatterns.md);
   zero hits required.
3. **Repo skill validation** — keep the kit consistent:
   ```bash
   python3 scripts/validate_skills.py --strict
   ```
4. **CI gate** — unit tests (Turbine) + critical-flow instrumented tests must
   pass before merge (see [`android-kotlin-testing`](../android-kotlin-testing/SKILL.md)).

## Examples

**Routing by intent:**

- "Why does my screen recompose?" → `android-kotlin-compose` + `10-performance`
- "Where does this state live?" → `android-kotlin-architecture`
- "Set up Navigation 3" → `07-navigation`
- "Audit this composable" → `android-kotlin-compose` REVIEW MODE + `00-banned-antipatterns`
- "K2 build is slow" → `18-gradle-build-logic` (KSP migration)

**Review-mode output shape** (from `android-kotlin-compose`):

```
## Compose audit — FeatureScreen.kt

| # | Status | Finding |
|---|--------|---------|
| 4 | FAIL   | Uses collectAsState() — row 4: switch to collectAsStateWithLifecycle() |

### Banned antipatterns hit
- Row 4: collectAsState() on Android → collectAsStateWithLifecycle()

### Suggested diff
- val items by viewModel.state.collectAsState()
+ val items by viewModel.state.collectAsStateWithLifecycle()
```

**Authoring-mode quick start** (new feature):

```kotlin
// presentation/FeatureViewModel.kt — MVI contract from the guardrails above
// presentation/FeatureScreen.kt   — stateless content + modifier param
// navigation: composable<Feature> { val route = it.toRoute<Feature>() ... }
// build: ksp(libs.hilt.compiler); implementation(platform(libs.compose.bom))
```

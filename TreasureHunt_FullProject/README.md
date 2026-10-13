# Grandpa's Treasure Hunt — Setup Instructions

## 1. Create the project
In Android Studio: **New Project → Empty Activity** (Kotlin, Jetpack Compose).
- Name: whatever you'd like (this code uses package `com.example.treasurehunt` —
  either match that package name when creating the project, or find/replace
  `com.example.treasurehunt` across all files if you use a different one).
- Min SDK: 26 or higher (required by some Compose/location APIs used here).

## 2. Copy in these files
Copy everything under `app/src/main/java/com/example/treasurehunt/` into your
new project's matching folder, replacing the template's default files.
Copy `app/src/main/res/raw/clues.json` and the `res/values/*.xml` files the
same way. Replace your `AndroidManifest.xml` with the one provided here.

## 3. Add dependencies
Merge the `dependencies { ... }` block from the included `build.gradle.kts`
into your project's `app/build.gradle.kts` (don't overwrite the whole file —
keep the `plugins {}` block and `namespace`/`applicationId` Android Studio
already generated). Then click **Sync Now**.

## 4. Update each clue's real-world coordinates
Edit `app/src/main/res/raw/clues.json` and replace the sample Seattle
coordinates with the actual latitude/longitude of the real-world spots you
want your hunt to use. `locationInfo` is the "you found it" text shown on
the Clue Solved / Completed screens; `hintText` is shown when Hint is tapped.

## 5. Fill in your name
Each file's header comment block has an `Author: [Your Name]` placeholder —
update those before submitting.

## 6. Test with the emulator
Use **Extended Controls → Location → Single points** to save GPS points
matching your clues' coordinates, then switch the emulator's location to
each one in turn while testing "Found It!" — see the chat for the full
demo/recording walkthrough.

## Project structure
```
app/src/main/java/com/example/treasurehunt/
├── MainActivity.kt
├── model/
│   ├── Clue.kt
│   └── ClueRepository.kt            # loads clues.json
├── location/
│   ├── LocationHelper.kt            # FusedLocationProviderClient wrapper
│   └── LocationUtils.kt             # Haversine formula
└── ui/
    ├── TreasureHuntViewModel.kt     # TreasureHuntUiState + ViewModel
    ├── TreasureHuntNavigation.kt    # Destinations + NavHost
    ├── PermissionsScreen.kt
    ├── StartScreen.kt
    ├── ClueScreen.kt
    ├── ClueSolvedScreen.kt
    ├── CompletedScreen.kt
    └── theme/ (Color.kt, Type.kt, Theme.kt)
```

This mirrors the structure of Assignment 5 (`MultiScreenAppP2`): a single
shared ViewModel created in the navigation file and passed down to every
screen, a `UiState` data class, a `Destinations` object for route names,
and each screen split into a stateful wrapper (talks to the ViewModel)
plus a stateless `...Content` composable (just takes state + lambdas).
The two differences from Assignment 5 are `TreasureHuntViewModel`
extending `AndroidViewModel` instead of plain `ViewModel` (it needs a
Context for GPS and resource loading, which Assignment 5's in-memory
models didn't), and `ClueScreen`'s `onFoundItClicked` branching on a
`FoundItResult` enum instead of navigating unconditionally, since
whether it advances the hunt depends on a live distance check.

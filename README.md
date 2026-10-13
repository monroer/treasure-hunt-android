Grandpa's Treasure Hunt 

A GPS-based scavenger hunt app for Android, built solo in Kotlin with Jetpack Compose. Players navigate to real-world coordinates to "solve" a sequence of location-based clues, with live distance feedback and hints along the way.


What It Does

The player is guided through a five-screen flow; permissions, start, clue, clue-solved, and completed. Navigating to a series of GPS coordinates in order. On each clue screen, tapping "Found It!" checks the player's live location against the clue's target coordinates using the Haversine formula; being close enough advances the hunt, being too far shows a wrong-location prompt, and a hint is available on request.

Architecture

Built with a unidirectional-data-flow MVVM pattern:

TreasureHuntViewModel (extends AndroidViewModel for GPS/Context access) exposes a single immutable TreasureHuntUiState via StateFlow, created once in the navigation graph and shared across all five screens
Each screen is split into a stateful wrapper (talks to the ViewModel) and a stateless ...Content composable (pure function of state + lambdas) — makes the UI layer trivially previewable and testable
LocationHelper wraps Google Play Services' FusedLocationProviderClient and exposes location updates as a cold Kotlin Flow, chosen over the legacy LocationManager for better accuracy/battery efficiency
LocationUtils implements the Haversine formula for great-circle distance between two lat/lng points
Clue data is loaded from a bundled clues.json via ClueRepository, keeping content decoupled from app logic
Tech Stack
Language: Kotlin
UI: Jetpack Compose, Material3, Navigation Compose
Concurrency: Kotlin Coroutines & StateFlow
Location: Google Play Services FusedLocationProviderClient
Architecture: MVVM, AndroidViewModel, single-source-of-truth UI state

Try It

Clone the repo, open in Android Studio, sync Gradle, and update the sample coordinates in app/src/main/res/raw/clues.json to real-world locations of your choice. Grant location permissions on first launch.

What I'd Add Next

Persisting hunt progress across app restarts, multiple hunt "packs," and a hunt-creation UI so users can build their own clue sets instead of editing JSON directly.

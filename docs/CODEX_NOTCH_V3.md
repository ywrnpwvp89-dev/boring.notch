# Codex Notch v3 — unofficial build

Adds an optional, read-only Codex activity surface to Boring Notch 2.7.3.

## Use

1. Quit an existing Boring Notch process before opening this build.
2. Open Settings → Codex and choose your actual `~/.codex` directory.
3. While a task runs, the closed notch shows its conversation name and the running count.
4. Open the notch to see up to 12 recent conversations. Running conversations appear first, followed by the most recent conversations. Status refreshes every two seconds.
5. The expanded panel displays the latest observed turn state: running, completed, interrupted, failed, or idle. “Completed” describes a turn, not the overall project outcome. A conversation without a recent observed turn is idle.
6. The Music & Calendar button temporarily returns to the original home view. The next normal home expansion returns to Codex. The shelf remains available.

The expanded labels in this personal build are Chinese. The upstream contribution uses English source strings and does not modify translation catalogs.

## Data access and local names

The selected directory is accessed through a read-only, security-scoped bookmark. The scanner opens the `state_*.sqlite` and `thread_history_*.sqlite` indexes read-only, queries task names and turn status, and correlates conversation IDs with runtime IDs using rollout filenames. It does not query transcript contents, credentials, prompts, or response bodies. The directory grant itself covers the selected directory, so macOS authorization should be reviewed accordingly.

A local name cache is maintained under the app container's Application Support directory:

`~/Library/Containers/theboringteam.boringnotch/Data/Library/Application Support/CodexNotch/task-titles.json`

The title cache is not included in source distributions or release archives. Archived conversations and internal subagent conversations are excluded from the panel. Recent turn lookup is limited to the last 24 hours to avoid showing abandoned old runtime records as indefinitely active.

Codex's local database schema and rollout naming are undocumented integration details and may change. This build does not infer approval requests or provide approval actions.

## Build

Requires Xcode with the project's macOS SDK and package dependencies. The build was verified with Xcode 27.0 (27A5194q) on macOS 27.2, Apple Silicon.

```sh
xcodebuild -project boringNotch.xcodeproj -scheme boringNotch \
  -configuration Release -destination 'platform=macOS' \
  -derivedDataPath build CODE_SIGN_IDENTITY=- CODE_SIGN_STYLE=Manual \
  DEVELOPMENT_TEAM= build
```

The published Apple Silicon application is locally ad-hoc signed and is not notarized. The archive contains the upstream GPL-3.0 license and third-party notices. The matching source archive is published alongside the application.

## Validation

- Full Debug build, local signature verification, and live UI inspection.
- Actual display of two simultaneously running conversations with resolved names and elapsed times.
- Synthetic database checks for ID mapping, multiple running conversations, latest-turn selection, completed/failed states, title persistence, and archived/subagent exclusion.
- Release build and archive signature verification recorded in the release notes.

This release preserves the main-based application version already exercised locally. A separate contribution branch adapts the feature to the upstream `dev` activity stack.

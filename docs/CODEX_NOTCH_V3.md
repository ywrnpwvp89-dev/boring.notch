# Codex Notch — unofficial personal build

This main-based build adds a Codex conversation panel to Boring Notch 2.7.3. Connect it by choosing the actual `~/.codex` directory in Settings → Codex. The app stores a read-only security-scoped bookmark for that directory.

The closed notch displays a running conversation on the left and the latest recorded 5-hour remaining percentage and reset time on the right. Opening the notch prioritizes playing media; otherwise it shows Codex when tasks are running. Use the header button to switch manually. Clicking a conversation title opens the corresponding Codex conversation. The expanded panel also shows the weekly usage window and the time at which the usage data was recorded. Interface labels are English; conversation titles retain their original names.

## Data and limitations

The activity scanner queries local SQLite indexes for conversation names and turn states and uses rollout filenames to correlate runtime and conversation IDs. It maintains a local task-title cache in the app container and excludes archived and internal subagent conversations.

To obtain usage, the app reads portions of recent local session JSONL files and extracts rate-limit metadata. Those files can also contain conversation text; the scanner filters for token-count events and does not retain or display their message text. The directory grant covers the selected directory as a whole. Usage is the latest locally recorded snapshot, not a live account request; it can be stale. Expired windows are shown as unavailable until a newer record appears. Codex's database and event formats may change.

## Build

Build the `boringNotch` scheme in Xcode for macOS. This personal version was built in Release on Apple Silicon using Xcode 27 beta and is ad-hoc signed, without Apple notarization. Follow the repository license and bundled third-party notices when distributing it.

The prior `codex-notch-v3` GitHub release archive contains an earlier version. This branch contains newer source; that release archive has not been rebuilt for these changes. The separate upstream `dev` contribution is tracked by PR #1724.

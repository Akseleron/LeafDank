# LeafDank

**LeafDank** is a work-in-progress Android Twitch chat client based on [DankChat](https://github.com/flex3r/DankChat), with optional advanced functionality inspired by [Leafyrino](https://github.com/Leafyzito/leafyrino).

> **Status:** planning and upstream audit. The current Android source still reflects DankChat 4.0.47. No LeafDank APK, custom package ID, Channel Points integration, or switchable UI is implemented yet. This is not an official DankChat or Leafyrino release.

## Product principles

- **Minimal by default:** preserve DankChat's clean, responsive chat experience and existing functionality.
- **Powerful when enabled:** expose expanded viewer, moderator and customization features without crowding the default UI.
- **One chat engine:** Classic, Modern and Custom layouts must share message, connection, authentication and persistence state.
- **Efficient on phones:** lazy rendering, bounded media caches, animation suspension off-screen, measured memory and battery impact.
- **Safe accounts:** no browser token extraction, remotely executed login scripts, or embedded client secrets.

## Planned capabilities (not yet implemented)

1. Classic / Modern / Custom UI with independently selectable components and per-channel overrides.
2. Extended badge and emote customization, GIF size controls, user cards and moderation.
3. Additional OAuth connections with explicit permissions and revocation.
4. Channel Points, rewards, polls and predictions **only after** validating supported authentication and Twitch integration constraints.
5. Reproducible performance tests and careful updates from DankChat upstream.

See [development rules](AGENTS.md), [architecture](docs/ARCHITECTURE.md), [roadmap](docs/ROADMAP.md), [source references](docs/SOURCE_REFERENCES.md) and [contributing](CONTRIBUTING.md).

## Origins and licensing

The starting codebase is DankChat (Kotlin/Jetpack Compose). Leafyrino (C++/Qt) is available as a **pinned Git submodule** in [`references/leafyrino/`](references/leafyrino), for source analysis only; it is **not** merged into the Android app or included in the APK. Port features case-by-case, preserving original copyright notices for any reused source.

See [LICENSE](LICENSE) for the inherited MIT license. Keep original notices and third-party licenses. LeafDank is not affiliated with Twitch, DankChat or Leafyrino.

## Builds

The Android build configuration, application ID and signing configuration still belong to the upstream DankChat fork. Do **not** distribute these APKs as LeafDank until branding, signing and independent installation have been implemented and tested.

## Source reference checkout

After cloning, run `git submodule update --init references/leafyrino` to download the pinned Leafyrino sources. Its nested C++ dependencies are not needed to study the code. See [source reference instructions](docs/SOURCE_REFERENCES.md).

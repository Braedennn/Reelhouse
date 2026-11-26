# Changelog

Notable Reelhouse releases and changes are documented here. Download packages and SHA-256 checksums are available from [GitHub Releases](https://github.com/Braedennn/Reelhouse/releases).

## [1.1.0] — November 26, 2026

This release improves stream selection and Android TV playback reliability on **Windows Desktop** and **Android TV**.

### Added

- StreamPlay's Vidlink source as a native fallback with quality variants and captions
- Automatic English audio preference when an Android TV track declares its language
- Measured playback and remote-control regression coverage

### Improved

- Auto source selection favors adaptive HLS and efficient progressive files over unnecessarily large remuxes
- Android TV uses a larger playback buffer while retaining explicit quality choices
- PlavioHD source handling preserves adaptive streams, source-specific headers, subtitles, and quality metadata
- Windows resolves PlavioHD, StreamPlay · Vidlink, and Internet Archive through the existing native provider interface

### Fixed

- Android TV D-pad left/right seeks exactly 10 seconds on the video surface and focused progress bar
- Late adaptive English streams are no longer discarded in favor of earlier high-resolution progressive files
- Provider failures are isolated so one unavailable source does not prevent fallback playback

### Distribution

- Windows Desktop x64 installer
- Non-debuggable Android TV APK for direct installation, signed with the development certificate
- Android package version 1.1.0 (versionCode 2)
- The former Windows TV prototype is not included in this release

## [1.0.0] — November 23, 2026

The first public Reelhouse release for **Windows Desktop** and **Android TV**.

### Added

- Cinematic movie and series discovery with title, cast, season, and episode details
- Personal saved list and planned, watching, watched, and dropped shelf states
- Continue Watching with locally stored resume progress
- Quality, subtitle, speed, volume, theater, fullscreen, and display controls
- Full-screen Android TV interface with directional remote navigation
- Native Media3 playback on Android TV
- Four-poster Films and Shows browsing grids on Android TV
- Automatic next-episode playback, including season transitions
- Local privacy controls for clearing library, progress, and preference data

### Improved

- Fullscreen remains active during automatic episode changes on Desktop and Android TV
- Details-page **Play** and **Continue** actions advance to the correct episode
- Completed episodes no longer clutter Continue Watching
- Source health checks and server failover improve playback resilience

### Distribution

- Windows Desktop x64 installer
- Android TV debug-signed APK for direct installation
- The former Windows TV prototype is not included in this release

[1.1.0]: https://github.com/Braedennn/Reelhouse/releases/tag/v1.1.0
[1.0.0]: https://github.com/Braedennn/Reelhouse/releases/tag/v1.0.0

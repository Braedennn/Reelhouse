# Reelhouse Privacy Policy

**Publisher:** TotallyNoire  
**Last updated:** November 26, 2026

This policy applies to **Reelhouse for Windows Desktop** and **Reelhouse for Android TV**. Third-party services contacted by the applications have their own policies and practices.

## Summary

- Reelhouse has no account system and includes no advertising or analytics/telemetry SDK.
- Saved titles, shelf states, playback progress, and player preferences are stored locally on the device.
- Reelhouse is not offline or fully local. Searches, title lookups, artwork, source discovery, health checks, subtitles, and playback contact third-party services.
- Those services can receive search text, IMDb or TMDB title IDs, season and episode numbers, media URLs, request tokens, referrer information, the device's public IP address, and ordinary connection metadata. The exact information depends on the feature and provider.
- A Windows loopback server does not change this: upstream requests still originate from the user's device and network.

**No account, advertising, or analytics integration does not mean that no data is sent to third parties.**

## Information handled by Reelhouse

### Searches, titles, and viewing selections

Reelhouse processes the text entered into search, the kind of catalog selected, title and IMDb identifiers, and selected season and episode numbers. Source resolution and playback also process media and subtitle URLs, referrer values, request headers, and short-lived provider or viewer tokens.

An IMDb ID identifies a movie or show, not a user or device. Together with a search or episode selection, however, it can reveal a user's viewing interest to the service receiving the request.

### Local library and preferences

Reelhouse can store the following records on the device:

| Record | Examples |
| --- | --- |
| `reelhouse.list` | Saved title IDs |
| `reelhouse.shelf` | Planned, watching, watched, or dropped states |
| `reelhouse.progress` | Resume position, duration, title/episode key, and update time |
| `reelhouse.journal` | Watch-history record structure, if used by a version of the app |
| `reelhouse.prefs` | Autoplay, next-episode, quality, subtitle, speed, volume, display, and preferred-provider settings |

The current applications do not send TotallyNoire a copy of these records merely because they are stored. Selecting a title or playing media can nevertheless disclose related title or episode information to upstream services as described below.

### Custom Windows providers

Windows Desktop can read user-supplied static or web-provider JSON files. A web provider can receive title variants, release year, season and episode details, and any headers configured for it. Static-provider media and subtitle hosts are contacted during health checks or playback.

A configured header can contain a cookie, token, or other credential if the person creating the provider file puts one there. That value is sent to the applicable upstream host with the request. Custom provider files and their credentials are not needed by the Android TV build.

## Information sent to third-party services

| Service or recipient | Platforms | What can be sent and why |
| --- | --- | --- |
| Cinemeta at `v3-cinemeta.strem.io` | All | Catalog requests; search text embedded in catalog URLs; and IMDb title IDs in metadata requests. This supplies catalog, title, cast, and episode metadata. |
| Metahub at `images.metahub.space` and `episodes.metahub.space` | All | Artwork requests whose paths can contain title IDs and episode identifiers. This loads posters, backdrops, logos, and episode stills. |
| Internet Archive at `archive.org` | Windows Desktop | Catalog/search parameters, item identifiers, artwork requests, metadata requests, and media-file requests. The Android TV build does not include the Archive shelves/provider. |
| PlavioHD at `hd.plavio.site` and `subs.plavio.site` | All | IMDb title ID, media kind, and, for shows, season and episode numbers used to discover streams and subtitles. |
| StreamPlay · Vidlink at `enc-dec.app` and `vidlink.pro` | All | A Cinemeta-derived TMDB title ID and, for shows, season and episode numbers used to resolve media and captions. No Reelhouse account credentials are sent. |
| Dynamically selected CDN, subtitle, and stream hosts | All | URLs, source-specific headers and tokens returned during source discovery; referrer values; playlist, encryption-key, subtitle, byte-range, segment, and media requests. Hosts can change as provider results change. The dormant vidsrc modules are not contacted. |
| Custom provider and associated media hosts | Windows Desktop | Provider-defined searches, title/year/episode information, configured headers, and media or subtitle requests. |
| YouTube | Where a Windows trailer link is offered | YouTube is contacted only after the user selects a trailer link that opens externally; merely viewing the title page does not itself open that YouTube link. |

Every internet host contacted in this process can normally see the device's public IP address and standard network/request metadata, such as request time, requested URL, user agent, and referrer where supplied. Providers may also use cookies or their own identifiers under their own policies.

### Source health checks happen before playback

Selecting or resolving a source can cause Reelhouse to request playlists, variants, and sample media bytes to measure availability, quality, and latency. These probes can disclose the selected title's stream URL and related tokens to candidate hosts **before ordinary playback begins**. A probe is still network communication with that host.

### How the Windows local server works

Reelhouse for Windows starts its application server on `127.0.0.1`, using a preferred local port or an available fallback. It is bound to the loopback interface, not offered as a publisher-operated cloud service. When the app retrieves metadata or proxies a stream, the local server makes the upstream internet request from the user's computer. The contacted service can therefore see the user's network address; the loopback address does not hide it.

Android TV has no embedded Next.js server. Its catalog/source clients use Capacitor native HTTP, and its Media3 player contacts applicable media hosts directly from the device.

## Local storage, logs, and caches

- **Windows:** application records are stored as JSON under Electron's application-data location in `Reelhouse/storage`. Filenames are hashes of storage keys; hashing a filename is not encryption of its contents.
- **Android TV:** library and preference records use the app WebView's local storage within the application environment. Cloud-backup rules exclude WebView local storage and shared preferences; device-to-device transfer rules include those records. Platform backup and transfer behavior can vary by Android version and device.
- **Runtime caches:** catalog, source, and token caches are primarily in memory and expire or disappear when the process ends. They are not a publisher cloud history.
- **Android playback files:** the native player writes localized playlists under `cacheDir/native-player/<UUID>`. It attempts recursive deletion when the player is destroyed, but a crash, forced stop, power loss, or cleanup failure can leave temporary data until Android clears the cache or app storage.
- **Diagnostics:** Android Media3 writes limited playback diagnostics, such as video dimensions, seek timestamps, and dropped-frame counts, to logcat. Capacitor logging is disabled in the reviewed configuration. Optional Windows boot diagnostics are local and are written only when their debug setting is enabled.

No advertising ID, account ID, contact list, precise location, microphone input, camera input, or payment information is requested by the reviewed applications. Android declares the `INTERNET` permission; this permission does not itself explain or limit the upstream disclosures described above.

## Retention and deletion

### Data on the device

Local records generally remain until overwritten, removed through an available control, cleared with application data, or erased with the containing storage. Removing a saved title or changing its shelf state does not necessarily remove every related progress record or preference. Restarting or completing an item can clear its individual resume point.

**Settings → Privacy & data → Clear local data** asks for confirmation and then removes the saved list, shelf, progress, journal, and preferences from the app's local store. On Windows, this clears Reelhouse's local store. On Android TV, it clears Reelhouse TV's own WebView records. The control does not remove provider configuration files, downloaded installers, caches or temporary playback files, or records held independently by third parties.

Additional manual removal options:

- **Windows:** close Reelhouse, back up anything needed, and remove the Reelhouse records from the `Reelhouse/storage` application-data directory. Use the operating system's application-data tools or a file manager; do not assume that uninstalling alone removes this separate data.
- **Android TV:** use the in-app control above or Android's app settings to clear Reelhouse TV's storage and, if desired, its cache. Clearing storage removes the app's local WebView records on that device. Cloud-backup rules exclude those records, while device-to-device transfer rules include them; a transferred copy is separate from the copy cleared on the current device.
- **Either platform:** uninstall behavior, transferred or otherwise restored copies, filesystem recovery, and crash remnants vary. Uninstalling is not represented here as guaranteed secure erasure.

### Data received by third parties

TotallyNoire cannot use the app's local deletion controls to erase logs, cookies, caches, or records held independently by Cinemeta, Metahub, Internet Archive, YouTube, PlavioHD, StreamPlay · Vidlink's service endpoints, a custom provider, or a stream host. Their retention periods and deletion mechanisms are governed by their own terms and privacy policies. A request to TotallyNoire cannot guarantee deletion from those independent services.

## Security

The fixed Cinemeta, Metahub, Internet Archive, PlavioHD, and Vidlink entry points in the reviewed application use HTTPS. The PlavioHD and Vidlink adapters accept only HTTPS media and subtitle URLs. Windows custom provider definitions and dynamically discovered resources can include HTTP URLs; information sent over HTTP is not protected by TLS. Redirects and downstream hosts also affect the protection of a particular request.

The Windows renderer uses Electron isolation settings and a loopback-only application server. Android keeps native playback files in the app cache and disables Capacitor request logging. These measures do not make every upstream connection secure, prevent a contacted service from observing a request, or guarantee protection against device compromise, malicious provider configuration, or network threats.

## Third-party terms and content

Reelhouse does not host the catalog's media. Metadata, artwork, streams, subtitles, and external links belong to or are supplied by third parties. Availability does not establish a license, public-domain status, or permission to redistribute or view content.

Internet Archive `licenseurl` filtering reflects an uploader-supplied metadata declaration; it is not independent verification of rights. The presence of a title in Cinemeta, a scraper result, or a custom provider is likewise not a rights grant. Users and distributors must comply with applicable third-party terms and ensure that their access and use are authorized.

## Children and age requirements

Reelhouse has no account registration and does not request an age or child profile. Catalog metadata and third-party media can include material with different age requirements. Parents or guardians should use appropriate device and store controls.

## Changes to this policy

This policy may be updated whenever a release changes its services, providers, data storage, backup behavior, SDKs, or distribution model. The published revision date will change with a substantive update.

# Monitor+ Changelog

## v1.1.2K — Grid Compliance Now Actually Ships

**Released:** September 4, 2026

- Grid-compliance monitoring is now compiled into the shipped Monitor+ build. Earlier packages documented `!gridcheck`, `!gridstatus`, `!gridlog`, and new-grid warnings, but the code was not included in the binary — those features did nothing until this release.
- Adds `EnableGridComplianceWarnings` as an on/off switch for the whole World Protection and Privacy feature. Set it to `false` to turn off all new-grid detection, in-game warnings, five-minute reminders, and Discord audit output. Defaults to `true`.
- The setting is a normal configuration value and is added to existing configuration files automatically on load, so it is visible and editable. A change takes effect on `!reload` without a full session restart.
- Adds `!gridstatus` (list tracked non-compliant player grids) and `!gridlog <on|off|status>` (control Discord audit output while in-game warnings continue).
- Players can run `!gridcheck` and `!gridcheck help` in Space Engineers chat to inspect only their own major-owned grids.
- Requirements per grid remain a placed beacon, at least 25 blocks (configurable), and a `FACTIONTAG-GridName` name. NPC-created grids are excluded. The plugin only warns and audits; it never deletes grids.
- Security: new configurations now default `AllowAnyTorchCommand` to `false` (allowlist-only). Only the commands listed in `AllowedTorchCommands` can be forwarded from Discord unless an owner explicitly opts back in. Existing configurations keep their current value.

## v1.1.1 — Monitor+ Identity and Save Notification Control

**Released:** September 1, 2026

- Changes the default in-game announcement sender from `TROA` to `Monitor+`.
- Automatically migrates the legacy `AdvertisementAuthor=TROA` value in existing configurations while preserving custom server-owner names.
- Makes Discord `{ts}` placeholders render as each viewer's local date and time.
- Adds `{ts_relative}` for Discord's localized relative time, such as “a few minutes ago.”
- Adds `SaveNotificationCooldownSeconds` with a 300-second default to suppress duplicate automatic-save messages caused by multiple world files updating during one save.
- Administrator-requested save confirmations bypass the duplicate-suppression window.

## v1.1.0J — Discord Access and In-Game Chat Styling

- Removes the administrator Discord-to-Steam mapping requirement for forwarded Torch commands.
- Keeps player linking separate for player-only features such as vote rewards.
- Adds owner-configurable Discord-to-game text and highlight colors through `DiscordGameTextColor` and `DiscordGameHighlightColor`.
- Accepts named, hexadecimal, and RGB highlight colors with a safe teal fallback for invalid values.
- Automatically adds the new color settings to existing configuration files during startup or reload.

## v1.1.0 — Discord Operations and Backup Update

**Released:** August 22, 2026  
**Torch compatibility:** .NET Framework 4.8, C# 5-compatible Torch environments.

### Added

- Native Discord slash-command menu for player and administrator tools.
- Public `/servercard` with live players, world grid count, simulation speed, CPU, memory, storage, restart information, voting, support links, and versions.
- Branded Discord embeds with a single server-local date and time in the `TROA Monitor` footer.
- Major timezone choices, including Indianapolis/Eastern for new configurations.
- Player Discord-to-Steam linking with in-game confirmation codes.
- Vote links, reward claims, cooldowns, reward tracking, and a top-voter leaderboard.
- Grid-compliance reminders for beacon, block-count, and `FACTIONTAG-GridName` requirements.
- NPC grid exclusion from player grid-compliance notifications and reports.
- Grid audit logging controls through `!gridlog`.
- Manual dated world snapshots with `!backupnow`.
- Automatic backup scheduling after startup and/or at a chosen interval with `!backupschedule`.
- Backup listing, details, and non-destructive restore requests.
- Expanded `!troasediscordhelp` command coverage.

### Changed

- `!servercard` replaces the older `!seserver` dashboard command.
- `/servercard` is the preferred public dashboard command.
- `!status` returns the detailed server card to administrators while players receive a concise public summary.
- Only global game chat is bridged to Discord. Faction, direct/private, `/f`, and `./f` chat remain private and are recorded in the server log only.
- New configuration files default `EnableRestartScheduler` to `false`.
- Existing configuration values are preserved. The plugin does not overwrite an explicitly configured restart-scheduler setting.
- Generated configuration is compacted to essential connection, channel, ownership, support, voting, and backup values.

### Fixed

- Discord Gateway reconnects now synchronize with the actual Torch session state.
- A reconnect after the world has loaded no longer sends a stale `Server is starting up.` message.
- Removed duplicate timestamps from command embeds.
- Dashboard/status output now reports live world grid count.

### Upgrade Instructions

1. Stop Torch completely.
2. Back up your existing `TROADiscordSEMonitor.cfg` somewhere private.
3. Download and extract `TROADiscordSEMonitor-v1.1.0.zip` into the Torch plugin directory.
4. Start Torch once so the plugin can update its storage/configuration, then review the resulting configuration.
5. Do not copy example tokens, IDs, mappings, or keys over your live configuration.
6. Start Torch and test `/servercard`, `!status`, and `!troasediscordhelp`.
7. Configure automatic backups only if wanted: `!backupschedule status`, `!backupschedule startup on`, or `!backupschedule every <minutes>`.

> `EnableRestartScheduler=false` already prevents the automatic restart timer from starting. This release fixes incorrect Discord status messages; it does not cause a disabled restart scheduler to restart the server.

## v1.0.2 — Discord Reconnect Status Hotfix

- Corrected misleading startup-status notifications following a Discord reconnect.
- Set the default restart scheduler value to disabled for newly generated configurations.

## v1.0.0 — Initial Public Release

- Initial public TROADiscordSEMonitor package.

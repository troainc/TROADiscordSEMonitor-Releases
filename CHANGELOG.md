# Monitor+ Changelog

## v1.1.5K — Restart Control Removed

- Removed Monitor+'s restart scheduler, timed restart and cancellation commands, countdown announcements, save-delay workflow, and Torch restart execution.
- Removed passive Essentials restart-schedule parsing and dashboard schedule output. Dashboards now report the server process start time instead.
- Removed restart-scheduler settings from newly generated and example configurations. Existing configurations remain compatible because obsolete XML elements are ignored during load.
- Manual Torch command forwarding remains available: administrators may still allow `restart` through `AllowedTorchCommands`, alongside the existing `save`, `stop`, and `start` entries.
- Restart management now belongs to the dedicated restart plugin; Monitor+ keeps ordinary monitoring, saves, backups, Discord reconnects, reloads, uptime reporting, and command forwarding.

## v1.1.4K.2 — Chat Bridge Stays in Its Own Lane

**Released:** September 14, 2026

- **Only real player chat is bridged to Discord now.** The in-game→Discord chat bridge previously relayed *all* global chat, including other plugins' system messages (e.g. Cleaner+ digests, TROA Profiler+ notices), which appeared in the Discord chat channel as if Monitor+ had sent them.
- The bridge now skips messages that carry no Steam identity (`AuthorSteamId == 0` — plugin/system senders, server broadcasts, and Monitor+'s own in-game advertisements), controlled by the new `RelayOnlyPlayerChat` setting (default `true`).
- Added `ChatBridgeIgnoredSenders` — an explicit list of in-game sender names never relayed to Discord, as a backstop for any plugin that chats under a real Steam ID. Defaults: `Cleaner+`, `TROA Profiler+`, `Monitor+`, `Server`.
- Set `RelayOnlyPlayerChat=false` to restore the old relay-everything behavior. Real player chat is unaffected.

## v1.1.4K.1 — Economy Connector, Discord Chat Colour Fix, Own-Lane Command Forwarding

**Released:** September 14, 2026

- **Economy connector (new, optional).** Players can check their in-game credit balance from Discord with `/monitorplus balance` and in-game with `!balance`. Monitor+ reads the server's built-in Space Engineers economy through reflection, so it takes no hard dependency on the economy and still loads on servers that do not run one. If no economy is active, the command says so instead of failing.
  - Off by default. Set `EnableEconomyConnector=true` to enable it. In Discord, `balance` resolves the caller through `DiscordSteamMappings`; in-game, `!balance` uses the calling player's own Steam account.
- **Discord chat colour fix.** The Discord username shown in-game was stuck in the default brown with no working way to change it. `DiscordGameTextColor` (message font) and `DiscordGameHighlightColor` (sender-name colour — a name, `#RRGGBB`, `#RRGGBBAA`, or `R,G,B`) are now honored. Defaults: `White` text, `#00B7C3` (teal) name.
- **Monitor+ stays in its own lane when forwarding commands.** When `AllowAnyTorchCommand=true`, Monitor+ previously grabbed *any* `!command` typed in the command channel — including commands owned by other plugins — and posted "Processing/accepted" chatter, an audit entry, and even re-badged the other plugin's own reply as a **Monitor+** embed. Now Monitor+ only posts Discord confirmations for its **own curated server commands** (the entries in `AllowedTorchCommands`, such as `save`/`restart`/`stop`/`start`). Any other forwarded command is passed to Torch quietly — no Discord chatter and no re-badging — and the activity is recorded in the **server log** instead. Commands in `AllowedTorchCommands` are unchanged.

> These are the first published releases to bundle everything since the August build: the grid-compliance on/off toggle (v1.1.2K), full slash-command parity and the complete command set (v1.1.3K), the grouped slash + in-game command model and slimmer config (v1.1.3.1K), plus the additions above. All of it ships in the v1.1.4K.2 binary.

## v1.1.3.1K — Command Model Rework: Grouped Slash Commands, In-Game Commands, Slimmer Config, Prettier Replies

Refines how commands are invoked and presented, and trims the configuration.

- **Grouped Discord slash commands.** Commands are now organised under two top-level slash commands: `/monitorplus <command>` for player commands and `/adminmonitorplus <command>` for administrator commands. `/adminmonitorplus` is hidden from non-administrators in Discord.
- **In-game player commands.** Player commands now work in Space Engineers game chat via `!`: `!server`, `!online`, `!rules`, `!discord`, `!support`, `!votelink`, `!topvoters` (alongside `!gridcheck`). Admin commands stay Discord-only.
- **Text fallback preserved.** The original `!command` form still works in the Discord command channel.
- **Audience-branded, prettier replies.** Replies are branded **Monitor+** for player commands and **Admin Monitor+** for admin commands, with a native Discord timestamp and a server-name footer. The server summary is a richer multi-line card (status, players, labelled simulation-speed rating).
- **Slimmer configuration.** The generated config drops from ~138 to ~62 settings — only channels, credentials, IDs, feature toggles, and the values owners actually tune remain. Cosmetic and rarely-changed settings became fixed sensible defaults. Existing configs still load; removed settings are ignored and use their defaults.

## v1.1.3K — Full Command Parity: Slash Commands and Complete Command Set

**Released:** September 4, 2026

Every command the documentation advertises now works on the Monitor+ build as both a text (`!`) command and a Discord **slash** command, with Monitor+ branded embed replies and webhook fallback.

- Restores Discord **slash commands**. Previously `/servercard` (and any `/` command) returned "The application did not respond" because the build only handled text commands — it never registered slash commands or answered interactions. The bot now registers its commands on connect (overwriting any stale registration), acknowledges interactions within Discord's 3-second window, and edits the reply in with the result.
- All command replies render as **Monitor+ branded embeds** (title, timestamp footer, colour by severity), whether invoked by slash command or text.
- Adds the public commands `server` / `online` (server summary), `rules`, `discord`, and `support` — no administrator authorization required.
- Adds player account linking: `link <Steam-ID-64>` and `link confirm <code>` link Discord to Steam via a one-time in-game code, so linked players can use `reward` without an administrator mapping.
- Adds the backups suite: `backups`, `backupnow`, `backupschedule`, `backupinfo <name>`, and `restorerequest <name>` (audited request only — never changes files).
- Adds `playerlookup <name-or-steam-id>`, `timezone <choice|list|status>` (with a slash-command time-zone picker), and `topvoters` (vote leaderboard).
- New configuration values: `PlayerRulesMessage`, `PlayerDiscordUrl`, `EnablePlayerSelfLinking`, `PlayerLinkCodeMinutes`, `PlayerDiscordSteamMappings`, `BackupDirectory`, `BackupListCount`, `EnableVoteLeaderboard`, `VoteLeaderboardCount`.

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
- Public `/servercard` with live players, world grid count, simulation speed, CPU, memory, storage, process uptime, voting, support links, and versions.
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

## v1.0.2 — Discord Reconnect Status Hotfix

- Corrected misleading startup-status notifications following a Discord reconnect.

## v1.0.0 — Initial Public Release

- Initial public TROADiscordSEMonitor package.

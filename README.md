# TROADiscordSEMonitor

`TROADiscordSEMonitor` is a Torch plugin for Space Engineers that bridges Discord and the server, reports live status, supports player voting and account linking, and gives administrators safe monitoring tools.

It targets **.NET Framework 4.8** and uses **C# 5-compatible** code.

## Features

- Discord ↔ in-game chat forwarding, faction channels, webhooks, advertisements, and player join/leave notices.
- Lifecycle status, startup progress, save notifications, Discord presence, and a persistent live-server dashboard.
- Correct Discord Gateway reconnect handling: reconnecting Discord while the Torch session is already loaded reports the server as online and does not replay a stale `Server is starting up.` message.
- Center-screen grid-compliance warnings for newly created grids, with reminders and a Discord audit log.
- Player-facing Discord commands for rules, support, server status, voting, rewards, account linking, and top voters.
- Secure player self-linking: a player confirms a short code shown only in-game before Discord and Steam are linked. Player links never grant administrator access.
- Vote rewards with a configurable cooldown and saved vote history/leaderboard.
- Health alerts for low simulation speed, low disk space, and stale save activity.
- Administrator tools for dashboard refresh, grid review, online player lookup, backup browsing, restore requests, announcements, saving, restarts, and command auditing.
- Optional Essentials restart tracking. The plugin's own restart scheduler is **off by default** for new configurations.

## Installation

1. Install the plugin package into Torch's plugin directory.
2. Start Torch once so it creates `TROADiscordSEMonitor.cfg`.
3. Stop the server and edit the configuration file.
4. Set at least `BotToken`, `ChatChannelId`, `CommandChannelId`, and the status/admin channel IDs you use.
5. Start Torch and use `!bridge-id` in a visible Discord channel to obtain channel and user IDs.

The package includes `TROADiscordSEMonitor.cfg.example` as a safe starting point. Do not publish a live `.cfg`: it contains the bot token and may contain vote-service credentials.

## Player Commands

Player commands can be used in `VotingCommandChannelId`. When that value is blank, they use `ChatChannelId`. They are also accepted in the configured command/admin channel.

| Command | Description |
| --- | --- |
| `!server`, `!status`, `!online` | Show server state, player count, and simulation speed. |
| `!rules` | Show `PlayerRulesMessage`. |
| `!discord` | Show `PlayerDiscordUrl`. |
| `!support` | Show the configured support URL and email. |
| `!votelink` | Show the configured Space Engineers vote page. |
| `!reward` | Verify and claim a vote reward for the linked Steam account. |
| `!link <Steam-ID-64>` | Start self-linking while that Steam player is online in-game. |
| `!link confirm <code>` | Confirm the one-time code displayed only to the player in-game. |
| `!topvoters` | Show the saved vote-reward leaderboard. |

`!reward` is a player command; a player does **not** need administrator access. Rewards require either a player self-link or an administrator-created mapping in `PlayerDiscordSteamMappings`.

## Administrator Commands

Administrator commands require either a Discord ID in `AdminDiscordUserIds` or a `DiscordSteamMappings` entry whose Steam ID is also in `AdminSteamIds`. A player mapping alone never grants administrator access.

| Command | Description |
| --- | --- |
| `!troasediscordhelp` | Show the command list in Discord. |
| `!seserver` | Refresh the live server dashboard card. |
| `!gridstatus` | List tracked grids that still need compliance work. |
| `!playerlookup <name-or-steam-id>` | Show the IDs for an online player. |
| `!backups` | List recent folders in `BackupDirectory`. |
| `!backupinfo <backup-name>` | Show the selected backup's size and timestamp. |
| `!restorerequest <backup-name>` | Record a manual restore request; it never changes files. |
| `!announce <message>` | Send an announcement to Discord and in-game. |
| `!save` | Forward a world-save request to Torch. |
| `!restartin <minutes> [reason]` | Schedule a save-first restart. |
| `!cancelrestart` | Cancel a scheduled restart. |
| `!addadmin <discord-id>:<steam-id-64>` | Add a Discord administrator mapping. |
| `!removeadmin <discord-id>` | Remove a Discord administrator mapping. |
| `!addport <game-port>` | Save the game port used by the dashboard. |
| `!reload` | Reload the plugin configuration and reconnect Discord. |
| `!bridge-id` | Return the current Discord channel ID and caller's Discord user ID. |

Permitted Torch commands can also be forwarded through Discord. Use `AllowedTorchCommands` to restrict them, or set `AllowAnyTorchCommand` only when that is intentional.

## Grid Compliance

When `EnableGridComplianceWarnings` is enabled, the plugin watches newly created grids. It sends the builder a centered in-game message explaining the requirements before the server's normal cleanup:

- a beacon;
- at least `GridComplianceMinimumBlocks` blocks (default: 25); and
- a name in the form `FACTIONTAG-GridName`.

It sends periodic reminders until the grid becomes compliant and writes an audit message containing the player name, Keen identity, grid name, and grid ID to `GridComplianceLogChannelId` (or `AdminLogChannelId` when blank). The monitor does **not** delete grids; the server's cleanup system remains responsible for deletion.

## Voting and Account Linking

Set these values to enable voting:

- `EnableVoting`
- `SpaceEngineersVotePageUrl`
- `SpaceEngineersVoteApiKey`
- `VoteRewardTorchCommand`
- `VotingCommandChannelId` (optional; falls back to `ChatChannelId`)

Use `{steamid}` in `VoteRewardTorchCommand` where the configured Torch reward command expects the player Steam ID. `VoteRewardCooldownHours` defaults to 24. Successful claims are stored under the plugin storage directory and power `!topvoters`.

For self-linking, leave `EnablePlayerSelfLinking` enabled and set `PlayerLinkCodeMinutes` as needed. The player must be online in-game for the code to be displayed. Links are stored in `PlayerDiscordSteamMappings`, separate from administrator mappings.

## Monitoring and Alerts

### Persistent Dashboard

Set `EnableStatusDashboard` and `StatusDashboardChannelId` to maintain a single live status card. It refreshes during startup/shutdown, player joins/leaves, and at `StatusDashboardIntervalMinutes`. Set `StatusDashboardAcceptCommands` to allow commands in that channel.

### Health Alerts

When `EnableHealthAlerts` is enabled, the plugin checks at `HealthAlertCheckMinutes` intervals and alerts `HealthAlertChannelId` (or `AdminLogChannelId` when blank) for:

- simulation speed below `HealthAlertMinimumSimSpeed`;
- disk space below `HealthAlertMinimumDiskGb`; and
- a save not observed within `HealthAlertSaveStaleMinutes`.

`HealthAlertCooldownMinutes` limits repeat alerts. A plugin cannot send a Discord alert after the server process itself has crashed; external process monitoring is required for that situation.

### Backups

Set `BackupDirectory` to the parent folder containing backup directories and use `BackupListCount` to limit results. `!backups` and `!backupinfo` are read-only. `!restorerequest` only logs an administrative request—it does not copy, restore, or delete any world files.

## Restart Behavior

`EnableRestartScheduler` defaults to `false` for **newly generated** configurations. Existing configurations keep their explicit setting.

When `EnableRestartScheduler=false`, the plugin does not create its automatic restart timer. The reconnect-status fix only prevents incorrect startup messages after a Discord Gateway reconnect; it does not cause or suppress server restarts.

Manual administrator restarts through `!restartin` are separate from the automatic scheduler and save the world first.

## Important Configuration Groups

| Area | Key settings |
| --- | --- |
| Discord bridge | `BotToken`, `ChatChannelId`, `CommandChannelId`, `CommandPrefix`, webhooks, chat formats |
| Status | `UseStatus`, `StatusChannelId`, startup-progress values, `UseDiscordPresence` |
| Player notices | `UsePlayerStatus`, `PlayerStatusChannelId`, `UseAdminPlayerLog`, `AdminLogChannelId` |
| Dashboard | `EnableStatusDashboard`, `StatusDashboardChannelId`, `StatusDashboardIntervalMinutes` |
| Compliance | `EnableGridComplianceWarnings`, `GridComplianceLogChannelId`, block/check/reminder settings |
| Player experience | `PlayerRulesMessage`, `PlayerDiscordUrl`, `ConnectSupportUrl`, `EnablePlayerWelcome` |
| Voting | `EnableVoting`, vote URL/API key, reward command, cooldown, leaderboard settings |
| Health | `EnableHealthAlerts`, alert channel, speed/disk/save thresholds and cooldown |
| Backups | `BackupDirectory`, `BackupListCount` |
| Administration | administrator IDs/mappings, allowed Torch commands, command audit settings |
| Restarts | `EnableRestartScheduler`, restart timing/messages, Essentials tracking |

When a configuration from an earlier version is loaded, the plugin adds missing new settings while retaining existing values. Review the generated `.cfg` after upgrading and fill in any new blank channel paths, URLs, or credentials.

## Security Notes

- Treat `BotToken` and `SpaceEngineersVoteApiKey` as secrets. Never commit or upload a live configuration.
- Keep `AdminDiscordUserIds`, `AdminSteamIds`, and `DiscordSteamMappings` restricted to trusted staff.
- Use `PlayerDiscordSteamMappings` for ordinary player reward links; it does not grant administrative access.
- Restrict `AllowedTorchCommands` and avoid enabling `AllowAnyTorchCommand` unless you understand the risk.
- Backup tools are intentionally non-destructive; restoring a world remains a manual server-owner procedure.

## Build

The included build script produces the private test package without copying any live configuration:

```powershell
.\build.ps1
```

The build requires Torch references and Space Engineers runtime dependencies. See `PROJECT.md` for the configured local dependency locations.


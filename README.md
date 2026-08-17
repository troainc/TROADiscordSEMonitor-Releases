# TROADiscordSEMonitor

TROADiscordSEMonitor is a native Discord bridge for Space Engineers Torch servers. It is designed for AMP hosts running Linux/Wine and also works with regular Torch installations.

The Discord bot connection is the primary delivery method. Webhooks are fallback-only and are not needed for normal chat, commands, embeds, or server notifications.

## About This Project

TROADiscordSEMonitor was built from the ground up to replace the outdated SEDiscordBot solution with a reliable, modern Discord bridge for Space Engineers Torch servers. Significant development time has gone into this plugin.

Feature requests are welcome and help guide future development. Please do not reupload, redistribute, or attempt to modify the plugin without TROAINC's prior written consent. The monitor is designed as a foundation that may also support additional game-server platforms in the future.

## Discord Bot Required

You must create a Discord application and bot before installing this plugin. Add that bot to your Discord server, enable its **Message Content Intent**, and enter its bot token and channel IDs in the plugin configuration. The monitor cannot send notifications, relay chat, or process Discord commands without a configured bot.

## Platform Support

TROADiscordSEMonitor supports both of these Torch hosting environments:

- **Windows:** Standard Windows-based Torch dedicated servers.
- **Linux/Wine:** AMP-managed Linux hosts running Torch through Wine.

Install the same plugin ZIP in Torch on either platform. Each server needs a compatible Torch and Space Engineers version, outbound HTTPS access on TCP port `443`, and its own Discord bot configuration.

## Copyright and License

Copyright (c) 2026 TROAINC. All Rights Reserved.

TROADiscordSEMonitor is proprietary software. No use, copying, modification, redistribution, reverse engineering, decompilation, or derivative work is permitted without TROAINC's prior written consent. See `LICENSE` for the full terms.

Fork requests must be emailed to `odin@therealmsofasgard.com` with the reason
for the request and why a fork is needed. Forking is prohibited unless
TROAINC provides prior written approval; TROAINC may deny any request.

## Features

- Two-way global chat relay between Space Engineers and Discord.
- Server starting, online, restart, and shutdown notifications.
- Player join/leave notices with Steam ID, Keen ID, and player count.
- Discord bot card with player count, player names, and simulation speed.
- Native Discord embeds for commands, output, server status, and the live server card.
- Remote Torch and compatible installed-plugin commands from a private Discord admin channel.
- Save notifications that distinguish automatic saves from Discord-admin requested saves.
- Save-first scheduled restarts, countdowns, and cancellation.
- Rotating Discord and in-game advertisements.
- One persistent `!seserver` dashboard card that updates every 30 minutes instead of creating channel clutter.
- CPU, memory, world size, sim speed, players, restart information, join address, and player-to-server latency.
- Essentials restart schedule detection.
- Space Engineers Server List voting, statistics, and optional reward claims.

## Requirements

- A working Torch Space Engineers server.
- A Discord bot application and bot token.
- Discord **Message Content Intent** enabled for the bot.
- Bot permissions: **View Channel**, **Read Message History**, and **Send Messages**.
- Outbound HTTPS/TCP port `443` from the server or AMP container.

## Installation

1. Stop the Space Engineers server.
2. Upload `TROADiscordSEMonitor.zip` to the Torch plugin area. Do not extract it.
3. Start Torch once so it creates `TROADiscordSEMonitor.cfg` in plugin storage.
4. Stop the server, add the bot token, channel IDs, administrator mappings, and optional service settings.
5. Start the server, or use `!reload` after later configuration changes.
6. Run `!bridge-id` in Discord to confirm the bot can see the channel and to obtain its IDs.

The plugin ZIP must contain exactly these root-level files:

- `TROADiscordSEMonitor.dll`
- `manifest.xml`

## Core Configuration

| Setting | Purpose |
| --- | --- |
| `BotToken` | Discord bot token. Keep it private. |
| `ServerName` | Name used in embeds, announcements, and bot messages. |
| `ChatChannelId` | Main two-way game chat channel. |
| `StatusChannelId` | Server lifecycle notification channel; falls back to chat. |
| `CommandChannelId` | Private command channel; multiple IDs may be comma-separated. |
| `AdminLogChannelId` | Detailed player and command audit channel; it can accept commands. |
| `CommandPrefix` | Command prefix, normally `!`. |
| `Embed` | Enables native Discord embeds for responses. |
| `StatusTimeZoneId` | Timezone for status and `{ts}` timestamps. Use `Eastern Standard Time` for US Eastern. |
| `ReconnectDelaySeconds` | Discord reconnect delay after a network interruption. |

Run this helper in any visible Discord channel:

```text
!bridge-id
```

It reports the Discord channel ID and your Discord user ID. It does not grant access.

## Administrator Access

Keep command channels private. Only trusted administrators should run remote Torch commands.

Administrator access can be verified by matching the sender's Discord user ID to their 64-bit Steam ID. This dual-identity check adds a security layer: a Discord account alone does not authorize remote server commands unless its mapped Steam ID is also approved in the configuration.

### Recommended Direct Discord Access

```xml
<AdminDiscordUserIds>
  <string>YOUR_DISCORD_USER_ID</string>
</AdminDiscordUserIds>
```

### Discord-to-Steam Mapping

```xml
<DiscordSteamMappings>
  <string>DISCORD_USER_ID:STEAM_ID_64</string>
</DiscordSteamMappings>
```

For administrator command access, the matching Steam ID must also be in `AdminSteamIds`, unless direct Discord access is used. Discord-to-Steam mappings are required for `!reward`, because vote rewards use the player’s Steam identity.

## Discord Commands

### Server and Bridge

| Command | Description |
| --- | --- |
| `!troasediscordhelp` | Shows the monitor command guide in Discord. |
| `!status` | Posts a detailed one-time server health embed. |
| `!seserver` | Immediately refreshes the single persistent server dashboard card. |
| `!votelink` | Posts the public Space Engineers Server List vote page. |
| `!reward` | Checks the caller’s mapped Discord ID for an eligible vote and runs the configured reward command. |
| `!announce <message>` | Sends an announcement to Discord and in-game. |
| `!addadmin <discord-id>:<steam-id-64>` | Adds an administrator and saves configuration. |
| `!addadmin <discord-id> <steam-id-64>` | Space-separated version of `!addadmin`. |
| `!removeadmin <discord-id>` | Removes administrator command access. |
| `!addport <game-port>` | Saves the Space Engineers port for the join address. |
| `!reload` | Reloads config and reconnects Discord without restarting Torch. |
| `!bridge-id` | Shows Discord IDs for setup verification. |

### Saves and Restarts

| Command | Description |
| --- | --- |
| `!save` | Requests a Torch save and reports completion. |
| `!restartin <minutes> [reason]` | Schedules a save-first restart. |
| `!cancelrestart` | Cancels the monitor’s scheduled restart. |

### Torch and Plugin Commands

Authorized administrators can run Torch commands and commands exposed by compatible installed Torch plugins.

```text
!lag inspect
!gridbackup list 76561198000000000
!gridbackup find "Grid Name"
!restart
```

Commands must use the exact syntax required by the installed plugin. Output returns to the same Discord command channel. Set `AllowAnyTorchCommand` to `false` to limit remote use to root command names in `AllowedTorchCommands`.

### Command Audit Trail

Every command received from Discord is recorded in the configured audit channel so server owners and administrators have a clear operational history. The audit entry identifies the Discord user who issued the command, the command text, when it was requested, and whether Torch or the target plugin accepted, rejected, returned output for, or could not find the command.

This provides accountability for remote administration: owners can verify saves, restarts, grid restores, profiling actions, and other administrative work without relying on memory or guessing who ran what. Keep `AdminCommandAuditChannelId` private and limit access to trusted staff, because its history may include sensitive operational details.

## Persistent `!seserver` Dashboard

Set `EnableStatusDashboard` to `true` to enable the main server card.

| Setting | Purpose |
| --- | --- |
| `StatusDashboardChannelId` | Channel that holds the persistent dashboard. |
| `StatusDashboardIntervalMinutes` | Refresh interval; default is `30` minutes. |
| `StatusDashboardAcceptCommands` | Allows authorized commands in the dashboard channel. |
| `ConnectWebsiteUrl` | Website shown in the dashboard's **CONNECT & SUPPORT** section. |
| `ConnectSupportUrl` | Support or ticket URL shown in the dashboard's **CONNECT & SUPPORT** section. |
| `ConnectSupportEmail` | Administrator/support email shown in the dashboard's **CONNECT & SUPPORT** section. |
| `ServerPort` | Space Engineers port used in the join address. Use `!addport` to set it. |

The plugin creates the card once, saves the Discord message ID in plugin storage, then edits the same message every update. `!seserver` refreshes that same card immediately. If the original message is deleted, the bot creates and tracks a replacement.

The **CONNECT & SUPPORT** section is editable through `ConnectWebsiteUrl`, `ConnectSupportUrl`, and `ConnectSupportEmail`. Leave a field empty to display `not configured` instead of publishing an incorrect link or email.

## Lifecycle Notifications

With `UseStatus` enabled, the monitor posts configured starting, online, restarting, and offline messages to `StatusChannelId` (or `ChatChannelId` when no status channel is set). The offline message is sent directly during normal Torch/AMP shutdown so it is delivered before the plugin is unloaded. A hard process kill, host crash, or network outage cannot send a final offline notice.

The card includes:

- Players online and daily unique players; daily count resets at midnight in `StatusTimeZoneId`.
- Simulation speed normalized to a maximum of `1.00`.
- Process CPU, memory, and world size.
- Server start time, last observed save, and four-hour restart-cycle reminder.
- Next monitor-scheduled restart or the next Essentials restart, whichever is sooner.
- Public join address and game port.
- Live player-to-server latency average, range, and sample count.
- Host, operating system, AMP/Linux/Wine detection, and timezone.
- Voting and rewards directly above **CONNECT & SUPPORT**.
- TROA website, support website, and administrator email.
- Torch version/build and the Space Engineers server's live AppVersion at the bottom of the card; Steam news is the fallback while the server is loading.

### Player-to-Server Ping

This is not an external internet ping test. The monitor reads live round-trip latency samples from the Space Engineers replication layer, so it represents connected players’ actual latency to the game server. It can show `waiting for live server data` or `not available` until the dedicated server exposes samples.

## Detailed `!status` Embed

`!status` is separate from the persistent dashboard. It posts a full on-demand health embed in the channel where it was run.

It reports Torch state/build, uptime, players, sim speed, CPU, memory, world size, free disk space, last save, world path, host details, network information, public IP, and timezone.

The color reflects sim speed:

- **Green:** `0.90` to `1.00`
- **Yellow:** `0.50` to `0.89`
- **Red:** below `0.50`

## Essentials Restart Tracking

When `EnableEssentialsRestartTracking` is true, the dashboard reads Essentials `AutoCommands` and finds the next restart. It supports recurring `Timed` schedules and fixed-time `Scheduled` schedules, including warning-step delays.

| Setting | Purpose |
| --- | --- |
| `EnableEssentialsRestartTracking` | Enables Essentials restart detection. |
| `EssentialsConfigPath` | Full Wine-visible path to `Essentials.cfg`; use this if automatic discovery fails. |

Run `!reload` after changing the Essentials path.

## Voting and Rewards

Voting uses the official Space Engineers Server List API. Keep its API key only in the server’s plugin configuration.

| Setting | Purpose |
| --- | --- |
| `EnableVoting` | Enables voting commands and dashboard information. |
| `SpaceEngineersVotePageUrl` | Public page opened by `!votelink`. |
| `SpaceEngineersVoteApiKey` | Private API key used for statistics and vote claim checks. Never post it in Discord. |
| `VoteStatisticsRefreshMinutes` | Statistics cache interval. The server-list service caches data for about three minutes. |
| `VoteRewardTorchCommand` | Optional Torch/plugin command executed after a valid vote. |

The card shows rank, votes, score, uptime, favorites, a clickable vote link, and a `!reward` reminder.

### Reward Flow

1. The player votes using `!votelink`.
2. The player runs `!reward` in a configured command-enabled channel.
3. The monitor checks the player’s Discord ID with the server-list API.
4. If an unclaimed vote exists, the monitor runs `VoteRewardTorchCommand`.
5. Only after that command succeeds does the monitor mark the vote as claimed.

`VoteRewardTorchCommand` supports:

| Placeholder | Value |
| --- | --- |
| `{steamid}` | Mapped player’s 64-bit Steam ID. |
| `{discordid}` | Player’s Discord user ID. |
| `{player}` | Player’s Discord display name. |

Leave `VoteRewardTorchCommand` blank while planning rewards. In that state, `!reward` confirms an eligible vote but does not mark it claimed or grant anything.

## Chat, Players, and Bot Card

### Chat Relay

- `ServerToDiscord` relays in-game global chat to Discord.
- `BotToGame` relays Discord messages to the game.
- `Format` controls game-to-Discord text; use `{p}` for player and `{msg}` for message.
- `Format2` controls the Discord sender label shown in-game.

### Player Notifications

Set `UsePlayerStatus` to true to post join/leave messages. Use `PlayerStatusChannelId`, or leave it blank to use the status/chat channel. `UseAdminPlayerLog` sends the detailed record to `AdminLogChannelId`.

| Placeholder | Meaning |
| --- | --- |
| `{p}` or `{player}` | Player name. |
| `{steamid}` | 64-bit Steam ID. |
| `{keenid}` | Keen identity ID when available. |
| `{count}` | Current online player count. |

### Player Identity Management

The admin connection log gives server owners a current identity record whenever a player joins or leaves. Each entry can include the player name, **Steam ID**, **Keen ID**, and current player count, so staff can investigate incidents with the identifiers that matter instead of relying only on display names.

- **Steam ID:** The player’s 64-bit Steam identifier. Use it as the durable player reference for administration, Discord-to-Steam authorization, bans, rewards, and cross-plugin commands.
- **Keen ID:** The currently observed Space Engineers identity identifier. Keen IDs can change as player identities are recreated or affected by game/server events, which can make historical investigations difficult. The monitor logs the latest observed Keen ID on each connection so staff can quickly find the player’s current in-game identity.
- **Discord ID:** The user’s Discord identifier. It is used to verify who issued Discord commands and, when configured with a Discord-to-Steam mapping, provides the extra authorization check for remote administration.

Configure `UseAdminPlayerLog` and `AdminLogChannelId` to keep this information in a private staff channel. Treat these logs as administrative records and restrict channel access to trusted staff.

### Discord Bot Card

Set `UseDiscordPresence` to true to update the Discord bot card.

| Setting | Purpose |
| --- | --- |
| `DiscordPresenceFormat` | Activity text; supports `{players}`, `{sim}`, and `{playerlist}`. |
| `DiscordPresenceShowPlayerNames` | Adds up to four player names when `{playerlist}` is not already in the format. |
| `DiscordPresenceUpdateSeconds` | Presence refresh interval; minimum is 15 seconds. |

Discord limits the activity to one short line. Steam IDs and Keen IDs are intentionally not shown on the public bot card.

## Saves, Timestamps, and Restarts

Set `UseSaveNotifications` to true for save messages.

| Setting | Purpose |
| --- | --- |
| `SaveCommandFormat` | Notice when a Discord admin requests a save. |
| `SaveCompletedFormat` | Notice for automatic/server saves. |
| `AdminSaveCompletedFormat` | Notice when a Discord-requested save completes. |
| `AdminSaveCorrelationSeconds` | Window associating a completed save with the Discord requester. |
| `EnableAutomaticSaveSchedule` | Enables the monitor’s regular Torch save schedule. |
| `AutomaticSaveIntervalMinutes` | Minutes between monitor-requested Torch saves; default is `15`. |
| `SaveCheckIntervalSeconds` | Silent save-file confirmation scan; default is `60` seconds and minimum is 30 seconds. |

The `{ts}` placeholder uses the configured timezone and a readable format such as `8/16/2026 9:54:33 PM EDT`, not a raw `-04:00` offset.

When `EnableAutomaticSaveSchedule` is true, the monitor runs Torch’s `save` command every `AutomaticSaveIntervalMinutes`. The monitor does not post a message every time it scans for a completed save; it only posts the configured completion notice after a save is actually detected. The scan remains in place so automatic saves and Discord-admin saves can be confirmed accurately.

For monitor-managed restarts, use `EnableRestartScheduler`, `RestartMinimumMinutes`, `RestartSaveWaitSeconds`, and the restart message formats.

## Advertisements and Startup Progress

Set `EnableAdvertisements` to true to rotate announcements.

- `DiscordAdvertisements` post to `AdvertisementChannelId` or `ChatChannelId`.
- `GameAdvertisements` send in-game messages as `AdvertisementAuthor`.
- `AdvertisementIntervalSeconds` controls the interval; minimum is 60 seconds.
- `AdvertisementColor` controls the in-game message color.

`UseStartupProgress` posts real Torch lifecycle milestones rather than a guessed loading percentage: bot connected, world loading, and server online. Configure the message percentages through `BotReadyProgressPercent`, `WorldLoadingProgressPercent`, and `OnlineProgressPercent`.

## Webhooks

Native bot posting is primary. `WebhookUrl`, `ChatWebhookUrl`, `StatusWebhookUrl`, and other webhook settings are only fallback options if normal bot posting fails. Webhooks are not required for chat, commands, dashboard updates, or voting.

## Troubleshooting

| Problem | What to Check |
| --- | --- |
| Bot is online but ignores commands | Enable Message Content Intent, verify command channel IDs, then use `!bridge-id`. |
| Command says unauthorized | Check `AdminDiscordUserIds`, `DiscordSteamMappings`, and `AdminSteamIds`, then run `!reload`. |
| Plugin command is rejected | Confirm it is registered with Torch and uses the exact required arguments. |
| Dashboard creates a replacement message | The prior persistent message was deleted or is unavailable to the bot. |
| Dashboard has no public IP | The AMP host cannot reach the public-IP service; hostname/LAN details remain available. |
| Player-to-server ping is unavailable | Wait for a player sample; the current dedicated-server build may not expose its replication table. |
| Vote stats say API key not configured | Set `SpaceEngineersVoteApiKey` in the live server config, then run `!reload`. |
| `!reward` finds no vote | Votes are limited to one per server per IP each day; confirm the Discord account is mapped and wait briefly for the server-list service. |
| `!reward` grants nothing | Configure `VoteRewardTorchCommand` with a valid Torch/plugin reward command. |
| Config change does not apply | Run `!reload` as an administrator or restart Torch. |

## Full Configuration Example

Download [`TROADiscordSEMonitor.cfg.example`](TROADiscordSEMonitor.cfg.example) for the complete, formatted configuration template. It includes every supported setting, placeholder values for Discord and Steam IDs, administrator mappings, dashboards, saves, restart tracking, voting, advertisements, and webhook fallback options.

**Before using it:** replace each `PASTE_*` value with your own value. Never publish or share a live bot token, server-list API key, administrator Steam ID, Discord user ID, or live configuration file.

After Torch first creates `TROADiscordSEMonitor.cfg`, you can use this template as a reference or copy its settings into the server's live configuration. Restart Torch or run `!reload` after saving configuration changes.

## Security

- Never publish your Discord bot token or server-list API key.
- Regenerate exposed secrets in their service dashboards.
- Keep `CommandChannelId` private.
- Give administrator mappings only to trusted staff.
- Treat `AllowAnyTorchCommand` and `VoteRewardTorchCommand` as powerful server-administration settings.
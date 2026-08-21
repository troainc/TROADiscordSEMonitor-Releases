# TROADiscordSEMonitor

**TROADiscordSEMonitor** is a native Discord integration, monitoring, administration, and player-services plugin for **Torch-powered Space Engineers Dedicated Servers**.

It connects a live Space Engineers server with Discord so server owners can monitor server health, relay chat, publish lifecycle events, maintain a persistent server dashboard, manage selected administrative workflows, track scheduled restarts, support voting and rewards, and provide players with useful self-service commands without requiring direct access to the Torch console.

The plugin targets **.NET Framework 4.8** and is written for **C# 5 compatibility** to remain compatible with the Torch and Space Engineers runtime environment. It is designed to work with traditional Windows Torch installations as well as AMP-hosted environments using Linux/Wine where the required Torch and Space Engineers dependencies are available.

> **Security first:** Never publish your live `TROADiscordSEMonitor.cfg`. It can contain your Discord bot token, voting API credentials, administrator mappings, channel IDs, and other server-specific information.

---

## Table of Contents

- [What the Plugin Does](#what-the-plugin-does)
- [Major Features](#major-features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Discord Bot Setup](#discord-bot-setup)
- [Initial Configuration](#initial-configuration)
- [Discord Commands](#discord-commands)
- [Administrator Authorization](#administrator-authorization)
- [Player Account Linking](#player-account-linking)
- [Persistent Server Dashboard](#persistent-server-dashboard)
- [Server Lifecycle and Startup Progress](#server-lifecycle-and-startup-progress)
- [Discord Reconnect Behavior](#discord-reconnect-behavior)
- [Chat Relay](#chat-relay)
- [Player Notifications](#player-notifications)
- [Grid Compliance](#grid-compliance)
- [Voting and Rewards](#voting-and-rewards)
- [Health Monitoring](#health-monitoring)
- [Save Monitoring](#save-monitoring)
- [Restart Management](#restart-management)
- [Essentials Restart Tracking](#essentials-restart-tracking)
- [Backup Visibility](#backup-visibility)
- [Advertisements](#advertisements)
- [Webhooks](#webhooks)
- [Security](#security)
- [Updating](#updating)
- [Troubleshooting](#troubleshooting)
- [Reporting Issues](#reporting-issues)
- [Building](#building)
- [Project Philosophy](#project-philosophy)

---

## What the Plugin Does

TROADiscordSEMonitor is more than a simple chat bridge. It is intended to give a Space Engineers community a useful Discord-facing operations layer while keeping Torch as the authoritative server-management platform.

The plugin can provide:

- Discord-to-game and game-to-Discord communication.
- Live server state and startup reporting.
- Player join/leave information.
- A persistent server dashboard rather than repeated status-message clutter.
- Server health and performance information.
- Controlled administrative command execution from Discord.
- Save and restart workflows.
- Essentials restart-schedule visibility.
- Player voting and configurable rewards.
- Secure Discord-to-Steam player linking.
- Grid-compliance notices and administrative audit information.
- Backup browsing and non-destructive restore-request workflows.
- Operational alerts and administrative logging.

The goal is to improve visibility and administration without turning Discord into an unrestricted remote shell for the game server.

---

## Major Features

### Discord Integration

- Native Discord Gateway connection.
- Discord REST communication.
- Two-way global chat relay.
- Discord bot presence/activity updates.
- Native Discord embeds for supported responses.
- Optional webhook fallback delivery.
- Configurable chat, status, command, audit, voting, dashboard, and alert channels.
- Automatic reconnect handling after temporary Discord/network interruptions.

### Space Engineers Monitoring

- Torch session state.
- Server online/offline lifecycle events.
- Real startup milestones.
- Online player count.
- Player names where enabled.
- Simulation speed.
- Process CPU and memory information.
- World/save information.
- Disk-space monitoring.
- Server uptime and host information.
- Live player-to-server latency where exposed by the Space Engineers replication layer.

### Administration

- Private Discord command channels.
- Discord administrator allowlisting.
- Discord-to-Steam administrator mapping.
- Configurable Torch command allowlist.
- Command auditing.
- Save requests.
- Save-first scheduled restarts.
- Restart cancellation.
- Server announcements.
- Configuration reloads.
- Online-player lookup.
- Backup browsing and restore-request logging.

### Player Services

- Server/status lookup.
- Rules and support information.
- Vote links.
- Vote reward claims.
- Secure player self-linking.
- Top-voter leaderboard.
- Optional welcome/player information.

---

## Requirements

At minimum you need:

- A working **Space Engineers Dedicated Server** managed by Torch.
- A compatible Torch installation.
- A Discord application with a bot user.
- A Discord bot token.
- Discord **Message Content Intent** enabled when command/message processing requires it.
- Permission to invite/configure the bot in the target Discord server.
- Outbound network access to Discord over HTTPS/TLS.
- Write access to the plugin's Torch storage directory.

Recommended Discord permissions include:

- View Channel
- Send Messages
- Read Message History
- Embed Links

Only grant additional permissions when a feature actually requires them. Administrator permission should not be necessary for normal operation and should not be granted merely as a troubleshooting shortcut.

### Runtime Compatibility

The plugin is built around:

```text
.NET Framework 4.8
C# 5-compatible source
Torch
Space Engineers Dedicated Server
Discord Gateway / REST APIs
```

---

## Installation

### Standard Torch Installation

1. Stop the Space Engineers/Torch server.
2. Obtain the current official TROADiscordSEMonitor release package.
3. Install the package in the Torch plugin location used by your server.
4. Start Torch once to allow plugin storage/configuration to be created.
5. Stop the server again before entering credentials.
6. Edit `TROADiscordSEMonitor.cfg`.
7. Configure the Discord bot token and required channel IDs.
8. Configure administrator access only for trusted staff.
9. Start Torch.
10. Run `!bridge-id` from a Discord channel visible to the bot to verify connectivity and obtain IDs.
11. Review the Torch log for configuration or connection errors.

Where the distributed package uses the standard Torch ZIP layout, the package contains the plugin DLL and manifest at the package root.

### AMP / Linux / Wine

TROADiscordSEMonitor can operate in AMP-hosted environments where Torch runs through Wine. Paths supplied to the plugin must be visible from the Wine/Torch environment. A host-native Linux path is not necessarily the same path Torch can access.

When configuring Essentials or backup paths, verify them from the runtime environment rather than assuming the host filesystem path is directly usable.

### First-Run Checklist

After installation verify:

- The plugin loads without a fatal exception.
- The Discord bot appears online.
- `!bridge-id` responds.
- The configured chat/status channels are correct.
- The server reaches the expected online state.
- Administrator-only commands reject unauthorized users.
- No secrets are visible in public Discord channels or logs you intend to share.

---

## Discord Bot Setup

Create or select a Discord application in the Discord Developer Portal and add a bot user.

Enable the intents required by your configuration. Message-based commands generally require **Message Content Intent**.

Invite the bot to the Discord server and grant only the permissions needed in the channels where it will operate.

### Finding IDs

Discord IDs are numeric identifiers for users, channels, and servers. TROADiscordSEMonitor includes a setup helper:

```text
!bridge-id
```

The command reports the current channel ID and the caller's Discord user ID. It does **not** grant administrative access.

### Bot Token Safety

Your bot token is effectively the bot's password.

Never:

- Commit it to GitHub.
- Paste it into a public issue.
- Post it in Discord.
- Include it in screenshots.
- Include it in logs sent to third parties.

If a token is exposed, regenerate it in Discord immediately and update the live plugin configuration.

---

## Initial Configuration

The live configuration is stored in `TROADiscordSEMonitor.cfg` under the plugin's Torch storage location.

Important core settings include:

| Setting | Purpose |
| --- | --- |
| `Enabled` | Enables or disables the plugin. |
| `BotToken` | Discord bot authentication token. Keep private. |
| `ServerName` | Friendly server name used in Discord output. |
| `ChatChannelId` | Main Discord/game chat bridge channel. |
| `StatusChannelId` | Server lifecycle/status channel. |
| `CommandChannelId` | Administrative command channel(s). |
| `AdminLogChannelId` | Administrative audit/player log channel. |
| `CommandPrefix` | Discord command prefix, normally `!`. |
| `UseStatus` | Enables lifecycle status messages. |
| `UseStartupProgress` | Enables startup milestone messages. |
| `UseDiscordPresence` | Enables bot presence/activity updates. |
| `ReconnectDelaySeconds` | Delay before Discord reconnection attempts. |
| `StatusTimeZoneId` | Timezone used for status timestamps. |

Channel settings that support multiple IDs should be configured according to the syntax expected by the current release.

### Configuration Reload

After changing supported settings, an authorized administrator can use:

```text
!reload
```

For major upgrades or changes involving plugin files, stop and restart Torch instead of relying solely on a live reload.

---

## Discord Commands

Available commands depend on configuration and release version.

### Player Commands

| Command | Description |
| --- | --- |
| `!server` | Shows current server information. |
| `!status` | Shows current server state/health information. |
| `!online` | Shows online/server information. |
| `!rules` | Shows the configured player rules message. |
| `!discord` | Shows the configured Discord/community information. |
| `!support` | Shows configured support information. |
| `!votelink` | Shows the configured server voting page. |
| `!reward` | Checks and claims an eligible vote reward. |
| `!link <Steam-ID-64>` | Starts secure player self-linking. |
| `!link confirm <code>` | Confirms the one-time in-game linking code. |
| `!topvoters` | Shows the stored vote-reward leaderboard. |

### Administrator Commands

| Command | Description |
| --- | --- |
| `!troasediscordhelp` | Displays the monitor's command help. |
| `!seserver` | Refreshes the persistent server dashboard. |
| `!gridstatus` | Shows grids still tracked for compliance. |
| `!playerlookup <name-or-steam-id>` | Looks up an online player's identifiers. |
| `!backups` | Lists recent backup directories. |
| `!backupinfo <backup-name>` | Shows information about a selected backup. |
| `!restorerequest <backup-name>` | Records a manual restore request without changing files. |
| `!announce <message>` | Sends an announcement to Discord and in-game. |
| `!save` | Requests a Torch world save. |
| `!restartin <minutes> [reason]` | Schedules a save-first restart. |
| `!cancelrestart` | Cancels a monitor-managed scheduled restart. |
| `!addadmin <discord-id>:<steam-id-64>` | Adds an administrator mapping. |
| `!removeadmin <discord-id>` | Removes administrator command access. |
| `!addport <game-port>` | Stores the game port used by the dashboard. |
| `!reload` | Reloads plugin configuration and reconnects Discord. |
| `!bridge-id` | Shows the channel ID and caller Discord ID. |

### Forwarded Torch Commands

Authorized administrators may be able to execute permitted Torch commands and commands exposed by compatible installed Torch plugins.

Examples:

```text
!save
!restart
!lag inspect
```

Exact command syntax is controlled by Torch or the plugin that owns the command. TROADiscordSEMonitor does not redefine third-party plugin syntax.

Use `AllowedTorchCommands` to restrict remote command roots. Do not enable unrestricted command forwarding unless you understand the security impact.

---

## Administrator Authorization

Player access and administrator access are intentionally separate.

### Direct Discord Administrators

Trusted Discord users can be placed in `AdminDiscordUserIds`.

### Discord-to-Steam Administrator Mapping

`DiscordSteamMappings` associates a Discord account with a Steam ID. Administrator access through this route also requires the mapped Steam ID to be authorized in `AdminSteamIds`.

A player account link does **not** automatically grant administrative privileges.

### Command Audit

When administrative command auditing is enabled, command activity can be sent to a dedicated audit/admin channel. This provides an operational record of who requested sensitive actions.

Administrators should periodically review:

- Discord administrator IDs.
- Steam administrator IDs.
- Discord-to-Steam administrator mappings.
- Allowed Torch commands.
- Command/audit channel permissions.

Remove access when it is no longer required.

---

## Player Account Linking

TROADiscordSEMonitor supports a player-facing Discord-to-Steam linking workflow for features such as vote rewards.

The self-linking flow is designed so possession of a Discord account alone is not enough to claim an arbitrary Steam identity.

Typical flow:

1. The player is online in Space Engineers.
2. The player runs `!link <Steam-ID-64>` in the appropriate Discord channel.
3. The plugin generates a short-lived confirmation code.
4. The code is displayed to that player in-game.
5. The player returns to Discord and runs `!link confirm <code>`.
6. The verified mapping is stored as a player mapping.

Player mappings are separate from administrator mappings and **do not grant Torch administrative command access**.

---

## Persistent Server Dashboard

Enable the dashboard with `EnableStatusDashboard` and configure `StatusDashboardChannelId`.

The dashboard is designed to maintain **one persistent Discord message**. Instead of posting a new status card every refresh, the plugin stores the message identity and edits that same card.

This reduces channel clutter and gives the community a predictable place to find current server information.

Depending on configuration and runtime availability, the dashboard can include:

- Server online state.
- Players online.
- Daily unique players.
- Simulation speed.
- CPU usage.
- Memory usage.
- World size.
- Last observed save.
- Server start/uptime information.
- Restart information.
- Essentials restart schedule information.
- Public join address and game port.
- Player-to-server latency samples.
- Host/runtime information.
- Voting statistics.
- Website and support information.
- Torch and Space Engineers version/build information.

Use:

```text
!seserver
```

to request an immediate refresh when authorized.

If the stored dashboard message is deleted or no longer available to the bot, the plugin may create and track a replacement.

---

## Server Lifecycle and Startup Progress

With lifecycle reporting enabled, TROADiscordSEMonitor reports meaningful Torch/Space Engineers states rather than pretending to know an exact game-loading percentage.

The default milestone model is:

```text
10%  - Discord bot connected; waiting for the server world.
60%  - World loading.
100% - Server online.
```

The percentages are configurable labels for real lifecycle milestones; they are not a byte-level measurement of world loading.

The configured lifecycle messages include starting, online, restarting, and offline states.

A graceful Torch/AMP shutdown gives the plugin an opportunity to send an offline notice. A hard process kill, host crash, power loss, or network outage may prevent a final Discord message because the process is no longer running to send it.

---

## Discord Reconnect Behavior

A Discord Gateway reconnect is **not** a Space Engineers server restart.

A bug was identified where a Discord reconnect could replay stale startup state and produce a confusing sequence such as:

```text
Server startup: 10% - Discord bot connected; waiting for the server world.
Server startup: 100% - Server online.
Server is online.
Server is starting up.
```

The reconnect-status fix synchronizes Discord with the actual Torch session state and prevents a loaded server from subsequently being reported as `Server is starting up.` merely because the Discord Gateway reconnected.

If an actual world load occurs, the normal world-loading lifecycle event is expected. If only Discord reconnects while Torch remains loaded, the plugin should report/synchronize the online state without falsely implying that the game server restarted.

---

## Chat Relay

The plugin can bridge global chat between Space Engineers and Discord.

Relevant settings include:

- `ServerToDiscord`
- `BotToGame`
- `ChatChannelId`
- `Format`
- `Format2`
- `GlobalColor`

Formatting placeholders allow the sender/player and message text to be inserted into configured formats.

Use dedicated channels and appropriate Discord permissions to avoid turning a public channel into an unintended administrative or spam surface.

---

## Player Notifications

Player join/leave reporting can provide:

- Player name.
- Steam ID.
- Keen identity ID when available.
- Current player count.

Public player-status messages and detailed administrative logs can be separated into different channels.

This allows communities to keep public messages readable while retaining richer operational information for staff.

---

## Grid Compliance

When grid-compliance monitoring is enabled, TROADiscordSEMonitor can watch newly created grids and warn builders about configured server requirements.

Current compliance concepts can include:

- Required beacon.
- Minimum block count.
- Naming convention such as `FACTIONTAG-GridName`.

The plugin can display centered in-game warnings/reminders and send an administrative audit record containing useful grid/player identifiers.

**TROADiscordSEMonitor does not need to be the component that deletes non-compliant grids.** The server's existing cleanup system can remain responsible for destructive cleanup while the monitor focuses on warning, visibility, and audit information.

This separation is intentional: monitoring and communication should not silently become destructive world-management behavior.

---

## Voting and Rewards

Voting integration can connect the server with the configured Space Engineers server-list voting service.

Important settings include:

- `EnableVoting`
- `SpaceEngineersVotePageUrl`
- `SpaceEngineersVoteApiKey`
- `VoteRewardTorchCommand`
- `VotingCommandChannelId`
- `VoteRewardCooldownHours`

### Typical Reward Flow

1. Player uses `!votelink`.
2. Player votes on the configured server-list page.
3. Player uses `!reward`.
4. The plugin resolves the player's verified Discord-to-Steam mapping.
5. The voting service is checked for an eligible vote.
6. The configured reward command is executed.
7. A successful claim is recorded for cooldown/history/leaderboard behavior.

The reward command can use supported placeholders such as the mapped Steam ID.

Leave the reward command blank while designing or testing rewards if you do not want claims to grant anything yet.

### Vote Security

Keep the vote API key private. It belongs in the server-side configuration, not in Discord, the README, screenshots, or public issues.

---

## Health Monitoring

When health alerts are enabled, the plugin can periodically evaluate server conditions and notify an administrative channel.

Monitored conditions can include:

- Simulation speed below a configured threshold.
- Available disk space below a configured threshold.
- No observed save within a configured period.

A cooldown prevents the same condition from flooding Discord repeatedly.

These alerts are **in-process monitoring**. If the entire Torch process crashes or the host becomes unreachable, the plugin cannot send a message after it has stopped running. Use external infrastructure/process monitoring when you need true host-down or process-down alerting.

---

## Save Monitoring

TROADiscordSEMonitor can distinguish between regular/automatic save activity and saves requested by a Discord administrator.

Relevant settings include:

- `UseSaveNotifications`
- `SaveCommandFormat`
- `SaveCompletedFormat`
- `AdminSaveCompletedFormat`
- `AdminSaveCorrelationSeconds`
- `EnableAutomaticSaveSchedule`
- `AutomaticSaveIntervalMinutes`
- `SaveCheckIntervalSeconds`

Save detection is used to confirm that a save actually occurred rather than assuming a command invocation means the world was successfully written.

---

## Restart Management

TROADiscordSEMonitor includes optional monitor-managed restart functionality.

### Safe Default

`EnableRestartScheduler` defaults to:

```xml
<EnableRestartScheduler>false</EnableRestartScheduler>
```

for **newly generated configurations**.

Existing configurations retain their explicitly saved value and should be reviewed during upgrades.

When `EnableRestartScheduler=false`, the plugin does not create its automatic restart timer.

### Manual Scheduled Restarts

Authorized administrators can use:

```text
!restartin <minutes> [reason]
```

for a save-first scheduled restart and:

```text
!cancelrestart
```

to cancel a pending monitor-managed restart.

Restart functionality should be intentionally enabled/configured. Server owners already using Essentials or another restart manager should avoid overlapping schedules unless they deliberately want multiple systems controlling restarts.

---

## Essentials Restart Tracking

TROADiscordSEMonitor can inspect Essentials restart configuration so the dashboard can show the next known Essentials-managed restart.

Relevant settings include:

- `EnableEssentialsRestartTracking`
- `EssentialsConfigPath`

This feature is intended for **tracking/visibility**. An Essentials schedule is separate from the monitor's own restart scheduler.

If automatic discovery does not work—especially under AMP/Wine—set `EssentialsConfigPath` to the path visible from the Torch/Wine runtime and reload the plugin.

---

## Backup Visibility

Backup-related commands are intentionally conservative.

With `BackupDirectory` configured, administrators can inspect available backup folders and basic backup information.

Commands include:

```text
!backups
!backupinfo <backup-name>
!restorerequest <backup-name>
```

`!restorerequest` records an administrative request. It does **not** automatically overwrite, delete, or restore live world files.

Actual restoration remains a deliberate server-owner procedure.

---

## Advertisements

Optional rotating advertisements can be sent to Discord and/or in-game.

Relevant settings include:

- `EnableAdvertisements`
- `AdvertisementIntervalSeconds`
- `AdvertisementChannelId`
- `AdvertisementWebhookUrl`
- `AdvertisementAuthor`
- `AdvertisementColor`
- `DiscordAdvertisements`
- `GameAdvertisements`

Use advertisements sparingly. Operational status channels are easier to use when they are not flooded with repetitive promotional messages.

---

## Webhooks

The native Discord bot connection is the primary communication path.

Webhook settings can provide fallback or specialized delivery where supported, including:

- `WebhookUrl`
- `ChatWebhookUrl`
- `StatusWebhookUrl`
- Feature-specific webhook settings

A webhook is not required for ordinary bot commands, chat bridging, dashboard updates, or other normal bot-driven behavior when the native Discord connection is functioning.

Webhook URLs are credentials. Treat them as secrets when they permit message posting.

---

## Security

TROADiscordSEMonitor exposes powerful integration points. Configure them deliberately.

### Never Publish

Do not publicly expose:

```text
Discord Bot Tokens
Voting API Keys
Webhook URLs containing secrets
Passwords
Authentication Tokens
Private Server Credentials
Live Configuration Files
```

### Remote Command Security

- Keep administrative command channels private.
- Restrict `AdminDiscordUserIds` to trusted staff.
- Review `AdminSteamIds` and administrator mappings regularly.
- Keep player mappings separate from administrator mappings.
- Restrict `AllowedTorchCommands`.
- Avoid unrestricted command forwarding unless explicitly required.
- Review command audit logs.
- Remove former staff access promptly.

### Principle of Least Privilege

The Discord bot should have only the Discord permissions it requires. The operating-system account running Torch should likewise have only the filesystem/network privileges required by Torch, Space Engineers, and configured plugin functionality.

### Secret Exposure Response

If a secret is exposed:

1. Revoke or rotate it immediately.
2. Update the live configuration.
3. Restart/reload the affected integration.
4. Remove the exposed value from public content where possible.
5. Review logs and administrator activity for unexpected use.

Deleting a message containing a token is not a substitute for rotating the token.

---

## Updating

Before updating a production server:

1. Stop Torch.
2. Back up the current plugin package.
3. Back up `TROADiscordSEMonitor.cfg` securely.
4. Review the release notes/changelog.
5. Install the new plugin version.
6. Compare new configuration options with your existing configuration.
7. Confirm restart-related settings before starting production.
8. Start Torch.
9. Verify Discord connectivity.
10. Verify the server reaches the online state.
11. Test player and administrator commands.
12. Review logs for warnings/errors.

Do not blindly replace a working configuration with an example configuration. Example files are templates and may contain defaults that differ from your production choices.

---

## Troubleshooting

| Problem | What to Check |
| --- | --- |
| Bot never appears online | Verify `BotToken`, outbound HTTPS/TLS access, Discord availability, and Torch logs. |
| Bot is online but ignores commands | Verify Message Content Intent and the configured command/channel IDs. Run `!bridge-id`. |
| Command reports unauthorized | Review `AdminDiscordUserIds`, administrator Steam mappings, and channel configuration. |
| Player cannot claim reward | Verify voting is enabled, the player is linked, the API key is configured, and cooldown/eligibility requirements are met. |
| Dashboard does not update | Verify dashboard is enabled, the bot can access the channel/message, and review logs. |
| Dashboard creates a replacement | The previous persistent dashboard message may have been deleted or become inaccessible. |
| Essentials restart is missing | Verify `EnableEssentialsRestartTracking` and `EssentialsConfigPath`, especially under Wine. |
| Unexpected restart messages appear | Confirm the current release includes the Discord reconnect-status fix and distinguish a Gateway reconnect from an actual Torch world-loading event. |
| Actual unexpected restarts occur | Review Torch/Essentials schedules, `EnableRestartScheduler`, restart commands, and logs immediately before the restart. |
| Config change does not apply | Use `!reload` where supported or restart Torch for major changes. |
| Ping/latency unavailable | Wait for live player samples; the current server runtime may not expose the required replication data. |
| Health alert did not report a process crash | In-process plugins cannot report after the process has died; use external monitoring. |

### Diagnosing Restart vs. Reconnect

A Discord reconnect and a server restart are different events.

For a suspected restart, collect:

- Torch log entries before and after the event.
- Timestamps of Discord status messages.
- Essentials restart configuration.
- Current `EnableRestartScheduler` value.
- Whether the normal world-loading lifecycle stage occurred.
- Whether players were actually disconnected.
- Whether the Torch/Space Engineers process restarted.

Do not diagnose a server restart solely from a Discord status message.

---

## Reporting Issues

When opening an issue, provide enough information to reproduce the behavior while removing all secrets.

Useful information includes:

```text
TROADiscordSEMonitor Version:
Torch Version:
Space Engineers Version:
Operating System / AMP / Wine Environment:
Problem Description:
Expected Behavior:
Actual Behavior:
Approximate Time of Event:
Steps to Reproduce:
Relevant Sanitized Torch Logs:
Troubleshooting Already Attempted:
```

Before posting configuration or logs, remove:

- Bot tokens.
- API keys.
- Webhook credentials.
- Passwords.
- Private authentication information.

For timing-related issues, include a few minutes of logs from **before and after** the event rather than only the final error line.

---

## Building

The source project targets the Torch/Space Engineers runtime and requires the appropriate Torch and Space Engineers references to build.

Where included in the source repository, the project build script can be run with:

```powershell
.\build.ps1
```

Build/development environments must provide the required Torch and Space Engineers dependencies. Never package a live production configuration, token, API key, or other secret into a release artifact.

The release package should contain only the files required for installation and should be tested on a non-production server before deployment.

---

## Project Philosophy

TROADiscordSEMonitor is built around several operational principles:

### Visibility Before Destruction

Monitoring features should inform administrators and players before taking destructive action. Grid compliance and backup tooling intentionally emphasize visibility, warnings, auditing, and deliberate administrative decisions.

### Secure Defaults

Features capable of changing server state should be opt-in or tightly controlled wherever practical. The automatic restart scheduler is disabled by default for newly generated configurations.

### Discord Is an Interface, Not the Authority

Discord provides a convenient management and community interface, but Torch and the Space Engineers server remain the authoritative runtime. Discord reconnects should never be mistaken for server restarts.

### Player and Administrator Identity Are Separate

A player linking Discord to Steam for rewards must not silently become an administrator. Administrative authorization is handled independently.

### Production Changes Should Be Observable

Administrative actions, restarts, saves, configuration changes, and health conditions should provide enough logging/auditing to diagnose problems without exposing secrets.

---

## Documentation

Additional installation, configuration, troubleshooting, FAQ, release-note, and changelog documentation is maintained in the project Wiki.

For production deployments, always review the documentation and release notes for the exact version you are installing.

---

## Copyright and License

Copyright (c) 2026 TROAINC. All Rights Reserved.

TROADiscordSEMonitor is proprietary software. Use and redistribution are governed by the license distributed with the project/release. Review the applicable `LICENSE` before copying, modifying, redistributing, reverse engineering, decompiling, or creating derivative works.

---

**TROADiscordSEMonitor**  
Space Engineers monitoring, Discord integration, player services, and controlled server administration for Torch-powered communities.

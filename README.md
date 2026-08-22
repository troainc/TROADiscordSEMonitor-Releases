# TROADiscordSEMonitor

## Your Space Engineers Server, Visible and Manageable From Discord

`TROADiscordSEMonitor` is a polished Discord companion for **Torch-powered Space Engineers servers**. It gives players clear server information and self-service tools, while giving owners live operational visibility, backups, safety controls, and auditable administration—without publishing private chat or server credentials.

Built for **.NET Framework 4.8** and **C# 5-compatible** Torch environments.

## Why Server Owners Use It

- Keep players informed with live server cards, slash commands, voting, links, and support tools.
- See the health of the server at a glance: population, simulation speed, grid count, saves, storage, network, versions, and restart information.
- Protect player privacy: only global game chat is bridged; faction and direct chat stays on the server.
- Keep grids clean with clear, in-game compliance reminders and staff audit visibility.
- Make safer operations practical: save-first restarts, announcements, health alerts, dated backups, and manual restore requests.
- Keep the configuration compact. It contains the channels and essential server details—not an ever-growing list of operational settings.

## Complete Feature Set

### Discord and Player Experience

- **Two-way global chat bridge** between Space Engineers and Discord.
- **Discord bot presence** updates with player count, simulation speed, and optional player names.
- **Player join/leave messages** can be separated into public player-status and staff audit channels.
- **Native Discord slash commands** with an autocomplete-style command menu. One-time replies are private; `/servercard` is public.
- **Beautiful branded embeds** with a single server-local date/time in the `TROA Monitor` footer.
- **Public server dashboard** via `/servercard`, showing live players, world grids, simulation speed, CPU/memory, storage, server address, restart state, voting, support links, and version information.
- **Quick player commands:** `!server`, `!online`, `!status`, `!rules`, `!discord`, and `!support`.
- **Player linking** lets players associate Discord with their Steam account using a short in-game confirmation code.
- **Voting tools:** vote link, reward claim, cooldown-aware tracking, reward history, ranking, score, and top-voter leaderboard.
- **Timezone support** for major North American, South American, European, African, Middle Eastern, Asian, and Pacific time zones. New configurations use Indianapolis/Eastern by default.

### World Protection and Privacy

- **Grid-compliance monitoring** checks new player grids for a beacon, required block count (25 by default), and `FACTIONTAG-GridName` naming.
- **Centered in-game reminder** tells the owner exactly what must be corrected before cleanup. Warning delivery is logged for staff.
- **NPC-safe by design:** NPC-created grids are excluded from player compliance reminders.
- **Staff compliance reports** list tracked grids and allow audit-log output to be enabled, disabled, or checked.
- **Private chat protection:** faction, direct/private, `/f`, and `./f` messages are never posted to Discord. They are retained in the Torch/server log only.

### Live Operations and Administration

- **Lifecycle reporting** gives meaningful 10% Discord-ready, 60% world-loading, and 100% online milestones.
- **Detailed status card** for administrators, including live world grid count, storage, current save time, host/network information, and Torch/Space Engineers versions.
- **Health monitoring** can alert staff to low simulation speed, low disk space, and stale saves.
- **Player lookup** helps staff identify online player names and IDs.
- **In-game announcements** can be sent from Discord and recorded in the command audit.
- **World save control** gives authorized staff a safe save request without direct server access.
- **Save-first restart controls** allow a timed restart with a reason, cancellation, and audit history.
- **Controlled Torch forwarding** lets owners expose only explicitly allowed Torch commands to trusted Discord administrators.
- **Channel and administrator setup tools** make it easy to collect IDs, set the server port, and maintain authorized staff mappings.
- **Essentials restart tracking** shows a detected Essentials schedule on the dashboard without taking control of it.
- **Optional webhooks** provide alternate delivery for chat, lifecycle, player, save, and advertisement messages.
- **Optional rotating advertisements** can be sent to Discord, in-game, or both.

### Backups and Recovery Workflow

- **Manual snapshots:** `!backupnow` copies the loaded world to a dated backup folder.
- **Automatic backup discovery:** blank `BackupDirectory` uses the world's sibling `Saves\\Backups` folder.
- **Owner-controlled scheduling:** back up after startup, every chosen number of minutes, or both. Interval scheduling has a five-minute minimum and defaults to off.
- **Backup browser commands** show detected snapshots, timestamps, sizes, and local locations in branded embeds.
- **Safe restore workflow:** restore requests are audited only; the plugin never changes world files automatically.
- **Host-friendly downloads:** use AMP or your server file manager for full snapshot downloads, avoiding Discord attachment-size limits.

### Reliable Startup and Restart Behavior

- **Gateway reconnect fix:** when Discord reconnects after the Torch world is loaded, the plugin synchronizes to the real session state and does not replay a stale `Server is starting up` message.
- **Clear progress notifications:** Discord ready, world loading, and server online are reported in the correct order during real startup.
- **Restart scheduler safety:** new configurations default `EnableRestartScheduler` to `false`. When it is false, the automatic restart timer does not start. Existing explicit settings are preserved.
- **Manual restarts remain separate** from the automatic scheduler and always request a save first.

## Install

1. Stop Torch.
2. Extract the release ZIP into Torch's plugin directory. Keep `TROADiscordSEMonitor.dll` and `manifest.xml` together.
3. Start Torch once to generate `TROADiscordSEMonitor.cfg` in plugin storage, then stop Torch again.
4. Enter your Discord bot token, required channel IDs, and administrator mappings.
5. Invite the Discord bot with both the `bot` and `applications.commands` scopes.
6. Start Torch. Run `!bridge-id` in Discord to display the current channel and your Discord user ID when configuring access.

Use a **full Torch restart** after replacing the DLL. `!reload` reloads configuration and reconnects Discord, but cannot load new plugin code.

## Essential Configuration

| Setting | Required | Purpose |
| --- | --- | --- |
| `BotToken` | Yes | Discord bot token. Keep it secret. |
| `ChatChannelId` | Yes | Public Discord/game chat bridge channel. |
| `CommandChannelId` | Yes | Discord channel for bot commands. |
| `AdminDiscordUserIds` | Yes | Discord user IDs allowed to run administrator commands. |
| `AdminSteamIds` and `DiscordSteamMappings` | Recommended | Links staff Discord identities to Steam identities. |
| `StatusDashboardChannelId` | Recommended | Channel used for the live server card. |
| `AdminLogChannelId` | Recommended | Channel used for audits and administrative events. |
| `GridComplianceLogChannelId` | Optional | Channel for grid-compliance audit records. |
| `HealthAlertChannelId` | Optional | Channel for health warnings. |
| `BackupDirectory` | Optional | Leave blank to automatically use `Saves\\Backups`. |

Never publish a live `.cfg` file. It may contain a bot token and voting API credentials. Run a separate bot token for each server; do not run two plugin instances using the same token at the same time.

## Player Commands

All supported commands are also available as slash commands. Use the configured prefix (normally `!`) for text commands.

| Command | What it does |
| --- | --- |
| `!server`, `!online` | Shows the public server state, player count, and simulation speed. |
| `!status` | Shows server status; administrators receive the detailed card. |
| `/servercard` | Publishes the polished public live server dashboard. |
| `!rules` | Shows the configured server rules. |
| `!discord` | Shows the configured community Discord link. |
| `!support` | Shows the configured website, support portal, and support email. |
| `!votelink` | Opens the Space Engineers server-list voting page. |
| `!reward` | Checks and claims an eligible vote reward. |
| `!topvoters` | Shows the voting leaderboard. |
| `!link <steam-id-64>` | Starts Discord-to-Steam account linking. |
| `!link confirm <code>` | Confirms the short code shown to the player in-game. |

## Owner and Administrator Commands

| Command | What it does |
| --- | --- |
| `!troasediscordhelp` | Shows the complete in-Discord command guide. |
| `!servercard` | Requests a dashboard refresh; use `/servercard` for the public card. |
| `!gridstatus` | Lists player grids needing compliance work. |
| `!gridlog on\|off\|status` | Controls Discord compliance audit output; in-game warnings continue. |
| `!playerlookup <name-or-steam-id>` | Finds an online player's IDs. |
| `!backups` | Lists detected backup folders with dates and sizes. |
| `!backupnow` | Creates a dated snapshot of the loaded world. |
| `!backupschedule status` | Shows startup and interval backup settings. |
| `!backupschedule startup on\|off` | Enables or disables a post-startup backup. |
| `!backupschedule every <minutes\|off>` | Sets a five-minute-or-greater interval, or disables it. |
| `!backupinfo <name>` | Shows a backup's date, size, and local path. |
| `!restorerequest <name>` | Records a restore request without changing files. |
| `!announce <message>` | Sends an in-game and Discord announcement. |
| `!save` | Requests a world save. |
| `!restartin <minutes> [reason]` | Schedules a save-first restart. |
| `!cancelrestart` | Cancels the pending manual restart. |
| `!addadmin <discord-id>:<steam-id>` | Adds an administrator mapping. |
| `!removeadmin <discord-id>` | Removes an administrator mapping. |
| `!addport <port>` | Sets the Space Engineers game port for the server card. |
| `!timezone <choice\|list\|status>` | Shows or changes the server time zone used in embeds. |
| `!reload` | Reloads configuration and reconnects Discord. |
| `!bridge-id` | Shows the current Discord channel and user IDs. |
| `!torch <allowed-command>` | Runs an explicitly allowed Torch command. |

## Safety, Privacy, and Limits

- The monitor **does not delete grids**. It communicates requirements and records compliance status for staff.
- The monitor **does not automatically restore backups**. A restore needs normal host/server-owner action with Torch stopped.
- The automatic restart scheduler is disabled by default. `EnableRestartScheduler=false` already prevents its timer from starting.
- Keep `AllowedTorchCommands` small and only grant administrator mappings to trusted staff.
- Full backups remain on the server. Download them through AMP or your host's file manager.

## Private Build

This private source repository produces release packages with `build.ps1`; the output ZIP is written under `release`. Public releases contain the plugin package and documentation only—never source code, live configuration files, or credentials.


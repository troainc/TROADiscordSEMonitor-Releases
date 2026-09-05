# Monitor+

## Your Space Engineers Server, Visible and Manageable From Discord

`TROADiscordSEMonitor` is a polished Discord companion for **Torch-powered Space Engineers servers**. It gives players clear server information and self-service tools, while giving owners live operational visibility, backups, safety controls, and auditable administration—without publishing private chat or server credentials.

Built for **.NET Framework 4.8** and **C# 5-compatible** Torch environments.

## How Commands Work

Commands run three ways, and replies come back as branded embeds (**Monitor+** for players, **Admin Monitor+** for admins):

- **Discord slash commands (recommended)** — grouped under two commands:
  - `/monitorplus <command>` — player commands, available to everyone.
  - `/adminmonitorplus <command>` — administrator commands, hidden from non-admins in Discord.
- **In-game (`!`)** — player commands work in Space Engineers game chat: `!server`, `!online`, `!rules`, `!discord`, `!support`, `!votelink`, `!topvoters`, `!gridcheck`.
- **Discord text fallback** — the classic `!command` form still works in the command channel.

## Monitor+ In-Game Identity and Save Messages

Player-facing system announcements use **Monitor+** by default. Server owners can still set `AdvertisementAuthor` to any name that fits their community. Existing installations using the old exact value `TROA` are migrated to `Monitor+`; custom values are left untouched.

## Why Server Owners Use It

- Keep players informed with live server cards, slash commands, voting, links, and support tools.
- See the health of the server at a glance: population, simulation speed, grid count, saves, storage, network, versions, and restart information.
- Protect player privacy: only global game chat is bridged; faction and direct chat stays on the server.
- Keep grids clean with clear, in-game compliance reminders and staff audit visibility.
- Make safer operations practical: save-first restarts, announcements, dated backups, and manual restore requests.
- Keep the configuration lean. It contains the channels and essential server details—not an ever-growing list of operational settings (about 60 settings, down from ~140).

## Complete Feature Set

### Discord and Player Experience

- **Two-way global chat bridge** between Space Engineers and Discord.
- **Discord bot presence** updates with player count, simulation speed, and optional player names.
- **Player join/leave messages** can be separated into public player-status and staff audit channels.
- **Grouped Discord slash commands** — `/monitorplus <command>` for players and `/adminmonitorplus <command>` for admins (hidden from non-admins). Player commands also work in-game with `!`; the classic `!command` form still works in the command channel.
- **Audience-branded embeds** — replies brand as **Monitor+** (players) or **Admin Monitor+** (admins), with a native Discord timestamp and a server-name footer.
- **Public server dashboard** via `/adminmonitorplus servercard`, showing live players, world grids, simulation speed, CPU/memory, storage, server address, restart state, voting, support links, and version information.
- **Quick player commands:** `server`, `online`, `rules`, `discord`, `support` (in Discord and in-game).
- **Player linking** lets players associate Discord with their Steam account using a short in-game confirmation code.
- **Voting tools:** vote link, reward claim, cooldown-aware tracking, ranking, and top-voter leaderboard.
- **Timezone support** for major North American, South American, European, African, Middle Eastern, Asian, and Pacific time zones.

### World Protection and Privacy

- **Optional feature, off with one setting.** The entire grid-compliance system is controlled by `EnableGridComplianceWarnings`. Leave it `true` to use the features below, or set it to `false` to stop every new-grid warning, reminder, and audit. The change applies after `!reload`; the player `!gridcheck` command stays available either way.
- **Grid-compliance monitoring** checks new player grids for a placed beacon, required placed block count (25 by default), and `FACTIONTAG-GridName` naming.
- **One teal centered in-game notice** tells the owner what must be corrected before cleanup. It is sent only once per newly tracked non-compliant grid.
- **Five-minute follow-up checks** keep monitoring non-compliant grids. Still-failing grids receive a teal in-game chat reminder and a staff audit entry.
- **Player grid checks:** players can type `!gridcheck` or `!gridcheck help` in Space Engineers chat to inspect only their major-owned grids.
- **NPC-safe by design:** NPC-created grids are excluded from player compliance reminders.
- **Staff compliance reports** list tracked grids and allow audit-log output to be enabled, disabled, or checked.
- **Private chat protection:** faction, direct/private, `/f`, and `./f` messages are never posted to Discord.

### Live Operations and Administration

- **Lifecycle reporting** gives meaningful Discord-ready, world-loading, and online milestones.
- **Detailed status card** for administrators, including live world grid count, storage, current save time, host/network information, and Torch/Space Engineers versions.
- **Player lookup** helps staff identify online player names and IDs.
- **In-game announcements** can be sent from Discord and recorded in the command audit.
- **World save control** gives authorized staff a safe save request without direct server access.
- **Save-first restart controls** allow a timed restart with a reason, cancellation, and audit history.
- **Controlled Torch forwarding** lets owners expose only explicitly allowed Torch commands to trusted Discord administrators.
- **Channel and administrator setup tools** make it easy to collect IDs, set the server port, and maintain authorized staff mappings.
- **Essentials restart tracking** shows a detected Essentials schedule on the dashboard without taking control of it.
- **Optional webhook** provides alternate delivery when a channel post fails.
- **Optional rotating advertisements** can be sent to Discord, in-game, or both.

### Backups and Recovery Workflow

- **Manual snapshots:** `backupnow` copies the loaded world to a dated backup folder.
- **Automatic backup discovery:** blank `BackupDirectory` uses the world's sibling `Saves\\Backups` folder.
- **Owner-controlled scheduling:** back up after startup, every chosen number of minutes, or both. Interval scheduling has a five-minute minimum and defaults to off.
- **Backup browser commands** show detected snapshots, timestamps, sizes, and local locations in branded embeds.
- **Safe restore workflow:** restore requests are audited only; the plugin never changes world files automatically.

## Install

1. Stop Torch.
2. Extract the release ZIP into Torch's plugin directory. Keep `TROADiscordSEMonitor.dll` and `manifest.xml` together.
3. Start Torch once to generate `TROADiscordSEMonitor.cfg` in plugin storage, then stop Torch again.
4. Enter your Discord bot token, required channel IDs, and administrator mappings.
5. Invite the Discord bot with both the `bot` and `applications.commands` scopes.
6. Start Torch. Run `/adminmonitorplus bridge-id` (or `!bridge-id`) in Discord to display the current channel and your Discord user ID when configuring access.

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
| `EnableGridComplianceWarnings` | Optional | Master on/off switch for World Protection and Privacy (grid-compliance) monitoring. `true` (default) sends new-grid warnings, reminders, and audits; set to `false` to turn the whole feature off. Takes effect on `!reload`. |
| `GridComplianceLogChannelId` | Optional | Channel for grid-compliance audit records. |
| `AllowAnyTorchCommand` | Optional | `false` (default) allows only `AllowedTorchCommands` from Discord; `true` forwards any Torch/plugin command for trusted admins. |
| `BackupDirectory` | Optional | Leave blank to automatically use `Saves\\Backups`. |

The generated config lists only the settings owners tune (about 60). Everything else uses sensible built-in defaults. Never publish a live `.cfg` file — it may contain a bot token and voting API credentials.

## Player Commands ( `/monitorplus <command>` )

| Command | What it does |
| --- | --- |
| `server`, `online` | Server summary: status, player count, and a labelled simulation-speed rating. |
| `rules` | Shows the configured server rules. |
| `discord` | Shows the configured community Discord link. |
| `support` | Shows the configured website, support portal, and support email. |
| `votelink` | Opens the Space Engineers server-list voting page. |
| `reward` | Checks and claims an eligible vote reward (Discord). |
| `topvoters` | Shows the voting leaderboard. |
| `link <steam-id-64>` / `link-confirm <code>` | Links a Discord account to Steam via a one-time in-game code (Discord). |
| `help` | Shows the command guide. |

Player commands `server`, `online`, `rules`, `discord`, `support`, `votelink`, `topvoters`, and `gridcheck` also work **in-game** with `!`.

## Owner and Administrator Commands ( `/adminmonitorplus <command>` )

Run these as `/adminmonitorplus <command>` (recommended) or with the `!` fallback in the command channel. Admin commands are Discord-only.

| Command | What it does |
| --- | --- |
| `help` | Shows the complete command guide. |
| `servercard` | Refreshes the live public server dashboard card. |
| `status` | Shows the detailed server status card. |
| `gridstatus` | Lists player grids needing compliance work. |
| `gridlog on\|off\|status` | Controls Discord compliance audit output; in-game warnings continue. |
| `playerlookup <name-or-steam-id>` | Finds an online player's IDs. |
| `backups` | Lists detected backup folders with dates and sizes. |
| `backupnow` | Creates a dated snapshot of the loaded world. |
| `backupschedule status\|startup on\|off\|every <minutes\|off>` | Shows or sets startup and interval backups (five-minute minimum). |
| `backupinfo <name>` | Shows a backup's date, size, and local path. |
| `restorerequest <name>` | Records a restore request without changing files. |
| `announce <message>` | Sends an in-game and Discord announcement. |
| `save` | Requests a world save. |
| `restartin <minutes> [reason]` | Schedules a save-first restart. |
| `cancelrestart` | Cancels the pending manual restart. |
| `addadmin <discord-id>` | Adds a Discord administrator. |
| `removeadmin <discord-id>` | Removes a Discord administrator. |
| `addport <port>` | Sets the Space Engineers game port for the server card. |
| `timezone <choice\|list\|status>` | Shows or changes the server time zone used in embeds. |
| `reload` | Reloads configuration and reconnects Discord. |
| `bridge-id` | Shows the current Discord channel and user IDs. |

## Safety, Privacy, and Limits

- The monitor **does not delete grids**. It communicates requirements and records compliance status for staff.
- The monitor **does not automatically restore backups**. A restore needs normal host/server-owner action with Torch stopped.
- Keep `AllowedTorchCommands` small and only grant administrator mappings to trusted staff.
- Full backups remain on the server. Download them through AMP or your host's file manager.

## Public Release Contents

This public repository contains release packages and documentation only. It does not contain private source code, live configuration files, bot tokens, voting API keys, webhooks, or other credentials.

# Monitor+

Current release: **v1.1.5K8** — Monitor+ owns monitoring and Discord transport; Econ+, Profiler+, Admin Overseer, Hangar+, GridVault, and Cleaner+ own their commands and webhooks.

## Your Space Engineers Server, Visible and Manageable From Discord

`TROADiscordSEMonitor` is a Discord bridge and server-monitoring plugin for **Torch-powered Space Engineers servers**. It provides server visibility, chat relay, account linking, and a reliable command path to the plugins that own game features.

Built for **.NET Framework 4.8** and **C# 5-compatible** Torch environments.

## How to Use Monitor+

A start-to-finish walkthrough for setting up and running Monitor+.

### 1. Create the Discord bot

1. Go to the [Discord Developer Portal](https://discord.com/developers/applications) → **New Application** → name it (e.g. "Monitor+").
2. Open **Bot** → **Reset Token** and copy the token. This is your `BotToken` — keep it secret.
3. Under **Bot**, enable the **Server Members Intent** and **Message Content Intent**.
4. Open **OAuth2 → URL Generator**, tick the **`bot`** and **`applications.commands`** scopes, then grant at least: View Channels, Send Messages, Embed Links, and Read Message History. Open the generated URL and invite the bot to your server.

### 2. Install the plugin

1. Stop Torch.
2. Extract the release ZIP into Torch's plugin directory (keep `TROADiscordSEMonitor.dll` and `manifest.xml` together).
3. Start Torch once to generate `TROADiscordSEMonitor.cfg` in plugin storage, then stop Torch again.

### 3. Configure it

Edit `TROADiscordSEMonitor.cfg` (see **Essential Configuration** below). At minimum set `BotToken`, `ChatChannelId` (public chat bridge), `CommandChannelId` (private admin channel), and your admin identities (`AdminDiscordUserIds` + `DiscordSteamMappings` + `AdminSteamIds`).

To get a channel or user ID, enable **Developer Mode** in Discord (User Settings → Advanced), then right-click a channel or user → **Copy ID**. Start Torch and run `/adminmonitorplus bridge-id` in your command channel to confirm the bot sees it.

### 4. Use the commands

See **How Commands Work** below for the three ways to run commands. Quick examples:

- Players: `/monitorplus server`, `/monitorplus rules` — or in-game `!server`, `!gridcheck`.
- Admins: `/adminmonitorplus status`, `/adminmonitorplus servercard`; send owner-plugin commands such as `!ova`, `!econadmin`, `!gridvault`, and `!cleanerplusadmin` in the configured command channel.

### 5. Common tasks

| I want to… | Do this |
| --- | --- |
| See server status | `/monitorplus server` (players) or `/adminmonitorplus status` (detailed) |
| Run an Econ+ command | Send the plugin's own command in the Monitor+ command channel |
| Back up or recover a grid | Use GridVault's `!gridvault` commands |
| Silence grid warnings | Set `EnableGridComplianceWarnings=false`, then `!reload` |
| Stop other plugins' chat reaching Discord | Leave `RelayOnlyPlayerChat=true` (default) — only real player chat is bridged |
| Let players link their account | They run `/monitorplus link <steam-id>`, then `/monitorplus link-confirm <code>` (code shown in-game) |
| Change the timezone on embeds | `/adminmonitorplus timezone` and pick a zone |
| Reload config without a restart | `/adminmonitorplus reload` (reloads config + reconnects; does not load new plugin code) |

> After editing the config, run `/adminmonitorplus reload` (or `!reload`). Replacing the plugin DLL still needs a full Torch restart.

## How Commands Work

Monitor+ commands use the Monitor+ presentation. Forwarded commands return the owning plugin's response text through the Discord bot:

- **Discord slash commands (recommended)** — grouped under two commands:
  - `/monitorplus <command>` — player commands, available to everyone.
  - `/adminmonitorplus <command>` — administrator commands, hidden from non-admins in Discord.
- **In-game (`!`)** — Monitor+ player commands work in Space Engineers game chat: `!server`, `!online`, `!rules`, `!discord`, `!support`, `!gridcheck`.
- **Discord text fallback** — the classic `!command` form still works in the command channel.

> Monitor+ is the command transport, not the owner of forwarded commands. Commands registered by Econ+, Profiler+, Admin Overseer, Hangar+, GridVault, and Cleaner+ pass through without command-list setup. Their replies are returned in the originating channel through Monitor+'s Discord bot connection; Monitor+ does not post them through its own webhooks. The chat bridge separately relays real player chat, not plugin system messages (see `RelayOnlyPlayerChat`).

Voting and rewards are handled by **TROA Admin Overseer**. To move existing Monitor+ reward reservations and voter history, start TROA Admin Overseer once to create its Rewards config, stop Torch, set `LegacyMonitorStorageDirectory` to Monitor+'s storage directory, then start a game session to import the data. Confirm the import in the TROA Admin Overseer log before removing Monitor+. Use its commands (`!ov vote`, `!ov claim`, `!ov rewards`, `!ov topvoters`; Discord `/adminoverseer reward`).

### Running other plugins' commands from Discord (e.g. Hangar+)

- **Administrators** can run commands registered by any of the six owner plugins in the command channel, including each plugin's admin and webhook-management commands. These commands do not need `AllowAnyTorchCommand` or an `AllowedTorchCommands` entry. Other Torch commands still follow those settings.
- **Linked players** (`!link <steam-id-64>` once) can run any Torch command declared player-level **as themselves**, even while offline. Monitor+ forwards the mapped Steam ID and refuses commands whose Torch permission level is above player. Commands needing an active character can still require in-game use.
- Commands that need your character in the world (store, load, claim, sell/look-at, LCD setup) only work in game. From Discord they reply with a short "use this in game" message.

## Monitor+ In-Game Identity and Save Messages

Player-facing system announcements use **Monitor+** by default. Server owners can still set `AdvertisementAuthor` to any name that fits their community. Existing installations using the old exact value `TROA` are migrated to `Monitor+`; custom values are left untouched.

## Why Server Owners Use It

- Keep players informed with live server cards, slash commands, links, and support tools.
- See the health of the server at a glance: population, simulation speed, grid count, saves, storage, network, versions, and process uptime.
- Protect player privacy: only global game chat is bridged; faction and direct chat stays on the server.
- Keep grids clean with clear, in-game compliance reminders and staff audit visibility.
- Keep monitoring, status, chat, and server communications available without taking over other plugins' operations.
- Keep the configuration lean. It contains the channels and essential server details—not an ever-growing list of operational settings (about 60 settings, down from ~140).

## Complete Feature Set

### Discord and Player Experience

- **Two-way global chat bridge** between Space Engineers and Discord. Only real player chat is bridged; other plugins' system messages are filtered out (`RelayOnlyPlayerChat`, default on).
- **Discord bot presence** updates with player count, simulation speed, and optional player names.
- **Player join/leave messages** can be separated into public player-status and staff audit channels.
- **Grouped Discord slash commands** — `/monitorplus <command>` for players and `/adminmonitorplus <command>` for admins (hidden from non-admins). Player commands also work in-game with `!`; the classic `!command` form still works in the command channel.
- **Audience-branded embeds** — replies brand as **Monitor+** (players) or **Admin Monitor+** (admins), with a native Discord timestamp and a server-name footer.
- **Public server dashboard** via `/adminmonitorplus servercard`, showing live players, world grids, simulation speed, CPU/memory, storage, server address, process uptime, support links, and version information.
- **Quick player commands:** `server`, `online`, `rules`, `discord`, `support` (in Discord and in-game).
- **Command pass-through:** sends supported plugin commands to Torch and returns their responses to the same Discord channel.
- **Player linking** lets players associate Discord with their Steam account using a short in-game confirmation code.
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
- **Plugin command transport** passes commands from Econ+, Profiler+, Admin Overseer, Hangar+, GridVault, and Cleaner+ directly to their registered Torch handlers. Responses return to the originating Discord channel through Monitor+'s bot connection; plugin webhooks remain owned by their plugins.
- **Channel and administrator setup tools** make it easy to collect IDs, set the server port, and maintain authorized staff mappings.
- **Optional webhook** provides alternate delivery when a channel post fails.
- **Optional rotating advertisements** can be sent to Discord, in-game, or both.

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
| `RelayOnlyPlayerChat` | Optional | `true` (default) bridges only real player chat to Discord; plugin/system chat (Cleaner+, TROA Profiler+, server broadcasts) is skipped. Set `false` to relay all global chat. |
| `ChatBridgeIgnoredSenders` | Optional | Extra list of in-game sender names never bridged to Discord (backstop for a plugin that chats under a real Steam ID). Defaults: `Cleaner+`, `TROA Profiler+`, `Monitor+`, `Server`. |
| `EnableGridComplianceWarnings` | Optional | Master on/off switch for World Protection and Privacy (grid-compliance) monitoring. `true` (default) sends new-grid warnings, reminders, and audits; set to `false` to turn the whole feature off. Takes effect on `!reload`. |
| `GridComplianceLogChannelId` | Optional | Channel for grid-compliance audit records. |
| `AllowAnyTorchCommand` | Optional | `false` (default) still passes commands owned by Econ+, Profiler+, Admin Overseer, Hangar+, GridVault, and Cleaner+, and explicit `AllowedTorchCommands` entries. Set `true` to pass any Torch command for authorized Discord administrators. Linked players can run player-level commands; higher Torch permission levels are refused. |

The generated config lists only the settings owners tune (about 60). Everything else uses sensible built-in defaults. Never publish a live `.cfg` file — it may contain a bot token or webhook URL.

Edit the generated file in Monitor+'s plugin storage folder. If a configured URL contains query parameters, XML normally requires `&amp;` between them; Monitor+ v1.1.5K7 repairs bare ampersands and preserves the URL automatically.

## Discord Channels

Monitor+ routes each kind of message to a configurable channel. Set the IDs in `TROADiscordSEMonitor.cfg` (enable Developer Mode in Discord, then right-click a channel -> Copy ID). Optional channels fall back to the chat channel when left blank.

| Setting | Purpose |
| --- | --- |
| `ChatChannelId` | Two-way public chat bridge between Discord and in-game global chat. |
| `CommandChannelId` | Private channel where administrators run bot/Torch commands. |
| `StatusDashboardChannelId` | The single live server dashboard card (`/adminmonitorplus servercard`). |
| `AdminLogChannelId` | Administrative audit events and staff join/leave logging. |
| `PlayerStatusChannelId` | Public player join/leave announcements. |
| `GridComplianceLogChannelId` | Grid-compliance audit records (initial notice + reminders). |

Keep the command channel private to staff -- Discord acts as an authenticated pass-through to Torch.

## Player Commands ( `/monitorplus <command>` )

| Command | What it does |
| --- | --- |
| `server`, `online` | Server summary: status, player count, and a labelled simulation-speed rating. |
| `rules` | Shows the configured server rules. |
| `discord` | Shows the configured community Discord link. |
| `support` | Shows the configured website, support portal, and support email. |
| `link <steam-id-64>` / `link-confirm <code>` | Links a Discord account to Steam via a one-time in-game code (Discord). |
| `help` | Shows the command guide. |

Player commands `server`, `online`, `rules`, `discord`, `support`, and `gridcheck` also work **in-game** with `!`. Economy commands belong to Econ+; vote and reward commands belong to TROA Admin Overseer.

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
| `announce <message>` | Sends an in-game and Discord announcement. |
| `save` | Requests a world save. |
| `addadmin <discord-id>` | Adds a Discord administrator. |
| `removeadmin <discord-id>` | Removes an administrator. |
| `addport <port>` | Sets the Space Engineers game port for the server card. |
| `timezone <choice\|list\|status>` | Shows or changes the server time zone used in embeds. |
| `reload` | Reloads configuration and reconnects Discord. |
| `bridge-id` | Shows the current Discord channel and user IDs. |

## Safety, Privacy, and Limits

- The monitor **does not delete grids**. It communicates requirements and records compliance status for staff.
- Monitor+ does not own grid backups or recovery; GridVault owns those operations.
- Each plugin owns its commands, command policies, and webhooks. Monitor+ only routes the command and response through Discord.
- **Player lane is permission-checked.** Linked players run commands as themselves and can run only commands Torch declares player-level. Monitor+ enforces `MinimumPromoteLevel`; admin commands remain available only to authorized Discord administrators. The chat bridge relays only real player chat, not other plugins' system messages.
- Keep `AllowedTorchCommands` small and only grant administrator mappings to trusted staff.

### TROA Digest administrator dashboard

Set `EnableStatusDashboard=true` and choose `StatusDashboardChannelId`. An authorized mapped administrator can use `!oval dashboard` or `/adminmonitorplus dashboard` to refresh the polished, persistent dashboard card. The card updates in place instead of filling the channel with duplicates; it uses the configured Monitor+ Discord connection and does not require a second webhook URL.

## Public Release Contents

This public repository provides release packages, operator documentation, and credential-free configuration examples.

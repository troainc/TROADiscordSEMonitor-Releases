# Monitor+

Current release: **v1.1.5K9**. Monitor+ handles server monitoring, Discord chat transport, account linking, and forwarding commands to the plugin that owns them. It does not own another plugin’s commands or webhooks.

## How to use Monitor+

### 1. Install and configure

1. Stop Torch and install the latest `TROADiscordSEMonitor` release ZIP in the Plugins folder.
2. Start Torch once so Monitor+ creates `TROADiscordSEMonitor.cfg`, then stop Torch.
3. Create a Discord bot, enable **Message Content Intent** and **Server Members Intent**, then invite it with permission to view channels, read message history, send messages, and embed links.
4. In the config, set `BotToken`, `ChatChannelId`, and `CommandChannelId`. Add administrator Discord IDs to `AdminDiscordUserIds`, matching Discord-to-Steam pairs to `DiscordSteamMappings`, and their Steam IDs to `AdminSteamIds`.
5. Start Torch and send `!bridge-id` in a channel the bot can read to confirm the bot connection and show the IDs needed for setup.
6. After changing settings, run `!adminmonitorplus reload` in the command channel.

Keep the bot token, webhook URLs, channel IDs, and live config private. The public [sample config](TROADiscordSEMonitor.cfg.example) uses placeholders; copy it as `TROADiscordSEMonitor.cfg` before editing.

### 2. Use player commands

In Discord, use the `!monitorplus` command group in the configured command channel. For example, send `!monitorplus server` or `!monitorplus rules`. Players can also use the supported short commands in Space Engineers game chat, such as `!server`, `!rules`, and `!gridcheck`.

To link a Discord account for player-level plugin commands, send `!monitorplus link <steam-id-64>`, then finish with `!monitorplus link-confirm <code>` using the code sent to you in-game.

### 3. Use administrator commands

Verified administrators use the `!adminmonitorplus` command group in the configured command channel. For example, send `!adminmonitorplus status`, `!adminmonitorplus gridstatus`, or `!adminmonitorplus reload`. The bot checks the Discord administrator list and matching Steam identity before running protected commands.

### 4. Forward commands owned by other plugins

Send the owning plugin’s command directly in the command channel. For example, use `!ova status` for Admin Overseer or the documented command root for Econ+, Profiler+, Hangar+, GridVault, or Cleaner+. Monitor+ forwards the command to Torch and returns the response in Discord. Each plugin remains responsible for its own features and webhook delivery.

Rewards and voting belong to Admin Overseer. Use its `!ov` player commands and `!ova` admin commands; Monitor+ does not provide a separate rewards system.

See the separate [command reference](COMMANDS.md) for Monitor+ commands and usage details. See the [changelog](CHANGELOG.md) for release notes.

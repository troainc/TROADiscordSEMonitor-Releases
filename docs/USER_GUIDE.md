# Monitor+ — server owner and player guide

Monitor+ is a Torch-to-Discord bridge for monitoring, global player chat, account linking, and approved command forwarding. It is not the owner of other plugins' commands, rewards, or webhooks. Plugin commands from Admin Overseer, Econ+, Profiler+, Hangar+, GridVault, and Cleaner+ should be sent to the owning plugin; Monitor+ forwards eligible requests and returns their replies.

## Configure Discord and Torch

Install the release in Torch and start once to create `TROADiscordSEMonitor.cfg`. Copy values from the credential-free [sample configuration](../TROADiscordSEMonitor.cfg.example). Create a Discord bot, invite it with the permissions needed to read the configured command/chat channels and send messages, then fill in the bot token and channel IDs. Configure administrator Discord IDs and Discord-to-Steam mappings. Never publish or share a live config; bot tokens, webhook URLs, and account mappings are private. Restart after first-time setup, then use `!adminmonitorplus reload` for supported live configuration reloads.

The default command prefix is `!`. Commands are typed in the configured Discord command channel; player and admin command lists are in the [command reference](../COMMANDS.md). Verify Discord and user IDs with `!adminmonitorplus bridge-id` (or `!bridge-id` where allowed).

## Chat and account linking

The default bridge relays global player chat to Discord. Faction and direct/private chat remain in game. Relay filtering can restrict messages to real player chat and ignore plugin/system senders. Configure the destination channel and test with a player message before enabling extra lanes. Account linking starts with `!monitorplus link <steam-id-64>` in Discord while the player is online; the player receives an in-game code, then completes it with `!monitorplus link-confirm <code>`. Linked identity allows a player to invoke only commands that Torch exposes to players. Admin commands remain restricted by configured administrator identity and command allow rules.

## Monitoring and administration

Players can request help, server summary, rules, invite, and support information in Discord or game chat. Administrators can inspect status, refresh server/digest dashboard cards, inspect grid compliance, toggle Discord compliance logging, announce, save, manage admin mappings and port, reload, look up players, and set the display timezone. Monitor+ can also publish configured status/startup/save/admin-audit notices and presence/dashboard updates. Each optional channel or feature can be disabled in config.

## Command forwarding and ownership

When a linked player sends an allowed player-level Torch command, Monitor+ forwards it using that Steam identity. A command needing an active character, a targeted grid, or an in-game look direction must be run in game. Named TROA plugin command roots are routed to their owner and should not be copied into Monitor+'s allow list. For example, use `!ova` for Overseer administration and the specific plugin's own root for its actions. Rewards and voting are owned by Admin Overseer.

## Troubleshooting

Check Torch startup logs for bot connection and channel permissions; confirm IDs are channel/user IDs, not names. If a command has no reply, verify the owning plugin is loaded, the command channel is correct, the account/role is authorized, and the command is valid in the current context. Do not broaden `AllowAnyTorchCommand` without understanding that it increases the command surface exposed through Discord. Keep replies in their originating Discord channel and treat logs as operational data.

# Monitor+ command reference

The Discord examples below use the default `!` prefix and are sent in the configured command channel. If `CommandPrefix` is changed, replace `!` with that prefix. Monitor+ also keeps short command aliases for compatibility. In-game commands are typed in Space Engineers chat.

## Player commands in Discord

| Command | Purpose |
|---|---|
| `!monitorplus help` | Show the Monitor+ command guide. |
| `!monitorplus server` | Show the public server summary. |
| `!monitorplus online` | Show the public server summary. |
| `!monitorplus rules` | Show the configured server rules. |
| `!monitorplus discord` | Show the community Discord link. |
| `!monitorplus support` | Show configured support details. |
| `!monitorplus link <steam-id-64>` | Begin linking your Discord identity to your Steam account. You must be online to receive the code. |
| `!monitorplus link-confirm <code>` | Complete account linking with the code sent in-game. `!monitorplus link confirm <code>` also works. |

After linking, a player may send a Torch command that is declared player-level. Monitor+ forwards it under that player's Steam identity and checks Torch permission metadata. Commands that need an active character or grid must still be run in game.

## Administrator commands in Discord

These commands require a configured Monitor+ administrator identity.

| Command | Purpose |
|---|---|
| `!adminmonitorplus help` | Show command help. |
| `!adminmonitorplus status` | Show detailed server status. |
| `!adminmonitorplus servercard` | Refresh the live server dashboard card. `seserver` is an alias. |
| `!adminmonitorplus dashboard` | Refresh the TROA Digest dashboard. |
| `!adminmonitorplus gridstatus` | List tracked grid-compliance issues. |
| `!adminmonitorplus gridlog <on|off|status>` | Control Discord grid-compliance audit messages. In-game warnings are unaffected. |
| `!adminmonitorplus announce <message>` | Send an announcement in game and Discord. |
| `!adminmonitorplus save` | Request a world save. |
| `!adminmonitorplus addadmin <discord-id>:<steam-id-64>` | Add an administrator identity mapping. |
| `!adminmonitorplus removeadmin <discord-id>` | Remove an administrator. |
| `!adminmonitorplus addport <game-port>` | Set the Space Engineers game port used in server details. |
| `!adminmonitorplus reload` | Reload Monitor+ configuration and reconnect the bot. |
| `!adminmonitorplus bridge-id` | Display the current Discord channel ID and your Discord user ID. `!bridge-id` also works in any channel the bot can read. |
| `!adminmonitorplus playerlookup <name-or-steam-id>` | Find an online player. |
| `!adminmonitorplus timezone <choice|list|status>` | View or change the timezone used for Discord timestamps. |

## Player commands in Space Engineers chat

| Command | Purpose |
|---|---|
| `!server` | Show the public server summary. |
| `!online` | Show the public server summary. |
| `!rules` | Show configured server rules. |
| `!discord` | Show the community Discord link. |
| `!support` | Show configured support details. |
| `!gridcheck` | Check your major-owned grids for the configured compliance requirements. |
| `!gridcheck help` | Explain grid-compliance requirements. |

## Commands owned by other plugins

Use each plugin's own command directly in the Discord command channel. Monitor+ forwards it to Torch and relays the result; it does not need a Monitor+ command registration or duplicate webhook configuration. Examples include `!ova status` for Admin Overseer and the command roots documented by Econ+, Profiler+, Hangar+, GridVault, and Cleaner+.

Voting and rewards are owned by Admin Overseer. Use its `!ov vote`, `!ov claim`, `!ov rewards`, and `!ov topvoters` player commands, and its documented `!ova` admin commands. Monitor+ no longer owns or handles those features.

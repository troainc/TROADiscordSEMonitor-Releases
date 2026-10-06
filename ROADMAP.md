# Monitor+ Roadmap

Monitor+ is the server monitoring and Discord transport layer. It relays global chat, links Discord and Steam identities, reports its own server health, and forwards text commands to the plugin that owns them. Other plugins own their commands, data, and webhooks.

## Current command interface

- `!monitorplus <command>` — player-safe Monitor+ commands in Discord.
- `!adminmonitorplus <command>` — administrator Monitor+ commands in Discord.
- In-game Monitor+ commands retain their documented `!` command forms.
- Monitor+ does not provide slash commands.

See [COMMANDS.md](COMMANDS.md) for the complete command list. See [README.md](README.md) for setup and use.

## Reliability priorities

1. Preserve exact command ownership while forwarding commands to Econ+, Profiler+, Admin Overseer, Hangar+, GridVault+, and Cleaner+.
2. Return each forwarded response to its originating Discord channel once, with clear timeout and failure feedback.
3. Keep faction and private chat out of the global Discord relay, and keep privileged output in authorized channels.
4. Report Monitor+'s own bridge, bot, queue, and reconnect health without claiming to own another plugin's status.

These are priorities, not release promises. Released behavior is recorded in [CHANGELOG.md](CHANGELOG.md).

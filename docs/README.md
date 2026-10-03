# Monitor+ documentation

Monitor+ connects Torch server monitoring and global player chat with Discord, links Discord accounts to Steam IDs, and forwards approved commands to the plugin that owns them. It does not own other plugins' commands, rewards, or webhooks.

- Start with [README: How to use Monitor+](../README.md#how-to-use-monitorplus).
- See the separate [command reference](../COMMANDS.md) for `!monitorplus` and `!adminmonitorplus` syntax.
- Copy the public-safe [sample config](../TROADiscordSEMonitor.cfg.example); keep tokens and IDs private.
- Check the [changelog](../CHANGELOG.md) for release-specific behavior.

Quick path: install the release ZIP in Torch, start once to generate the config, add bot/channel and account-link settings, restart, verify with `!bridge-id`, then reload changes with `!adminmonitorplus reload`. Only global player chat is relayed; faction and private chat remain in game.

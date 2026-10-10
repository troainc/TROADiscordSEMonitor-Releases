# Bifrost Plugin Panel — TROA Monitor+

## What this page is

Bifrost Plugin Panel is the operator interface hosted by TROA Admin Overseer inside the same Torch process as TROA plugins. Open the Panel itself through Admin Overseer; TROA Monitor+ does not need its own web server, Panel login, or a Panel URL. The plugin continues to own its settings, commands, events, metrics, and actions. Admin Overseer discovers loaded plugins through Torch and calls supported plugin-owned paths.

## Connect to the Panel

1. Install a TROA Admin Overseer release that includes Bifrost Plugin Panel and the matching public release of this plugin in the same Torch `Plugins` folder. Restart Torch after installing or updating either package.
2. Beside the other Admin Overseer configuration files, copy the example supplied with the Admin Overseer release as `TROA Admin Overseer Webserver.cfg` if the live file has not been created. Set `Enabled=true`, choose an unused `Port`, and select a `BindAddress`: `127.0.0.1` for local-only access, the server IPv4 address for one interface, or `0.0.0.0` for all IPv4 interfaces. Set `PublicUrl` to the address operators enter in their browser; it can be an IPv4 URL or domain and is separate from the bind address.
3. Allow the chosen port through the server firewall and any router or proxy. For access over an untrusted network, put HTTPS in front of the Panel with a reverse proxy; do not expose an unencrypted login page to the public internet.
4. Start Torch and wait for the green log message **“Bifrost Webpanel is online and ready to use.”** Open the configured `PublicUrl` (or the local address and port).
5. On first setup, retrieve the one-time owner code from `TROA Admin Overseer/BifrostPanel/OwnerSetupCode.txt` in the plugin storage path and create the owner login. After that, sign in with the account. The owner can create named Admin and Moderator accounts in **Access & roles**; use separate accounts rather than sharing the owner login.
6. Select **Plugin systems** from the side menu, find **TROA Monitor+**, and choose **Open workspace**. Use **Refresh** to request the latest data. The workspace is available only while Torch has loaded the plugin.

For screenshots, webserver options, account setup, and the full Panel guide, see the [TROA Admin Overseer public repository](https://github.com/troainc/TROA-Admin-Overseer) and its `docs/BIFROST-PANEL.md` guide.

## What operators can expect here

When supplied by the installed Monitor+ build, the workspace can show bot/bridge state and monitoring data the plugin exposes. Monitor+ transports supported Discord commands to their owning plugins; it does not take ownership of those commands or another plugin webhooks. Configure bot credentials and Discord routing in Monitor+ protected configuration, not in the Panel. Do not expect rewards or plugin-specific actions to appear unless their owner exposes them.

The exact cards depend on the plugin build and the data it reports. The Panel uses actual plugin-provided values and timestamps; it should label missing or old data as unavailable/stale rather than inventing readings. A chart only contains the history retained and exposed by the plugin.

## Settings, commands, and roles

- **Editable setting shown:** edit a value, choose **Review & save**, inspect the before/after values, validate, then confirm. Save and reload are sent through the owning plugin’s supported path. Review the result message before assuming the change took effect.
- **Read-only settings / Integration needed:** this loaded version has not exposed a safe save/reload operation for that setting. Use the plugin’s documented config and reload command instead; do not assume the Panel writes plugin files directly.
- **Commands:** use the workspace command list to inspect commands registered by this plugin. The **Command console** can submit only commands actually registered with Torch and permitted for your account. If a command is absent or rejected, use the plugin’s command reference and Torch/in-game command path; the Panel does not create or proxy unregistered commands.
- **Access:** owners control accounts and role policies. Admins can use only the actions granted to their role. Moderators are read-only. Hidden plugin/metric views and disabled actions are permission policy, not a connection failure.
- **Secrets:** configure tokens, webhook URLs, and other credentials through the plugin’s protected configuration path. Secret values are masked; a blank secret field preserves the stored value when the plugin integration supports that behavior.

## If the workspace is missing or blank

1. In Torch, confirm the plugin loaded without an initialization error and that the installed package version is the one expected.
2. In the Panel, refresh **Plugin systems** and check whether the plugin is listed as enabled/connected.
3. Open its workspace and read the status label. A read-only or integration-needed label means that build does not currently expose the requested save/reload path; it does not mean the plugin’s own commands/configuration stopped working.
4. Check the plugin’s Torch log and current-version README/command/config guide. Compare sample timestamps before diagnosing a graph as frozen.
5. Confirm your account’s view and action permissions in **Access & roles**. Ask the owner to adjust policy if a section or action is intentionally hidden.

TROA Monitor+ remains the source of truth for its domain. Panel availability is version-dependent; a feature listed in the plugin’s own guide does not automatically imply that it has a Panel control.

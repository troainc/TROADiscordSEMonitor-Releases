# TROADiscordSEMonitor v1.0.2

## Fixed

- Discord Gateway reconnects now synchronize lifecycle output with the actual Torch session state.
- A loaded server no longer receives a stale `Server is starting up.` status message after Discord reconnects.
- Startup and online notices are tracked as pending delivery only.

## Restart scheduler

New configuration files now default `EnableRestartScheduler` to `false`. Existing configurations retain their explicitly configured value. `EnableRestartScheduler=false` already prevents the monitor restart timer from starting.

## Package integrity

SHA-256: `3FA7ABC9F008C0A2D65DAC3088FC9FA7787358910083B5C96EB78369576602C1`

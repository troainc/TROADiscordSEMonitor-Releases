# Monitor+ Roadmap

Planned and in-progress work for **TROADiscordSEMonitor (Monitor+)**. Scope and timing may change — nothing here is a firm commitment.

## Next — v1.1.4K.3

### Online player list

- `!players` (Discord command channel + in-game, alias in-game `!who`) lists everyone currently connected with **name, Steam ID, and live ping**.
- Admin-gated — Steam IDs and ping are staff data. Long lists cap at 30 with an "N more not shown" tail.
- Per-player ping extends the existing replication-layer ping reflection to key ping per player, and falls back to `n/a` plus an aggregate ping line on hosts where per-player ping can't be resolved (e.g. some AMP / Wine setups).

## Under consideration

- `/players` slash parity under the `/adminmonitorplus` group.
- Optional public, names-only variant of the player list for players.
- Confirm per-player ping keying across AMP / Wine hosts.

## Requesting a feature

Open an issue on the repository or raise it in the staff channel.

---

Released history lives in [CHANGELOG.md](CHANGELOG.md).

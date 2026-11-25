# lib-log

The runtime-agnostic leveled logging **contract** — the port side of
the port/impl split (the `posix`/`stdio` convention). Backends
(`log-c` over libc stdio, `log-js` over the JS console) export arrows
of these types.

## Layout

| path | role |
| --- | --- |
| `src/index.hnl` | the .hnl contract — signatures + docs |
| `pkg.hnl` | package manifest |
| `scripts/hnl-gate.sh`, `workflows/gate.yaml` | the gate / bupha entry |

## Contract

- Levels `"trace" < "debug" < "info" < "warn" < "error" < "fatal"`;
  `level_rank` maps to 0..5 (-1 unknown).
- Structured attrs (`{ key: Value }` or `null`), merged
  child-before-event, right-hand keys win.
- Loggers are immutable `{ min_level, attrs }` records; `child` /
  `with_level` derive new ones; `should_log` is the pure filter.
- `log(l, lvl, msg, attrs)` + six per-level conveniences, all
  `~[1]~>`; `format(ts, lvl, msg, attrs)` is the canonical pure text
  rendering every backend shares.

## Verification

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.

<!-- myco:agent-awareness -->

## Agent awareness

- Session start / any work discussion: run `myco status --prompt`
  before proposing next steps — know the board first.
- User describes new work → offer to capture it (`myco todo add`,
  `issue new`, `proposal new`). Don't wait to be asked.
- Picking up a task: `myco resume <ID>` is the briefing.
- Wrapping up: `myco handoff <ID>` anything resumable; keep statuses
  honest (claimed → in-progress → blocked/done).

<!-- /myco:agent-awareness -->

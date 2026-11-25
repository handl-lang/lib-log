# lib-log

The runtime-agnostic leveled logging contract — the port side of the
port/impl split (the `posix`/`stdio` convention). `src/index.hnl`
declares signatures + docs; backends (`log-c` over libc stdio,
`log-js` over the JS console) export arrows of these types.

Model (aligned with Go `log/slog`, pino/winston, syslog severity):

- **Levels** — `"trace" < "debug" < "info" < "warn" < "error" <
  "fatal"`; `level_rank` maps to 0..5 (-1 unknown).
- **Attrs** — structured key-value records (`{ key: Value }` or
  `null`), merged child-before-event with right-hand keys winning.
- **Loggers** — immutable `{ min_level, attrs }` records;
  `child`/`with_level` derive new ones, `should_log` is the pure
  filter.
- **Emission** — `log(l, lvl, msg, attrs)` plus the six per-level
  conveniences, all `~[1]~>`; `format(ts, lvl, msg, attrs)` is the
  canonical pure text rendering every backend shares.

## Layout

```text
src/index.hnl       the .hnl contract (signatures + docs)
pkg.hnl             package manifest
scripts/hnl-gate.sh the gate
workflows/gate.yaml               bupha entry (handl-gates)
```

## Gate

```bash
HNL=../host-cli/target/debug/hnl scripts/hnl-gate.sh
```

## License

MIT.

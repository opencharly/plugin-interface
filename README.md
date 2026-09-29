# plugin-interface

The `interface` check verb for [opencharly/charly](https://github.com/opencharly/charly) —
probe a network interface's presence, MTU, and addresses via `ip` on the live
deployment.

The verb's `RunVerb` runs against the live check engine (`sdk/kit.CheckContext`),
so it is **compiled-in only**.

## What it provides

| Capability | Surface |
|---|---|
| `verb:interface` | the declarative `interface:` check step any candy or box can bake into its plan |

## The verb

An authored `interface: <name>` step (scalar sugar) or
`interface: {interface: …, mtu: …, addrs: …}` (map form). The
`interface`-exclusive fields live in the plugin's own `#InterfaceInput`
(`schema/interface.cue`).

| Field | Meaning |
|---|---|
| `interface` | the interface name to probe (the verb discriminator) |
| `mtu` | an optional required MTU |
| `addrs` | optional required addresses (substring match) |

The probe runs `ip -o addr show <iface>`; a missing interface fails with
`interface not found`. The MTU is read via `ip -o link show`.

```yaml
- check: the loopback interface exists
  id: interface-lo
  interface: lo
  context: [runtime]
```

## How to use it

Compose the plugin candy in a box or check bed's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-interface/candy/plugin-interface:<tag>'
```

## Layout

- `candy/plugin-interface/` — the plugin module: `plugin.go`,
  `schema/interface.cue` (the self-contained `#InterfaceInput`),
  `params/cue_types_gen.go`, `cmd/serve/main.go`.
- `candy/plugin-interface/charly.yml` — the `plugin-interface:` candy entity.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-check:check` — the check verb catalog. This candy
  carries no `skill:` entity of its own; the gap is tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.

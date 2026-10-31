---
title: "Overview"
weight: 10
---

# wasmdesk {#wasmdesk}

A desktop in the browser — a Wayland-inspired compositor and window manager, a dock, a login portal, OCI app packaging and a coreutils suite.

Part of the **Desktop & widgets** family of the
[pure-Go ecosystem](https://go-desktop.github.io/) — 7 modules,
all `CGO_ENABLED=0`.

## What is here {#what-is-here}

This site is the organisation's reference index: what each module is, and where its
API documentation lives. The API itself is generated from the source and served by
[pkg.go.dev](https://pkg.go.dev/), which is always current with the released tags —
duplicating it here would only create a second copy to go stale.

- **[Modules]({{< relref "modules.md" >}})** — every module in wasmdesk, with its source and its reference.

## Sound {#sound}

A browser worker has no `AudioContext`, so wasmbox relays client sound to the page. A
client writes interleaved float32 PCM into a lock-free single-producer/single-consumer
ring in a `SharedArrayBuffer` and sends `audio_open`; the page owns one `AudioContext`
and plays each ring from an `AudioWorkletNode` that reads it on the audio thread (no
`postMessage` per buffer), through one gain node per window. The context resumes on the
first click or key; rates, channel count and ring size are bounded and validated at both
hops. Quake and Doom play through it. Go clients import
[`github.com/wasmdesk/wasmbox/audioring`](https://pkg.go.dev/github.com/wasmdesk/wasmbox/audioring);
the wire format is in the wasmbox repository's
[`docs/protocol.md`](https://github.com/wasmdesk/wasmbox/blob/main/docs/protocol.md).

## What every module here is held to {#what-every-module-here-is-held-to}

- `CGO_ENABLED=0`: no cgo, and no shelling out to a command-line tool in place of a
  library.
- Built and tested on amd64, arm64, riscv64, loong64, ppc64le and s390x — the last
  being big-endian, which keeps every on-disk and on-wire encoding honest.
- 100% statement coverage as a CI gate, error branches included.
- BSD-3-Clause.

The standard is described in full on the
[ecosystem map](https://go-desktop.github.io/docs/latest/standards/).

# itsmygithubacct

I build systems software whose important behavior can be inspected and
reproduced. The public work ranges from integer-exact LLM inference and
byte-exact Bitcoin settlement to terminal desktops, native C games, and tools
for operating tmux sessions from both human and machine interfaces.

The recurring pattern is composition: establish a small, testable core; extract
stable boundaries; keep optional capabilities outside that core; and build
complete products from independently useful pieces.

## Portfolio

| System | Foundations | Products and extensions |
| --- | --- | --- |
| Verifiable computation and settlement | [integer_inference_engine](https://github.com/itsmygithubacct/integer_inference_engine), [chain_c](https://github.com/itsmygithubacct/chain_c) | [bonsai-notary](https://github.com/itsmygithubacct/bonsai-notary), [bsv_third_entry](https://github.com/itsmygithubacct/bsv_third_entry), [chain_c_wallet](https://github.com/itsmygithubacct/chain_c_wallet) |
| Terminal-native computing | [kilix](https://github.com/itsmygithubacct/kilix), [kitty-framebuffer](https://github.com/itsmygithubacct/kitty-framebuffer), [kitty-keyboard](https://github.com/itsmygithubacct/kitty-keyboard), [soft-raster](https://github.com/itsmygithubacct/soft-raster), [pcm-mixer](https://github.com/itsmygithubacct/pcm-mixer) | Desktop sessions, compatibility layers, media tools, and nine native games |
| tmux operations and automation | [tmux-cli](https://github.com/itsmygithubacct/tmux-cli), [tmux-browse](https://github.com/itsmygithubacct/tmux-browse) | Agent, sandbox, LAN federation, and QR-sharing extensions |

## Verifiable computation and settlement

### [bonsai-notary](https://github.com/itsmygithubacct/bonsai-notary)

Bonsai Notary composes deterministic inference with cryptographic receipts and
an on-chain third entry. A generation is bound to a committed model artifact,
input, output, and trace; another machine can re-execute the committed artifact
and verify the same bytes.

The composition deliberately keeps its major parts independently versioned:

- **[integer_inference_engine](https://github.com/itsmygithubacct/integer_inference_engine)**
  is an integer-only engine for low-bit LLMs. A CPU integer oracle is the
  canonical verifier, while native C and CUDA producers are checked for
  byte-exact parity. The guarantee is scoped honestly: inference from a
  committed artifact is reproducible; re-importing a floating-point source
  model is not assumed to recreate that artifact.
- **[chain_c](https://github.com/itsmygithubacct/chain_c)** is a roughly
  29,000-line C11 port of a TypeScript/sCrypt Bitcoin SV chain layer. It
  reconstructs contracts and transaction artifacts byte for byte against
  golden vectors, uses established cryptographic libraries, exposes nine CLIs,
  and keeps real broadcasts behind explicit confirmation gates.
- **[bsv_third_entry](https://github.com/itsmygithubacct/bsv_third_entry)**
  is the Python orchestration boundary between inference receipts and the
  chain_c CLIs. It supports persistent agent identities and one-shot
  publication while remaining dry-run by default.
- **[chain_c_wallet](https://github.com/itsmygithubacct/chain_c_wallet)** is a
  pure-C command-line wallet over the same chain core. It adds key lifecycle,
  an encrypted vault, address management, and safe spend orchestration without
  reimplementing security-bearing transaction or cryptographic code.

This stack is about more than repeatable output. It separates determinism from
model fidelity, treats receipts as re-executable claims, and verifies
cross-language parity at the serialized-byte boundary.

## Terminal-native computing

### The Kilix desktop and application stack

**[kilix](https://github.com/itsmygithubacct/kilix)** wraps a maintained
**[kitty fork](https://github.com/itsmygithubacct/kitty)** with Tilix-style
clickable pane controls, pages, browsing, GUI-app streaming, session sharing,
and a host SDK for desktop providers.

The surrounding repositories take that terminal from application to
environment:

- **[Pleb](https://github.com/itsmygithubacct/pleb)** installs a fullscreen
  Kilix kiosk session alongside an existing Linux desktop.
- **[Plebian OS](https://github.com/itsmygithubacct/plebian-os)** provisions a
  regular Debian installation whose graphical session is Pleb, with pinned,
  rollback-safe component updates.
- **[Kilix 95](https://github.com/itsmygithubacct/kilix-95)** is a
  framebuffer-rendered Windows 95/XP-style desktop provider with its own
  widgets, windows, settings, applications, and isolated runtime state.
- **[dosbox-kilix](https://github.com/itsmygithubacct/dosbox-kilix)** is a
  DOSBox-X derivative with opt-in Kitty and ANSI terminal output backends,
  direct keyboard/mouse input, remote-terminal operation, and no requirement
  for X11 or Wayland on the emulation host.
- **[kilix-amp](https://github.com/itsmygithubacct/kilix-amp)** is a C11/SDL2
  Winamp 2.x recreation with skins, a ten-band equalizer, spectrum analysis,
  playlists, MIDI playback, and a waveform editor.

### Reusable C runtime

The games led to a set of small libraries with narrow ownership:

| Repository | Contract |
| --- | --- |
| **[kitty-framebuffer](https://github.com/itsmygithubacct/kitty-framebuffer)** | Threaded, newest-frame-wins RGBA presentation through the Kitty graphics protocol, with synchronized updates and crash-safe terminal restoration |
| **[kitty-keyboard](https://github.com/itsmygithubacct/kitty-keyboard)** | Allocation-free Kitty keyboard parser plus optional POSIX lifecycle, including independent press, repeat, release, and held-key state |
| **[kitty-terminal-session](https://github.com/itsmygithubacct/kitty-terminal-session)** | Correctly ordered composition of framebuffer and keyboard ownership, including normal and emergency shutdown |
| **[soft-raster](https://github.com/itsmygithubacct/soft-raster)** | Pure C11 framebuffer primitives, alpha sprites, bitmap text, PPM loading, and letterbox scaling with byte-pinned blending behavior |
| **[pcm-mixer](https://github.com/itsmygithubacct/pcm-mixer)** | Background PCM mixing, pitched voices, controlled loops, music crossfades, live generators, strict WAV loading, and graceful command-line sink failure |

These are intentionally not a monolithic engine. Presentation, input,
rasterization, audio transport, and game policy remain separable and testable.

### Games

Nine released games exercise that stack across very different simulations:

| Project | What it is |
| --- | --- |
| **[Bashed Earth](https://github.com/itsmygithubacct/bashed-earth)** | Falling-sand artillery with destructible terrain, water, weather, fire, an economy, and five AI personalities |
| **[Chess Bash](https://github.com/itsmygithubacct/chess-bash)** | Animated isometric chess with complete rules, battle sequences, Stockfish support, and a built-in fallback engine |
| **[Joustix](https://github.com/itsmygithubacct/joustix)** | Flying-joust arcade combat with held-key input, enemy classes, eggs, waves, and lava hazards |
| **[Kilix Fishtank](https://github.com/itsmygithubacct/kilix-fishtank)** | An arcade aquarium with real fish species, shark attacks, fishing, boat battles, and a lightweight water simulation |
| **[Kilix Pong](https://github.com/itsmygithubacct/kilix-pong)** | Deterministic paddle-ball with local two-player input, bounded collision substeps, and a reproducible fixed-point sound generator |
| **[Kilix Rancher](https://github.com/itsmygithubacct/kilix-rancher)** | A creature-raising game with weekly care decisions, training, real-time positional battles, rivals, and progression |
| **[Kitty Brokeout](https://github.com/itsmygithubacct/kitty-brokeout)** | Breakout with fixed-step physics, varied bricks, powerups, multiball, particles, and deterministic tests |
| **[Pleb Driver](https://github.com/itsmygithubacct/pleb-driver)** | A native terminal racer with projected terrain, traffic, jumps, boost, fixed-step physics, and a speed-responsive audio mix |
| **[Terminal Lander](https://github.com/itsmygithubacct/terminal_lander)** | Lunar Lander with four difficulty models, physical sound banks, exact held controls, and headless simulation checks |

The games render real pixels without SDL, X11, or ncurses in their runtime
path. Most expose deterministic headless simulation, render, input, and audio
checks for CI. Their audio work combines original deterministic synthesis with
provenance-tracked CC0 Foley, cue-by-cue human audition, in-process mixing, and
safe silence or procedural fallback when a sink or asset is unavailable.

## tmux operations and automation

**[tmux-cli](https://github.com/itsmygithubacct/tmux-cli)** provides tb, a
stdlib-only Python CLI for reading, writing, creating, and managing tmux
sessions. Its tables serve people; its stable JSON envelopes, distinct exit
codes, and command-output capture serve automation and LLM tool loops.

**[tmux-browse](https://github.com/itsmygithubacct/tmux-browse)** builds a web
dashboard over the same core, presenting local sessions as embedded terminals
with ordering, alerts, controls, authentication, and TLS support. Optional
capabilities stay in separately versioned repositories:

- **[tmux-browse-agent](https://github.com/itsmygithubacct/tmux-browse-agent)**
  adds agent runs, workflows, scheduling, a conductor, tools, CLI verbs, and UI
  surfaces.
- **[tmux-browse-sandbox](https://github.com/itsmygithubacct/tmux-browse-sandbox)**
  isolates agent tool calls inside a Docker-hosted tmux environment.
- **[tmux-browse-federation](https://github.com/itsmygithubacct/tmux-browse-federation)**
  adds explicit-pairing LAN discovery and aggregates sessions from multiple
  hosts.
- **[tmux-browse-qr](https://github.com/itsmygithubacct/tmux-browse-qr)**
  round-trips dashboard configuration between devices using QR codes.

The extension split is deliberate: a single-host dashboard remains lean and
stdlib-only, while network discovery, Docker, browser scanning, and model
execution are opt-in.

## Engineering principles

- Define the exact boundary of a guarantee; do not let “deterministic” or
  “verified” imply more than the implementation proves.
- Prefer small components with closed contracts over convenience-layer
  duplication or a monolith.
- Make failure modes explicit: dry-run money operations, fail-closed
  publication, transactional updates, bounded queues, safe terminal restore,
  and silent degradation for optional audio.
- Give humans and automation first-class interfaces to the same capability.
- Pin byte-level behavior where it matters, but test user-facing assets as a
  person experiences them.
- Treat provenance, tests, and documentation as product surfaces.

## Start here

- Verifiable inference and on-chain receipts:
  **[bonsai-notary](https://github.com/itsmygithubacct/bonsai-notary)**
- A complete terminal desktop:
  **[kilix](https://github.com/itsmygithubacct/kilix)** and
  **[Plebian OS](https://github.com/itsmygithubacct/plebian-os)**
- Building a native Kitty-protocol application:
  **[kitty-framebuffer](https://github.com/itsmygithubacct/kitty-framebuffer)**,
  **[kitty-keyboard](https://github.com/itsmygithubacct/kitty-keyboard)**, and
  **[soft-raster](https://github.com/itsmygithubacct/soft-raster)**
- Operating tmux from a browser or machine interface:
  **[tmux-browse](https://github.com/itsmygithubacct/tmux-browse)** and
  **[tmux-cli](https://github.com/itsmygithubacct/tmux-cli)**

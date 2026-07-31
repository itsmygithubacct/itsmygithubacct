# itsmygithubacct

I build systems software whose important behavior can be inspected and
reproduced. The public work ranges from integer-exact LLM inference and
byte-exact Bitcoin settlement to a terminal-native desktop platform,
deterministic C runtimes and games, and tools for operating tmux sessions from
both human and machine interfaces.

The recurring pattern is composition: establish a small, testable core; extract
stable boundaries; keep optional capabilities outside that core; and build
complete products from independently useful pieces.

## Portfolio

| System | Foundations | Products and extensions |
| --- | --- | --- |
| Verifiable computation and settlement | [integer_inference_engine](https://github.com/itsmygithubacct/integer_inference_engine), [chain_c](https://github.com/itsmygithubacct/chain_c) | [bonsai-notary](https://github.com/itsmygithubacct/bonsai-notary), [bsv_third_entry](https://github.com/itsmygithubacct/bsv_third_entry), [chain_c_wallet](https://github.com/itsmygithubacct/chain_c_wallet) |
| Kilix terminal platform | [kilix](https://github.com/itsmygithubacct/kilix), [kilix-content](https://github.com/itsmygithubacct/kilix-content), [kitty-pty-broker](https://github.com/itsmygithubacct/kitty-pty-broker) | Operating-system sessions, four desktop providers, local speech and model tools, media applications, and semantic remoting |
| Native runtimes and games | [kilix-game-sdk](https://github.com/itsmygithubacct/kilix-game-sdk), [kitty-terminal-session](https://github.com/itsmygithubacct/kitty-terminal-session), [soft-raster](https://github.com/itsmygithubacct/soft-raster), [pcm-mixer](https://github.com/itsmygithubacct/pcm-mixer) | Deterministic engines, authoring and release tools, language bindings, and twelve native games |
| tmux operations and automation | [tmux-cli](https://github.com/itsmygithubacct/tmux-cli), [tmux-browse](https://github.com/itsmygithubacct/tmux-browse), [tmux-tui](https://github.com/itsmygithubacct/tmux-tui) | Human-facing TUI and web interfaces plus agent, sandbox, LAN federation, and QR-sharing extensions |

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

### The Kilix platform

**[kilix](https://github.com/itsmygithubacct/kilix)** wraps a maintained
**[kitty fork](https://github.com/itsmygithubacct/kitty)** with Tilix-style
clickable pane controls, pages, crash-persistent PTYs, bounded session logs,
local read-aloud and dictation controls, browsing, GUI-application streaming,
session sharing, and a host SDK for desktop providers.

**[kilix-content](https://github.com/itsmygithubacct/kilix-content)** gives
those providers one unprivileged catalog and installer for immutable games and
applications. Git content is commit-pinned, recursively checked out, built in
a private staging directory, verified against its origin and clean state, and
atomically selected. Privileged host provisioning consumes only declarative
capabilities; it never executes catalog build commands.

The operating-system boundary remains separately versioned:

- **[Pleb](https://github.com/itsmygithubacct/pleb)** installs a fullscreen
  Kilix kiosk session alongside an existing Linux desktop.
- **[Plebian OS](https://github.com/itsmygithubacct/plebian-os)** provisions a
  regular Debian installation whose graphical session is Pleb, with pinned,
  rollback-safe component updates.

Four providers turn the same terminal into different desktops:

| Repository | Desktop contract |
| --- | --- |
| **[Kilix 95](https://github.com/itsmygithubacct/kilix-95)** | A framebuffer-rendered Windows 95/XP-style environment with its own windows, settings, applications, catalog integration, and isolated runtime state |
| **[Kilix Cap](https://github.com/itsmygithubacct/kilix-cap)** | A native C mansion whose physical rooms and objects launch desktop functions, games, documents, monitors, and bounded housekeeping actions |
| **[Kilix Land](https://github.com/itsmygithubacct/kilix-land-desktop)** | A walkable, avatar-driven desktop whose rooms and interactive objects expose operating-system functions |
| **[Kilix TUI](https://github.com/itsmygithubacct/kilix-tui-utils)** | A text-native desktop over the same shared utility suite, usable through Kitty graphics, SSH, tmux, or a bare console |

The application layer includes:

- **[kilix-bonsai](https://github.com/itsmygithubacct/kilix-bonsai)**, a
  self-describing catalog and terminal interface for local BitNet text, image,
  and speech models, including a pinned CPU runtime.
- **[Kilix Voice](https://github.com/itsmygithubacct/kilix-voice)**, the
  local-only read-aloud and click-to-talk dictation engine behind Kilix's
  speaking-head and microphone controls.
- **[dosbox-kilix](https://github.com/itsmygithubacct/dosbox-kilix)**, a
  DOSBox-X derivative with opt-in Kitty and ANSI terminal output backends,
  direct keyboard and mouse input, and no X11 or Wayland requirement on the
  emulation host.
- **[kilix-amp](https://github.com/itsmygithubacct/kilix-amp)**, a C11/SDL2
  Winamp 2.x recreation with skins, a ten-band equalizer, spectrum analysis,
  playlists, MIDI playback, and a waveform editor.

Three narrower components handle persistence and remote presentation:

| Repository | Contract |
| --- | --- |
| **[kitty-pty-broker](https://github.com/itsmygithubacct/kitty-pty-broker)** | Keeps each PTY and process group alive independently of its graphical frontend while forwarding terminal bytes unchanged |
| **[kitty-frame-presenter](https://github.com/itsmygithubacct/kitty-frame-presenter)** | Presents damage-aware RGB frames from Python event loops, with bounded shared-memory or compressed-inline transport and an optional read-only frame tap |
| **[kilix-multiplexer](https://github.com/itsmygithubacct/kilix-multiplexer)** | Remotes graphical Kilix sessions as typed layout, cell, still-image, motion, audio, and input planes instead of flattening the whole desktop into video |

### Native runtime and bindings

The native applications led to small libraries with narrow ownership:

| Repository | Contract |
| --- | --- |
| **[kitty-framebuffer](https://github.com/itsmygithubacct/kitty-framebuffer)** | Threaded, newest-frame-wins RGBA presentation through the Kitty graphics protocol, with synchronized updates and crash-safe terminal restoration |
| **[kitty-keyboard](https://github.com/itsmygithubacct/kitty-input/tree/main/third_party/kitty_keyboard)** | Allocation-free Kitty keyboard parser plus optional POSIX lifecycle, including independent press, repeat, release, and held-key state |
| **[kitty-input](https://github.com/itsmygithubacct/kitty-input)** | Preserves wire order across keyboard, mouse, focus, paste, and controller input, then maps those events to rebindable semantic actions |
| **[kitty-terminal-session](https://github.com/itsmygithubacct/kitty-terminal-session)** | Correctly ordered composition of framebuffer and input ownership, including normal and emergency shutdown |
| **[soft-raster](https://github.com/itsmygithubacct/soft-raster)** | Pure C11 framebuffer primitives, alpha sprites, bitmap text, image loading, and letterbox scaling with byte-pinned blending behavior |
| **[pcm-mixer](https://github.com/itsmygithubacct/pcm-mixer)** | Background PCM mixing, pitched voices, controlled loops, music crossfades, live generators, strict WAV loading, and graceful command-line sink failure |
| **[chip-sequencer](https://github.com/itsmygithubacct/chip-sequencer)** | Deterministic fixed-point chip synthesis and pattern sequencing that produces audio without owning a device, pipe, or thread |
| **[kilix-state](https://github.com/itsmygithubacct/kilix-state)** | Bounded, ownership-checked, CRC-protected application state with atomic durable replacement and endian-stable codecs |

**[soft-raster/python](https://github.com/itsmygithubacct/soft-raster/tree/main/python)**
and
**[kilix-state/python](https://github.com/itsmygithubacct/kilix-state/tree/main/python)**
expose
the native raster and state contracts to Python without moving their
security- or ownership-bearing behavior out of C.

These are intentionally not a monolithic engine. Presentation, input,
rasterization, audio transport, persistence, and application policy remain
separable and testable.

### Game SDK and authoring stack

**[kilix-game-sdk](https://github.com/itsmygithubacct/kilix-game-sdk)** makes
cross-library updates atomic while keeping each component independently useful:

| Component | Ownership boundary |
| --- | --- |
| **[kilix-game-kit](https://github.com/itsmygithubacct/kilix-game-sdk/tree/main/kilix-game-kit)** | Pins the terminal, input, raster, audio, and state runtime and adds fixed-step timing, reversible host lifecycle, semantic audio, and headless test support |
| **[kilix-assets](https://github.com/itsmygithubacct/kilix-game-sdk/tree/main/kilix-assets)** | Validates and owns bounded image decoding, manifests, atlases, caching, and animation; games retain semantic asset meaning |
| **[kilix-story](https://github.com/itsmygithubacct/kilix-game-sdk/tree/main/kilix-story)** | Evaluates bounded story state, conditions, transactional actions, and dialogue traversal without defining game rules |
| **[kilix-world](https://github.com/itsmygithubacct/kilix-game-sdk/tree/main/kilix-world)** | Supplies projection-independent grids, paths, sight, regions, interactions, and portals without owning rendering or mutable actors |
| **[kilix-top-down-engine](https://github.com/itsmygithubacct/kilix-game-sdk/tree/main/kilix-top-down-engine)** | Deterministic orthographic cameras, framebuffers, pixel-art drawing, and logical viewport fitting |
| **[kilix-tactics-engine](https://github.com/itsmygithubacct/kilix-game-sdk/tree/main/kilix-tactics-engine)** | Deterministic isometric projection, picking, pathfinding, line of sight, cover, and stable draw ordering |
| **[kilix-ui](https://github.com/itsmygithubacct/kilix-game-sdk/tree/main/kilix-ui)** | Game-rule-free menus, panels, dialogue, meters, and RPG interface composites |
| **[kilix-game-tools](https://github.com/itsmygithubacct/kilix-game-sdk/tree/main/kilix-game-tools)** | Standard-library validation plus byte-reproducible release archives; it never enters the runtime path |

**[python-sound-generator](https://github.com/itsmygithubacct/python-sound-generator)**
provides a lazy, modular CLI for deterministic procedural audio and
provenance-tracked CC0 or public-domain Foley. It keeps authoring-time audio
generation outside the native runtime.

### Games

Twelve released games exercise that stack across very different simulations:

| Project | What it is |
| --- | --- |
| **[Bashed Earth](https://github.com/itsmygithubacct/bashed-earth)** | Falling-sand artillery with destructible terrain, water, weather, fire, an economy, and five AI personalities |
| **[C-COM](https://github.com/itsmygithubacct/c-com-ufo-defense)** | A complete strategy and turn-based tactics campaign with a persistent globe, bases, interception, research, manufacturing, tactical missions, and ironman saves |
| **[Chess Bash](https://github.com/itsmygithubacct/chess-bash)** | Animated isometric chess with complete rules, battle sequences, Stockfish support, and a built-in fallback engine |
| **[Joustix](https://github.com/itsmygithubacct/joustix)** | Flying-joust arcade combat with held-key input, enemy classes, eggs, waves, and lava hazards |
| **[Kilix Fishtank](https://github.com/itsmygithubacct/kilix-fishtank)** | An arcade aquarium with real fish species, shark attacks, fishing, boat battles, and a lightweight water simulation |
| **[Kilix JPAK](https://github.com/itsmygithubacct/kilix-jpak)** | A clean-room action-puzzle game with 100 deterministic starvaults, route-aware generation, hazards, machines, gates, and persistent unlocks |
| **[Kilix Lights](https://github.com/itsmygithubacct/kilix-lights)** | A full-color Lights Out puzzle with guaranteed-solvable boards, keyboard and pointer controls, undo, and persistent settings |
| **[Kilix Pong](https://github.com/itsmygithubacct/kilix-pong)** | Deterministic paddle-ball with local two-player input, bounded collision substeps, and a reproducible fixed-point sound generator |
| **[Kilix Rancher](https://github.com/itsmygithubacct/kilix-rancher)** | A creature-raising game with weekly care decisions, training, real-time positional battles, rivals, and progression |
| **[Kilix Brokeout](https://github.com/itsmygithubacct/kilix-brokeout)** | Breakout with fixed-step physics, varied bricks, powerups, multiball, particles, and deterministic tests |
| **[Super Kilix](https://github.com/itsmygithubacct/super-kilix)** | An original side-scrolling platformer with 32 deterministic starvaults, runtime-drawn art, and in-memory synthesized audio |
| **[Kilix Lander](https://github.com/itsmygithubacct/kilix-lander)** | Lunar Lander with four difficulty models, physical sound banks, exact held controls, and headless simulation checks |

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

**[tmux-tui](https://github.com/itsmygithubacct/tmux-tui)** is the
keyboard-and-mouse terminal manager shipped by Kilix, Pleb, and Plebian OS. It
does not implement a second tmux control layer: every snapshot and mutation
goes through tmux-cli's stable JSON contract.

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
- Keep privileged provisioning, untrusted content builds, and user-owned
  runtime state on separate, explicit sides of the system.
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
  **[kilix](https://github.com/itsmygithubacct/kilix)**,
  **[Plebian OS](https://github.com/itsmygithubacct/plebian-os)**, and
  **[kilix-content](https://github.com/itsmygithubacct/kilix-content)**
- Building a native Kitty-protocol application:
  **[kilix-game-sdk](https://github.com/itsmygithubacct/kilix-game-sdk)** or
  its smaller **[kitty-terminal-session](https://github.com/itsmygithubacct/kitty-terminal-session)**,
  **[soft-raster](https://github.com/itsmygithubacct/soft-raster)**, and
  **[pcm-mixer](https://github.com/itsmygithubacct/pcm-mixer)** components
- Persisting or remoting a graphical terminal session:
  **[kitty-pty-broker](https://github.com/itsmygithubacct/kitty-pty-broker)**
  and **[kilix-multiplexer](https://github.com/itsmygithubacct/kilix-multiplexer)**
- Operating tmux from a browser or machine interface:
  **[tmux-browse](https://github.com/itsmygithubacct/tmux-browse)** and
  **[tmux-cli](https://github.com/itsmygithubacct/tmux-cli)**

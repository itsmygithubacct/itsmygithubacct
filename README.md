# itsmygithubacct

I build systems software with a bias toward **determinism, minimal dependencies,
and verifiability**. The work spans three areas: verifiable LLM inference
(integer-exact, receipt-carrying), byte-exact on-chain settlement written in C,
and terminal tooling designed to be useful to humans and automation alike.

## Featured Projects

### [bonsai-notary](https://github.com/itsmygithubacct/bonsai-notary)

A deterministic-inference **notary**: it runs a low-bit (BitNet-style) LLM
through a byte-exact integer engine, emits a cryptographic triple-entry
**receipt** for every generation, and anchors the receipt on Bitcoin SV — so a
third party can re-execute the run, get the same bytes, and verify what the
model produced. A deliberately thin composition layer over three
independently-versioned projects:

- **[integer_inference_engine](https://github.com/itsmygithubacct/integer_inference_engine)** —
  integer fixed-point inference that is byte-identical across hardware (threads,
  SIMD width, GPU, batch size), which is what makes a generation *verifiable*.
  A CPU integer oracle is the canonical verifier; byte-exact C/CUDA producers
  are gated against it.
- **[chain_c](https://github.com/itsmygithubacct/chain_c)** — the on-chain layer
  (see below).
- **[bsv_third_entry](https://github.com/itsmygithubacct/bsv_third_entry)** —
  Python orchestration that drives the `chain_c` CLIs to publish the on-chain
  "third entry" and run the agent lifecycle.

What it demonstrates:

- End-to-end system design across Python, C, and CUDA
- Reproducibility as a hard contract — commit, sign, hash-chain, re-execute
- Cryptographic receipts (secp256k1) bound to on-chain commitments
- Composing independently-versioned repos into one coherent product

### [chain_c](https://github.com/itsmygithubacct/chain_c)

A faithful, **byte-exact** C reimplementation of a BSV chain layer for on-chain
agent identity, anchored receipts, and Ricardian charters — reproducing every
on-chain artifact (receipt preimages, locking scripts, Rabin signatures, sighash
preimages) down to the byte against an authoritative TypeScript reference.
~29k lines of C across 8 tiers, 9 CLIs, and a CTest suite that builds with **0
warnings** under `-Wall -Wextra`. Its wallet front-end lives in
**[chain_c_wallet](https://github.com/itsmygithubacct/chain_c_wallet)**.

What it demonstrates:

- Low-level C with real crypto libraries (libsecp256k1, OpenSSL, libcurl)
- Byte-for-byte cross-language parity pinned by golden vectors
- Constant-time secret-path handling and money-safe dry-run defaults
- CMake + CTest with per-module isolation, verified against a live BSV node

### [tmux-browse](https://github.com/itsmygithubacct/tmux-browse)

A tmux toolkit with two surfaces: a web dashboard for browsing local tmux
sessions and `tb`, a CLI designed for both humans and LLM tool-use loops. The
CLI core is split out as its own repo,
**[tmux-cli](https://github.com/itsmygithubacct/tmux-cli)**.

- Dashboard with per-session ttyd terminals, hot buttons, idle alerts, and
  restart controls
- `tb` CLI for listing, inspecting, creating, scripting, and driving tmux
  sessions
- Stable machine-readable JSON output and pragmatic human-facing terminal output
- Optional Bearer-token auth and TLS, with stdlib-only Python on the server side

What it demonstrates:

- tmux/session orchestration
- terminal-first product design
- HTTP and CLI interface design in the same codebase
- practical automation tooling for agent and operator workflows

### [kilix](https://github.com/itsmygithubacct/kilix)

A self-contained wrapper around a fork of kitty that gives each pane a
Tilix-style title bar with clickable split / maximize / close buttons —
GPU-rendered speed with Tilix's pane chrome, leaving any existing kitty install
untouched.

What it demonstrates:

- Working inside a large GPU-terminal codebase (Go + C)
- Pragmatic UX ports that respect an existing tool's users
- Self-contained packaging with graceful prebuilt fallbacks

## Technical Themes

- Determinism and integer fixed-point arithmetic; reproducible-by-construction
- Cryptographic receipts, secp256k1, Bitcoin SV, byte-exact artifact parity
- Systems C with library-backed crypto; CMake + CTest verification
- Python CLIs and stdlib-first services
- tmux/terminal orchestration and GPU terminal UX
- Human-readable and machine-readable interface design in the same codebase

## Working Style

- Prefer simple deployment and low operational overhead
- Bias toward explicit contracts, stable output, and clean failure modes
- Treat reproducibility and verification as features, not afterthoughts
- Keep tooling scriptable first, then make the human UX pleasant
- Treat documentation as part of the product

## Links

- GitHub: https://github.com/itsmygithubacct
- bonsai-notary: https://github.com/itsmygithubacct/bonsai-notary
- chain_c: https://github.com/itsmygithubacct/chain_c
- tmux-browse: https://github.com/itsmygithubacct/tmux-browse

# itsmygithubacct

I build small, sharp tools around Linux, terminals, and developer workflows.
The work leans heavily toward systems tooling, minimal-dependency software, and
interfaces that are useful both to humans and to automation.

## Featured Projects

### [bash_linux](https://github.com/itsmygithubacct/bash_linux)

A bootable Linux system with a tiny bash-centric userspace. It currently ships
four build variants, all boot-tested and all under 3 MB.

- Pure bash variant with GNU Bash plus 28 integrated loadable builtins
- Full variant with a practical tiny userland via `sbase-box` and `awk`
- Disk variant using squashfs + overlayfs + optional live 9P host-dir sharing
- Users variant adding multiple identities and lightweight login/su tooling

What it demonstrates:

- Linux build pipelines and boot flows
- Tiny-system design under explicit size budgets
- QEMU-driven validation and reproducible build/test workflows
- Clear technical documentation for a non-trivial systems project

### [tmux-browse](https://github.com/itsmygithubacct/tmux-browse)

A tmux toolkit with two surfaces: a web dashboard for browsing local tmux
sessions and `tb`, a CLI designed for both humans and LLM tool-use loops.

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

## Technical Themes

- Linux, QEMU, initramfs, overlayfs, squashfs, 9P
- Python CLIs and stdlib-first services
- Bash and Unix process orchestration
- Human-readable and machine-readable interface design
- Documentation that explains both usage and architecture

## Working Style

- Prefer simple deployment and low operational overhead
- Bias toward explicit contracts, stable output, and clean failure modes
- Keep tooling scriptable first, then make the human UX pleasant
- Treat documentation as part of the product, not an afterthought

## Links

- GitHub: https://github.com/itsmygithubacct
- bash_linux: https://github.com/itsmygithubacct/bash_linux
- tmux-browse: https://github.com/itsmygithubacct/tmux-browse

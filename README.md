# Developer Profile

## Summary

Python developer specializing in CLI tools, system monitoring, and log analysis. Builds clean, tested, well-documented command-line applications following modern packaging standards. Comfortable with Linux systems, DevOps workflows, and infrastructure tooling.

## Technical Skills

| Category | Technologies |
|----------|-------------|
| **Languages** | Python 3.9+ |
| **CLI Frameworks** | Click |
| **System & OS** | psutil, Linux administration, process management |
| **Data Formats** | JSON, Common Log Format, structured logging |
| **Testing** | pytest (86 tests across two projects, 100% pass rate) |
| **Packaging** | setuptools, pyproject.toml, PEP 621 |
| **Version Control** | Git, GitHub |
| **Output Design** | Terminal dashboards, visual bars, colored output, structured JSON |

## Portfolio Projects

### sysmon - System Monitoring Dashboard
A fast CLI tool for real-time system health monitoring.

- **What it does:** Displays CPU (per-core), memory, disk, network, and top processes as a colored terminal dashboard or structured JSON
- **Key features:** Watch mode with configurable refresh, JSON output for piping into `jq` or monitoring systems, visual bar charts
- **Tech:** Python, Click, psutil
- **Tests:** 43 passing (collector, formatter, CLI integration)
- **Demonstrates:** Systems programming, real-time data collection, terminal UI design, clean API output

### log-analyzer - Log File Analysis Tool
A CLI tool that parses, filters, and summarizes log files.

- **What it does:** Auto-detects Common Log Format and JSON-lines, filters by severity/status/path, produces summary reports
- **Key features:** Streaming line-by-line processing (memory-efficient), stdin support for piping, multi-format auto-detection
- **Tech:** Python, Click
- **Tests:** 43 passing (parser, analyzer, formatter, CLI integration)
- **Demonstrates:** Text parsing, data aggregation, streaming I/O, multi-format support, filtering pipelines

## Strengths

- **Test-driven:** Every module has thorough unit and integration tests
- **Unix philosophy:** Tools that do one thing well, support piping, and produce both human and machine-readable output
- **Documentation:** Comprehensive READMEs with real usage examples, sample output, and project structure
- **Modern Python:** src layout, pyproject.toml, typed APIs, clean CLI argument handling

## What I'm Looking For

Open to freelance work, contract roles, or full-time positions involving:
- Python CLI/backend development
- DevOps and infrastructure tooling
- System monitoring and observability
- Log processing and data pipelines
- Automation and scripting

## Links

- GitHub: https://github.com/itsmygithubacct

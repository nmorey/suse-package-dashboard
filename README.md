# SUSE Package Dashboard

This tool tracks the git sync status of packages across multiple workflow stages. It supports git submodules or standalone repositories and visualizes the status (synced, merged, behind, diverged) in an HTML dashboard.

## Usage

```bash
./suse-package-dashboard [options] [REPO_PATHS...]
```

If no `REPO_PATHS` are provided, it defaults to the repositories configured in the configuration file or the current directory.

### Options

*   `-o`, `--output FILE`: Specify output HTML file (default: `package_dashboard.html`).
*   `--open`: Open the generated dashboard with `xdg-open`.
*   `-q`, `--quiet`: Suppress all information messages.
*   `--flow NAME`: Select the flow to use (defaults to `default_flow` in config).
*   `--add-flow DEFINITION`: Add a new flow to config (Format: `name:stage1:stage2:...`).
*   `-h`, `--help`: Prints help message.

## Configuration

The configuration is stored in `$XDG_CONFIG_HOME/suse-dashboard.yaml` (usually `~/.config/suse-dashboard.yaml`).

### Sample Configuration

```yaml
flows:
  default:
  - private/main
  - devel/main
  - origin/factory
  - suse-private/slfo-main
  - sle/slfo-main
default_flow: default
open_by_default: false
repos:
- "."
excludes:
- ":(exclude)README.md"
- ":(exclude)_scmsync.obsinfo"
- ":(exclude)build.specials.obscpio"
status_colors:
  synced: "#dcfce7"
  merged: "#ecfccb"
  behind: "#fee2e2"
  diverged: "#ffedd5"
  missing: "#f3f4f6"
  error: "#f3f4f6"
```

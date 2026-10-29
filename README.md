# SUSE Package Dashboard

This tool tracks the git sync status of packages across multiple workflow stages. It supports git submodules or standalone repositories and visualizes the status (synced, merged, behind, diverged) in a tabbed HTML dashboard.

## Usage

```bash
./suse-package-dashboard [options] [REPO_PATHS...]
```

If no `REPO_PATHS` are provided, it defaults to the repositories configured in the configuration file or the current directory.

### Options

*   `-o`, `--output FILE`: Specify output HTML file (default: `package_dashboard.html`).
*   `--open`: Open the generated dashboard with `xdg-open`.
*   `-q`, `--quiet`: Suppress all information messages.
*   `--flow NAME`: Select flow(s) to use (comma-separated or multiple flags; 'all' for all flows; defaults to `default_flow` in config).
*   `--all-flows`: Analyze all configured flows.
*   `--add-flow DEFINITION`: Add a new flow to config (Format: `id:name:stage1:stage2:...`).
*   `-h`, `--help`: Prints help message.

### Multi-Workflow Tabs

When analyzing one or more workflows, each workflow is rendered in its own tab in the generated HTML dashboard:
*   **Workflow Display Names**: Tabs show the full workflow name along with a count badge indicating the number of packages with issues (`behind`, `diverged`, `error`).
*   **Isolated Diff Cache**: Ignored diffs are cached per workflow in browser `localStorage`. A diff ignored in one workflow is not ignored in another workflow even if comparing identical commits.
*   **Scoped Reset**: The "Reset Ignored Diffs" button resets ignored diffs only for the currently active tab workflow.
*   **Direct Navigation**: Active tabs are tracked in the URL hash (`#flow-<id>`) so bookmarking or reloading preserves the active tab.

## Configuration

The configuration is stored in `$XDG_CONFIG_HOME/suse-dashboard.yaml` (usually `~/.config/suse-dashboard.yaml`).

### Sample Configuration

```yaml
flows:
  factory:
    name: "Factory"
    stages:
      - private/main
      - devel/main
      - origin/factory
  slfo:
    name: "SLFO-main"
    stages:
      - origin/factory
      - suse-private/slfo-main
      - sle/slfo-main
default_flow:
  - factory
  - slfo
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

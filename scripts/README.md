# Workflow Scripts

This directory contains shell scripts for running the project workflow in stages.

```text
scripts/
|-- README.md
|-- all_stages.sh   # runs stage1.sh to stage4.sh in order
|-- stage1.sh       # downloads data/raw/noshow.db if it is missing
|-- stage2.sh       # runs data ingestion, cleaning, and validation
|-- stage3.sh       # runs feature engineering
`-- stage4.sh       # runs training, calibration, evaluation, logging, and persistence
```

The top-level `run.sh` is a convenience entry point. It delegates to `scripts/all_stages.sh`, which runs `stage1.sh` through `stage4.sh` in order.

```bash
./run.sh
```

This lets the full workflow be run from the repository root without remembering the internal script path.

## Stage Commands

`stage2.sh`, `stage3.sh`, and `stage4.sh` run selected Kedro pipeline namespaces. For example:

```bash
./scripts/stage3.sh
# kedro run --namespaces=feature_engineering
```

`stage1.sh` is separate because it downloads the raw SQLite database instead of running Kedro.

## Shared Setup

The Kedro stage scripts use this setup before calling `kedro run`:

```bash
KEDRO="${KEDRO:-$PROJECT_ROOT/.venv/bin/kedro}"
export KEDRO_DISABLE_TELEMETRY="${KEDRO_DISABLE_TELEMETRY:-1}"
export MPLCONFIGDIR="${MPLCONFIGDIR:-/private/tmp/kedro-mplconfig}"
mkdir -p "$MPLCONFIGDIR"
```

- `KEDRO=...` chooses which Kedro executable to run. If `KEDRO` is not already set, the scripts use the project's `.venv/bin/kedro`.
- `KEDRO_DISABLE_TELEMETRY=1` tells Kedro not to send anonymous usage information to the Kedro project maintainers when these scripts run.
- `MPLCONFIGDIR=...` gives Matplotlib a writable place for its own internal cache files, such as font cache files, during script runs.
- `mkdir -p "$MPLCONFIGDIR"` creates that Matplotlib cache directory if it does not already exist.

These defaults make the scripts less dependent on the user's shell environment.

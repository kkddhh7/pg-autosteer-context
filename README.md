# AutoSteer Dynamic Load Evaluation Context

## Overview

This repository contains Python scripts, SQL benchmarks, and utilities used to evaluate AutoSteer query optimization under varying CPU and I/O load conditions. It is intended as a research/experiment context rather than a packaged library.

## Repository Layout

- `autosteer/`: core exploration logic
- `inference/`: model training and inference helpers
- `connectors/`: database connector implementations
- `benchmark/`: SQL benchmarks (e.g., TPC-H queries)
- `load/` and `burden/`: CPU and I/O load generators
- `utils/`: shared utilities (argument parsing, logging, helpers)
- `conf/` and `configs/`: experiment configuration files
- `evaluation/` and `results/`: generated logs and SQLite outputs
- `main.py`: primary entry point for training/inference
- `run.sh`: Docker-based evaluation runner

## Setup

- Python 3
- Install dependencies: `pip install -r requirements.txt`
- A target database matching the selected connector (PostgreSQL is used in the examples)
- Optional: Docker if you run `run.sh` or use `Dockerfile.postgres`

## Running

### main.py

Training mode:

```bash
python3 main.py --training --database postgres --benchmark benchmark/queries/tpch
```

Inference mode:

```bash
python3 main.py --inference --database postgres --benchmark benchmark/queries/tpch
```

The benchmark path must point to a directory containing `.sql` files.

### run.sh

```bash
./run.sh
```

This script starts a PostgreSQL Docker container and runs training and inference across configuration variants, writing logs under `evaluation/`. It uses `conf/postgres*.conf` and invokes `sudo docker`.

```bash
./run.sh clean
```

This variant also clears the local data directory used by the script (`postgres_data/`).

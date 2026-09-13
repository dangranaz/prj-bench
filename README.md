# prj-bench

Public, reproducible benchmarks for the engines — full transparency: the exact
tool, the exact tests, and real reference results anyone can re-run.

## Layout

```
prj-bench/
└── crates/
    └── engine-mlx/     ← benchmark harness + reference results for engine-mlx
```

## What's here

- **The harness** (`crates/engine-mlx/`): a cross-engine, OpenAI-compatible
  HTTP benchmark (Rust + Hurl). Same tool, same 5 tests, any engine — point it
  at a URL. See its `README.md` for the tests, CLI, and how to run.
- **Reference results** (`crates/engine-mlx/results/`): real reports committed
  on purpose, so the numbers we publish can be verified, not just trusted.

## Transparency

- Numbers are produced by the included harness — run it yourself and compare.
- Correctness/coherence tests run at `temperature=0` with tolerant matching so
  they travel across engines and model families; full model outputs are
  captured in each report.
- Reports record the environment (engine, model, timestamp, host class) and the
  thresholds used. Host names are anonymized; only the hardware class is kept.

## Reference numbers (Qwen3-1.7B-MLX-4bit, Apple Silicon)

Indicative, measure-only (reproduce with `run.sh`):
throughput ~23–41 t/s, sustained-degradation ~6%, length-ramp ~16%. See
`crates/engine-mlx/results/` for the full report.

# prj-bench

**prj-bench** is a public, reproducible benchmark suite for the
[engine-mlx](https://github.com/dangranaz/engine-mlx) inference engine — and for
any OpenAI-compatible server. It speaks only HTTP, so the **same tool** and the
**same 5 tests** run against any engine: point it at a URL, get timestamped
JSON + Markdown reports. Full transparency: the exact harness, the exact tests,
and **real reference results you can re-run**.

> [!IMPORTANT]
> **Numbers you can verify, not trust.** Every figure we publish is produced by
> the harness in this repo. Correctness and coherence tests run at
> `temperature=0` with tolerant matching so they travel across engines and model
> families; full model outputs are captured in each report. Host names are
> anonymized — only the hardware class is kept.

---

## What's here

```
prj-bench/
└── crates/
    └── engine-mlx/          ← the harness (Rust + Hurl) + reference results
        ├── src/             ← nxm-bench: throughput / degradation / ramp
        ├── hurl/            ← declarative API / coherence / perf gates
        ├── models/          ← per-model start/stop launchers
        └── results/         ← real reference reports (committed on purpose)
```

---

## The 5 standard tests

| # | Test                  | Category    | What it checks                              |
|---|-----------------------|-------------|---------------------------------------------|
| 1 | Factual recall        | correctness | Deterministic answer contains `Paris`       |
| 2 | Instruction following | correctness | Obeys a strict "reply DONE" format          |
| 3 | Sustained throughput  | degradation | N identical requests don't lose t/s (leak guard) |
| 4 | Length ramp           | degradation | t/s across 128 / 512 / 1024 tokens (KV scaling) |
| 5 | Multi-step reasoning  | coherence   | Word problem → correct answer with working  |

---

## Requirements

- Rust (`cargo`) to build the harness.
- A running OpenAI-compatible server (e.g. [engine-mlx](https://github.com/dangranaz/engine-mlx),
  or start one with [prj-scripts](https://github.com/dangranaz/prj-scripts)).
- `curl`; optionally `hurl` (`brew install hurl`) for the declarative gates.

---

## Quick start

```sh
cd crates/engine-mlx

# 1. build the harness
cargo build --release

# 2. point it at a live server and run the 5 tests
./target/release/nxm-bench run \
  --url http://127.0.0.1:11435 --engine engine-mlx --model Qwen3-1.7B-MLX-4bit

# 3. or run the full suite (Hurl gates + measurements) via run.sh
./run.sh
```

Reports are written to `results/<engine>/<timestamp>.{json,md}`.

---

## Reference numbers (engine-mlx, Apple Silicon)

From the committed reports in `crates/engine-mlx/results/`:

| Model               | Generation speed | Sustained (20×) | Length ramp 128→1024 |
|---------------------|------------------|-----------------|----------------------|
| Qwen3-1.7B-MLX-4bit | ~34–41 t/s       | ~6% degradation | ~16% drop            |

Re-run the harness to get numbers for your own chip.

---

## Benchmarking another engine

It's engine-agnostic — point it at any OpenAI-compatible server:

```sh
./target/release/nxm-bench run --url http://127.0.0.1:8081 --engine vllm --assert
```

See `crates/engine-mlx/README.md` for the full CLI, thresholds, assert mode, and
the `compare` subcommand (diff two runs or two engines side by side).

---

## Related

- [engine-mlx](https://github.com/dangranaz/engine-mlx) — the engine under test.
- [prj-scripts](https://github.com/dangranaz/prj-scripts) — start/stop the server.

## ⭐ Support the project

If these benchmarks are useful — or you value transparent, reproducible numbers
you can re-run yourself — please **give the repository a star** and share it.
Feedback, issues, and suggestions are very welcome.

## License

MIT.

# LLM-assisted optimizer prototype

This prototype keeps symbolic mining inside PeTTa. Python performs dataset
profiling, configuration validation, process measurement, output parsing, and
selection. When `OPENAI_API_KEY` is set, an OpenAI-compatible chat endpoint
proposes configurations. Without a key, a deterministic proposal generator is
used so the experiment harness remains testable.

The current repository defines:

```text
(pattern-miner output-space input-space init-pattern min-support depth mode normalization db-ratio conj-exp)
```

The adapter exposes all nine arguments because the checked-in implementation and
tests use that signature. The older root `run_miner.metta` example omits the
initial-pattern argument; that discrepancy is reported in the research notes
and is not silently hidden by this tool.

Example:

```bash
python3 optimizer/prototype.py \
  --dataset experiments/data/very-small-ugly.metta \
  --petta /path/to/PeTTa/run.sh \
  --trials 4 \
  --output optimizer-report.json
```

For the manual baseline, use the same runner and dataset with an explicit
configuration file:

```bash
python3 optimizer/prototype.py \
  --dataset experiments/data/very-small-ugly.metta \
  --petta /path/to/PeTTa/run.sh \
  --config-file optimizer/manual-baseline.json \
  --output manual-report.json
```

The JSON report contains the dataset profile, proposed configurations, measured
results, statuses (`ok`, `invalid`, `failed`, or `timeout`), memory estimate,
pattern counts, support values, surprisingness values, and the selected run.


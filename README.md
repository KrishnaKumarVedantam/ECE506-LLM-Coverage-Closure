# ECE506-LLM-Coverage-Closure

ECE 506 course project (Fall 2026, Dr. Song)
Venkata Krishna Kumar Vedantam, vedantam@pdx.edu

## What this project is

An LLM-driven coverage closure agent measured against a random-stimulus baseline on an async FIFO.

The agent runs a cocotb testbench on the RTL in Verilator, reads the real coverage report, picks one uncovered bin, asks Claude to write a directed test for it, runs that test and merges the coverage. This project compares it against plain random stimulus with the same tools and settings, and measures the trade-off.

Proposal: [docs/ECE506_Proposal_Vedantam.docx](docs/ECE506_Proposal_Vedantam.docx)

## Starting point

This repo starts from a copy of my own earlier project, AMBU-CovAgent
(github.com/KrishnaKumarVedantam/AMBU-CovAgent). The first commit is that copy, cut down to the async FIFO only. Everything after it is new work for ECE 506.

## Goal

Close the async FIFO to 100% functional coverage (36/36 bins) with the agent, and show whether seeded random stimulus on the same setup reaches 100% across many seeds and longer runs.

## Status

| Step | Status |
|---|---|
| Starting copy of agent, FIFO RTL and testbench | done |
| Configurable stimulus length and seed in the testbench | in progress |
| Baseline sweep script (seeds x lengths, CSV output) | to do |
| Agent logging (sim cycles, wall time, tokens, cost) | to do |
| Baseline sweep and repeated agent runs | to do |
| Results table, plots, trade-off | to do |

## How to run

Open the repo in GitHub Codespaces. The `.devcontainer` builds Verilator and cocotb on first launch (about 5 to 10 minutes).

Base random testbench (no API key needed):

```bash
make SIM=verilator MODULE=tb.tb_fifo
```

Full agent loop (needs `ANTHROPIC_API_KEY`):

```bash
rm -f LESSONS.md
python3 agent/agent.py config.yaml
```

## Tools

Verilator 5.048+, cocotb 2.0.1+, cocotb-coverage, Python 3.11, Claude API (claude-sonnet-4-6).

## Layout

```
rtl/         async FIFO RTL (SystemVerilog)
tb/          cocotb testbench, coverage model, scoreboards
agent/       coverage closure agent
framework/   shared coverage and scoreboard base classes
docs/        proposal, later the report
```

## Credits

The async FIFO RTL, testbench and agent are my own work. cocotb, cocotb-coverage and Verilator are open-source tools. License: BSD 3-Clause (see LICENSE).

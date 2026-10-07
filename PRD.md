# PRD.md:  "The Why"; Product requirements, benchmark metadata table, high-level requirements, etc.

Product Requirements Document

"The Why"; Product requirements, benchmark metadata table, high-level requirements, etc. Defines the benchmark goal, validation objective, test name, benchmark number, category, and high-level success criteria.

## Benchmark Matrix Document Metadata (via benchmark_specification.json)

This PRD.md section is populated from benchmark_specification.json, which is the structured source of benchmark-specific product requirements.

## Workload Number
408

## Workload Name
iPerf3 TCP/UDP Sweep

## Execution Summary (Run and Measure)
Run iperf3 -J for each yaml protocol against 127.0.0.1 unless BENCHMARK_IPERF_PEER is set, spawning iperf3 -s -1 for loopback, to measure TCP throughput, UDP jitter, loss, and retransmits. reverse_mode is passed as -R. window_size, buffer_length, omit_initial_seconds, and interval are not passed to iperf3

## Main Goal
Measure TCP/UDP iperf3 throughput and stability

## Validation Objective
Validates that each protocol JSON run completes. Default path is loopback, not VM-to-VM, unless BENCHMARK_IPERF_PEER is set

## Workload Category
Memory, Bandwidth & Data Movement

## Validation Requirement

The benchmark must include an automated SQLite-integrated validation layer that verifies persisted results from `results/benchmark.db`. Validation must confirm:

1. The benchmark run completed successfully with no tool errors.
2. Required samples and aggregate metrics were persisted for every swept shape.
3. Metrics are finite and physically sensible (positive, within plausible bounds).
4. Measured values satisfy configured thresholds when the workload defines pass/fail gates.
5. The benchmark fails validation when required data is missing, invalid, or outside bounds.

## Non-Functional Requirements

| Requirement | Target |
|---|---|
| Automation | Runs to completion without manual intervention after `bash run_benchmark.sh` |
| Idempotency | Re-running `run_benchmark.sh` appends a new run; never corrupts existing rows |
| Persistence | All metrics survive script exit; `results/benchmark.db` is the durable record |

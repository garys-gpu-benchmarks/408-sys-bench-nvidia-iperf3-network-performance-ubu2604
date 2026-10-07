# SPEC.md: "The Human How"; Exact technical requirements, environment setup, implementation details, etc.

## Execution Description

Runs iperf3 -J client tests for each yaml protocol. Default peer is 127.0.0.1 (loopback); override with BENCHMARK_IPERF_PEER. protocol: tcp and/or udp. parallel_streams: -P. Sweep dimensions: protocol, port, reverse_mode, window_size, buffer_length, parallel_streams, bandwidth_limit, omit_initial_seconds.

## Parameters

| Parameter | CLI Flag | Tested Values | Default | Description |
| --- | --- | --- | --- | --- |
| protocol | `--protocol` | smoke=tcp,udp, baseline=tcp,udp, extended=tcp,udp | tcp,udp | From Parameter list; see Execution Description With Parameters. |
| port | `--port` | smoke=5201, baseline=5201, extended=5201 | 5201 | From Parameter list; see Execution Description With Parameters. |
| reverse_mode | `--reverse-mode` | smoke=false, baseline=false, extended=false | false | From Parameter list; see Execution Description With Parameters. |
| window_size | `--window-size` | smoke=256K, baseline=256K, extended=256K | 256K | From Parameter list; see Execution Description With Parameters. |
| buffer_length | `--buffer-length` | smoke=128K, baseline=128K, extended=128K | 128K | From Parameter list; see Execution Description With Parameters. |
| parallel_streams | `--parallel-streams` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| bandwidth_limit | `--bandwidth-limit` | smoke=10G, baseline=10G, extended=10G | 10G | From Parameter list; see Execution Description With Parameters. |
| omit_initial_seconds | `--omit-initial-seconds` | smoke=0, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| interval | `--interval` | smoke=1, baseline=1, extended=1 | 1 | From Parameter list; see Execution Description With Parameters. |
| time | `--time` | smoke=2, baseline=100, extended=290 | 100 | From Parameter list; see Execution Description With Parameters. |

## Invocation

```bash
Run iperf3 client (-J) and, for loopback, iperf3 -s -1
```

## Raw Output Format

iperf3 -J JSON plus one normalized CSV row per protocol. The endpoint column records 127.0.0.1 unless BENCHMARK_IPERF_PEER is set

sample_index,status,protocol,iperf_endpoint,parallel_streams,tcp_loopback_gbps,udp_throughput_gbps,udp_jitter_ms,packet_loss_rate,tcp_retransmit_count,bits_per_second,error_message
0,ok,tcp,127.0.0.1,1,40,,,,,40000000000,

## Metrics

- **#1: TCP loopback throughput** — stored as `tcp_loopback_gbps`.
- **#2: UDP loopback throughput** — stored as `udp_throughput_gbps`.
- **#3: UDP jitter** — stored as `udp_jitter_ms`.
- **#4: Packet loss, percent** — stored as `packet_loss_rate`.
- **#5: TCP retransmit count per run** — stored as `tcp_retransmit_count`.

## Framework

Runs iperf3 -J client tests for each yaml protocol. Default peer is 127.0.0.1 (loopback); override with BENCHMARK_IPERF_PEER. protocol: tcp and/or udp.

## Installation and Execution Summary

Run iperf3 -J for each yaml protocol against 127.0.0.1 unless BENCHMARK_IPERF_PEER is set, spawning iperf3 -s -1 for loopback, to measure TCP throughput, UDP jitter, loss, and retransmits. reverse_mode is passed as -R. window_size, buffer_length, omit_initial_seconds, and interval are not passed to iperf3

## Platform Portability

- **AMD (primary):** ```bash
Run iperf3 client (-J) and, for loopback, iperf3 -s -1
```
- **NVIDIA:** Native NVIDIA CUDA workload. Execute on the stated Ubuntu release with the host NVIDIA driver and CUDA userspace. ROCm porting notes do not apply.

## Model Context Protocols

- **Active:** None

## Execution-Loop Validation Contract

EXECUTION CHAIN: `run_benchmark.sh` ➔ raw output ➔ `scripts/parse_results.py` ➔ `results/benchmark.db` ➔ `scripts/validate_results.py`

This benchmark uses a lightweight, SQLite-integrated execution loop for result validation. All validation is performed by `scripts/validate_results.py`.

### Validation script usage

```bash
export BENCHMARK_PYTHON=/usr/bin/python3.13  # optional; select the installed interpreter

# After a live run:
".venv/bin/python" scripts/validate_results.py --db results/benchmark.db

# CI / no-GPU path (seeds fixture and validates it):
".venv/bin/python" scripts/validate_results.py --seed-fixture --quiet

# Override DB path via environment variable:
BENCHMARK_DB=tests/fixtures/benchmark.db \
  ".venv/bin/python" scripts/validate_results.py
```

### Run artifact contract

iperf3 -J JSON plus one normalized CSV row per protocol. The endpoint column records 127.0.0.1 unless BENCHMARK_IPERF_PEER is set

sample_index,status,protocol,iperf_endpoint,parallel_streams,tcp_loopback_gbps,udp_throughput_gbps,udp_jitter_ms,packet_loss_rate,tcp_retransmit_count,bits_per_second,error_message
0,ok,tcp,127.0.0.1,1,40,,,,,40000000000,

```bash
bash run_benchmark.sh --help
bash run_benchmark.sh --profile smoke --validate
bash run_benchmark.sh --profile baseline --validate
bash run_benchmark.sh --profile extended --validate
```
`run_benchmark.sh --help` prints usage and exits. The harness calls `scripts/ensure_setup.sh` when `.setup_state` is absent.

### Required integrity checks (built into `validate_results.py`)

1. Latest run exists and `runs.status = 'ok'`.
2. `run.error_message` is NULL.
3. `started_at` and `finished_at` are valid ISO-8601 UTC strings.
4. All required aggregate metrics in `runs` are non-NULL and finite.
5. All required aggregate metrics are physically sensible (positive values). Runs iperf3 -J client tests for each yaml protocol. Default peer is 127.0.0.1 (loopback); override with BENCHMARK_IPERF_PEER. protocol: tcp and/or udp.
6. At least 2 sample rows exist for the latest `run_id` (sweep coverage).
7. No sample has `status = 'error'`.
8. Runs iperf3 -J client tests for each yaml protocol. Default peer is 127.0.0.1 (loopback); override with BENCHMARK_IPERF_PEER. protocol: tcp and/or udp.

### Baseline / Threshold configuration (`config/benchmark_config.yaml`)

Expected ranges and gates live in `config/benchmark_config.yaml` under `baselines:` or `thresholds:`. To update them, edit that file — never edit validation code directly.

Threshold key suffixes encode comparison direction when `thresholds:` is present: `_min` → observed value must be ≥ threshold. `_max` → observed value must be ≤ threshold. Informational `baselines:` ranges are not pass/fail gates.

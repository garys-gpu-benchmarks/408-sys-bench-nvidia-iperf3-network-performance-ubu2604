# Results: 408 iPerf3 TCP/UDP Sweep

**Main goal:** Measure TCP/UDP iperf3 throughput and stability  
**Tools:** Run iperf3 client (-J) and, for loopback, iperf3 -s -1

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_148 · recorded 2026-11-19). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 NVL (95830 MiB) · Intel(R) Xeon(R) Platinum 8452Y · Ubuntu 26.04.1 LTS · kernel 6.8.0-138-generic · driver 595.112.07
- **Run:** 2026-11-19T15:10:44Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 00:50 · benchmark 00:06 · whole run 01:02
- **Source:** run 408_20261119_151044
- **Command:** `bash run_benchmark.sh --smoke --protocol tcp,udp --port 5201 --reverse-mode false --window-size 256K --buffer-length 128K --parallel-streams 1 --bandwidth-limit 0 --omit-initial-seconds 0 --time 2`

| # | Metric | Result |
|---|---|---|
| 1 | TCP loopback throughput (tcp_loopback_gbps) | 33.80381460176417 |
| 2 | UDP loopback throughput (udp_throughput_gbps) | 38.48489601631281 |
| 3 | UDP jitter (udp_jitter_ms) | 0.00218566344174526 |
| 4 | Packet loss, percent (packet_loss_rate) | 0.025829255029907558 |
| 5 | TCP retransmit count per run (tcp_retransmit_count) | 0.0 |
<!-- /profile:smoke -->

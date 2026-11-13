# Results: 408 iPerf3 TCP/UDP Sweep

**Main goal:** Measure TCP/UDP iperf3 throughput and stability  
**Tools:** Run iperf3 client (-J) and, for loopback, iperf3 -s -1

Reference results measured by the maintainer on the system shown in each section (generator TEMPLATE_00_139 · recorded 2026-11-13). Your own runs write `results/RESULTS_ACTUAL.md` in the same format.

> Smoke runs use deliberately small sizes. They check that the workload installs and runs end to end; their numbers are not performance results. Baseline and extended are.

<!-- profile:smoke -->
## smoke

- **System:** 1× NVIDIA H100 PCIe (81559 MiB) · Intel(R) Xeon(R) Platinum 8352Y CPU @ 2.20GHz · Ubuntu 26.04.1 LTS · kernel 6.8.0-138-generic · driver 595.91.07
- **Run:** 2026-11-13T13:58:42Z · status **PASS** (exit 0)
- **Time (mm:ss):** setup 00:43 · benchmark 00:06 · whole run 01:01
- **Source:** run 408_20261113_135842
- **Command:** `bash run_benchmark.sh --smoke --protocol tcp,udp --port 5201 --reverse-mode false --window-size 256K --buffer-length 128K --parallel-streams 1 --bandwidth-limit 0 --omit-initial-seconds 0 --time 2`

| # | Metric | Result |
|---|---|---|
| 1 | TCP loopback throughput (tcp_loopback_gbps) | 34.8044260509954 |
| 2 | UDP loopback throughput (udp_throughput_gbps) | 40.8671039629493 |
| 3 | UDP jitter (udp_jitter_ms) | 0.001567975008843173 |
| 4 | Packet loss, percent (packet_loss_rate) | 0.22422384055193562 |
| 5 | TCP retransmit count per run (tcp_retransmit_count) | 1.0 |
<!-- /profile:smoke -->


# Traffic Restoration and Stabilization Latency in a BGP/MPLS L3VPN

**A reproducible network-resilience measurement study by Ghufran Shakeel**

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22983728.svg)](https://doi.org/10.5281/zenodo.22983728)
[![License: MIT](https://img.shields.io/badge/Code-MIT-blue.svg)](LICENSE)
[![Paper License: CC BY 4.0](https://img.shields.io/badge/Paper%20%26%20Data-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)

A reproducible measurement study of traffic restoration and stabilization in a BGP/MPLS Layer 3 VPN under five network-failure scenarios.

**Research areas:** IP/MPLS · BGP · OSPF · LDP · L3VPN · Network Resilience · Failure Recovery · Reproducible Networking

## Research at a glance

This study investigates whether different failure conditions produce different traffic-recovery behavior in a multi-router MPLS L3VPN.

The experiment covers five scenarios: primary-link failure under default OSPF timers, primary-link failure under sub-second OSPF timers, LDP-IGP synchronization, silent customer-flow blackholing, and LDP-session failure while the IGP remains up.

Across 20 trials per scenario, the measurement harness records two service-level timestamps:

- **T2 — First-packet restoration:** time from failure injection to the first successful ICMP reply across the VPN.
- **T3 — Traffic stabilization:** time from failure injection until five consecutive successful ICMP replies.

The distinction matters: receiving one successful reply does not necessarily mean traffic has stabilized.

## Key findings

| Scenario | Median T2 | p95 T2 | Median T3 | p95 T3 |
|---|---:|---:|---:|---:|
| A — Link-down, default OSPF timers | 562.72 ms | 611.04 ms | 5092.04 ms | 5490.65 ms |
| B — Link-down, sub-second OSPF timers | 573.41 ms | 621.26 ms | 4753.96 ms | 5663.50 ms |
| C — LDP-IGP synchronization | 565.66 ms | 577.41 ms | 4766.64 ms | 5168.48 ms |
| D — Silent customer-flow blackhole | 565.98 ms | 578.55 ms | 4725.34 ms | 5339.10 ms |
| E — LDP-session failure, IGP up | 562.71 ms | 3530.67 ms | 4975.28 ms | 7911.74 ms |

**Interpretation:** Median first-packet restoration alone does not capture the full recovery distribution. Scenario E shows a pronounced T2 tail, while T3 remains several seconds across all five scenarios.

These results motivate measuring both initial restoration and short-run stability rather than reporting a single convergence number.

## Experimental topology

```text
CE1 — PE1 — P1 — P2 — PE2 — CE2
         \   \ /   \ /
          \   X     X
           \ / \   / \
            P3 ——— P4
```

The study uses eight IOL routers in EVE-NG. The provider core runs OSPF area 0 and LDP; the provider edge uses MP-BGP VPNv4 and a customer VRF named `CUST-A`.

## Repository contents

| Path | Purpose |
|---|---|
| `paper.md` | Research manuscript |
| `configs/` | Router configurations |
| `harness/measure_v3.py` | Python measurement harness |
| `data/` | Raw per-trial CSV results |
| `figures/` | Plot scripts and generated figures |

## Reproducibility

1. Review the topology and router configurations.
2. Build the documented topology in a compatible EVE-NG/IOL environment.
3. Inspect the measurement harness and its timing definitions.
4. Run the experiment using the documented failure scenarios.
5. Compare newly generated results with the committed raw CSV data and figures.

**Important:** Results may vary with router images, timer settings, topology, load, and measurement implementation. The reported values describe this experiment; they should not be interpreted as universal production-network performance guarantees.

## Research output

- **Versioned preprint and archived release:** [Zenodo DOI](https://doi.org/10.5281/zenodo.22983728)
- **Source code and experiment data:** [GitHub repository](https://github.com/ghufr7an/mpls-l3vpn-measurement)
- **Researcher identifier:** [ORCID 0009-0006-3683-8405](https://orcid.org/0009-0006-3683-8405)

## Citation

Shakeel, G. (2026). *Traffic Restoration and Stabilization Latency in a BGP/MPLS L3VPN Under Five Failure Scenarios* (Version v1.0). Zenodo. https://doi.org/10.5281/zenodo.22983728

## Author

**Ghufran Shakeel**  
Network Engineer | ISP Networks | IP/MPLS | Network Resilience

Research interests include service-provider routing, MPLS VPNs, network convergence, EVPN-VXLAN, data-center networking, and network automation.

[GitHub](https://github.com/ghufr7an) · [ORCID](https://orcid.org/0009-0006-3683-8405) · [Research DOI](https://doi.org/10.5281/zenodo.22983728)

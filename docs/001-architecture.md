# Architecture

## Repository Split

This project uses two repositories.

`trex-core` is a fork of upstream Cisco TRex and contains source changes only.

`trex-modernization` contains the lab, documentation, build containers, OCI infrastructure, test harnesses, traffic profiles, scenarios, and result schemas.

The split is intentional: source-code changes stay reviewable in the fork, while lab machinery and research notes can evolve quickly without polluting the upstream fork.

## Branching Model

`trex-core` should keep `master` aligned with upstream. Modernization work should happen on explicit `dev/*` branches.

Suggested branch families:

- `dev/ubuntu-24.04-baseline`
- `dev/dpdk-25.11`
- `dev/python-modernization`
- `dev/arm64`
- `dev/mlx5`

`trex-modernization` can use `main` for documentation and lab work. Scenario-specific changes can use `dev/*` branches when they are substantial.

## Test Ladder

The dataplane validation ladder is intentionally incremental:

1. Virtio validates build and basic runtime behavior.
2. Generic SR-IOV validates VFIO, hugepages, PCI, and DPDK mechanics.
3. Mellanox PF validates mlx5 hardware.
4. Mellanox SR-IOV validates mlx5 VF behavior.
5. ARM64 repeats the ladder after amd64 is stable.

## Dataplane Families

### Generic DPDK

Generic DPDK devices include virtio, Intel, OCI SR-IOV VFs, and other non-Mellanox devices.

- Prefer `vfio-pci` when hardware and IOMMU support allow it.
- Treat `uio_pci_generic` as a weak fallback for smoke testing only.
- Do not make `igb_uio` a default dependency on modern kernels.

### Mellanox/NVIDIA mlx5

Mellanox/NVIDIA ConnectX devices are different from generic DPDK NICs.

- Keep mlx5 ports bound to kernel drivers such as `mlx5_core`, `mlx5_ib`, and `ib_uverbs`.
- Use `libibverbs`/`libmlx5` from distro `rdma-core` first.
- Run TRex with `--no-ofed-check` when not using NVIDIA OFED.
- Add NVIDIA OFED/MLNX_EN only when distro `rdma-core` is insufficient.

## Container Strategy

Build containers are phase 1 and should be used from the beginning.

Runtime containers are phase 2. DPDK runtime containers need host integration for hugepages, `/dev/vfio`, PCI sysfs, locked memory, CPU pinning, and privileges.

For mlx5 runtime containers, add verbs devices and libraries to the runtime contract as well:

- `/dev/infiniband`
- `rdma-core` libraries and providers
- matching host kernel drivers
- firmware and link mode checks outside the container

The project should avoid making runtime containers a prerequisite for proving the dataplane. Host runtime comes first; containers follow once the device model is understood.

## Build Architecture

The build system should support native and containerized builds.

Build dimensions:

- Architecture: amd64 first, arm64 later.
- OS baseline: Ubuntu 24.04 first.
- TRex baseline: v3.08 lineage first.
- DPDK baseline: upstream-carried DPDK first, then DPDK 25.11 LTS.
- Python baseline: Python 3.12 first, Python 3.14 later.

Build outputs should include:

- build metadata
- dependency versions
- git SHA and branch
- compiler and linker versions
- TRex binary/package artifacts
- a machine-readable manifest

## Traffic Methodology

The initial traffic harness should follow the IPng-style approach:

- warmup at low rate
- ramp offered load
- hold target rate
- record TRex counters
- record DUT counters when available
- store machine-readable JSON
- render graphs later

Later, add CSIT-style NDR/PDR searches.

## Result Architecture

Every test run should produce a structured result document. JSONL is the simplest durable format at the start; Parquet can be added when analysis volume grows.

Result documents should include:

- `run`: timestamp, scenario id, operator/automation id, run duration, pass/fail status.
- `build`: TRex version, git SHA, branch, DPDK version, Python version, compiler, container image when used.
- `host`: architecture, OS, kernel, CPU model, sockets, cores, NUMA, hugepages, IOMMU mode.
- `nic`: vendor, model, PCI IDs, firmware, driver, link speed, queue settings, PF/VF mode.
- `runtime`: TRex command line, config file hash, core count, port mapping, profile, tunables.
- `traffic`: packet size/profile, unidirectional/bidirectional, offered load, ramp/hold shape.
- `trex_metrics`: tx/rx packets, pps, L1/L2 bps, loss, queue-full, CPU, latency and jitter when enabled.
- `dut_metrics`: interface counters, drops, errors, CPU, and device-specific counters when available.
- `environment`: OCI/on-prem, instance type, hypervisor, SR-IOV settings, container runtime when used.

The schema should be owned by `trex-modernization` and versioned explicitly.

## Observability Architecture

Observability has two layers.

### Live metrics

Live metrics are useful while a run is active.

- TRex counters from console/API.
- Host counters from Linux tools, procfs, sysfs, and perf tooling where needed.
- DUT counters from VPP CLI/API, Linux, firewall/router APIs, or `testpmd`.
- Optional Prometheus exporter once the harness stabilizes.

### Historical metrics

Historical metrics are for regression and trend analysis.

- Store raw run artifacts first.
- Convert or ingest into a queryable backend later.
- Keep the backend pluggable: JSONL/Parquet locally, then Prometheus remote write, OpenSearch/Elasticsearch, or PostgreSQL/Timescale if useful.
- Build Grafana dashboards against the chosen backend.

Grafana dashboards should show:

- throughput and packet rate over time
- loss and drop-rate over time
- latency min/avg/max/HDR summaries
- CPU utilization and normalized throughput per core
- queue-full and saturation signals
- DUT-specific counters
- comparison by architecture, NIC, driver, DPDK version, Python version, and scenario

## Regression Architecture

Regression testing should not depend only on hard-coded golden thresholds.

The first regression layer is deterministic smoke testing:

- build succeeds
- TRex starts
- ports are discovered
- traffic starts and stops
- counters increase in expected directions
- result artifact validates against schema

The second layer is baseline comparison:

- compare current run against the last known-good result for the same scenario
- warn on throughput, loss, latency, or CPU drift
- fail only on clear regressions until enough history exists

The third layer is trend analysis:

- use rolling windows by scenario
- track performance stability over time
- detect step changes after dependency updates
- annotate graphs with git SHA, DPDK version, Python version, kernel, firmware, and hardware

## Scenario Architecture

Scenarios should be small, explicit, and reproducible.

Examples:

- `oci-amd64-virtio-smoke`
- `oci-amd64-sriov-vfio`
- `onprem-amd64-cx6lx-pf`
- `onprem-amd64-cx6lx-sriov`
- `oci-arm64-virtio-smoke`
- `onprem-arm64-cx7-pf`
- `onprem-arm64-cx7-sriov`

Each scenario should define:

- prerequisites
- topology
- host setup
- NIC setup
- TRex config
- traffic profiles
- metrics collected
- expected artifacts
- known limitations

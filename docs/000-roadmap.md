# Roadmap

## Goal

Bring TRex forward to supported dependencies and reproducible builds, first on amd64 and later on arm64, while preserving useful traffic-generation behavior.

The first milestone is not to change everything at once. The first milestone is a boring, repeatable Ubuntu 24.04 baseline that can build, start, send simple traffic, collect metrics, and produce comparable result artifacts.

Current task status, next actions, acceptance criteria, and evidence links are tracked in [Progress tracking](002-progress-tracking.md).

## Dependency Tracks

### Track A: supported baseline

- Ubuntu 24.04 LTS.
- TRex v3.08 lineage.
- DPDK version already carried by upstream TRex, currently DPDK 25.07 in the v3.07+ line.
- Python 3.12 from Ubuntu 24.04.
- Distro `rdma-core` first for Mellanox/NVIDIA.

This is the control track. It should prove that the current upstream baseline can be made usable before we move other dependency axes.

### Track B: DPDK LTS update

- Ubuntu 24.04 LTS.
- TRex v3.08 lineage plus required patches.
- DPDK 25.11 LTS.
- Python 3.12.

This track isolates DPDK movement while keeping Python stable.

### Track C: future Python compatibility

- Ubuntu 24.04 LTS where practical, with newer Python supplied explicitly.
- DPDK 25.11 LTS.
- Python 3.14.

This track is for future-proofing. It should not block Track A or Track B.

## Workstreams

- Dataplane enablement: virtio, generic SR-IOV, mlx5 PF, mlx5 SR-IOV.
- Dependency modernization: DPDK, Python, Scapy, bundled shared libraries, kernel module assumptions.
- Build reproducibility: native builds first, then containerized build images.
- Runtime packaging: host runtime first, runtime containers later.
- Test harness: profiles, scenarios, ramp tests, result schema, and repeatable reports.
- Observability and regression: metrics collection, result storage, trend analysis, Grafana dashboards, regression gates.

## Phase 1: amd64 Track A baseline on OCI virtio

- Build stock upstream TRex.
- Prefer TRex v3.08 as the baseline.
- Run low-rate virtio/software-mode smoke tests.
- Inventory vendored and runtime dependencies.
- Define result JSON schema and baseline reports.
- Validate that the management NIC stays under Linux and dataplane NICs are dedicated to TRex.
- Treat performance as functional signal only; do not use virtio smoke tests as final dataplane benchmarks.

## Phase 2: amd64 Track A baseline on OCI SR-IOV

- Validate hugepages, VFIO, PCI discovery, and DPDK EAL behavior.
- Validate TRex startup against a real PCI VF.
- Prefer `vfio-pci` for generic DPDK devices.
- Avoid relying on `igb_uio`; use current `igb_uio` fixes only as research or fallback.
- Exercise runtime container requirements, but do not make runtime containers a blocker.
- Capture a first comparable result set in JSON/JSONL.

## Phase 3: amd64 Track A baseline on-prem CX6 LX

- Validate mlx5 PF mode.
- Use distro `rdma-core` first.
- Build and run with `--no-ofed-check`.
- Keep ConnectX ports on `mlx5_core`/`mlx5_ib`; do not bind Mellanox ports to `vfio-pci`.
- Introduce NVIDIA OFED/MLNX_EN only if distro `rdma-core` fails or lacks needed features/performance.
- Run baseline stateless traffic profiles.
- Collect TRex, host, and DUT metrics.
- Build Grafana dashboard prototypes from stored run data.

## Phase 4: amd64 Track A baseline on-prem CX6 LX SR-IOV

- Validate mlx5 VF mode.
- Confirm PF/VF provisioning workflow.
- Compare PF and VF behavior.
- Record firmware, driver, `rdma-core`/OFED, NUMA, and queue configuration in every result.

## Phase 5: amd64 Track B DPDK 25.11 LTS

- Move from upstream DPDK 25.07 lineage to DPDK 25.11 LTS.
- Keep Python 3.12 stable.
- Review and adapt upstream PRs for kernel/Python/dependency issues.
- Re-run Phase 1 through Phase 4 scenarios and compare results against Track A.

## Phase 6: amd64 Track C Python 3.14

- Move Python-side tooling and client usage toward Python 3.14.
- Replace removed/deprecated Python APIs, including `imp` usage.
- Remove or replace stale bundled Python dependencies where practical.
- Re-run enough of the harness to catch client, console, Scapy, and automation breakage.

## Phase 7: arm64 Track A on OCI virtio

- Build on native arm64.
- Run non-mlx virtio smoke tests.
- Identify architecture-specific build assumptions.
- Verify that containerized build images are multi-arch-capable.

## Phase 8: arm64 Track A on-prem CX7

- Enable mlx5 build/runtime path on arm64.
- Validate CX7 PF mode.
- Run the same amd64 traffic profile matrix.
- Compare CX6 LX amd64 and CX7 arm64 behavior without changing multiple variables at once.

## Phase 9: arm64 Track A on-prem CX7 SR-IOV

- Validate CX7 VF mode.
- Compare amd64 and arm64 behavior.
- Prepare final build/runtime packaging.

## Regression and Reporting Milestones

### R1: local artifacts

- Every run writes a structured JSON or JSONL result.
- Results include build, host, NIC, driver, firmware, runtime options, scenario, profile, counters, and pass/fail status.
- Artifacts are kept locally under `results/` or uploaded by CI/lab automation.

### R2: analysis

- Add scripts to summarize multiple runs.
- Compute throughput, loss ratio, CPU-normalized throughput, latency summaries, and scenario comparisons.
- Support baseline comparison between Track A, Track B, and Track C.

### R3: dashboarding

- Add Grafana dashboards for trend views.
- Track per-scenario throughput, packet loss, latency, CPU, queue-full counters, and DUT-side metrics.
- Keep the metrics backend pluggable: JSONL/Parquet first, then Prometheus and OpenSearch/Elasticsearch or PostgreSQL/Timescale if useful.

### R4: regression gates

- Define soft warnings for drift and hard failures for clear regressions.
- Avoid brittle static thresholds as the only signal.
- Use trend and baseline comparisons once enough historical data exists.

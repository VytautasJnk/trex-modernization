# TRex Modernization Lab

Research, planning, lab automation, and test methodology for modernizing Cisco TRex across amd64 and arm64 targets.

The project starts with a conservative Ubuntu 24.04 + TRex v3.08 baseline on amd64, then moves one variable at a time: generic DPDK/SR-IOV, Mellanox/NVIDIA mlx5 on ConnectX-6 LX, DPDK 25.11 LTS, future Python compatibility, and finally arm64 with ConnectX-7.

## Repositories

- [`trex-modernization`](https://github.com/VytautasJnk/trex-modernization.git) - This repository. It owns documentation, lab scenarios, OCI notes, build-container plans, traffic profiles, result schemas, observability design, and regression methodology.
- [`trex-core`](https://github.com/VytautasJnk/trex-core.git) - Fork of upstream TRex. Source changes, dependency bumps, DPDK updates, Python fixes, and architecture fixes belong there.
- [`cisco-system-traffic-generator/trex-core`](https://github.com/cisco-system-traffic-generator/trex-core) - Upstream TRex source. Local `trex-core` should keep `master` aligned with this upstream.

## Current Direction

The research so far points to four strong rules:

- Start from TRex v3.08 on Ubuntu 24.04 with Python 3.12.
- Treat generic DPDK devices and Mellanox/NVIDIA mlx5 devices as different dataplane families.
- Prefer `vfio-pci` for generic PCI devices; do not rely on `igb_uio` on modern kernels.
- For ConnectX, keep ports on `mlx5_core`/`mlx5_ib`, use distro `rdma-core` first, and run with `--no-ofed-check` before introducing NVIDIA OFED/MLNX_EN.

Observability and regression testing are first-class project goals. Every meaningful run should eventually emit a structured result artifact and feed trend analysis or Grafana dashboards.

## Documentation Map

Start with these documents:

- [Roadmap](docs/000-roadmap.md) - Dependency tracks, phase sequence, and reporting milestones.
- [Architecture](docs/001-architecture.md) - Repository split, dataplane families, build/runtime design, result schema, observability, regression, and scenario architecture.
- [Progress tracking](docs/002-progress-tracking.md) - Working status table, next actions, acceptance criteria, and evidence links.
- [Test methodology](docs/test-methodology.md) - Topologies, traffic profiles, metrics, result artifacts, dashboards, and regression rules.
- [Dependency inventory](docs/dependency-inventory.md) - Current dependency areas and modernization concerns.
- [Research links](docs/research-links.md) - Articles, vendor docs, upstream PRs, VPP/CSIT/IPng methodology links, and observability references.
- [Docs index](docs/README.md) - Short index for the `docs/` tree.

## Directory Map

- [docs/](docs/README.md) - Design notes, roadmap, progress tracking, architecture, methodology, dependency inventory, and research links.
- [scenarios/](scenarios/README.md) - Named lab scenarios such as OCI virtio, OCI SR-IOV, on-prem CX6 LX, and arm64 CX7.
- [profiles/](profiles/README.md) - Traffic profile definitions and conventions: 1514B, IMIX, 64B multi-flow, and 64B single-flow.
- [harness/](harness/README.md) - Planned runner, counter collection, result writing, analysis, and regression tooling.
- [containers/](containers/README.md) - Build container plans and later runtime-container considerations.
- [infra/](infra/README.md) - Infrastructure entry point.
- [infra/oci/](infra/oci/README.md) - OCI-specific notes for amd64/arm64 virtio and SR-IOV labs.
- [results/](results/README.md) - Result schema notes, small examples, and artifact-storage policy.
- [`.gitignore`](.gitignore) - Keeps generated builds, large raw results, packet captures, Python caches, and editor files out of Git.

## Dependency Tracks

Track A is the supported baseline:

- Ubuntu 24.04 LTS
- TRex v3.08 lineage
- Upstream-carried DPDK, currently DPDK 25.07 in the v3.07+ line
- Python 3.12
- Distro `rdma-core` first for ConnectX

Track B isolates the DPDK LTS update:

- Ubuntu 24.04 LTS
- TRex v3.08 lineage plus required patches
- DPDK 25.11 LTS
- Python 3.12

Track C isolates future Python compatibility:

- DPDK 25.11 LTS
- Python 3.14
- Python/client/tooling cleanup after Track A and B are stable

## Validation Ladder

1. OCI amd64 virtio smoke tests.
2. OCI amd64 SR-IOV with generic VFIO/DPDK validation.
3. On-prem amd64 ConnectX-6 LX PF.
4. On-prem amd64 ConnectX-6 LX SR-IOV.
5. DPDK 25.11 LTS update on the proven amd64 scenarios.
6. Python 3.14 compatibility checks.
7. OCI arm64 virtio smoke tests.
8. On-prem arm64 ConnectX-7 PF.
9. On-prem arm64 ConnectX-7 SR-IOV.

## Expected Workflow

1. Document the scenario in [scenarios/](scenarios/README.md).
2. Select or create the traffic profile in [profiles/](profiles/README.md).
3. Build or install the matching TRex baseline from `trex-core`.
4. Run through the harness flow described in [harness/](harness/README.md).
5. Write result artifacts following [results/](results/README.md) and [test methodology](docs/test-methodology.md).
6. Compare against previous runs and feed Grafana or another analysis backend when available.

Source changes should be made in `trex-core` on `dev/*` branches. Lab documentation, scenarios, profiles, dashboards, and result schemas should be kept here.

## Observability and Regression

The first result backend should be simple local JSON or JSONL artifacts. From there, the project can add:

- Pandas or DuckDB analysis for local summaries.
- Prometheus-style live metrics for active runs.
- Grafana dashboards for trend views.
- OpenSearch/Elasticsearch or PostgreSQL/Timescale if searchable long-term history becomes useful.

Regression should start with deterministic smoke gates, then add baseline comparisons, then trend-based detection once enough history exists. Avoid making brittle static thresholds the only signal.

# Progress Tracking

This document is the working status view for the modernization effort. The [roadmap](000-roadmap.md) defines the intended sequence; this file records what is pending, active, blocked, or complete.

Last reviewed: 2026-05-06.

## Status Key

- `TODO` - Not started.
- `DOING` - Active work in the local repo or lab.
- `BLOCKED` - Waiting on access, hardware, upstream input, or a decision.
- `DONE` - Accepted and linked to evidence.
- `DEFERRED` - Intentionally postponed.

## Tracking Rules

- Keep one row per trackable unit of work.
- Every non-trivial row should have acceptance criteria and an evidence location.
- Use `trex-core` `dev/*` branches for source changes.
- Use `trex-modernization` for documentation, scenarios, profiles, harness plans, result schemas, dashboards, and lab notes.
- When GitHub issues or a project board are introduced, keep this file as the high-level status index and link rows to the issue or board item.

## Current Milestone

Prepare a complete documentation and lab-planning baseline, then start amd64 Track A on Ubuntu 24.04 with OCI virtio smoke testing.

## Tracker

| Status | ID | Area | Task | Repo | Branch | Acceptance Criteria | Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- |
| `DOING` | DOC-001 | Documentation | Establish main project documentation tree. | `trex-modernization` | `main` | Main README links roadmap, architecture, methodology, dependency inventory, research links, and progress tracking. | [README](../README.md), [docs index](README.md) |
| `DOING` | DOC-002 | Research | Capture external research links with brief notes. | `trex-modernization` | `main` | All known user-provided and researched links are listed with short descriptions. | [research links](research-links.md) |
| `TODO` | DOC-003 | Tracking | Create GitHub issues or project board from this tracker. | `trex-modernization` | `main` | Project tasks have issue or board references and this tracker links to them. | Pending |
| `TODO` | SRC-001 | Upstream workflow | Create first modernization branch for amd64 Track A work. | `trex-core` | `dev/amd64-track-a-ubuntu2404` | Branch is based on upstream `master`, pushed to project remotes, and ready for patches. | Pending |
| `TODO` | A1-001 | amd64 Track A | Build stock upstream TRex on Ubuntu 24.04 amd64. | `trex-core` | `dev/amd64-track-a-ubuntu2404` | Build completes with documented host packages and reproducible commands. | Pending |
| `TODO` | A1-002 | amd64 Track A | Run OCI virtio smoke test. | `trex-modernization` | `main` | TRex starts, sends low-rate traffic or software-mode traffic, and exits cleanly with logs. | Pending |
| `TODO` | A1-003 | Results | Define first result JSON or JSONL schema. | `trex-modernization` | `main` | A sample artifact records build, host, NIC, driver, scenario, profile, counters, and pass/fail status. | [results](../results/README.md) |
| `TODO` | A1-004 | Harness | Define first repeatable runner flow. | `trex-modernization` | `main` | Runner steps are documented for setup, execution, counter collection, result writing, and comparison. | [harness](../harness/README.md) |
| `TODO` | A2-001 | amd64 SR-IOV | Validate OCI generic SR-IOV path. | `trex-core`, `trex-modernization` | `dev/amd64-track-a-ubuntu2404` | VFIO, hugepages, PCI discovery, and TRex startup are captured in a result artifact. | Pending |
| `TODO` | A3-001 | amd64 mlx5 | Validate on-prem CX6 LX PF path. | `trex-core`, `trex-modernization` | `dev/amd64-track-a-ubuntu2404` | TRex runs with mlx5 kernel drivers, distro `rdma-core`, and `--no-ofed-check`. | Pending |
| `TODO` | A4-001 | amd64 mlx5 SR-IOV | Validate on-prem CX6 LX VF path. | `trex-core`, `trex-modernization` | `dev/amd64-track-a-ubuntu2404` | PF/VF provisioning and VF traffic-generation behavior are documented and compared with PF mode. | Pending |
| `TODO` | B-001 | DPDK | Start DPDK 25.11 LTS branch after Track A is stable. | `trex-core` | `dev/dpdk-25.11-lts` | Track B branch builds and reruns Track A scenarios for comparison. | Pending |
| `TODO` | C-001 | Python | Start Python 3.14 compatibility branch after Track A and Track B are stable. | `trex-core` | `dev/python-3.14` | Client, console, Scapy, and automation paths run under Python 3.14. | Pending |
| `TODO` | OBS-001 | Observability | Prototype local analysis from JSON/JSONL artifacts. | `trex-modernization` | `main` | Multiple run artifacts can be summarized and compared locally. | Pending |
| `TODO` | OBS-002 | Observability | Prototype Grafana dashboards. | `trex-modernization` | `main` | Dashboard shows throughput, loss, latency, CPU, queue-full counters, and DUT-side metrics for stored runs. | Pending |
| `TODO` | ARM-001 | arm64 | Start OCI arm64 virtio validation after amd64 baseline. | `trex-core`, `trex-modernization` | `dev/arm64-track-a-ubuntu2404` | Native arm64 build and virtio smoke test complete with result artifact. | Pending |
| `TODO` | ARM-002 | arm64 mlx5 | Validate on-prem DGX Spark with CX7. | `trex-core`, `trex-modernization` | `dev/arm64-track-a-ubuntu2404` | CX7 PF and VF paths are tested using the same profile matrix as amd64. | Pending |

## Next Actions

1. Finish and commit the documentation baseline.
2. Create `trex-core` branch `dev/amd64-track-a-ubuntu2404`.
3. Prepare the Ubuntu 24.04 amd64 build host notes.
4. Attempt the first stock TRex build.
5. Record the first smoke-test result artifact shape, even if the run fails.

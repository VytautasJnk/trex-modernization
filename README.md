# TRex Modernization Lab

Research and implementation workspace for modernizing Cisco TRex across amd64 and arm64 targets.

The project starts with amd64 in OCI using virtio, then OCI SR-IOV, then on-prem amd64 with Mellanox ConnectX-6 LX. After that, the same work moves to arm64 using OCI virtio smoke tests and finally on-prem arm64 with ConnectX-7.

## Repositories

- `trex-modernization`: documentation, OCI infrastructure, container builders, test harnesses, and methodology.
- `trex-core`: fork of upstream TRex source. This is where source changes, dependency bumps, DPDK updates, and architecture fixes belong.

## Initial Phases

1. OCI amd64 with virtio: reproducible builds and software-mode smoke tests.
2. OCI amd64 with SR-IOV: generic VFIO, hugepage, PCI, and DPDK runtime validation.
3. On-prem amd64 with CX6 LX: first true mlx5 dataplane validation.
4. On-prem amd64 with CX6 LX SR-IOV: mlx5 VF validation.
5. OCI arm64 with virtio: ARM64 build/runtime smoke.
6. On-prem arm64 with CX7: ARM64 mlx5 port and performance validation.
7. On-prem arm64 with CX7 SR-IOV: final VF path.

## Start Here

- [Roadmap](docs/000-roadmap.md)
- [Architecture](docs/001-architecture.md)
- [Dependency Inventory](docs/dependency-inventory.md)
- [Test Methodology](docs/test-methodology.md)


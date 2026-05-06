# Scenarios

This directory will hold named, reproducible lab scenarios. A scenario describes topology and environment; traffic intent belongs in [profiles](../profiles/README.md), and execution belongs in [harness](../harness/README.md).

See also:

- [Main README](../README.md)
- [Roadmap](../docs/000-roadmap.md)
- [Architecture: Scenario Architecture](../docs/001-architecture.md#scenario-architecture)
- [Test methodology](../docs/test-methodology.md)
- [OCI infrastructure](../infra/oci/README.md)
- [Results](../results/README.md)

## Planned Scenarios

- `oci-amd64-virtio-smoke` - OCI amd64 build/start/config/API validation with virtio.
- `oci-amd64-sriov-vfio` - OCI amd64 generic SR-IOV validation with VFIO.
- `onprem-amd64-cx6lx-pf` - On-prem amd64 ConnectX-6 LX physical-function validation.
- `onprem-amd64-cx6lx-sriov` - On-prem amd64 ConnectX-6 LX VF/SR-IOV validation.
- `oci-arm64-virtio-smoke` - OCI arm64 build/start/config/API validation with virtio.
- `onprem-arm64-cx7-pf` - On-prem arm64 ConnectX-7 physical-function validation.
- `onprem-arm64-cx7-sriov` - On-prem arm64 ConnectX-7 VF/SR-IOV validation.

## Scenario Template

Each scenario should document:

- purpose and dependency track
- topology diagram
- hardware or cloud shape
- OS image and kernel
- NIC model, driver, firmware, and link mode
- hugepages, IOMMU, VFIO, or mlx5/rdma-core setup
- TRex config path and expected port mapping
- profiles to run
- counters and metrics to collect
- expected result artifacts
- known limitations

The scenario name should be included in every result artifact.

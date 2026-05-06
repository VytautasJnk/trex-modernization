# Infrastructure

This directory is the entry point for lab infrastructure. Provider-specific infrastructure lives in subdirectories.

See also:

- [Main README](../README.md)
- [Roadmap](../docs/000-roadmap.md)
- [Architecture](../docs/001-architecture.md)
- [Scenarios](../scenarios/README.md)

## Subdirectories

- [oci/](oci/README.md) - Oracle Cloud Infrastructure notes for amd64/arm64 virtio and SR-IOV labs.

## Scope

Infrastructure should describe how to create repeatable test environments, not how to patch TRex itself. Source changes belong in `trex-core`; lab definitions, cloud-init, Terraform, and environment notes belong here.

Infrastructure work should record enough metadata for [result artifacts](../results/README.md): instance type, architecture, kernel, NIC shape, hypervisor, SR-IOV settings, hugepages, and IOMMU/VFIO state.


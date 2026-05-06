# OCI Infrastructure

This directory will hold Oracle Cloud Infrastructure setup code and notes.

See also:

- [Infrastructure overview](../README.md)
- [Main README](../../README.md)
- [Roadmap](../../docs/000-roadmap.md)
- [Architecture](../../docs/001-architecture.md)
- [Scenarios](../../scenarios/README.md)
- [Results](../../results/README.md)

## Planned Contents

- Terraform for amd64 virtio smoke-test instances.
- Terraform for amd64 SR-IOV instances.
- Terraform or notes for arm64 virtio smoke-test instances.
- Cloud-init for build dependencies.
- Cloud-init for hugepages, IOMMU, and VFIO setup.
- Notes for OCI image selection and shape limitations.

## OCI Scenario Targets

- `oci-amd64-virtio-smoke` - Build/start/config/API validation. Performance is functional signal only.
- `oci-amd64-sriov-vfio` - Generic SR-IOV, VFIO, hugepage, PCI, and DPDK validation.
- `oci-arm64-virtio-smoke` - Native arm64 build and smoke validation before moving to on-prem CX7.

## Metadata To Capture

Every OCI run should capture:

- region and availability domain
- image name and OCID
- shape
- architecture
- kernel
- VNIC count and attachment layout
- SR-IOV/VFIO state where relevant
- hugepage configuration
- TRex version, DPDK version, Python version

These fields should flow into the result schema described in [test methodology](../../docs/test-methodology.md) and [results](../../results/README.md).

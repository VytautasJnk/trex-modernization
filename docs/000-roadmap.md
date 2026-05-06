# Roadmap

## Goal

Bring TRex forward to supported dependencies and reproducible builds, first on amd64 and later on arm64, while preserving useful traffic-generation behavior.

## Phase 1: OCI amd64 Virtio

- Build stock upstream TRex.
- Establish containerized build images.
- Run low-rate virtio/software-mode smoke tests.
- Inventory vendored and runtime dependencies.
- Define result JSON schema and baseline reports.

## Phase 2: OCI amd64 SR-IOV

- Validate hugepages, VFIO, PCI discovery, and DPDK EAL behavior.
- Validate TRex startup against a real PCI VF.
- Exercise runtime container requirements, but do not make runtime containers a blocker.

## Phase 3: On-Prem amd64 CX6 LX

- Validate mlx5 PF mode.
- Validate NVIDIA OFED/DOCA/rdma-core compatibility.
- Run baseline stateless traffic profiles.
- Collect TRex, host, and DUT metrics.

## Phase 4: On-Prem amd64 CX6 LX SR-IOV

- Validate mlx5 VF mode.
- Confirm PF/VF provisioning workflow.
- Compare PF and VF behavior.

## Phase 5: OCI arm64 Virtio

- Build on native arm64.
- Run non-mlx virtio smoke tests.
- Identify architecture-specific build assumptions.

## Phase 6: On-Prem arm64 CX7

- Enable mlx5 build/runtime path on arm64.
- Validate CX7 PF mode.
- Run the same amd64 traffic profile matrix.

## Phase 7: On-Prem arm64 CX7 SR-IOV

- Validate CX7 VF mode.
- Compare amd64 and arm64 behavior.
- Prepare final build/runtime packaging.


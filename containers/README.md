# Build Containers

This directory will hold container definitions for reproducible TRex builds. Runtime containers are intentionally later than build containers because DPDK and mlx5 runtime need host devices, hugepages, PCI sysfs, capabilities, and driver alignment.

See also:

- [Main README](../README.md)
- [Roadmap](../docs/000-roadmap.md)
- [Architecture: Container Strategy](../docs/001-architecture.md#container-strategy)
- [Dependency inventory](../docs/dependency-inventory.md)

## Planned Builder Images

- `trex-builder:amd64-ubuntu24.04`
- `trex-builder:arm64-ubuntu24.04`
- Optional legacy comparison image: `trex-builder:amd64-ubuntu22.04`

## Build Dimensions

- Architecture: amd64 first, arm64 later.
- OS: Ubuntu 24.04 first.
- TRex: v3.08 lineage first.
- DPDK: upstream-carried DPDK first, DPDK 25.11 LTS later.
- Python: 3.12 first, 3.14 compatibility later.

## Expected Outputs

Build containers should eventually produce:

- TRex build artifacts.
- Build manifest with git SHA, branch, compiler, DPDK version, Python version, OS image, and architecture.
- Logs suitable for regression analysis.

Runtime container notes should link back to [scenarios](../scenarios/README.md) once device-specific runtime experiments begin.

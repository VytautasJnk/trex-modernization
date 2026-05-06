# Dependency Inventory

This file tracks the baseline and target dependency versions.

## Baseline

| Component | Observed baseline | Notes |
|---|---:|---|
| TRex | v3.08 | Current upstream version observed during research. |
| DPDK | 25.07 | TRex v3.07 release notes mention upgrade to DPDK 25.07. |
| Python client | 3.12 support | TRex v3.08 release notes mention Python 3.12 client support. |
| OpenSSL | 1.1.0f vendored | High priority to remove or replace. |
| libzmq | 4.3.1 vendored | Update after stock build is reproducible. |
| PyYAML | 3.11 vendored | High priority to update. |
| Scapy | 2.4.3 vendored | Needs compatibility testing. |
| yaml-cpp | 0.3.0 vendored | Prefer distro/system or current vendored version. |
| jsoncpp | vendored amalgamation | Prefer distro/system or current vendored version. |
| BIRD | 2.0.8 | Used by optional BIRD build path. |

## Target Direction

| Component | Initial target |
|---|---:|
| DPDK | 25.11 LTS |
| OpenSSL | system OpenSSL 3.x |
| libzmq | 4.3.5 |
| PyYAML | 6.x |
| Scapy | current stable after compatibility tests |
| yaml-cpp | current distro package or 0.8.x |
| jsoncpp | current distro package or 1.9.x |
| BIRD | current 2.x first, 3.x only after explicit testing |

## Rules

- Change one dependency layer at a time.
- Keep DPDK unchanged while updating Python and user-space libraries.
- Move to DPDK 25.11 LTS only after stock v3.08 is reproducible.
- Do not treat OCI SR-IOV as mlx5 validation unless OCI exposes Mellanox hardware.


# Architecture

## Repository Split

This project uses two repositories.

`trex-core` is a fork of upstream Cisco TRex and contains source changes only.

`trex-modernization` contains the lab, documentation, build containers, OCI infrastructure, test harnesses, traffic profiles, scenarios, and result schemas.

## Test Ladder

The dataplane validation ladder is intentionally incremental:

1. Virtio validates build and basic runtime behavior.
2. Generic SR-IOV validates VFIO, hugepages, PCI, and DPDK mechanics.
3. Mellanox PF validates mlx5 hardware.
4. Mellanox SR-IOV validates mlx5 VF behavior.
5. ARM64 repeats the ladder after amd64 is stable.

## Container Strategy

Build containers are phase 1 and should be used from the beginning.

Runtime containers are phase 2. DPDK runtime containers need host integration for hugepages, `/dev/vfio`, PCI sysfs, locked memory, CPU pinning, and privileges.

## Traffic Methodology

The initial traffic harness should follow the IPng-style approach:

- warmup at low rate
- ramp offered load
- hold target rate
- record TRex counters
- record DUT counters when available
- store machine-readable JSON
- render graphs later

Later, add CSIT-style NDR/PDR searches.


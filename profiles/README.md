# Traffic Profiles

This directory will hold reusable TRex traffic profiles and profile metadata.

See also:

- [Main README](../README.md)
- [Test methodology: Traffic Profiles](../docs/test-methodology.md#traffic-profiles)
- [Scenarios](../scenarios/README.md)
- [Harness](../harness/README.md)
- [Results](../results/README.md)

## Planned Profiles

- `1514b` - Large-packet throughput.
- `imix` - Mixed packet sizes approximating internet-like traffic.
- `64b-var2` - Worst-case packet-rate profile with varied source/destination fields.
- `64b-single-flow` - Worst-case single-flow/queue behavior.

Each profile should support unidirectional and bidirectional variants when possible.

## Profile Metadata

Each profile should eventually document:

- packet size or IMIX distribution
- protocol fields
- flow variation strategy
- default rate mode
- supported tunables
- latency-stream behavior
- known hardware or TRex limitations

Profiles should be reusable across virtio, SR-IOV, PF, VF, amd64, and arm64 scenarios. Scenario-specific wiring belongs in [scenarios](../scenarios/README.md), not in the profile itself.

# Test Methodology

## Topologies

### Single DUT

```text
TRex port0 -> DUT -> TRex port1
TRex port1 -> DUT -> TRex port0
```

### VM Smoke

```text
TRex VM port0 -> bridge-left  -> DUT VM -> bridge-right -> TRex VM port1
```

## Traffic Profiles

- `1514b`: large packet throughput.
- `imix`: internet-like mixed packet sizes.
- `64b-var2`: worst-case packet rate with varied source and destination IPs.
- `64b-single-flow`: worst-case packet rate on a single flow/queue.

Each profile should run unidirectional and bidirectional variants.

## Run Shape

1. Warmup at low rate.
2. Ramp offered load.
3. Hold target rate.
4. Collect counters.
5. Mark saturation when loss exceeds threshold.
6. Save JSON.

## Core Metrics

- offered load
- tx/rx packets
- tx/rx bps L1 and L2
- tx/rx pps
- loss packets and loss ratio
- latency min/avg/max/HDR when enabled
- TRex CPU
- queue-full counters
- host CPU, NUMA placement, interrupts
- DUT counters where available

## Later Metrics

For VPP DUTs, collect:

- interface packets/bytes
- drops/errors
- node calls
- node vectors
- node clocks
- clocks per vector
- vectors per call

## Search Algorithms

Start with simple ramp tests. Add CSIT-style NDR/PDR search once the harness is reliable.


# Test Methodology

The harness should make tests repeatable before it makes them fancy. Each run should be reconstructable from its scenario file, TRex config, profile tunables, host metadata, and result artifact.

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

### Two-Node Linear

```text
TRex TX node -> network/DUT/path -> TRex RX node
```

This topology is useful later for distributed testing, but one-way latency requires a clock strategy. Start with loopback/single-DUT latency before treating distributed one-way latency as authoritative.

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

The initial harness should support IPng-style warmup/ramp/hold tests. CSIT-style NDR/PDR searches come after basic result collection is reliable.

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

## DUT Metrics

For VPP DUTs, collect:

- interface packets/bytes
- drops/errors
- node calls
- node vectors
- node clocks
- clocks per vector
- vectors per call

For Linux DUTs, collect:

- interface packets/bytes
- drops/errors
- softirq and CPU utilization
- `ethtool -S` counters where available
- route and neighbor state

For `testpmd` DUTs, collect:

- port statistics
- forwarding mode
- queue counts
- burst size
- descriptor counts
- offload settings

For firewall/router DUTs, collect:

- interface counters
- policy/NAT hit counters where relevant
- CPU and dataplane utilization
- drops and errors

## Result Artifacts

Every run should write a result document with:

- run id and timestamp
- scenario id
- build metadata
- host metadata
- NIC and driver metadata
- TRex command line and config hash
- profile and tunables
- ramp and hold parameters
- core metrics
- DUT metrics when available
- pass/fail/warn status
- notes and known limitations

Use JSON or JSONL first. Keep raw artifacts immutable so later analysis can improve without rerunning the test.

## Observability

The first implementation can be pull-based: the harness polls TRex, host, and DUT counters during a run.

Later, add live metric export:

- Prometheus endpoint or textfile exporter for active runs.
- Grafana dashboard for current run and historical trends.
- Optional OpenSearch/Elasticsearch or PostgreSQL/Timescale ingestion for long-term querying.

Dashboards should make regressions visible by scenario, not just by machine:

- throughput over time
- Mpps/Gbps per core
- packet loss and drop-rate
- latency and jitter
- queue-full and saturation counters
- CPU utilization
- NIC/driver/firmware changes
- DPDK/Python/TRex version changes

## Search Algorithms

Start with simple ramp tests. Add CSIT-style NDR/PDR search once the harness is reliable.

## Regression Rules

Regression detection should evolve in stages.

Stage 1: smoke gates

- Build succeeds.
- TRex starts.
- Ports are discovered.
- Traffic starts and stops.
- Counters move.
- Result schema validates.

Stage 2: scenario baselines

- Compare against last known-good run for the same scenario.
- Warn on moderate throughput, CPU, loss, or latency drift.
- Fail on severe loss, startup failures, schema failures, or clearly broken counters.

Stage 3: trend-based detection

- Use rolling windows per scenario.
- Annotate changes with git SHA, kernel, firmware, DPDK, Python, and hardware.
- Prefer trend shifts over brittle one-off thresholds.

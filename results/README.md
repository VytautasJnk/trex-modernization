# Results

This directory tracks result schemas, small examples, and notes about artifact handling.

See also:

- [Main README](../README.md)
- [Architecture: Result Architecture](../docs/001-architecture.md#result-architecture)
- [Architecture: Regression Architecture](../docs/001-architecture.md#regression-architecture)
- [Test methodology: Result Artifacts](../docs/test-methodology.md#result-artifacts)
- [Harness](../harness/README.md)
- [Scenarios](../scenarios/README.md)

## Storage Policy

Small examples and schema files belong in Git.

Large raw results should live outside Git:

- artifact storage
- OCI Object Storage
- GitHub Releases
- local lab storage

The repository ignores `results/raw/` to avoid accidentally committing large captures or run outputs.

## Initial Result Format

Use JSON or JSONL first. Each run should include:

- run id and timestamp
- scenario id
- build metadata
- host metadata
- NIC and driver metadata
- TRex command line and config hash
- traffic profile and tunables
- ramp and hold parameters
- TRex metrics
- host metrics
- DUT metrics where available
- pass/fail/warn status

Later analysis can convert raw JSONL into Parquet, Prometheus metrics, OpenSearch/Elasticsearch documents, or PostgreSQL/Timescale rows.

## Dashboard Path

Grafana dashboards should be built from the same result data, not from hand-entered summaries. The first dashboards should cover throughput, Mpps/Gbps per core, loss, latency, CPU, queue-full counters, and version/hardware annotations.

# Test Harness

This directory will contain the TRex runner, metrics collection, result writing, and regression-analysis code.

See also:

- [Main README](../README.md)
- [Architecture: Result Architecture](../docs/001-architecture.md#result-architecture)
- [Architecture: Observability Architecture](../docs/001-architecture.md#observability-architecture)
- [Test methodology](../docs/test-methodology.md)
- [Profiles](../profiles/README.md)
- [Scenarios](../scenarios/README.md)
- [Results](../results/README.md)

## Initial Goals

- Run named traffic profiles.
- Run named topology scenarios.
- Collect TRex counters.
- Collect host counters.
- Collect DUT counters where available.
- Write JSON or JSONL results.
- Validate result schema.
- Produce simple local summaries.

## Later Goals

- Add CSIT-style NDR/PDR searches.
- Add Prometheus-style live metric export.
- Add Grafana dashboard inputs.
- Add baseline and trend-based regression checks.
- Support OCI and on-prem scenario metadata consistently.

## Harness Shape

The harness should keep scenario, profile, runner, and result concerns separate:

- `scenarios/` describes the topology and environment.
- `profiles/` describes packet/profile intent and tunables.
- `harness/` executes runs and collects metrics.
- `results/` defines the artifact format and examples.

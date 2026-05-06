# Documentation

This directory contains the project-level research and design documents. The top-level project overview is in the [main README](../README.md).

## Index

- [Roadmap](000-roadmap.md) - Dependency tracks, phase order, and regression/reporting milestones.
- [Architecture](001-architecture.md) - Repository split, dataplane families, build/runtime design, result schema, observability, regression, and scenario architecture.
- [Progress tracking](002-progress-tracking.md) - Working status table, next actions, acceptance criteria, and evidence links.
- [Dependency inventory](dependency-inventory.md) - Current dependency areas and modernization risks.
- [Test methodology](test-methodology.md) - Topologies, profiles, run shape, metrics, result artifacts, dashboards, and regression rules.
- [Research links](research-links.md) - External articles, vendor docs, upstream PRs, and methodology references gathered during research.

## How To Use This Directory

- Put durable project decisions in [Architecture](001-architecture.md).
- Put sequencing and milestone changes in [Roadmap](000-roadmap.md).
- Put current status and next actions in [Progress tracking](002-progress-tracking.md).
- Put benchmarking and observability details in [Test methodology](test-methodology.md).
- Put external source material in [Research links](research-links.md).
- Put package/library/runtime concerns in [Dependency inventory](dependency-inventory.md).

Scenario-specific instructions should live in [scenarios/](../scenarios/README.md), reusable traffic definitions in [profiles/](../profiles/README.md), and runner or result-collection details in [harness/](../harness/README.md).

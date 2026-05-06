# TRex modernization research links

This file tracks source material for the TRex modernization project. It includes links supplied during planning and links found during follow-up research.

## Project repositories

- [VytautasJnk/trex-core](https://github.com/VytautasJnk/trex-core.git) - Project fork of upstream TRex core.
- [VytautasJnk/trex-modernization](https://github.com/VytautasJnk/trex-modernization.git) - Documentation, lab notes, scenarios, and modernization planning repository.
- `ssh://git@forge.dblngtvnfnt.eu/dblngtvnfnt/trex-core.git` - Local Forgejo mirror/remote for the TRex core fork.
- `ssh://git@forge.dblngtvnfnt.eu/dblngtvnfnt/trex-modernization.git` - Local Forgejo mirror/remote for the modernization repository.
- [cisco-system-traffic-generator/trex-core](https://github.com/cisco-system-traffic-generator/trex-core) - Upstream TRex core repository.
- [Cisco Systems Traffic Generators GitHub org](https://github.com/cisco-system-traffic-generator) - Upstream organization containing TRex core, EMU, GUI, and related repositories.

## Baseline TRex references

- [TRex home](https://trex-tgn.cisco.com/) - Main TRex site with mode summaries and documentation entry points.
- [TRex release notes](https://trex-tgn.cisco.com/trex/doc/release_notes.html) - Key source for current TRex version notes; v3.08 adds Python 3.12 client support and v3.07 moved to DPDK 25.07.
- [TRex stateless documentation](https://trex-tgn.cisco.com/trex/doc/trex_stateless.html) - Stateless mode, stream profiles, flow stats, latency, and IEEE 1588-assisted latency notes.
- [TRex Mellanox appendix](https://trex-tgn.cisco.com/trex/doc/trex_appendix_mellanox.html) - TRex guidance for Mellanox/NVIDIA NICs and OFED-related checks.
- [TRex analytics report](https://trex-tgn.cisco.com/trex/doc/trex_analytics.html) - Historical public performance trend reports generated from TRex regression data.

## Ubuntu 24.04 and packaging

- [Cisco TRex v3.08 on Ubuntu 24.04](https://qiita.com/fluge/items/ab88e97fd67eafc60de2) - Ubuntu 24.04.3 + TRex v3.08 test. Useful notes on `vfio-pci`, `igb_uio` breakage, `uio_pci_generic` instability, `binutils`, hugepages, and line-rate 10G results.
- [Running T-Rex on Ubuntu 24.04 with a Mellanox NIC](https://scottstuff.net/posts/2025/01/15/running-trex-on-ubuntu-24.04/) - Practical Ubuntu 24.04 + Mellanox walkthrough for TRex v3.06. Useful for rdma-core/no-OFED path, Python 3.11 workaround, `--no-ofed-check`, and bundled `libstdc++` issue.
- [OCI Ubuntu 24.04 images](https://docs.oracle.com/en-us/iaas/images/ubuntu-2404/index.htm) - Oracle Cloud Infrastructure Ubuntu 24.04 image catalog for amd64/aarch64 lab baselines.
- [Ubuntu Python availability](https://documentation.ubuntu.com/ubuntu-for-developers/reference/availability/python/) - Ubuntu reference for Python versions available across Ubuntu releases.
- [Ubuntu package: libibverbs-dev](https://packages.ubuntu.com/libibverbs-dev) - Confirms `libibverbs-dev` availability for Noble/24.04 and multiple architectures.
- [Ubuntu Noble package: ibverbs-utils](https://launchpad.net/ubuntu/noble/+package/ibverbs-utils) - RDMA/verbs utility package useful for `ibv_devinfo` and Mellanox validation.
- [Ubuntu Noble package: infiniband-diags](https://www.ubuntuupdates.org/package/core/noble/universe/base/infiniband-diags) - InfiniBand/RDMA diagnostic tools referenced in Mellanox setup notes.

## DPDK and Mellanox/NVIDIA

- [DPDK 25.11 release notes](https://doc.dpdk.org/guides-25.11/rel_notes/release_25_11.html) - Target LTS DPDK release for the modernization B/C tracks.
- [DPDK 25.07 release notes](https://doc.dpdk.org/guides/rel_notes/release_25_07.html) - Release used by upstream TRex v3.07/v3.08 lineage.
- [DPDK NVIDIA MLX5 common driver](https://doc.dpdk.org/guides/platform/mlx5.html) - Primary DPDK source for mlx5 prerequisites: `libibverbs`, `libmlx5`, `mlx5_core`, `mlx5_ib`, `ib_uverbs`, firmware, and OFED/EN alternatives.
- [NVIDIA ConnectX TRex quick start](https://docs.nvidia.com/networking/display/public/sol/qsg+for+configuring+trex+in+a+few+steps+using+nvidia+connectx+adapters) - Vendor quick start for building TRex with `--no-ofed-check`, using rdma-core, MAC-based config, ConnectX ports, and `testpmd` as peer/DUT.

## Virtio, KVM, and practical setup walkthroughs

- [Cisco TRex Packet Generator - Step by Step](https://blog.hacksbrain.com/cisco-trex-packet-generator-step-by-step) - Hands-on KVM/virtio walkthrough with management NIC plus two TRex dataplane NICs, DUT static routes, generated `/etc/trex_cfg.yaml`, and a first `cap2/http_simple.yaml` test.
- [TRex issue #1183 comment on Ubuntu 24.04 workarounds](https://github.com/cisco-system-traffic-generator/trex-core/issues/1183#issuecomment-3152336995) - Notes on `igb_uio` DMA mask build failure, Python 3.11 workaround for v3.06, and replacing bundled `libstdc++.so.6`.
- [TRex issue #1183](https://github.com/cisco-system-traffic-generator/trex-core/issues/1183) - Original issue showing Ubuntu 24.04 / kernel 6.8 `igb_uio` compile failure and `uio_pci_generic` bind failure symptoms.

## Upstream PRs relevant to modernization

- [PR #1198: Rework pci_set_dma_mask() in kernel module](https://github.com/cisco-system-traffic-generator/trex-core/pull/1198) - Open PR replacing deprecated kernel DMA mask APIs in `igb_uio.c`; directly relevant to newer Linux kernels.
- [PR #1196: Escape backslash in Python scripts](https://github.com/cisco-system-traffic-generator/trex-core/pull/1196) - Open PR addressing Python regex escape warnings on newer Python versions.
- [PR #1195: Python 3.13.5 needs importlib instead of imp](https://github.com/cisco-system-traffic-generator/trex-core/pull/1195) - Open PR replacing removed/deprecated `imp` usage with `importlib`.
- [PR #1194: Remove outdated scapy-2.4.3 from external libs](https://github.com/cisco-system-traffic-generator/trex-core/pull/1194) - Open PR removing old bundled Scapy in favor of system Scapy; relevant to dependency modernization.
- [PR #1197: Compile with C++17 standard](https://github.com/cisco-system-traffic-generator/trex-core/pull/1197) - Open PR that may matter when moving compilers/toolchains forward.
- [PR #1192: Unblock Arm build on DPDK 24.03](https://github.com/cisco-system-traffic-generator/trex-core/pull/1192) - Open PR relevant to the later aarch64/DGX Spark track.
- [PR #1209: mlx5 autoconf check](https://github.com/cisco-system-traffic-generator/trex-core/pull/1209) - Open PR touching mlx5 build detection; relevant to ConnectX modernization review.

## VPP, traffic methodology, and metrics

- [FD.io VPP v20.09 use cases](https://fd.io/docs/vpp/v2009/usecases/) - VPP use case index; includes the VPP + TRex simple performance walkthrough.
- [VPP with Iperf3 and TRex](https://fd.io/docs/vpp/v2009/usecases/simpleperf/) - Old but useful VPP/TRex topology and methodology reference for using TRex against a VPP DUT.
- [CSIT TRex traffic generator methodology](https://csit.fd.io/cdocs/methodology/overview/trex_traffic_generator/) - Current CSIT overview of how TRex is used in FD.io performance testing.
- [CSIT packet latency methodology](https://csit.fd.io/cdocs/methodology/measurements/packet_latency/) - CSIT methodology for latency streams, NDR/PDR context, HDRH, latency bias, and load levels.
- [IPng Networks](https://ipng.ch/) - Source site for practical VPP/TRex articles and lab-style load testing notes.
- [IPng: Loadtesting at Coloclue](https://ipng.ch/s/articles/2021/02/27/loadtesting-at-coloclue/) - Practical TRex interactive and scripted load-test article with `trex-loadtest.py`, warmup/ramp/hold behavior, and JSON output.
- [IPng: Fitlet2 VPP load testing source](https://git.ipng.ch/ipng/ipng.ch/src/commit/85b41ba4e061c2516c2a09a3c07a491f6e71acaf/content/articles/2023-02-12-fitlet2.md) - Source article showing ramp-up tests, unidirectional/bidirectional runs, 1514/IMIX/64B profiles, and JSON-to-HTML graph generation.
- [IPng: Netgate 6100 source article](https://git.ipng.ch/ipng/ipng.ch/src/commit/5042f822ef7b077efa6d8283b17b534bca2fbca8/content/articles/2021-11-26-netgate-6100.md) - Useful comparison methodology across pfSense, Ubuntu, and VPP with 1514B, IMIX, 64B multi-flow, and 64B single-flow cases.
- [trex-loadtest-viz](https://github.com/wejn/trex-loadtest-viz) - Visualizer used by IPng to turn TRex load-test JSON output into interactive graphs.

## Distributed TRex and clock synchronization

- [CodiLime: A traffic generator for measuring network performance](https://codilime.com/blog/a-traffic-generator-for-measuring-network-performance/) - Research article on splitting TRex into TX/RX nodes, a test director, and PTP/NTP considerations for distributed one-way latency.
- [CodiLime TRex fork wiki mirror](https://github-wiki-see.page/m/codilime/trex-core/wiki/Installation) - Mirrored wiki with custom distributed TRex config fields such as `latency_measurement`, `timesync_method`, `timesync_transport`, and `ptp_ip_dest`.
- [codilime/trex-core](https://github.com/codilime/trex-core) - CodiLime TRex fork referenced by the distributed latency work.

## Observability and regression tracking

- [Cisco Community: tracking TRex performance with Elasticsearch, Grafana and Pandas](https://community.cisco.com/t5/networking-blogs/how-do-we-track-trex-performance-using-elasticsearch-grafana-and/ba-p/3661540) - Historical Cisco article on storing performance results, trending over time, and avoiding brittle static thresholds.
- [TRex upstream doc tree](https://github.com/cisco-system-traffic-generator/trex-core/tree/master/doc) - Contains analytics documentation and scripts referenced by the Cisco performance-tracking article.
- [TRex regression ELK client](https://github.com/cisco-system-traffic-generator/trex-core/blob/master/scripts/automation/regression/trex_elk.py) - Upstream Elasticsearch client and schema for regression/performance reporting; useful as a design reference, not something to copy blindly.
- [TRex analytics blog source](https://github.com/cisco-system-traffic-generator/trex-core/blob/master/doc/analyticsBlog.asciidoc) - Source version of the Cisco analytics blog in the upstream repository.
- [TRex analytics web report script](https://github.com/cisco-system-traffic-generator/trex-core/blob/master/doc/AnalyticsWebReport.py) - Historical report generator using Elasticsearch or Google Analytics as a source.
- [TRex ELK connector](https://github.com/cisco-system-traffic-generator/trex-core/blob/master/doc/ELKConnect.py) - Historical Elasticsearch query/parsing helper for the analytics report.


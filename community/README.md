# Community Meeting Notes

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/P7WKiyZSTpCeyfpj3Lm2Fw/badge)](https://hackmd.io/P7WKiyZSTpCeyfpj3Lm2Fw)

[TOC]

# May 18, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** May 18, 2021 3:30 PM UTC
- **Host:** @luxas
- **Participants:**
    - Lucas Käldström, @luxas
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
    - Jerry, @badgateway666
    - Marian, @mrhat
    - Daniel Maslowski, @cyrevolt
    - Aditya Singh, @Ad
- **Agenda:**

1. Welcome and introductions round-table `10min`
	> [name=Lucas]
1. History, context and long-term plans for Racklet as a project `20min`
    > [name=Lucas, Dennis]
1. ~~Shorter-term working areas, relevant RFCs, and next steps `15min`~~
    > [name=Lucas, Dennis, Verneri]
1. ...Add your own agenda topic here...  `XXmin`
:::

## Notes

To be filled in during the meeting

- Round of introductions
  - Nice to have people from many different backgrounds, both hardware and software people :)
  - Company that does Confidential Computing: [immune GmbH](https://immu.ne/), related to [9eSec](https://9esec.io/)

- Q&A about Racklet and general stuff
  - Raspberry Pi vs some more open platform e.g. Rockchip, Olimex A64-OLinuXino, or Libre Computers: RPi for ease of use and accessibility, and Rockchips for security. Other SBCs TBD
  - How to pronounce Kubernetes? k8s a shortening for Kubernetes.
  - ARM vs x86 and other platforms: plans to stay cross-platform
  - Kubernetes progress, [Cluster API](https://github.com/kubernetes-sigs/cluster-api)
  - PoE discussion vs "custom PCBs"
  - Want something similar to https://hackerboards.com/ for our use-cases
  - Use case: Racklet can be generic or specialized, storage per-SBC using [Rook](https://rook.io/)
  - Education use-case is important, and so is accessibility.
  - Bit-banging vs standardized protocols like HTTP: We want to go the more standardized route. Thanks to OSFW for calling us out on the earlier SMBus plans.
  - Backplane discussion: right-angle GPIO headers on the HAT that attach to GPIO on the backplans for the "busbar".
  - Intriguing, but not modular/standardized: https://turingpi.com/
    - We want to re-use existing SBCs to minimize e-waste
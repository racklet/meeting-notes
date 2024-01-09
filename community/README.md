# Racklet Community Meeting Notes 2024

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Tuesday at [4 PM UTC](https://dateful.com/convert/utc?t=4pm) (on **even** weeks). Check the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/xsZjT3XtS2GzG-8ewl8lRg/badge)](https://hackmd.io/xsZjT3XtS2GzG-8ewl8lRg)

[TOC]

# January 23, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** January 23, 2024 4:00 PM UTC 
- **Host:** @twelho
- **Participants:**
:::

# January 9, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** January 9, 2024 4:00 PM UTC 
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
    - Verneri Hirvonen, @chiplet
:::

## Biweekly Recap

### Dennis

- Back in Finland, setting up development workflow again
- Need to do a bit of maintenance on the Racklet repos
- Building a concept of the software (Kubernetes) stack for Racklet
    - Targeting my home cluster (5 x86_64 machines) for now, but should be cross-architecture
    - Working on defining a clear infrastructure/workload separation API
        - [CNI](https://www.cni.dev/)
        - [Gateway API](https://gateway-api.sigs.k8s.io/)
        - [CSI](https://kubernetes-csi.github.io/docs/)
        - [CSI Secrets Store](https://secrets-store-csi-driver.sigs.k8s.io/)
    - Test driving distributed storage based on JuiceFS + MinIO + TiKV
    - Flux OCI source as single source of truth
- Applied some fixes and improvements to my 3D printer

### Verneri

- Did not have a lot of time to work on projects over the festive season. Courses are ramping up for the beginning of 2024 so I don't have a lot of free time for the next six weeks either.
- Got my 3D printer assembled and calibrated in preparation to experiment with custom 19" rack mounting brackets.
- Experimented with using [Shelly Plus Plug S](https://www.shelly.com/en-fi/products/product-overview/shelly-plus-plug-s) for monitoring 19" rack power consumption.

### Daniel

- Started [UEFI exploitation live sessions](https://www.youtube.com/watch?v=nq9jS2R7MxY&list=PLenOHeTI_A9PIW3zelb6c2vhd9V9GhCgF&pp=gAQBiAQB)
  - Drafted [EFI memory scanner tool](https://github.com/platform-system-interface/ems)
- Initial [repo for a parser for Intel ME](https://github.com/fiedka/me_fs_rs)
- Two more JH7110 boards: Mars and Mars CM + DFRobot carrier board
  - oreboot works thus far, DRAM init is fine :)

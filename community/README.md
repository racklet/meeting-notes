# Racklet Community Meeting Notes 2024

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Tuesday at [4 PM UTC](https://dateful.com/convert/utc?t=4pm) (on **even** weeks). Check the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/xsZjT3XtS2GzG-8ewl8lRg/badge)](https://hackmd.io/xsZjT3XtS2GzG-8ewl8lRg)

[TOC]

# February 6, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** February 6, 2024 4:00 PM UTC 
- **Host:** @orangecms
- **Participants:**
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Daniel

- Minor success with Zephyr on the JH7110, getting spurious interrupts
    - Source apparently the video out subsystem, though it should be off
- Continued reversing the JH7110 mask ROM and the header
    - Erratum found in the manual: DTIM really starts at `0x0110_0000`
    - Figured some fields so far where key material is put in the header
- We did few steps on the TH1520 at FOSDEM, mostly just discussed oreboot design/architecture

# January 23, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** January 23, 2024 4:00 PM UTC 
- **Host:** @orangecms
- **Participants:**
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Daniel

- JH7110 is now based on PAC and HAL crates in oreboot
    - Upgraded `embedded-hal` to the recently released version 1.0
    - Brought up [Little Kernel](https://github.com/littlekernel/lk) (almost working!) on hart 1 (non-SMP)
        - https://mastodon.social/@CyReVolt/111802036890714525
        - https://mastodon.social/@CyReVolt/111784797440445427
    - Will see about running Zephyr on hart 0
        - WIP: https://github.com/pfarwsi/zephyr-visionfive2/tree/visionfive2
        - Many SoCs are now AMP (asymmetric multi-processing) systems
        - Want to gear oreboot toward that use case
- Will present at RISC-V Munich meetup on Thursday
- We'll have a hackathon from Tuesday before FOSDEM on
    - Featuring [TH1520 boards](https://sipeed.com/licheepi4a) (Lichee Pi 4A, Lichee Console 4A)
    - Philipp Hug has the first setup parts drafted
- Will probably attend https://2024.rustnl.org/

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

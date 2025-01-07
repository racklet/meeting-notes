# Racklet Community Meeting Notes 2025

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Tuesday at [6 PM CET/CEST](https://dateful.com/convert/berlin-germany?t=6pm) (on even weeks). Check the [#racklet](https://osfw.slack.com/messages/racklet/) channel in the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/PRLxctjyT8OUb0zvsMpgLw/badge)](https://hackmd.io/PRLxctjyT8OUb0zvsMpgLw)

[TOC]

# January 21st, 2025 6:00 PM CET/CEST

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** January 21st, 2025 6:00 PM CET/CEST
- **Host:** @twelho
- **Participants:**
:::

# January 7th, 2025 6:00 PM CET/CEST

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** January 7th, 2025 6:00 PM CET/CEST
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Daniel

- More thorough test of the TH1520 showed its DRAM wasn't really working
- Got back on the Bouffalo Lab BL808
    - Started a [loader tool](https://github.com/orangecms/bl_boot) as usual
        - Slowly figuring out how the boot process and the many efuse bits work
    - Lots of [open resources from the community](https://github.com/openbouffalo/) and [the vendor](https://github.com/bouffalolab/)
- 38C3 was a blast :tada: - blog post on OSFF website coming soon:tm:
    - We bootstrapped the Canaan K230 SoC during a workshop
    - Reversed its mask ROM in a previous one (got started)

### Dennis

- More work on the x86 Racklet software architecture test cluster
    - Ceph (distributed storage) is now running very well
    - Encountered some bugs in Cilium (networking)
    - Talos Linux has an [annoying bug](https://github.com/siderolabs/talos/pull/10060) that prevents upgrades right now
- Taught myself KiCad over the holidays, received first PCB from JLC
- Haven't yet gotten into messing with the RISC-V ESP32C6
- Explored Linux real-time scheduling on a Raspberry Pi 3
    - `threadirqs` is very useful for multicore embedded systems

# Racklet Community Meeting Notes 2025

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Tuesday at [6 PM CET/CEST](https://dateful.com/convert/berlin-germany?t=6pm) (on even weeks). Check the [#racklet](https://osfw.slack.com/messages/racklet/) channel in the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/PRLxctjyT8OUb0zvsMpgLw/badge)](https://hackmd.io/PRLxctjyT8OUb0zvsMpgLw)

[TOC]

# February 18th, 2025 6:00 PM CET/CEST

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** February 18th, 2025 6:00 PM CET/CEST
- **Host:** @twelho
- **Participants:**
:::

# February 4th, 2025 6:00 PM CET/CEST

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** February 4th, 2025 6:00 PM CET/CEST
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- Debugging Ceph performance on the x86 test cluster
    - Very fast read IOPS and decently fast write IOPS
    - The MDS (file creations/deletions etc.) is for some reason limited to only around 15 files/second, which is very slow
    - The MDS journal is probably the culprit, have yet to dig into the profiling details
- Kernel 6.14 has support for C906 vector extension (0.7) [errata](https://www.phoronix.com/news/Linux-6.14-RISC-V)

### Daniel

- Was at FOSDEM, including 2 OSFW hack days
    - Ported [DRAM init for K230D to oreboot](https://github.com/oreboot/oreboot/pull/764)
    - Extended [`kendryte_boot`](https://github.com/orangecms/kendryte_boot) so you can load additional code to RAM
    - Added `main` stage in oreboot and dummy SBI
    - Simple SBI print test works
    - Currently debugging mtimer, `mtime` is stuck at `0`

# January 21st, 2025 6:00 PM CET/CEST

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** January 21st, 2025 6:00 PM CET/CEST
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- Daniel hinted at the newly established [mini rack project](https://mini-rack.jeffgeerling.com/) from Jeff Geerling
    - Uses the same 10" rack form factor as Racklet
    - 3D-printed plates to mount Pi's etc.
    - Other useful shelves/mounts listed there as well
    - Power delivery is a challenge – as we've already discovered with Racklet
        - PDU discussion [here](https://github.com/geerlingguy/mini-rack/issues/5)
        - UPS discussion [here](https://github.com/geerlingguy/mini-rack/issues/1)
        - These are mostly 110/230 V though, nothing yet for USB-C PD etc.
    - Network switch lists are useful for Racklet
        - Looking forward to the community discovering powerful-enough PoE switches for multiple nodes
    - Software/firmware side severely lacking though:
        - No firmware security/boot integrity/Open Compute -level secure boot
        - Very basic (mutable) Kubernetes setups only
    - TL;DR: Useful for sourcing parts for the Racklet hardware side, though we want more density and better firmware security + actual Open Compute -level software (immutable Kubernetes)
- Achieved over 14k 4K direct random read QD2 IOPS with Ceph on the x86 test cluster! :chart_with_upwards_trend:
    - In layman's terms: distributed filesystems can be made very snappy at a small scale as well
    - This proves the viability of Ceph on lower-end hardware, SBCs such as the Rock 5B should not have any issues running this stack
    - 8 GiB of RAM, 4 threads and Gigabit ethernet is sufficient

### Daniel

- Made lots of progress on the BL808
    - Got the loader tool to the point where it can load any binary for any core to run immediately
    - Got [PSRAM working](https://mastodon.social/@CyReVolt/113852399724541310)
    - Did another [dev live stream on it](https://www.youtube.com/watch?v=wCvZkpvgpm0)

## Additional notes

We took a closer look at the RISC-V SBI, including the Harte State Management extension and specifically RustSBI, some projects using RustSBI, and how global singletons are initialized in them.

Refs:
- https://github.com/riscv-non-isa/riscv-sbi-doc/releases/tag/v3.0-rc3
- https://github.com/rustsbi/rustsbi/network/dependents
- https://github.com/arceos-hypervisor/riscv_vcpu
- https://github.com/rustsbi/rustsbi-d1/blob/main/see/src/main.rs
- https://docs.rs/rcore-console/latest/src/rcore_console/lib.rs.html
- https://github.com/rustsbi/rustsbi/blob/main/sbi-rt/src/dbcn.rs
- https://github.com/rcore-os/rCore-Tutorial-v3/blob/main/os/src/console.rs
- https://github.com/mvdnes/spin-rs/blob/master/src/once.rs

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

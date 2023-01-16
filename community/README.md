# Racklet Community Meeting Notes 2023

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Monday at [4 PM UTC](https://dateful.com/convert/utc?t=4pm) (on odd weeks). Check the the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/1esalu2VQcSqy_dShd0o7A/badge)](https://hackmd.io/1esalu2VQcSqy_dShd0o7A)

[TOC]

# January 30, 2023 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** January 30, 2023 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
:::

# January 16, 2023 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** January 16, 2023 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
:::

## Biweekly Recap

### Dennis

- Did some community discovery:
    - Browsing through the history of the [#arm64](kubernetes.slack.com/messages/arm64) channel on the Kubernetes Slack – lots of relevant content for running the Kubernetes ecosystem on Racklet
    - https://www.reddit.com/r/picluster/
- Watched the first recording of Daniel's series on the VisionFive
    - It's a really great resource for beginners and more experienced people as well
    - Highly recommend following along either live or checking out the [playlist](https://www.youtube.com/playlist?list=PLenOHeTI_A9PSGshDnEc4dYK-GSnCshk6)

## Racklet Rack Design Update

- Dennis: thought a bit more about the physical rack after last week's mention of the Antmicro cluster and came up with a solid design:
    - **2U chassis**: 7 compute modules with 2.5" SSDs/HDDs behind SBCs of the Raspberry Pi form factor
    - **3U chassis**: 7 compute modules with 3.5 HDDs behind SBCs with a port-side width of at most 12 cm (Nano-ITX, although the maximum allowable depth must be double-checked)
    - Both chassis dedicate the rightmost compute unit for chassis management and RMC tasks
    - 14 compute nodes in total, which fits very well with a 16-port Ethernet switch (basically the largest size available for the 10" form factor)
        - Final two ports can be used for chaining/debugging and for providing a WAN connection
        - Single-chassis setups can also be served by a commodity 8-port switch
    - 6 nodes per tray dedicated to the workload
        - Ideal for e.g. Kubernetes HA: 3+3 master/worker split
    - Fully loaded reference configuration of (from the top) 3U chassis, network switch and 2U chassis fits into the nominal 6U rack size
        - 2U chassis and network switch shallow enough to fit the power supplies (probably a reduced size, e.g. SFX) behind them
    - Minimal configuration with one 2U chassis and a 8-port network switch can fit into just 3 rack units

# January 2, 2023 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** January 2, 2023 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
:::

## Biweekly Recap

### Daniel

- Got [SMP to work on the JH7100](https://github.com/oreboot/oreboot/pull/606), can now run a full M-mode OS or head on to OpenSBI + U-Boot blob from vendor
    - Will do a JH7100 / VisionFive recap stream on Wednesday and close [this current chapter](https://www.youtube.com/playlist?list=PLenOHeTI_A9PSGshDnEc4dYK-GSnCshk6)
- Got first output from [Rust on the BL808](https://github.com/oreboot/oreboot/pull/659) (Sipeed M1S Dock board)
    - Currently only 32-bit (aka MCU / M0) core
    - 64-bit (aka MM / D0) core is WIP
- Gave two talks at JEV / FireShonks
    - [Platform System Interface - Computing as a Whole](https://media.ccc.de/v/fire-shonks-2022-49154-platform-system-interface-design-und-evaluation-holistischer-computerarchitektur)
        - Slides <https://metaspora.org/platform-system-interface-computing-as-whole.pdf>
        - Featuring Racklet :-)
    - [oreboot on RISC-V - Comparing D1 vs JH7100](https://media.ccc.de/v/fire-shonks-2022-49164-oreboot-auf-risc-v-vergleichende-analyse-zweier-implementierungen)
        - Slides <https://metaspora.org/oreboot-comparison-riscv-d1-jh7100.pdf>
    - Talks in German, slides in English, English translations available

### Marvin

- Worked on u-bmc again
    - Build system basically ready -> Builds even faster and reproducible (using new Dagger Engine v0.4)
    - Worked out layout and bootloader/kernel done for v1
    - Last work on userspace now being done
        - TODO: Adding auth to REST and RPC interfaces missing (similar to kubectl)
        - TODO: Adding TUI for Builder and Client tooling
        - WIP: WebUI additions for cluster management (need to push those)
        - TODO: Hardware definitions for some boards still missing
        - Most other userspace bits are there now for v1.0.0
    - Moved some other WIP features to v1.1.0 or later so I can get things out in a timely manner
- Generally working more on ARM systems now -> gained experience can flow into Racklet more
- Stumbled upon [this](https://antmicro.com/blog/2022/08/scalerunner-open-source-compute-cluster/) blog entry recently from Antmicro about their Raspberry Cluster

### Dennis

- Got together with Verneri for some discussions around the electronics and physical form factor
    - With the increasing amount of non-RPi form factor SBC boards, it might be best to mount the HDDs/SSDs behind those boards in the slide-out modules
    - The above would require some kind of SATA adapter plane between the disk and the SBC
- Power and signal delivery: USB-C?
    - Researched some newer (low-end) STM32 chips such as the STM32G0 series, USB-C power delivery controllers come as standard now
    - USB-C could be used to select the right voltage and deliver power to both the BMC and the SBC board
        - Issue: 3.5" HDDs might separately require a 12 volt supply
- Should we integrate ethernet with the back/top plane and avoid wires on the front?
    - The compute module connector (e.g. USB-C) could be used to carry this as well
- Off topic: tried to (unsuccessfully) nuke the ME from a laptop and [reported my findings](https://github.com/corna/me_cleaner/issues/3#issuecomment-1364690512)


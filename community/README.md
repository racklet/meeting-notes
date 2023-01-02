# Racklet Community Meeting Notes 2023

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Monday at [4 PM UTC](https://dateful.com/convert/utc?t=4pm) (on odd weeks). Check the the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/1esalu2VQcSqy_dShd0o7A/badge)](https://hackmd.io/1esalu2VQcSqy_dShd0o7A)

[TOC]

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

## Agenda/Notes

### Biweekly Recap

#### Daniel

- Got [SMP to work on the JH7100](https://github.com/oreboot/oreboot/pull/606), can now run a full M-mode OS or head on to OpenSBI + U-Boot blob from vendor
    - Will do a JH7100 / VisionFive recap stream on Wednesday and close [this current chapter](https://www.youtube.com/playlist?list=PLenOHeTI_A9PSGshDnEc4dYK-GSnCshk6)
- Got first output from [Rust on the BL808](https://github.com/oreboot/oreboot/pull/659)
    - Currently only 32-bit (aka MCU / M0) core
    - 64-bit (aka MM / D0) core is WIP


#### Marvin

- Worked on u-bmc again
    - Build system basically ready -> Builds even faster and reproducible
    - 

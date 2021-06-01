# Racklet Community Meeting Notes

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/P7WKiyZSTpCeyfpj3Lm2Fw/badge)](https://hackmd.io/P7WKiyZSTpCeyfpj3Lm2Fw)

[TOC]

# June 8, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** June 8, 2021 3:30 PM UTC
- **Host:** 
- **Participants:**
- **Agenda:**

1. LFX Update
:::

# June 1, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** June 1, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Lucas Käldström, @luxas
    - Daniel Maslowski, @cyrevolt
- **Agenda:**

1. Recap of the week
1. LinuxBoot, u-root and kexec talk
1. Details of the BMC
1. RP2040 for the BMC
1. New developments around the Pi boot flow
:::

## Notes

- Recap of the week
    - Lucas has bought an apartment, half unexpectedly (really good opportunity)
    - CyReVolt has bought an A64-OLinuXino SBC; want to use that as the main development target for now
    - DHL has soon customs cleared our PINE64 stuff
    - Lucas will still check in with Dims on the LFX mentorship stuff, didn't quite yet have time
    - Lucas and Dennis will still this week have an exam
- Next week
    - Start to formalize RFCs and documentation
    - @CyReVolt will put LinuxBoot on the A64, can help with the software side
- LinuxBoot
    - The main focus has been on x86 so far; good benchmark to see if it can be ported to ARM
    - How to kexec reliably, document gotchas
    - @CyReVolt will check how this ties into MicroOS and Kubic
    - LinuxBoot is a concept, not an implementation.
        - "Just use a Linux kernel in your boot flow"
    - Hyperscalers wanted to replace crappy UEFI device drivers for the ones in Linux.
    - Built u-root for a MIPS target (OpenWRT), used that as a chroot, and this opened a new world
    - Want the "bootstrap kernel" as minimal as possible (i.e. only network and cryptography); the "target kernel" can be pretty bloated (e.g. KVM + Kubernetes stuff) and customized.
    - Different Kconfig for (main)boards, for SoCs, and for use-case. Compare to x86 where one kernel can run on most boards "out of the box".
    - Can use "known good kernel" when booting if the new one panics
    - To integrate TUF into webboot; there would be a "middleware" that verified the artifacts before booting them. Plan is to initially fork webboot to add this middleware verification step, then add TUF as one such middleware, and then contribute upstream to webboot or u-root.
    - [u-root](https://u-root.org/) provides an API for loading and kexecing a kernel, load initramfs, etc.
    - https://metaspora.org/u-root-bootloaders-LinuxDay2020.pdf
    - https://github.com/theupdateframework/go-tuf
    - u-root has three directories: boot, core, and exp. Maybe could add TUF to exp.
    - webboot moved to its own repo, was some kind of GSoC project. When building webboot you are pulling in u-root.
    - u-root the project, u-root the command. Can add a command into your initramfs using u-root. 
    - `u-root core boot github.com/theupdateframework/go-tuf/cmd/tuf`
    - Does u-root have UART/SPI/etc. support? On ARM, we read the Device Tree (in u-root), a linux driver would use that interface, make a linux device under `/dev/xxx`, which u-root the Go application can use.
    - We can probably use https://github.com/google/periph for the SPI communication.
    - https://github.com/golang/go/issues/32204
    - https://www.fastly.com/blog/quic-is-now-rfc-9000
    - https://github.com/golang/go/issues/44886
    - u-root doesn't force you to write things in Go; you can also include binaries, but that can easily get messy.
    - https://github.com/orangecms/arm-cpu/blob/main/hisilicon/HI3536DV100/build-arm-cpio.sh
    - For MCU debugging: https://probe.rs/
    - https://github.com/u-root/u-bmc: @bluecmd, working on TCG storage stuff
        - u-bmc follows a similar ethos to Racklet (IPMI is replaced with gRPC, and SNMP with OpenMetrics)
- The USB-C port can be used for booting as well, nice! Can be used instead of SD Card emulation.
- RP2040 now released! (As an invididual chip, not available quite yet.)
    - https://www.raspberrypi.org/blog/raspberry-pi-rp2040-on-sale/
    - Not super good Rust support yet.
        - ask for help; maybe [pfalcon](https://github.com/pfalcon)?
    - Google using Zephyr as an RTOS; but no Rust support yet for that either.

# May 25, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** May 25, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
    - Jerry, @badgateway666
    - Daniel Maslowski, @cyrevolt
- **Agenda:**

1. Recap of the week
1. Software stack setup – things to be aware of
    > [name=CyReVolt]
1. Raspberry Pi booting/SD card emulation (technical)
    > [name=Dennis]
1. Electronics Update
    > [name=Verneri]
1. PoE update (PoE+ hat from Pi Foundation)
    > [name=Dennis]
1. Racklet proposal submitted to KubeCon NA 2021
    > [name=Dennis]
:::

## Notes

- Recap of the week
    - Not that many participants this time around (people are busy/sick)
    - Badgateway's homelab
- Software stack setup
    - What are the current ideas, what can be contributed now?
    - [webboot](https://github.com/u-root/webboot), u-root integration
        - Tiny bootloader on top of kexec
        - Helps in avoiding TFTP -> uses https
        - kexec fails in some specific conditions?
        - In LinuxBoot -> kernel includes fully-featured initramfs
        - Potential to run with separate FS as well
        - In u-boot: parsers for different bootloaders, grub etc.
            - capable of booting distro live-ISOs as well
  - [MicroOS](https://microos.opensuse.org/)?
    - Immutable FS -> separate persistent storage
    - Container-image like build process
    - webboot integration still WIP (and for x86 only for now)
- [Kubic](https://kubic.opensuse.org/) is a k8s distro based on MicroOS
    - Using Bottlerocket:
        - We don't necessarily need the GRUB A/B partitioning (we're aiming for diskless)
        - systemd is kind of heavy
    - Usual boot flow (of an x86 system)
        - Written-once boot rom
        - Bootloader from local NAND/SPI flash
            - May be very heavy, hundreds of drivers
        - Operating system
    - On a Raspberry Pi:
        - EEPROM
        - start.elf
        - Bootloader
        - OS
    - In Racklet:
        - BMC provides inventory repo URL and keys
        - u-root binary checks inventory repository for what to specialize to
        - u-root binary downloads OS from target repository and kexec
    - Possibility to self-host the Racklet OS build infrastructure (on Racklet itself)
        - GitLab, Gitea + [Drone](https://www.drone.io/)
        - Nodes don't have persistent storage (just for applications)
- Electronics update Q&A
    - PSU dev board mostly ready
    - Can the BMC reset the Pi? Yes, it has power control support.
- Raspberry Pi booting & SD emulation
      - Pi (and other SBCs) only accept SD protocol mode, no SPI mode support
      - Emulating SD protocol is hard and potentially legally questionable
      - USB boot could work, but SSD boot sector attack is hard to control
      - Pi has a (beta) feature to boot from the USB-C power connector -> private channel for the BMC to serve boot files
- Usual case in x86
    - Server <-> BMC (bidirectional relationship)
- Indicators of the OS (e.g. for detecting compromise)
- Out of band access
    - All BMCs are connected to the backplane (an USB HUB)
    - The backplane upstream can be connected to one of the compute units in the rack (enables e.g. OoB access) or another system
    - BMCs communicate OpenMetrics/REST over USB, support for authenticated reboots etc.
- PoE+ update
    - [PoE+ HAT](https://www.raspberrypi.org/blog/announcing-the-raspberry-pi-poe-hat/)
    - High-voltage PoE support
    - Works on only Pis essentially though
    - Requires active cooling
    - Only one MicroTik switch (hEX PoE) in the price and availability range
        - RouterOS sources gone missing...
- Networking security and proprietaryness...
    - [SONiC](https://azure.github.io/SONiC/) in OCP
- Firmware trust
    - [TF-A (TrustedFirmware-A)](https://trustedfirmware-a.readthedocs.io/)

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

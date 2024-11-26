# Racklet Community Meeting Notes 2024

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Tuesday at [6 PM CET/CEST](https://dateful.com/convert/berlin-germany?t=6pm) (on **even** weeks). Check the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/xsZjT3XtS2GzG-8ewl8lRg/badge)](https://hackmd.io/xsZjT3XtS2GzG-8ewl8lRg)

> **NOTE:** The meeting time has been changed from UTC to follow CET/CEST, use the link above to check the time in your local time zone!

[TOC]

# December 10th, 2024 6:00 PM CET/CEST

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** November 26th, 2024 6:00 PM CET/CEST
- **Host:** @twelho
- **Participants:**
:::

# November 26th, 2024 6:00 PM CET/CEST

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** November 26th, 2024 6:00 PM CET/CEST (**note the updated time!**)
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Daniel

- Rounded up the current work for me_fs_rs with a talk
    - https://metaspora.org/look-at-me-in-rust-labortage2024.pdf
    - Reversed the [first parts of Gen 2 MFS](https://github.com/fiedka/me_fs_rs/tree/mfs)
- Went to Hamburg for Arch Summit
    - We are rewriting the [Arch Linux Package Management in Rust](https://gitlab.archlinux.org/archlinux/alpm/alpm) :crab: 
    - Interesting discussion around a [signing service](https://gitlab.archlinux.org/archlinux/signstar/)

### Dennis

- Submitted a talk for the KubeCon EU 2025 CFP!
- Looking into the DNG (Digital Negative) and JPEG-XL ecosystem
    - Including Rust encoding/decoding support: [jxl-rs](https://github.com/libjxl/jxl-rs)
- Through the signing service Daniel brought up discovered the [NetHSM](https://www.nitrokey.com/products/nethsm)
    - Basically a Xeon-based (with disabled ME) networked programmable "Yubikey server"
    - The [docs](https://docs.nitrokey.com/nethsm/) don't mention Coreboot support...
    - Might be useful as a concept for Racklet as well, downscaled of course

# November 12th, 2024 6:00 PM CET/CEST

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** November 12th, 2024 6:00 PM CET/CEST (**note the updated time!**)
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- Looking into Gokrazy

### Daniel

- Back on parsing Intel ME firmware
    - https://github.com/fiedka/me_fs_rs/tree/mfs
    - Added a FIT parser
    - Can now parse 2nd gen ME firmware directories
    - Mostly based on MEAnalyzer and Igor Skochinsky's work

# October 29th, 2024 6:00 PM CET/CEST

:::info
No meeting
:::

# October 15th, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** October 15th, 2024 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- Quite busy with my thesis work
- Looked into [TamaGo](https://github.com/usbarmory/tamago) again
    - This (and the USB armory) are apparently under WithSecure
- Looked into how they do DRAM init in bare metal Go for the USB armory with Daniel
    - Apparently, i.MX6(ULL) SoCs have their DRAM init in hardware, one only needs to provide DRAM parameters (and some other options) via a [config](https://github.com/usbarmory/tamago/blob/6b22a26933dc5218f773bdb5c7a5ba833d944e74/board/usbarmory/mk2/imximage.cfg) through u-boot's [mkimage](https://github.com/usbarmory/armory-boot/blob/656160cd9b230e971b42b7cc7c5757e6111d0c3f/Makefile#L103-L106) and the mask ROM handles the rest

### Daniel

- Started [extending Romulan](https://github.com/orangecms/romulan/blob/CLI-to-teh-limitz/) yet again
    - Including a diff feature, starting with AMD
    - Looking at many projects to gather information on processors, including coreboot
- DRAM init still [almost done](https://github.com/oreboot/oreboot/pull/756) for TH1520, needs debugging now

# October 1st, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** October 1st, 2024 4:00 PM UTC (**note the updated time!**)
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- Not much direct progress on Racklet
- Investigated driver support for a microcontroller on an X570 motherboard

### Daniel

- Got very far with [Intel ME file system parsers](https://github.com/fiedka/me_fs_rs)
    - MFS extraction mostly works for older ME versions (~11)
    - [PoC](https://mastodon.social/@CyReVolt/113194461404852156) integration of FPT and CPDs in Fiedka :octopus: 
- DRAM init [almost done](https://github.com/oreboot/oreboot/pull/756) for TH1520 (mostly by Kanak Shilledar)
    - DRAM PHY appears to be a variant of [ARC](https://en.wikipedia.org/wiki/ARC_(processor)) for which there is an [open toolchain](https://github.com/foss-for-synopsys-dwc-arc-processors/toolchain/)

# September 17th, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** September 17th, 2024 4:00 PM UTC (**note the updated time!**)
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- On-and-off work on the x86 test cluster software, not much time for other Racklet work
- Looking a bit more into pub/sub platforms, particularly for metrics, but nothing to report yet

### Daniel

- Got back to fiedka.app, upgraded its dependencies
    - Draft for ME partition and file system parsing

## Live session

We got code to run on a Rockchip RK3566 (OKdo ROCK 3) in mask ROM mode.
https://forum.radxa.com/t/solved-is-there-a-way-to-load-a-program-directly-into-sram-using-usb-otg/5475/9

Using [xrock](https://github.com/xboot/xrock) and [rkbin](https://github.com/rockchip-linux/rkbin):
```sh
_DIR=../rkbin/bin/rk35
_DRAM=$_DIR/rk3566_ddr_528MHz_ultra_v1.10.bin
_USB=$_DIR/rk356x_usbplug_v1.17.bin

xrock maskrom $_DRAM $_USB --rc4-off
sleep 2
xrock version
# RK3568(3568): 0x33353638 0xffffffff 0xffffffff 0xffffffff
```

# September 3rd, 2024 4:00 PM UTC

:::info
No meeting
:::

# August 20, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** August 20, 2024 4:00 PM UTC (**note the updated time!**)
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- New SATA cables for x86 software development cluster installed
- Attempting to migrate Rook Ceph into [PVC cluster mode](https://rook.io/docs/rook/latest/CRDs/Cluster/pvc-cluster/) using the [Local Persistence Volume Static Provisioner](https://github.com/kubernetes-sigs/sig-storage-local-static-provisioner)
    - Should allow for much more flexibility, especially with dedicated DB+WAL devices
- Experimenting with [Gateway API](https://kubernetes.io/docs/concepts/services-networking/gateway/), will be important for Racklet too
- More work on https://github.com/twelho/talos-bootstrap for improving Talos ease-of-use

### Daniel

- Finished reversing and translating the K1x DRAM training code
    - Tested on BPI-F3 4GB - works
    - Tested on MuseBook 16GB - works :)
- Got back to [dtvis](https://github.com/platform-system-interface/dtvis/)
    - Got permission to use the DeviceTree logo from Linaro
- Forked a [Rust library for Fastboot](https://github.com/platform-system-interface/fastboot)

## Discussion Topics

- Raspberry Pi Pico 2 with RP2350 released
    - 2x Cortex-M33 + 2x Hazard3 (RISC-V)
    - Beefier and (officially) faster PIO
        - Still no clock input: no advantage in synchronizing with SD host signaling compared to RP2040, however, upon reading the datasheet I discovered that one can disable the input synchronizers to gain 2 cycles of latency (also available on RP2040) -> needs further testing
    - `probe-rs` needs some work
    - [DEF CON 2024 badge](https://www.hackster.io/news/def-con-32-s-raspberry-pi-rp2350-powered-badge-sits-at-the-center-of-a-major-disagreement-05e96385a3dc) is based on RP2350
- [WHY2025 badge](https://badge.team/docs/badges/why2025/) will have an ESP32-P4 (main SoC) + ESP32-C3 (for radios) + WCH CH32Vxxx (keyboard)
- [Akeana](https://www.techpowerup.com/325522/akeana-exits-stealth-mode-with-comprehensive-risc-v-processor-portfolio) as a new RISC-V vendor
- C910 vector extension broken -> [GhostWrite](https://ghostwriteattack.com/)

# August 6, 2024 3:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
    - Temporarily switched to https://meet.ffmuc.net/racklet-community because the CCC Hamburg one misbehaved
- **Date:** August 6, 2024 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- Acquired (one of) the world's last Rock Pi 5B's with more than 8 GiB of RAM (why are these so rare? The blue PCB version seems to be available a bit more frequently...)
    - Will try some u-root/DT experiments on the platform
- x86 software develompent cluster developed SATA communication issues: now waiting for new cables to arrive...
- Framework RISC-V mainboard (JH7110)
- Snapdragon X (Elite) UEFI and ACPI + DT?
- The Signaloid C0-microSD module finally saw its first code drop after 4 months: https://github.com/signaloid/C0-microSD-Hardware
    - Seems to be binary-only atm., no sources yet (but hopefully soon)
    - The docs page is very promising, they've managed to implement block reads and writes from the SD protocol (the one we need, not SPI or SDIO): https://c0-microsd-docs.signaloid.io/hardware-overview/communication-over-sd-interface.html

### Daniel

- Been reversing the SpacemiT K1x DRAM training binary blob (RISC-V and open source... not)
    - https://github.com/orangecms/oreboot/blob/all-the-things-wip/src/mainboard/spacemit/k1x/bt0/src/dram.rs
    - https://gist.github.com/orangecms/8624aa82bab4931f787b5eee572f7668
    - Some things are a bit confusing due to register reuse and stack allocation
    - So far the code runs without crashing
    - Vendor code seems sorta buggy, putting data in stack memory that appears unused
- Got `kexec` to work on the JH7110 including purgatory
    - Got a patch for alignment in purgatory upstream
    - Got kexec file to work (other syscall) based on extra patches (pending in LKML)
    - QSPI was actually misbehaving due to MTD drivers missing, but adding them made it work fine

# June 11, 2024 3:00 PM UTC

:::info
No meeting until August
:::

# May 28, 2024 3:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** May 28, 2024 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- Busy with thesis work, not much time to work on Racklet currently

### Daniel

- Full-stack demo of oreboot + sbitest on Milk-V Mars (SG2002)
- Got Linux 6.9.2 up and running with oreboot on the JH7110 + hacks
    - Need native platform timer patches (pending for upstream)
    - Defer probing ethernet, temperature sensor and QSPI to the end
    - `cpu` works, but not `kexec`

# May 14, 2024 3:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** May 14, 2024 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
:::

## Biweekly Recap

### Dennis

- Major work on my x86 cluster serving as the Racklet software testbed
    - Upgraded from USB3 HDD docks to real M.2 PCIe SATA controllers
        - 4Kn mode for HDDs
        - Performance impact surprisingly small, the USB HDD docks do a pretty good job -> advantageous for Racklet
    - JuiceFS metadata performance is rather slow, tested various databases:
        - KeyDB -> very fast initially, large amount of operations slow it to a crawl. Lots of segfaults, a bunch of tweaking needed to get running somewhat reliably.
        - FoundationDB -> very consistent and "lag-free" performance, but rather long latencies causing low performance
        - TiKV -> middle ground, not nearly as fast as KeyDB but also maintains consistent performance like FoundationDB, wins longer benchmarks like creating 100000 files etc. Rather CPU-heavy though
    - Storage system is consuming basically all of the CPU capacity -> FUSE overhead is enormous, and starving the backing MinIO and TiKV instances of CPU time leading to pretty abysmal performance, even with SSD caching enabled
    - Testing Ceph BlueStore next...

### Daniel (couldn't really join, sorry)

- Sophgo/CVITek SoCs
    - Got a few more SG2002 boards (Sipeed LicheeRV Nano)
    - Added boot mode buttons to the IO boards for Duo + Duo 256M
        - https://mastodon.social/@CyReVolt/112430910613557486
    - Reversed a good bunch of the CV1800B and SG200x mask ROMs
        - Mostly SG200x, including the Arm one
    - Really neat target, allowing for loading binaries over USB (serial gadget) anytime
        - That is a mask ROM function to just call into

# April 30, 2024 3:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** April 30, 2024 3:00 PM UTC (**note the updated time!**)
- **Host:** @orangecms
- **Participants:**
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Daniel

- Got a Milk-V Duo S (SG2000) and a Duo 256M (SG2002)
    - Figured out how to run code on each
    - Wrote [a simple Rust tool to do so](https://github.com/orangecms/sg_boot)
        - The vendor tool is tightly coupled with their build tool
        - [Helps others](https://github.com/milkv-duo/duo-buildroot-sdk/issues/8) :)
    - Got basic hello on all 3, including the earlier Duo (CV1800B)
    - Ported most of DRAM init for the Duo S (DDR3, 512M @ 1866)
        - Working fine, some parts of BIST (built-in self-test) remain
        - Reference code is very long (> 10k LoC), but well documented

# April 16, 2024 3:00 PM UTC

:::info
No meeting
:::

# April 2, 2024 3:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** April 2, 2024 3:00 PM UTC (**note the updated time!**)
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
    - Verneri Hirvonen, @chiplet
    - Ron Minnich, @rminnich
:::

## Biweekly Recap

### Dennis

- Visited KubeCon + CloudNativeCon EU 2024
    - Multiple Racklet-style x86/ARM portable mini-clusters were on show
        - Mostly older or more specialized implementations
        - Intel Atom/Celeron & RK3588
        - No major RISC-V adoption in the cloud native space yet
    - Talos Linux (also Racklet's base OS) is picking up steam
- For Racklet's backplane/HAT MCU platforms, researched [aWsm](https://github.com/gwsystems/aWsm) and ARM's [bento boxes](https://github.com/arm-software/bento-linker) as an interesting way to bundle/deploy memory-isolated MCU applications
- More work on getting JuiceFS running on my x86 development cluster
    - The MinIO operator is [quite](https://github.com/minio/operator/issues/1100) [unusable](https://github.com/minio/operator/pull/2056)
- Finally acquired some ChipQuik flux!

### Verneri

- Reinstalled workstation OS. Setting up HW development tooling on the new system, but now as a container (run with [distrobox](https://github.com/89luca89/distrobox)) to document the setup: https://github.com/chiplet/vernerin-kontit/blob/main/racklet-hw-env/Containerfile
    - FPGA blinky works already!

### Daniel

- Working on oreboot design things
    - Zephyr + Linux codesign (or any combo of OSes?)
    - We can run Zephyr on the JH7110 S7 core with oreboot
        - https://github.com/zephyrproject-rtos/zephyr/pull/70514
    - Experimenting with DTFS (Device Tree File System) ideas

# March 19, 2024 4:00 PM UTC

:::info
No meeting
:::

# March 5, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** March 5, 2024 4:00 PM UTC 
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
:::

## Biweekly Recap

### Dennis

- Back again after some traveling
- Master's thesis work started, will see how much extra time I still have
- Working further on developing the Racklet storage solution prototype on my x86 testing cluster
    - One of my 2 TB development disks broke: bought 6 * 16 TB disks at a very good price (but still expensive in total...)
    - At least now I have extra 2 TB disks for testing fitment into the Racklet mini-rack
- Managed to completely brick an old WD MyBook Live Duo NAS
    - Needed to re-flash U-Boot, but turns out erasing the NOR flash where U-Boot is installed from the prompt and then running a command that references said flash is not a good idea :sweat_smile:
- KiCad 8.0 released: https://www.kicad.org/blog/2024/02/Version-8.0.0-Released/
- Joining KubeCon EU 2024, so won't be able to host the next meeting

# February 20, 2024 4:00 PM UTC

:::info
- **Location:** https://jitsi.hamburg.ccc.de/racklet-community
- **Date:** February 20, 2024 4:00 PM UTC 
- **Host:** @orangecms
- **Participants:**
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Daniel

- Just discovered terminal graphics, and especially vector graphics protocols
    - Made a [ReGIS demo](https://hostile.education/regis-demo-mountainbytes2024.webm) for MountainBytes
    - Currently summarizing everything from the 60ies on
    - Deriving new specs from it for a seemless web native environment
        - WebALE: Web Application Launch Environment
        - WebGAP: Web General Application Protocol
    - Might be interesting for Racklet as well
    - Think of Plan 9 `drawterm` and `cpu`, but with applications in mind
    - Main idea: **self-serving** binaries, somewhat like [syslets](https://lwn.net/Articles/236206/) and [the other syslets](https://www.syslets.com/)
        - you drag them over to e.g. a Racklet SBC
        - out comes an app plus additional data over time
        - this is much like web apps are deployed today
        - maybe I'll call them _netlets_ or _sidelets_ or... ideas?

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

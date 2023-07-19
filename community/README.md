# Racklet Community Meeting Notes 2023

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Monday at [3 PM UTC](https://dateful.com/convert/utc?t=3pm) (on odd weeks). Check the the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/1esalu2VQcSqy_dShd0o7A/badge)](https://hackmd.io/1esalu2VQcSqy_dShd0o7A)

[TOC]

# July 17, 2023 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** July 3, 2023 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
:::

## Biweekly Recap

### Dennis

- Setting up a software development platform for Racklet
    - Idea: get going with the Kubernetes stack development for Racklet and try out different components not yet ported to ARM/RISC-V before putting in the (potentially large) porting effort
    - Based on 5 old dual-core amd64 laptops, will be running the Talos Linux stack and the distributed storage system concept based on JuiceFS
    - 6 * 2 TB of HDD, NVMe disks as cache
- JuiceFS storage stack updates: substituting KeyDB with Minio and TiKV makes sense
    - TiKV for metadata storage leverages RocksDB for persistence (in a much better way than KeyDB), which should run pretty fast off of NVMe drives
    - MinIO supports transparent compression, DirectPV should also make the provisioning quite straightforward
- Working on fixing a small [compatibility issue](https://github.com/juicedata/juicefs-csi-driver/pull/680) with JuiceFS and Talos
- As part of my day job, I'm bootstrapping [testk8s-platform](https://github.com/testk8s-platform), which will be a self-contained, standardized development and testing environment with observability and metrics
    - Based on Talos QEMU VMs running in Docker Compose
    - Parts of this, such as the metrics and observability stack, will be directly usable by Racklet as well
    - The experimental JuiceFS filesystem might live under this org as well so that it has a testing environment that is easy to work with

### Daniel

- Started hacking on an Allwinner R40 based [thin client](https://linux-sunxi.org/ShareVDI_R1)
    - Can run code via `sunxi-fel`, a stripped down U-Boot (mainline)
- Started hacking on Amlogic
    - Started [`aml_boot`](https://github.com/orangecms/aml_boot), a Rust rewrite of the [loader tool](https://github.com/superna9999/pyamlboot)
    - Khadas VIM1 (S905X)
        - Can [blink the LED](https://mastodon.social/@CyReVolt/110709120273900894) via `aml_boot`, i.e., MMIO
    - A [TV-Box based on S905X4](https://gist.github.com/orangecms/1c23845593a91470fb7c1895dd2cd0ee)
        - Can run a stock AOSP kernel almost into userspace, but it's [hacked up as heck](https://mastodon.social/@CyReVolt/110726302627975958)
- oreboot is getting further again
    - [Library for flash layout as FDT revived](https://github.com/oreboot/oreboot/pull/697)
    - [D1 SD card support (pending/draft)](https://github.com/oreboot/oreboot/pull/701)

# July 3, 2023 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** July 3, 2023 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
    - Marvin Drees, @MDr164
    - Felix Singer
:::

## Biweekly Recap

### Dennis

- Working on the distributed filesystem concept for Racklet
    - JuiceFS is installing easily, but there are some [issues](https://github.com/juicedata/juicefs-csi-driver/pull/680) with Talos, should however be quite simple to solve (hopefully)
    - The original plan to use KeyDB for data/metadata storage might not be usable (yet)
        - Active-active mode has [reliability](https://github.com/Snapchat/KeyDB/issues/679) [issues](https://github.com/Snapchat/KeyDB/issues/619)
        - KeyDB on FLASH similarly has [issues with data loss](https://github.com/Snapchat/KeyDB/issues/678)
        - Crashes and bugs [all](https://github.com/Snapchat/KeyDB/issues/676) [over](https://github.com/Snapchat/KeyDB/issues/674) [the](https://github.com/Snapchat/KeyDB/issues/693) [place](https://github.com/Snapchat/KeyDB/issues/694)
    - Now looking into MinIO for data storage and TikV for metadata
        - MinIO [can detect and repair](https://github.com/minio/minio/issues/6937) silent corruption
        - TikV [leverages RocksDB](https://tikv.org/deep-dive/key-value-engine/rocksdb/) for persistent storage, and RocksDB [can detect and prevent corruption](https://rocksdb.org/blog/2022/07/18/per-key-value-checksum.html)
    - Discovered [DirectPV](https://min.io/directpv), this might be useful
        - Limitation: only supports XFS (only basic integrity protection)
        - Shouldn't be an issue if the higher layers can deal with it
- Some more (generic) improvements to Talos Linux
    - https://github.com/siderolabs/talos/pull/7404

### Daniel

- Sloooowly getting there with SBI on JH7110/VF2
    - [Almost through init](https://gist.github.com/orangecms/ec608a807df268a5cb3cb7231f7e4686)
    - Will try attaching gdb over serial
- Talked about "small computers" at Tuebix, feat. Racklet
    - [Abstract (German)](https://www.tuebix.org/2023/programm/58-die-wirre-welt-der-kleinen-computer)
    - [Slides (English)](https://metaspora.org/sbcs-and-socs-tuebix-2023.pdf)

### Marvin

- As usual busy with BMC work (open source BMC gets more traction it seems)
    - u-bmc maybe rebranding
    - full release 0.1.0 in the making
    - OpenBMC spec being drafted for multi host bmc setups (racklet but larger)
    - actually several companies now interested in both u-bmc and OpenBMC support -> more time spent on fleshing out multi host architecture for racklet

# June 19, 2023 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** June 19, 2023 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
:::

## Biweekly Recap

### Daniel

- JH7110 SBI in oreboot is getting there, veeeeeeeeery close to done
- Decompression library in oreboot now factored out, working just fine

### Dennis

- Too busy to check out Onshape, will try to get to it later, currently researching stuff on the platform layer...
- Prototyping the proposed KeyDB + JuiceFS distributed storage stack as my summer job
    - Also working on a standardized Kubernetes testing platform, some parts of this should also be usable for Racklet

## UKI loading without UEFI

- UKIs are just PE files with [standardized sections](https://uapi-group.org/specifications/specs/unified_kernel_image/)
- In our minimal u-root based boot environment we don't necessarily want any UEFI stuff, not even a loader
    - u-root's existing loader: https://github.com/u-root/u-root/blob/main/pkg/boot/uefi/uefi.go
- It should be possible to develop a specific UKI loader, especially since PE routines are part of the [Golang stdlib](https://pkg.go.dev/debug/pe)
- UKIs could provide a neat, standardized way of shipping the Racklet kernel + initramfs bundles to the LinuxBoot + u-root environment

# June 5, 2023 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** June 5, 2023 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
:::

## Biweekly Recap

### Daniel

- JH71{0,1}0 merged into oreboot main 🥳
    - [VisionFive 2](https://github.com/oreboot/oreboot/pull/686)
    - [VisionFive 1](https://github.com/oreboot/oreboot/pull/606) from RISC-V dev board program
    - Both can share a lot of code for DRAM init, an open TODO
- Reworking SBI from sunxi/nezha for reuse
    - https://github.com/oreboot/oreboot/pull/693
- Gave a [talk on bootloaders in RISC-V International Tech Session](https://sites.google.com/riscv.org/riscv-technical-sessions/home#h.hc2jd4kzivkt)
    - Missed mentioning Racklet, sorry! -_-
- JH7110 based [Milk-V Mars](https://milkv.io/mars) is now official
    - M.2 E-key slot for PCIe
    - 1x gigabit ethernet
    - SPI flash
- India is also going to release RISC-V [SoCs and SBCs](https://gitlab.com/shaktiproject) based on the [Shakti](https://shakti.org.in/) series
- The [RISE (RISC-V Software Ecosystem) project](https://riseproject.dev/) has nothing to do with the [RISE (Reconfigurable Intelligent Systems Engineering) group](https://iitm-riselab.github.io/), the [team behind Shakti](https://shakti.org.in/team.html)

### Dennis

- Verneri discovered [typst](https://typst.app/), a Google Docs -style LaTeX and Markdown replacement written in Rust
    - OSS compiler, seems to be very powerful with integrated scripting etc.
    - Source: https://github.com/typst/typst
    - Should the (more advanced) docs for Racklet be written using this?
- Busy last week with a malware analysis event from the university program, not much time to work on Racklet
- Setting up Onshape and creating some initial casing/tray designs this week

### Marvin

- OSFC planning ongoing, venue still not 100% decided but looks like Sunnyvale might be it
- Got involved in a 'new project' (RUST/RISC-V)
- BMC work ongoing as per usual, might not be able to target SLSA3 but only SLSA2, need to discuss with OSSF
    - Saw they have a tool for orgs called [allstar](https://github.com/ossf/allstar)
    - Maybe also interesting for Racklet further down the road?

# May 22, 2023 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** May 22, 2023 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
:::

## Biweekly Recap

### Daniel

- Got the JH7110 to initialize DRAM at last, confused base addresses again
    - Both VisionFive 1 + 2 merged in oreboot main now
    - Next up is RustSBI
- Got the Lichee Pi 4A \o/
    - No SoC manual just yet
    - [Code](https://github.com/sipeed/LicheePi4A/tree/pre-view/external/revyos) is available, including DRAM init and Linux
- Milk-V are releasing a [JH7110 board in the RPi form factor](https://twitter.com/MilkV_Official/status/1660565131586322432)

### Dennis

- Got the SD card emulator development setup up and running again
    - Some issues with Sigrok/PulseView in Flatpak, expect to resolve soon
- Updated the [interface diagrams](https://github.com/racklet/racklet/tree/arch-diagrams/docs/diagrams)
    - Shrink the tray width from 32 mm to 30 mm and move everything to the right to accommodate the ATX 24-pin connector ([39-28-1243](https://www.molex.com/en-us/products/part-detail/39281243)) on the backplane (on the top) and to provide space for front-facing USB headers, buttons, LEDs etc.
    - Will set up OnShape and do some 3D sketching next

# May 8, 2023 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** May 8, 2023 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Veeti Poutsalo
:::

## Biweekly Recap

### Daniel

- Spent lots of days and nights on the JH7110 / VisionFive 2
    - Very familiar with U-Boot now, still having trouble grasping early init
    - Something is still fishy :fishing_pole_and_fish: in the DRAM init -_-
    - Also got quite some more understanding of DRAM controllers, especially OpenEdges [Orbit](https://www.openedges.com/memorycontroller), the one used in the JH7100 and JH7110 SoCs
    - If you want to dive deep into DRAM controllers, go check out [Onur Mutlu's work](https://people.inf.ethz.ch/omutlu/) (professor at ETH Zürich)
- Ordered a [LicheePi 4A](https://sipeed.com/licheepi4a/) (TH1520 SoC)

### Dennis

- Getting started with the `hatlet` hardware, lots of soldering
    - "Reviving" the old SD emulator hardware, as there's one oversight that we can't easily do with the new hardware until another PCB is designed
        - We're missing a PMOD board to plug in an existing SD card for probing existing communication
    - "Blink sketch" already working
- Update from Verneri:
    - Banana plugs fit on `hatlet-dongle`, power supply yet untested
- Research on USB hubs
    - Turns out that USB (2.0) hubs are pretty simple devices, all messages from the host are unconditionally replicated to all devices in the chain, and it's the reponsibility of the individual devices to discard messages not addressed to them
    - However, upon the host's request, messages from a particular device towards the host get a private channel that is not replicated to other devices
    - Implications for Racklet: any private configuration, secret keys etc. from the host (RMC) towards a particular device (BMC) must be encrypted to avoid sniffing on the backplane USB bus. The simplest way to do this is to simply request a symmetric key from the BMC and encrypt all traffic from the RMC to the BMC with that.

### Marvin

- A **lot** work went into u-bmc
    - Missed the release due to "urgent" customer requests
    - Release rescheduled to just a few days from now (mainly docs)
    - Actual customer interest now due to showcase at OCP summit
    - Undergoing initial preparations for SLSA and OpenSSF certifications
    - Need 100% Redfish compatibility for OCP acceptance so working on Redfish gRPC gateway (initial work done by Ed Tanous but incomplete)
- Started work on OpenBMC multi-node rework
- Started work on coreboot rework on RISC-V SoCs (collab with coworker for his thesis)
    - Might be interesting for customized cluster booting with LinuxBoot payload

## Other Discussions

- Discussions and insights about [openSIL](https://www.phoronix.com/news/AMD-openSIL-Replace-AGESA)
    - All code running on the x86 cores will be open source (eventually)
    - PSP and its server equivalent, i.e., the ARM core, will still be closed for the forseeable future
        - However, has no network stack
        - Only needed for DRAM init and TPM on client platforms, can be disabled
        - Additionally handles RAS, SEV/SMM on servers, and thus can't be fully disabled

# April 24, 2023 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** April 24, 2023 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
    - Ron Minnich, @rminnich
:::

## Biweekly Recap

### Daniel

- Hi from Canada
- Almost got JH7110 DRAM working, searching for awesome bugs

### Dennis

- Just came back from KubeCon EU 2023 in Amsterdam, lots of updates from the cloud-native ecosystem. In summary:
    - eBPF-based networking and observability is taking off
        - [Cilium](https://cilium.io/) is doing much of the heavy lifting right now
            - Gaining a lot of new features
            - High performance and low overhead, especially relevant for the limited computing potential of Racklet
            - Upcoming service mesh implementation
            - Wireguard for securing inter-node traffic
        - Various other observability tools based on eBPF (syscalls etc.)
    - A lot of talk around on-prem (with server hardware)
    - Some Raspberry Pi / SBC clusters from Canonical on the show floor:
        - ![canonical-clusters](https://i.imgur.com/GhOOug0.jpg)
        - ![canonical-rpi-cu](https://i.imgur.com/XWjom95.jpg)
        - The top one is an old Raspberry Pi -style cluster design that Canonical did some custom orders for from a third-party casing manufacturer, while the bottom one is essentially a one-off that runs NUC-style compute units with i5 CPUs
        - The top one on the other hand showcases the use of SSDs with USB-to-SATA adapters, a very similar setup will be deployed for the 2U compute units in Racklet (with the SSD being behind instead of next to the SBCs)
            - The board on top is the Raspberry Pi PoE HAT, this will be replaced with the BMC hat in Racklet (which also provides better power delivery)
        - The bottom cluster setup is very close to the planned Racklet 3U chassis in terms of physical form factor, seeing this in person really helped visualize the scale of the end result
    - The talk by SpectroCloud and Tevel on autonomous fruit picking on the edge touched upon relevant hardware and platform security topics
        - Immutable, TPM-verified OS
        - Using the TPM to encrypt container images
        - As they're working on an Intel platform:
            - Workload isolation with memory enclaves (SGX)
            - Dynamic assessment of device during runtime (TXT)
            - We might want something similar on ARM/RISC-V for Racklet?
        - It's a fun presentation, highly recommend checking out the [slides](https://static.sched.com/hosted_files/colocatedeventseu2023/1f/SpectroCloud-Tevel-Fruit-Picking-Robots-Powered-by-K8s.pdf) and [recording](https://youtu.be/PB8TdTGmvKA)

# April 10, 2023 3:00 PM UTC

:::info
No meeting
:::

# March 27, 2023 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** March 27, 2023 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
:::

## Biweekly Recap

### Daniel

- Got `cpu` and LinuxBoot working on the VisionFive 2 (JH7110 SoC)
    - Based on U-Boot
    - Created defconfigs for devel and upstream branches from StarFive
        - https://github.com/orangecms/linux/tree/vf2-devel
        - https://github.com/orangecms/linux/tree/vf2-upstream
    - Ported `kexec` patches back to the 5.15.0 based devel kernel
        - https://github.com/orangecms/linux/tree/vf2-devel-kexec
        - That one supports the PCIe controller, so we can boot from NVMe
        - https://twitter.com/OrangeCMS/status/1640083580888948738
    - Did live streams on the effort, to be published soon :tm:
- Went to Embedded World :steam_locomotive: 
    - Got RISC-V :socks: and stickers :stuck_out_tongue:
    - Talked to Antmicro folks regarding ECP5 DC-SCM for u-bmc and ARVSOM for oreboot
        - ECP5 DC-SCM boards were only made for internal use
        - ARVSOM not really produced
        - Keeping in touch via email :email: 
    - ASUS Tinker V board based on a Renesas SoC with an Andes core
        - Held one in my hands, chose not to run away with it
- Sipeed announced selling LM4A / Lichee Pi 4A (T-Head TH1520 SoC) in two weeks
    - https://twitter.com/SipeedIO/status/1640345193592365059
- [OSFC 2023](https://www.osfc.io/) will be in the US :us: October 10-12

### Verneri

- Defined backplane connector [pinout](https://github.com/racklet/electronics-prototyping/issues/35) with Dennis.
    - Added corresponding symbols and footprints to [racklet-kicad-lib](https://github.com/racklet/racklet-kicad-lib)
- Finished and ordered version `0.1.0` of `hatlet` and `hatlet-dongle`
    - PCBs are done already, waiting for PCB assembly and shipping now
    - TODO: merge to `main` and add manufacturing outputs as tags/releases

### Dennis

- My final PRs for the golang DHCP library have finally been [merged](https://github.com/insomniacslk/dhcp/pulls?q=is%3Apr+author%3Atwelho) :tada:
    - The [Talos Linux DHCP rework](https://github.com/siderolabs/talos/pull/5897) is now finally unblocked, will finish it up early this week to get it merged for Talos 1.4.0
- Work on the Racklet HAT prototyping hardware (`hatlet`) with Verneri, initial order submitted
- Thought about HA using load balancers for Racklet: we can actually add an [EdgeRouter X](https://eu.store.ui.com/collections/operator-edgemax-routers/products/edgerouter-x) to enable BGP-based "hardware" load-balancing and HA
    - This is nowadays directly [supported](https://docs.cilium.io/en/v1.13/network/bgp-control-plane/) in Cilium, which should make it very straight-forward
- [Going to KubeCon + CloudNativeCon EU 2023](https://events.linuxfoundation.org/kubecon-cloudnativecon-europe/), April 18-21 :cloud:

# March 13, 2023 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** March 13, 2023 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
:::

## Biweekly Recap

### Daniel

- Just returned back from Chemnitzer Linux-Tage
    - Had an [interesting talk](https://chemnitzer.linux-tage.de/2023/de/programm/beitrag/133) on [Clevis](https://github.com/latchset/clevis), a tool for key exchange/establishment for booting with encrypted drives
- oreboot now has a decent log crate that works well on 4 SoCs
    - Allwinner D1
    - Bouffalo Lab BL808
    - StarFive JH7100
    - StarFive JH7110
- JH7110 / VisionFive 2 making progress
    - Preliminary version of the manual is now [available as a PDF](https://doc-en.rvspace.org/JH7110/PDF/JH7110_TRM_StarFive_Preliminary.pdf)
    - Got better understanding now thanks to U-Boot sources and manual
    - VF1+2 [stack up nicely](https://twitter.com/OrangeCMS/status/1633552096606691340/photo/1)
- Will be at Embedded World on Wednesday
    - Plus [PCB Arts](pcb-arts.com) after party
- T-Head hosted [a conference](https://twitter.com/SipeedIO/status/1631121626178936835)
- ByteDance hosted a [Cloud Firmware event](https://www.phoronix.com/news/Bytedance-CloudFW-Open-Source)
    - [Christian Walter from 9eSec spoke there](https://twitter.com/OrangeCMS/status/1632952760952823808)
    - Lenovo [presented their efforts wit coreboot+LinuxBoot](https://www.phoronix.com/news/Lenovo-LinuxBoot-ByteDance)
- Sipeed are releasing [more FPGA boards](https://twitter.com/SipeedIO/status/1635229233491615753)
    - Tang Nano 20K with LiteX support

### Dennis

- VisionFive2 [PMU](https://www.kernel.org/doc/html/v5.7/riscv/pmu.html) (*Performance Measurement* Unit) support [upstream](https://www.phoronix.com/news/SoC-Drivers-Linux-6.3)
- Work on the [arch-diagrams](https://github.com/racklet/racklet/tree/arch-diagrams) branch of the main repo (to be migrated to a better place soon-ish)
- Revising the [interface diagrams](https://github.com/racklet/racklet/tree/arch-diagrams/docs/diagrams) to check for physical fitment and requirements
    - Fitting the power supply behind the SBCs and disks is going to be a challenge
    - 16-port 10 inch rack network switches come in various depths
        - Good option: [ZyXEL GS1100-16](https://www.zyxel.com/global/en/products/switch/8-16-24-port-gbe-unmanaged-switch-gs1100-series)
        - Most of these are directly AC powered, but that still works for us

### Verneri

- Implemented the first revision (0.1.0) of [hatlet](https://github.com/racklet/electronics-prototyping/tree/hatlet/kicad-projects/hatlet)
    - Hatlet is the prototyping platform of the SBC HAT board acting as the BMC
    - Otherwise finalized, but we need to design the backplane connector now
    - Also in the repo: `socket-to-c` (0.1.0), which is a dongle to connect emulate the backplane interface with a PC for development purposes, power measurements, etc.

# February 27, 2023 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** February 27, 2023 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
:::

## Biweekly Recap

### Daniel

- Got a VisionFive 2 (yea...)
    - Forked/improved [XMODEM Rust library](https://github.com/nycresistor/xmodem.rs/pull/1) and made a [Rust CLI tool](https://github.com/orangecms/vf2-loader) replacing the [C tool](https://github.com/xypron/JH71xx-tools) that [Heinrich had to fix up](https://github.com/starfive-tech/Tools/issues/3)
    - Someone [rewrote the tool adding the necessary header to a firmware image based on my reversing](https://github.com/starfive-tech/Tools/issues/1), picked up by StarFive again
    - Docs are not really there yet, just [some HTML draft, confusing and missing lots of necessary information](https://doc-en.rvspace.org/JH7110/TRM/JH7110_TRM/gpio_oen_list.html)
    - Folks [use Linux patches for reference](https://zyedidia.github.io/blog/posts/2-baremetal-visionfive/) or U-Boot (me)
    - [Draft PR](https://github.com/oreboot/oreboot/pull/686) for oreboot
        - Got a blinky and UART out to work, but things are flaky
        - Clocks ... please don't ask
    - Maybe [JTAG](https://github.com/starfive-tech/VisionFive2/blob/JH7110_VisionFive2_devel/bsp/env/freedom-u500-unleashed/openocd.cfg)?
- Drafted oreboot for Allwinner H616 (Arm64)
    - MangoPi MQ-Quad board
    - Supports both `aarch32` and `aarch64`, starting in `aarch32`
- Forked and fix up Rust [library](https://github.com/orangecms/aw-fel-rs) and [CLI](https://github.com/orangecms/aw-fel-cli) for Allwinner FEL
    - Reading from and writing to memory works fine
    - Can run payload from SRAM
    - Can dump mask ROM
- Rockchip tools
    - There is [rkflashtool](https://github.com/linux-rockchip/rkflashtool)
        - Community made, protocol not yet fully reversed
        - See also [the Radxa wiki](https://wiki.radxa.com/Rock/flash_the_image) (other pages have other notes...)
    - There is [rkdeveloptool](https://github.com/rockchip-linux/rkdeveloptool)
        - Proprietary vendor tool
        - Notes scattered around several places
        - [Rockchip wiki](https://opensource.rock-chips.com/wiki_Rkdeveloptool)
        - [Radxa wiki](https://wiki.radxa.com/Rockpi4/install/rockchip-flash-tools)
        - [96 Boards](https://www.96boards.org/documentation/consumer/rock/installation/linux-mac-rkdeveloptool.md.html)
    - xboot also made a [Rockchip CLI](https://github.com/xboot/xrock)
- RustWasm in Fiedka drafted
    - Using / contributing to [System76/Romulan firmware parser library](https://github.com/orangecms/romulan)
    - Working fine, together with transform and rendering in UI already
        - Might present on that at EuroRust, waiting for CfP

### Dennis

- Working on finalizing the [DHCP rework in Talos](https://github.com/siderolabs/talos/pull/5897)
    - Will try once more to re-spin [the DHCP library PR](https://github.com/insomniacslk/dhcp/pull/470) to get the maintainer's attention

### Verneri

- Started designing the "hatlet" board.
    - [electronics-prototyping: hatlet](https://github.com/racklet/electronics-prototyping/tree/hatlet)
- Current plan:
    - Modular RP2040 dev board with:
        - Peripherals exposed over PMOD headers:
            - SPI
            - 2x I2C
            - UART
            - GPIO / PIO (at least 6 pins)
            - Anything else?
        - Interchangeable Host/Device interface board for USB
            - Expose USB with to "PMOD"-like connector
            - Make separate boards with:
                - USB type A (RP2040 as host)
                - USB type C (RP2040 as device)
                - USB hub board with corresponding headers

## Misc.

- [EuroRust](https://eurorust.eu) 🇪🇺:crab: will be held in Brussels and online, October 12-13

# February 13, 2023 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** February 13, 2023 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
:::

## Biweekly Recap

### Daniel

- Been to FOSDEM, still recovering a bit - interesting talks
    - [Hardware-backed attestation in TLS](https://fosdem.org/2023/schedule/event/security_hw_backed_attestation/) (Cloud and IoT POV, with Parsec+Veraison)
    - [Rust based Shim-Firmware for confidential container
](https://fosdem.org/2023/schedule/event/cc_online_rust/) (Intel)
    - [Open Source Confidential Computing with RISC-V](https://fosdem.org/2023/schedule/event/cc_riscv/)
      (Salus hypervisor by Rivos)
- Did more dev streams
    - [oreboot on BL808, second video](https://www.youtube.com/playlist?list=PLenOHeTI_A9MwA0HlNogiJVvU5RtsDSz9)
    - [LinuxBoot, Fiano and Fiedka playlist with 2 videos already](https://www.youtube.com/playlist?list=PLenOHeTI_A9OolDEoreItEjylw5do4XxH)
        - Fiedka tutorial for various firmware images and injecting LinuxBoot
        - Fiano PR review for parsing Intel microcode updates
- Worked on oreboot docs
    - Rewrote the [README](https://github.com/oreboot/oreboot/#oreboot-readme) (sorry, it's been outdated for too long)
    - TODO: build Rust docs for the boards, deploy them to GitHub pages

### Dennis

- Finishing up school work, haven't had time to look into SD emulation further yet
- Dug into the details of various RK3588(S) SBCs
    - Will probably acquire a [ROCK 5 Model B](https://wiki.radxa.com/Rock5/5b) 16 GB
        - Exclusively uses USB-C PD for power input, but later figure out that you can actually [power it over 5V through GPIO](https://forum.radxa.com/t/powering-rock-5b/10759/5), which is great news for Racklet
            - Small PD inconsistency results in some PD adapters not being compatible: https://wiki.radxa.com/Rock5/5b/power_supply
        - Full PCIe 3.0 x 4 on the bottom for NVMe
        - 2.5 Gbit ethernet is an additional bonus for future-proofing
        - "Stock" heatsink quite tall, but mounting pattern seems simple enough so that we can make/acquire [a lower-profile one](https://shop.allnetchina.cn/products/rock5-b-acrylic-protector?variant=39877626429542)
    - The [Orange Pi 5](https://www.cnx-software.com/2022/11/11/orange-pi-5-most-affordable-rockchip-rk3588s-sbc/) has the cut-down RK3588S SoC, quite large footprint and questionable long-term support
    - The [Khadas Edge2](https://www.khadas.com/edge2) lacks connectors and is very expensive for some reason
    - The [NanoPi R6S](https://www.cnx-software.com/2022/10/28/nanopi-r6s-rockchip-rk3588s-router-mini-pc-dual-2-5gbe-gbe-hdmi-2-1/) has 3 gigabit or above Ethernet ports, might work well for routing purposes
- Off-topic: VisionFive 2 seems to also be [available](https://shop.allnetchina.cn/collections/top-new-arrivals/products/starfive-visionfive-2-quad-core-risc-v-dev-board) now

### Verneri

- Busy with studies

# January 30, 2023 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** January 30, 2023 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Daniel Maslowski, @orangecms
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Verneri Hirvonen, @chiplet
:::

## Biweekly Recap

### Daniel

- Sipeed may offer a 64-core board with a few DRAM slots
    - https://twitter.com/SipeedIO/status/1620011587720589312
    - It likely comes with LinuxBoot by default
    - https://github.com/sophgo/bootloader-riscv/tree/master/u-root
    - SoC etc would technically be small enough for a Racklet fit
- People doing interesting stuff on RISC-V
    - http://oberon.wikidot.com/project-oberon-v an Oberon port
- Factored out the log library in oreboot
    - Took a few iterations, now based on a Mutex
    - https://github.com/oreboot/oreboot/pull/670 was the last PR so far
    - Works on D1 and BL808 (both C906 core), not yet on JH7100 (U74 something core, seems to hang on atomics)
    - SBI to follow, would be factored out from D1

### Dennis

- Thought about and evaluated options for distributed storage – is it actually feasible enough for Racklet, or should we abandon the idea and just go with a JBOD solution?
    - Seems to be feasible, though no current solution is really suitable
    - Ceph (through [Rook](https://rook.io/)) is too heavy
    - Longhorn uses outdated iSCSI (i.e. won't work in Talos) and is quite inefficient with storage (has nice snapshots though)
        - Nice dashboard
        - Work ongoing to migrate to NVMEoF (great for Racklet), but still [some ways out](https://github.com/longhorn/longhorn/issues/3202)
    - OpenEBS Jiva also uses iSCSI, but is at least somewhat supported by Talos, performance is another question entirely
    - OpenEBS Mayastor (NVMEoF) is *very* early alpha software, missing crucial reliability and scalability features
- We can however implement our own storage stack based on the modern, but already battle-tested [JuiceFS](https://juicefs.com/en/), details [below](#Thoughts-on-Distributed-Storage)
- Following the (slow-ish) RK3588 u-boot enablement:
    - There's now an [AUR repo](https://aur.archlinux.org/packages/rk3588-uboot-git)
    - Some work potentially heading upstream [here](https://github.com/rockchip-linux/u-boot)

### Marvin

- Quite some work went into the BMC stack again
    - Likely first real showcase during [OCP regional summit](https://www.opencompute.org/summit/regional-summit) (19-20 April, 2023)
    - Boards now integrated into test-rack
    - WebUI for cluster management still WIP
    - Complete source release planned in March (*hard deadline for OCP, so this time for real*)
    - Found a much easier way for A/B scheme after not being satisfied with Androids A/B
    - Potentially adding optional OpenAPI/Swagger support as it can be generated from protobufs
- Raspberry Pi [Compute Blades](https://www.jeffgeerling.com/blog/2023/hyperscale-your-homelab-compute-blade-arrives) entered Kickstarter :)
    - https://computeblade.com/
- Sidenote: We (9e) are now ARM SystemReady [partners](https://www.arm.com/architecture/system-architectures/systemready-certification-program/partners), so expect some more ARM work

### Verneri

- Reorganized electronics-prototyping repo a bit.
- Thinking about designing a racklet dev board (for sw prototyping / bringup). What interfaces do we need?
    - RP2040
    - USB
    - ...
- Modular (inspired by https://oxide.computer/podcasts/oxide-and-friends/1173990)
    - rackletlet? :D

## Thoughts on Distributed Storage

- Dennis: thought about composing a distributed, high-performance storage solution that is both modern and lightweight enough for Racklet
    - [JuiceFS](https://juicefs.com/en/) CSI driver backed by [KeyDB](https://docs.keydb.dev/) running in [Active Replication ("Active Active")](https://docs.keydb.dev/docs/active-rep) and [FLASH](https://docs.keydb.dev/docs/flash/) mode (leveraging [RocksDB](https://rocksdb.org/)) for both metadata and objects
    - This should be full HA and tolerate e.g. spontaneous node removal
        - "Active Active" mode has split brain prevention
    - JuiceFS offers amazing flexibility if we want to back the storage of a Racklet cluster externally
    - Resource consumption should be *much* lower than Ceph
    - Performance should be noticeably higher than Ceph/Longhorn/Jiva
        - **TODO:** to be benchmarked, right now this setup is purely theoretical
        - Most of the performance of Mayastor without all of its reliability downsides and lack of features
    - Notably all components of this stack are actively tested and verified to work on ARM

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


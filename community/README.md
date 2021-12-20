# Racklet Community Meeting Notes 2021

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting for 2021. Check the the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/P7WKiyZSTpCeyfpj3Lm2Fw/badge)](https://hackmd.io/P7WKiyZSTpCeyfpj3Lm2Fw)

[TOC]

# December 20, 2021 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** December 20, 2021 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @cyrevolt
- **Agenda:**
    1. Biweekly recap
    1. Minimal `start.elf`
    1. Nehza update
    1. DSLogic Plus
:::

## Notes

- Biweekly recap
    - Nezha boots Linux and `kexec`s another Linux kernel now! \o/
    - Marvin has a new [hackable keyboard](https://kprepublic.com/products/bm60-rgb-60-gh60-hot-swappable-pcb-programmed-qmk-firmware-type-c)
    - twelho:
        - I got the DreamSourceLab DSLogic Plus logic analyzer!
        - Going to do my Bachelor's thesis about Racklet's SD emulation
    - Rust binary sizes: `objcopy` is doing weird stuff, `strip` helps even with `no_std` binaries...
    - [TamaGo](https://github.com/f-secure-foundry/tamago) on ASpeed BMCs close to working
        - Replaces magic boot assembly
- Minimal `start.elf`
    - There is a cut down `start.elf` version: `start_cd.elf`
    - The cut down size is ~780 KiB instead of ~4 MiB
    - Should be much easier to work with when emulating
    - Has reduced graphics functionality, but shouldn't matter for Racklet
    - https://github.com/raspberrypi/firmware/blob/master/boot/start_cd.elf
    - https://www.raspberrypi.com/documentation/computers/configuration.html#boot-folder-contents
- TI AM64x dev board update
    - Contains Cortex-A53, Cortex-M4F and [Cortex-R5F](https://developer.arm.com/ip-products/processors/cortex-r/cortex-r5) cores
    - Embedded JTAG allows debugging all of the different cores
        - Cortex-A is normally debugged using CoreSight
    - The board is quite large due to all the integrated debugging tools, the actual chip is roughly the same size as the Pi SoC
- Nezha update
    - RustSBI set up; key feature is delegating traps, but _not_ illegal instructions
    - Linux 5.15.5 with patches not yet in mainline; FPU _must_ be configured
    - network (ethernet), MMC, GPIO all work; USB disabled for now
    - WIP: move code from RustSBI to oreboot directly/reduce it (initially already working)
    - talk ["Hacking on RISC-V and Operating Systems"](https://pretalx.c3voc.de/rc3-2021-r3s/me/submissions/FTPACX/) at rC3 (German, probably with translation, slides will be in English)
    - smaller Lichee RV board almost working as well, needs Device Tree adjustments https://linux-sunxi.org/Category:D1_Boards
- DSLogic Plus
    - Searched for the cheapest [sigrok-supported](https://sigrok.org/wiki/Supported_hardware#Logic_analyzers) logic analyzer that goes past 100 MHz
    - The [DSLogic Plus](https://www.dreamsourcelab.com/shop/logic-analyzer/dslogic-plus/)
        - Available on AliExpress for ~100 €
        - Goes to 400 Mhz (supposedly, needs better probes to do that though)
        - 16 channels
        - 3 channel 100 MHz streaming capture (over USB 2.0)
        - 256 Mbit memory for capture-to-RAM
    - Plugged it in and installed firmware from Arch AUR, PulseView picked it up right away and recognized all speeds/channels/triggers etc.
    - Starting a capture works (blinking LED), but haven't gotten around to wiring up anything to analyze yet

# December 6, 2021 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** December 6, 2021 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Biweekly recap
    1. SD emulation
    1. RP2040 Rust progress, PIO
:::

## Notes
- Biweekly recap
    - TI AM64x dev board arrived
        - Quite large, exposes all interfaces of the biggest chip variant
        - Multiple CAN interfaces, intergrated JTAG emulator
        - No testing yet
        - 250€ ($300) directly from TI, but TI won't sell it
        - ~12€ for the (top end) SoC
    - twelho: Ordered a logic analyzer, might write a thesis about the SD emulation
    - Oxide introduced [Hubris](https://github.com/oxidecomputer/hubris) and [Humility](https://github.com/oxidecomputer/humility) at OSFC 2021
        - Other options: [Tock OS](https://www.tockos.org/), [RTIC](https://rtic.rs/dev/book/en/)
- u-bmc
    - Marvin presented at OSFC
    - Basic Raspberry Pi port
    - Git cleanup
    - Support for the TI AM64x dev board on the horizon
        - Blobless boot
            - One u-boot on the RT cores
            - Another u-boot on the main cores
        - Everything documented
- SD Emulation
    - Requirements
        - serial clock
            - up to 400kHz clock during init
            - up to 25MHz clock after init
        - adjustable max delay before response (in clock cycles)
            - it's possible to read data from a slower medium, but the response needs to generated within the 25MHz clock
    - Using a microcontroller
        - Infeasible with low end microcontrollers
            - Driving memory mapped IO is too slow, no time for control flow even with interrupts
        - Raspberry Pi Pico Programmable IO (PIO) could work
    - Using an FPGA
        - Meeting timing requirements for 25MHz is trivial
        - Open source SD emulation core(s) already exist:
            - https://github.com/fusesoc/sd_device
            - https://github.com/enjoy-digital/litesdcard
        - TODO:
            - Proof of concept with an FPGA dev board
                - e.g. with [TinyFPGA_BX]
                    - 16 kB of BRAM
                    - 750 kB of user flash area
                - Need a small boot payload for this
- RP2040 Rust progress, PIO
    - RP2040 Rust support has finally become actually usable in [rp-hal](https://github.com/rp-rs/rp-hal)
    - PIO (Programmable I/O) is also supported: https://github.com/rp-rs/pio-rs
    - The PIO can run at the speed of the main cores (133 MHz, can be overclocked)
        - This is fast enough to "oversample" the SD signal, and external triggering of the PIO using the SD clock signal should also be possible
        - The idea is to "capture" an entire SD command/packet using the FPGA-like PIO, and then DMA the result into memory for the main processor running the state machine
            - This might be feasible due to the drastic reduction in (costly) interrupts to the main CPU compared to it being directly "clocked" by the SD clock.

[TinyFPGA_BX]: https://tinyfpga.com/

# November 22, 2021 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** November 22, 2021 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Verneri Hirvonen, @chiplet
    - Daniel Maslowski, @cyrevolt
    - Lucas Käldström, @luxas
- **Agenda:**
    1. Biweekly recap
    1. Thoughts about USB on the BMC, SD(IO) emulation back?
    1. Nezha / D1 update (RustSBI etc)
    1. Improve repo/org structure
:::

## Notes
- Biweekly recap
    - The meeting time has inadvertently shifted to 4 PM UTC due to daylight savings in Finland, should we keep this time or move it back by one hour?
    - TI AM64x dev board on the way
        - Might even arrive in a week
        - Direct/alternative RMC, Racklet interface compatibility
        - The only docs that cannot be accessed without NDA is secureboot
            - Even design guides and silicon guides are open
        - Supports something called fast serial interface
            - Needs kernel modules, might be able to talk to a Pi
            - Protocol made by TI, sort of proprietary but well documented
                - Emulation via e.g. an FPGA
        - The guides explain how to design USB-C with power management etc.
- Thoughts about USB on the BMC, SD(IO) emulation back?
    - All "low-end" MCUs only have one USB controller, but they all have CAN
        - Replace the USB hubs in the backplane with a CAN network?
            - This would free up the USB interface of each BMC for communication with the compute unit
        - CAN networks might require complicated termination and impedance matching etc.
        - CAN is a flat routing protocol, but that doesn't matter in our usecase
        - The hardware-software interface of CAN controller is tricky to work with (based on experience)
    - Using CAN doesn't fix the issue that some SBCs simply won't boot off of anything other than SD(IO)
        - E.g. the [Odroid C4](https://www.hardkernel.com/shop/odroid-c4/) will only boot U-Boot off of SD or EMMC
        - SD emulation (SDIO not SPI) is required for (at least) half of the SBCs on the market, including earlier Raspberry Pi
        - All SBCs that support USB booting should also support SD booting, so if SD emulation would be implemented, the USB upstream could be dedicated to the backplane
    - SPI flash probably has strict time requirements
        - https://trmm.net/SPI_flash
        - This means we likely don't have time to fetch memory blocks from the RMC over USB in time
        - BMCs must have their own local SPI flash
            - 16 MiB is the most common amount, and should fit our u-root boot environment
            - 32 MiB would enable A/B partitioning, but not strictly necessary (corrupt/invalid firmware in the flash doesn't brick the compute unit, just delays its bootup as the BMC needs to re-flash it)
        - Three options to handle write protection:
            - Do a "MitM" using the BMC MCU and drop write requests, is this fast enough?
            - Do a "MitM" using an FPGA, how cost effective is this?
                - https://trmm.net/Spispy/
            - Write protect the flash and and it over to the MCU in its entirety
                - https://github.com/felixheld/qspimux#how-to-setup-the-hardware
        - ASpeed chips support hardware SPI write protection
        - Most controllers allow downgrading Quad-SPI to regular SPI
            - This should hopefully be true for SBC-integrated bootloaders as well
- Nezha / D1 update (RustSBI etc)
    - Linux for sunxi/D1 is being pushed to mainline (Samuel Holland)
        - clock https://lkml.org/lkml/2021/11/18/1320
        - watchdog https://lkml.org/lkml/2021/7/25/303
        - media https://lkml.org/lkml/2021/11/18/1268
        - etc etc; browse LKML :)
    - oreboot -> RustSBI -> payload somewhat works
        - https://riscv.org/wp-content/uploads/2017/05/riscv-privileged-v1.10.pdf
        - RustSBI is a library, and you implement it per platform (as of now)
        - https://github.com/orangecms/rustsbi-nezha/tree/rustsbi-nezha
        - transitions from M-mode to S-mode and sets up trap handlers
        - will see if we pull the implementation into oreboot directly
        - trap handlers work :)
    - oreboot -> xv6 is fine, stays in M-mode
    - Linux for D1 does not support M-mode, so we use RustSBI
        - oreboot -> RustSBI -> Linux errors in `head.S` (`InstructionPageFault`)
        - https://github.com/orangecms/linux/tree/riscv/d1-wip
        - more work needed around memory setup :)
        - https://blog.stephenmarz.com/2021/02/01/wrong-about-sfence/ not helping, but nice read
- Improve repo/org structure
    - Move from RFC-based structure to self-documenting code and releases via tagging
        - Each component (hardware, electronics, firmware, etc.) should reside in its own repo and tag its own "releases"
            - Documentation is version controlled with Git, and follows the tags, the content of the relevant RFCs is moved into the right repos
        - The racklet/racklet main repo pulls all components in as submodules and tags full releases of Racklet, e.g. Racklet 0.1.0 contains hardware 0.1.2, electronics 0.2.5 etc.
            - The content of the main overview RFC is moved here

# November 8, 2021 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** November 8, 2021 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Verneri Hirvonen, @chiplet
    - Daniel Maslowski, @cyrevolt
- **Agenda:**
    1. Biweekly recap
    1. Major design update: shield, backplane, RMC
:::

## Notes
- Biweekly recap
    - There is a complete list of components provided by the LCSC assembly services available (as a CSV)
        - Could be quite easily integrated into kicad-rs
        - Extend the current kicad-rs YAML format so it can be used for any BOM, not just LCSC
    - u-bmc being prepared for OSFC
    - Better architecture for booting an actually trusted image on the RMC
        - Signature verification updated (ec-based)
        - Incorporate TrustZone?
    - TI devboard update: ETA late November/early December
    - u-bmc for any ARM/ARM64 device, only kernel/device tree required
        - This should run TrustZone etc.
        - Updates using go-tuf
        - BMCs have limited storage, A/B partitioning may be a challenge
        - Control (fans, GPIOs etc.) via GRPC
        - Ethernet or IP over USB?
        - Stick formfactor like USB Armory II possible
    - Nehza update:
        - UART support, payload execution in oreboot main
        - Reset button working :)
    - RISC-V on the horizon
        - Sunxi is going to release something new?
        - [MangoPi](https://www.hackster.io/news/mangopi-mq1-is-an-ultra-compact-soon-to-be-open-source-allwinner-d1-risc-v-dev-board-eb5388783a0e) (in [Chinese](https://mangopi.org/mangopi_mq)) are going to release a new board based on D1s, probably C906 core
        - [Sipeed LicheeRV, based on D1/C906, compute module style thing](https://twitter.com/SipeedIO/status/1456864395573673985) at $20?
        - C910 core with more power - [Android phone next year](https://twitter.com/SipeedIO/status/1457529282134089734)
    - Fiedka will also be presented at OSFC
    - Talk on TPMs this year
    - Remote attestation agent (re)written in Rust: https://github.com/keylime/rust-keylime
    - twelho: Been rethinking the three major issues/blockers:
        - RMC SBC form factor, availability, etc.
        - Backplane PCB to mount the RMC SBC, what kind of USB/Ethernet support should be present?
        - Cost (and component count) of the shields to mount on top of each compute node
- Major design update: shield, backplane, RMC
    - RMC SBC form factor
        - Bringing back an "old" idea:
            - Backplane is a simple USB hub / power distribution board
            - One compute node acts as the RMC
                - Attractive opetion since there are so many compute nodes
        - USB B port for upstream connection from RMC
            - Compatible with any existing SBC
    - Compute shield power delivery hat
        - Cost is an important factor
        - 5V DCDC converter for local step down
            - redundancy
            - lower transmission losses
            - complications
                - How to deal with variable input voltages?
                - Cannot support 12V SBCs
    - Using an ATX power supply
        - from around 50€ range
        - Voltage rails (3.3V, 5V, 12V, -12V, 5V standby)
            - 3.3V and 5V have a shared power budget
                - Current capability: 15A to 25A
            - 12V output can be used for 12V SBCs
                - 100s of watts of power
            - Pros
                - No need for custom DCDC converters
                - 5V standby (always on)
                    - typically between 2-3A (can go up to 5A)
                    - Can be used for powering RMC
                    - Short green wire to ground to turn on PSU
                    - Can be used to power a 5V eth switch (for always on network connectivity)
                - 12V line can be used for 12V SBCs/eth switches
                - Tray-level redundancy:
                    - One tray (set of 10 compute units with RMC) has one direct path to power
                    - In case of PSU failure only that set is lost
                    - Individual trays can be plugged into UPS devices
                - Can directly provide power to SATA hard drives
            - Cons
                - Cheat PSUs don't have power factor correction
                    - Large load on 12V line will rise 3.3V and 5V lines
                - RMC needs to run from 5V to have it always on
                - Lower power control granularity
        - Backplane looks like a fancy motherboard
            - compatible with standardized ATX connector
            - No (custom) SBCs, the backplane is only for power/USB
    - Required changes
        - Update compute hat - backplane connector
            - Add lines for 3.3V, 12V
        - RMC Linux Multi-function device
            - Mass device + ethernet
        - BMC choices?
            - STM32
            - STM32 with serial to uart
            - FPGA (with STM32)
            - Two STM32s connected together via high-speed UART?
    - General consensus positive, moving on with this design
        - Better documentation will come with the repo/org refactor

# October 25, 2021 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** October 25, 2021 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Verneri Hirvonen, @chiplet
    - Daniel Maslowski, @cyrevolt
- **Agenda:**
    1. Biweekly recap
    1. More thoughts about the BMC/RMC
    1. Nezha board update
    1. kicad-rs automatic component lookup and BOM generation
:::

## Notes
- Biweekly recap
    - RMC/BMC relationship, can we offload most of the BMC work to the RMC?
    - 2.5" and 3.5" hard drive mounts for the 9.5 or 10 inch rack
    - Custom RMC based on TI Sitara controller with real-time capabilities? Useful for u-bmc, would work for Racklet as well
        - Long term goal, all parts would be readily available but still more pricey than off-the-shelf parts
        - Ordering an evaluation board (TMDS64GPEVM) seems to be hard...
    - More community engagment around the Nehza D1 RISC-V board
- More thoughts about the BMC/RMC
    - If the RMC is more capable, most of the BMC's tasks could be offloaded to the RMC instead making the BMC firmware much more simple
        - Potentially allows us to downgrade the BMC µC, making it cheaper en masse
    - Off-the-shelf vs own design
        - Own design will be more expensive and take much more time to finalize but be tailored for the project
        - Off-the-shelf should be easily available and ideally not boot using blobs
    - Custom RMC based on the TI Sitara AM64x
        - The old 32-bit Sitaras are still being used by the OCP in their power controllers
        - Keep design simple
            - AM64X based small SBC with USB, Ethernet and serial only
            - Sitara CPUs are supported in mainline Linux
            - Boots blobless (apart from what runs in the Cortex-R5F?)
        - Solidrun devs: shouldn't be harder than any other custom design
            - They already got a SoM in the pipeline to be sold "soon"
            - Documentation is public even when you don't own it
    - u-bmc instead of OpenBMC as the RMC? It's not fully featured yet, so might be better to stick to the latter for now.
        - u-bmc planned to be usable for most tasks by ~Q3 2022
        - Porting OpenBMC can be a painful task, try finding a already supported board
- Nezha board update
    - Weekly community call on Wednesdays for now (8pm UTC at https://meet.jit.si/nezha-community)
        - smaeul who ported BSP patches for mainline Linux/U-Boot etc participated
    - The (RISC-V) cores of the Nehza D1 board are now open source!?
        - [T-head open-sourced](https://github.com/T-head-Semi) their
            - RISC-V cores, including C906 (on D1/Nezha), C910, and others
            - GCC toolchain with extensions
            - Yocto project
        - The C910 core is more powerful, but might be a bit expensive
        - Sipeed are also releasing [a compute module thing named Lichee-RV](https://twitter.com/SipeedIO/status/1443486484112183298) based on the D1
        - [RVB-ICE (C910)](https://www.t-head.cn/product/c910?lang=en) board has been on [presale](https://pt.aliexpress.com/item/1005003395978459.html)
    - Custom code compiled for the D1 is kind of working
        - Daniel has GPIO and UART working [based on bigmagic123's code](https://github.com/orangecms/d1-nezha-baremeta)
        - not yet in Rust / oreboot though objdumps look quite close already
    - Nezha UEFI boot flow also includes GRUB for some reason
    - U-Boot UEFI docs and reasoning is reasonable
        - easier to read than Tianocore/EDK2 docs
- kicad-rs automatic component lookup and BOM generation
    - At the end of the KiCad evaluation/parsing/classifying pipeline we get a categorized list of components in a schematic, using that information automatic lookup and BOM construction is possible
    - APIs
        - Digikey
            - OAuth 2.0 authorization flow aimed towards third party apps
                - example for integration in [Daniel's Coal Mines teaching project](https://github.com/coalmines/coalmines-api/blob/master/src/lib/auth.js)
            - Difficult to automate
                - Need to register a third-party application
                - Need to also authorize personal account to use the application
                - Hard to do in e.g. GitHub CI, needs a lot of secrets
            - Rate limited to 1000 requests/day and 120 requests/minute.
        - Octopart
            - API token
            - Many limitations
            - Quite a bit pricier
            - 20k/month limit doesn't scale to automation
        - Findchips?
            - No API
            - Provides supply chain "risk" scores
        - LCSC?
            - Component supplier for JLCPCB
                - Could enable easy ordering of assembled PCBs
            - Some sort of API exists
                - https://github.com/yaqwsx/jlcparts
                - https://yaqwsx.github.io/jlcparts/#/
                - https://github.com/cs2dsb/lcsc-scrape.rs
    - Using official API vs. scraping
        - No real need to authenticate if only looking for parts

# October 11, 2021 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** October 11, 2021 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @cyrevolt
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Biweekly recap
    1. U-Boot UEFI
    1. Physical dimensions of the "10 inch" rack format
    1. BMC-RMC communication (gRPC over USB)
    1. RMC considerations
:::

## Notes

- Biweekly recap
    - Marvin: TUF seems reasonable for the BMC, looking into it
    - Dennis: received a 10" audio rack tray, have ordered [a 6U 10" rack](https://www.thomann.de/gb/flyht_pro_stage_rack_95_6u_double_door.htm) (no idea when this is going to arrive)
    - Daniel: Fiedka website is now live, looked into UEFI on U-Boot
- U-Boot UEFI
    - Someone said UEFI is going to be dropped from U-Boot? (unconfirmed rumors)
    - UEFI interface is needed for ACPI on ARM
    - U-Boot supports TPMs (at least somewhat, provided you have working SPI)
        - Is this support wired up for UEFI as well?
    - For Racklet's case we don't necessarily need UEFI, but are the boards which need to pull it in due to ACPI?
    - ASpeed BMCs used to use U-Boot in u-bmc, but nowadays minimal custom bootloader
    - TF-A can be used to directly jump to the kernel and launch LinuxBoot
- Physical dimensions of the "10 inch" rack format
    - In the audio space this is called the 9.5 inch rack, and there are some dimension differences in comparison to the 10 inch "IT" rack spec
        - The IT rack measures 10 inches from ear to ear, while the audio rack is designed to fit between rails spaced 9.5 inches apart
        - The above means that the audio rack is roughly 10.7 inches wide when measured from ear to ear
        - This difference in dimensions will need to be taken into account when designing rack-mounted trays for Racklet, it might be best to standardize around the audio standard and provide adapters for the slightly narrower IT standard
        - The height of the rack modules, i.e. the rack unit, seems to thankfully be the same (based on [this 1U tray I ordered](https://www.thomann.de/fi/flyht_pro_rack_tray_1u_95.htm))
- BMC-RMC communication (gRPC over USB)
    - If we go for this route, Cloudflare's [quiche](https://github.com/cloudflare/quiche) would likely be the base library
    - There's a [QUIC working group](https://github.com/quicwg)
    - [related architecture and layering discussion from ASP.NET Core](https://github.com/dotnet/aspnetcore/issues/4772#issue-390516235) who would treat USB (or serial etc) as a transport
    - A more "standard" approach would maybe be to expose the BMC as an USB ethernet adapter, would this require writing a Linux driver or does a generic USB ethernet driver already exist upstream?
- RMC considerations
    - If we go with the 10 inch form factor, the "tray" that hosts many SBCs and slides into the rack could have its own RMC SBC that all BMCs plug into
    - The RMC could host all boot partition firmware in secure, authenticated memory, and through the USB Ethernet emulation the BMCs could "network mount" them
    - ARM dynamic root of trust? There's a [TPMConf talk about D-RTM on ARM](https://linuxplumbersconf.org/event/11/contributions/1122/attachments/881/1689/Dynamic%20Root%20of%20Trust%28D-RTM%29%20on%20non-x86%20architectures%20like%20ARM%20and%20POWER9.pdf).
    - [NanoPi NEO Core2](https://wiki.friendlyarm.com/wiki/index.php/NanoPi_NEO_Core2) might be a good RMC option, 3x USB and gigabit ethernet, barebones
        - https://datasheet.lcsc.com/lcsc/1810010421_Realtek-Semicon-RTL8211E-VB-CG_C90735.pdf
        - Should have upstream support according to [Armbian](https://www.armbian.com/nanopi-neo-core-2-lts/)
        - Challenge: The [H5 is allegedly out of production](https://forum.armbian.com/topic/13758-allwinner-h5-phased-out/) and this board can't be purchased anywhere anymore
    - There's also the H3-based alternative [NEO Core](https://wiki.friendlyarm.com/wiki/index.php/NanoPi_NEO_Core)
        - 100 Mbit/s ethernet, but pin compatible
    - Preferably the form factor should be quite LTS since the backplane would be designed around it. It's no good if we need to keep releasing new hardware to compensate for obscure SBC form factors going out of production.
    - Some alternative SBCs:
        - [Lichee Pi Zero](https://licheepizero.us/)
        - [ROCK Pi S](https://wiki.radxa.com/RockpiS)
        - [Orange Pi One Plus](http://www.orangepi.org/OrangePiOneplus/)
        - Potentially DIY Custom SBC? Accessibility becomes an issue.

# September 27, 2021 3:00 PM UTC

:::info
**No meeting**
:::

# September 13, 2021 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** September 13, 2021 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Verneri Hirvonen, @chiplet
    - Marvin Drees, @MDr164
    - Lucas Käldström, @luxas
- **Agenda:**
    1. Recap of the week
    1. ASpeed bootup process
    1. Quartz64 Model B
    1. Learning USB by porting a DFU bootloader to a 3D printer motherboard
    1. Changing the Racklet rack form factor to a 10 (or 9.5) inch rack?
:::

## Notes

- Recap of the week
    - Daniel contacted Realtek: they have a BMC-like management IC on a workstation MB (management software for Windows)
    - Marvin is the new maintainer of [u-bmc](https://github.com/u-root/u-bmc)
        - New build system, work on porting to RPi 4
        - Fixing up QEMU support
        - RPi Zero support would be interesting for Racklet
    - [UBIFS](https://en.wikipedia.org/wiki/UBIFS) for SPI flash
- ASpeed bootup process
    - Small amount of assembly
        - No binary blobs required
    - Verification by loader
    - Boot runtime kernel (GPG verified)
- Quartz64 Model B
    - https://www.pine64.org/quartz64b/
    - Looks promising, but doesn't ship yet as it's brand new
- Learning USB by porting a DFU bootloader to a 3D printer motherboard
    - DFU is a bit complex:
        - https://github.com/devanlai/dapboot/pull/41
        - https://github.com/devanlai/dapboot/pull/43
    - Learned a lot about the USB stack, and how unergonomic and unsafe C is
- Changing the Racklet rack form factor to a 10 (or 9.5) inch rack?
    - @twelho: it occurred to me that the "half rack" exists, more precisely it's the [10 inch rack](https://en.wikipedia.org/wiki/19-inch_rack#10-inch_rack) compared to the standard 19 inch width
    - The 10 inch rack standard has the same "rack unit" height as the 19 inch variant and the current plans for the custom Racklet rack
    - Some very well suited [casings](https://www.thomann.de/gb/all_racks.html?shp=eyJjb3VudHJ5IjoiZ2IiLCJjdXJyZW5jeSI6NCwibGFuZ3VhZ2UiOiJlbiJ9&feature-53839%5B0%5D=9%2C5%22&filter=true)
    - 10 inch hardware is [available](https://www.deltaco.fi/tuoteryhm%C3%A4t/10%EF%BC%82-19%EF%BC%82-tuotteet/kaappi-ja-telinetarvikkeet/hyllyt/10-33HYL) (site in Finnish)
    - ![10 inch rack dimensions](https://upload.wikimedia.org/wikipedia/commons/8/84/19_inch_vs_10_inch_rack_dimensions.svg)
    - Benefits:
        - Racklet tray designs could be printed using the most common 22x22 cm build volume present on most printers (e.g. Ender-3) and fit perfectly inside the rack
        - Raspberry Pi (or other computer) trays, HDD mounts, Ethernet switches, power strips, UPS devices and everything else following the standard can be mounted in the rack easily
        - Very high compute density can be achieved using a narrower variant of Ivan Kuleshov's design while preserving single-Pi hot-swappability:
            - ![Raspberry Pi Server Mark III](https://uplab.pro/wp-content/uploads/2020/12/UPTIME-Raspberry-Pi-Platform-pi18nossd-v1.0-1024x681.jpg)
        - Small rack designs can be [had quite affordably](https://www.bcedirect.co.uk/products/10-inch-rack-mount-8u-soho-wall-cabinet) or DIYed
            - ![10 inch rack example](https://cdn.shopify.com/s/files/1/2271/6867/products/8usohocabinetinside_1000x1000_crop_center@2x.png?v=1613077756)
            - ![10 inch rack for audio](https://thumbs.static-thomann.de/thumb/orig/pics/bdb/429742/13774186_800.webp)
        - Two 10 inch tray modules can potentially be slid into a 19 inch rack side-by-side allowing easier scalability
        - Avoiding the XKCD Standards scenario by not inventing a new "Racklet rack" standard
    - Drawbacks:
        - Larger size than the custom solution (at least four times the footprint), but still carryable
        - Backplane and BMC design needs to adapt
        - If opting to make a 1U Raspberry Pi tray, all Pis on that tray will be a single availability zone (not a problem for Ivan's design)
    - Neutral:
        - According to this [Reddit thread](https://www.reddit.com/r/homelab/comments/7lbvq2/noobie_question_time_the_10_rack/) four years ago these were still quite unknown, so they're a somewhat recent invention
    - Initial impressions for this change are positive

# September 7, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** September 7, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @cyrevolt
    - Lucas Käldström, @luxas
- **Agenda:**
    1. Recap of the week
    1. Upstreaming YAML work
:::

## Notes

- Recap of the week
    - Oxide is hosting a weekly [Twitter Spaces event](https://github.com/oxidecomputer/twitter-spaces) every Friday
        - For Finland this is sadly in the middle of the night (at 3 AM)
    - Marvin Drees from 9elements is working on [u-bmc](https://github.com/u-root/u-bmc), it might be relevant again as an modern OpenBMC alternative based on u-root
    - Announcement blog post coming up hopefully today
    - Lucas is taking a week off, expect a new blog post from him next week
- Upstreaming YAML work
    - Lucas will start working on upstreaming https://github.com/luxas/deklarative, which will take quite a bit of time
        - He's also writing a thesis about declarative schemas alongside this

# August 31, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** August 31, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @cyrevolt
- **Agenda:**
    1. Recap of the week
    1. Nezha update
    1. Adafruit HW bug & Logic level analyzers
    1. LFX mentorship ends -> Racklet work/meetings continue
:::

## Notes

- Recap of the week
    - Daniel is now a maintainer of https://github.com/9elements/tpmtool/
        - Related tooling:
            - https://github.com/google/go-attestation
            - https://github.com/Foxboron/go-uefi
    - Fiedka (standalone) is now running on desktop
    - Announcement blog post close to ready
    - USB stack now working on RTIC 0.6-alpha
    - [fwts](https://wiki.ubuntu.com/FirmwareTestSuite) for testing ACPI/UEFI compatibility
    - Raw tables are in `/sys/firmware/acpi/tables/`
    - x86 support for Racklet: there are x86 (Intel) SBCs with coreboot support, e.g. the [UP Squared](https://doc.coreboot.org/mainboard/up/squared/index.html)
- Nezha update
    - Work on fiddling with porting to oreboot from references and docs -> can now reset clock
- Adafruit HW bug & Logic level analyzers
    - The ItsyBitsy M4 has an incorrect SWDCLK pullup resistor causing trouble, see [here](https://github.com/racklet/bmc-experiments#psa-unstable-swd-debugging-in-both-openocd-and-probe-rs) for more details
    - We might want to get a logic analyzer for easier debugging of these kinds of issues:
        - Apparently cheap but good one: https://www.amazon.de/-/en/Saleae-Analyzer-8Channel-saleae-support/dp/B00DPCDEV2
        - Standard open source software for driving them: https://sigrok.org/
- LFX mentorship ends -> Racklet work/meetings continue
    - Mentorship officially ends today (August 31st)
    - Work on Racklet will continue (alongside studies of the core developers)
    - The current weekday/time works for the core team and Daniel, but I remember hearing about some other people's preferences to move it half an hour earlier
        - Next week's meeting will be at the same time for now until we have more information about people's schedules

# August 24, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** August 24, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @cyrevolt
    - Lucas Käldström, @luxas
- **Agenda:**
    1. Recap of the week
    1. Tracing and telemetry
    1. Embedded Rust BMC work updates
:::

## Notes

- Recap of the week
    - Lucas has real plumbers on their way to his house; so as a nice segway Daniel pointed out we could submit a talk to the Linux Plumbers Conference :D
    - We want to start writing some posts on the Racklet blog as the LFX term comes to an end
    - Daniel is working on Fiedka, it'll be interesting to see its visualizations (maybe we can also poke at the Raspberry Pi bootrom binary?)
- Tracing and telemetry
    - Lucas made some contributions to OpenTelemetry:
        - https://github.com/open-telemetry/opentelemetry-go/pull/2195
        - https://github.com/open-telemetry/opentelemetry-go/pull/2196
    - libgitops has eventually settled on using the builder pattern
    - Option pattern is kind of hard to extend or override
    - Go [contexts](https://pkg.go.dev/context) should be heavily used
- Embedded Rust BMC work updates
    - Two weeks of research into the state of the art embedded Rust development and debugging workflow has led to Racklet using [`probe-rs`](https://probe.rs/) and [`probe-run`](https://github.com/knurling-rs/probe-run) from the [knurling-rs](https://knurling.ferrous-systems.com/) project
    - `probe-rs` probe support is still a bit work-in-progress, using [dap42](https://github.com/devanlai/dap42) on an ST-LINK clone seems to be the best option and has worked reliably
        - There were many hardware hurdles in the way though, e.g. the ItsyBitsy M4 has an incorrect resistor which partially breaks SWD communication... blog post coming soon :D
    - We're switching to the latest alpha of [RTIC](https://rtic.rs/dev) since it's syntactically much nicer to work with
        - Some challenges with the embedded Rust USB stack lifetimes but otherwise it seems to be working
    - PRs in [bmc-experiments](https://github.com/racklet/bmc-experiments):
        - [In-depth README describing how to get started with the development workflow (and some pitfalls)](https://github.com/racklet/bmc-experiments/pull/3)
        - [`probe-rs` switchover configuration for the repository and examples](https://github.com/racklet/bmc-experiments/pull/4)

# August 17, 2021 3:30 PM UTC

:::info
**No meeting**
:::

# August 10, 2021 3:30 PM UTC

:::info
**No meeting**
:::

# August 3, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** August 3, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @cyrevolt
- **Agenda:**
    1. Recap of the week
    1. Embedded Rust + RTIC and USB
    1. Nehza debugging adventures
    1. KubeCon NA update
:::

## Notes

- Recap of the week
    - Last week: Embedded Rust USB experiments using ATSAMD51G (ItsyBitsy M4)
    - Lucas working on pitching and documenting libgitops
    - Nehza OpenOCD doesn't work
- Embedded Rust + RTIC and USB
    - [RTIC](https://rtic.rs/) is amazing, got interrupts and timers working
    - Next up working on getting USB CDC-ACM multi-serial and mass storage support working
        - Multiple instances of a USB class should technically be possible: https://github.com/r2axz/bluepill-serial-monster
        - The embedded Rust [USB stack](https://docs.rs/usb-device/0.2.8/usb_device/) is still a bit non-standard and at least preliminary tests couldn't get multi-class devices to work out of the box
    - All USB components we need techincally already implemented in an UF2 bootloader for STM32: https://github.com/cs2dsb/stm32-usb.rs
- Nehza debugging adventures
    - Chip documentation (memory mappings, etc.) proprietary and confidential :(
    - Memory dumps suggest it's running some modified u-boot with EFI support
    - Figuring out the right GDB and OpenOCD is confusing, apparently the ELF variant targets bare metal?
- KubeCon NA update
    - Our Racklet talk didn't sadly get accepted for KubeCon NA 2021
        - Overwhelming amount of submissions, only a third got in
    - Lucas still has another CFP queued for presenting on libgitops (one of Racklet's core components) in the colocated Cloud Native Conference event

# July 27, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** July 27, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @cyrevolt
    - Lucas Käldström, @luxas
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Recap of the week
    1. PCB update
    1. Similar / related projects
    1. libgitops framing update
    1. KiCad evaluator update
:::

## Notes

- Recap of the week
    - Finishing the all originally intended features of the KiCad evaluator
    - More CAD modeling work
    - libgitops and framing toolkit advances
    - PCB work
    - Nezha board arrived
        -  debugging with [JTAG on SD card pins](https://linux-sunxi.org/JTAG)
        - working on NAND support for [xfel](https://github.com/orangecms/xfel)
- PCB update
    - New USB hub chip with 8 ports
    - Potential problem: running out of power on the backplane DC-DC step-down
    - BD9E chip current limit: 3A (with over-current protection at 5.2A)
    - Power consumers:
        - BMC chips
        - USB hub chip
        - Fans
        - Backplane microcontroller / RMC controller
        - Network switch
    - 3A can technically be exceeded with everything running at full power simultaneously, but very unlikely scenario
    - We'll go with the BD9E for the prototype to check if it's enough, and do some changes if it's not
- Similar / related projects
    - https://github.com/xboot/xconch ([translated](https://translate.google.com/translate?hl=en&sl=zh-CN&u=https://github.com/xboot/xconch&prev=search&pto=aue))
        - Goal (as far as could be inferred): Design a standardized casing and connector for SBCs
        - Pretty comprehensive documentation of the work done, but repo seems to be stagnant right now
- libgitops framing update
    - Parsing/framing system now matches and even exceeds the features and robustness of k8s parsers etc.
    - YAML/JSON have a lot of nuances and implementation details, need to handle parsing bugs/vulnerabilities as well
    - YAML to JSON -> large values will overflow on 32-bit platforms
    - Strict parsing by default, not doing this will allow for a lot of user errors (typos, indentation)
    - Storage system builds on top of framing/parsing for interacting with git using a high-level interface
        - Implement support for semantic diffs for e.g. propagating specific staging changes to production
    - Full [OpenTelemetry](https://opentelemetry.io/) support in libgitops
- KiCad evaluator update
    - All intended features now implemented, model for globals implemented in [#25](https://github.com/racklet/electronics-prototyping/pull/25)
    - Pretty printing for value fields still coming as a late addition
    - KiCad hierarchical schematics can actually form a [DAG](https://en.wikipedia.org/wiki/Directed_acyclic_graph) with diamond inheritance
        - The kicad_rs pipeline has been made with a tree structure in mind, but does work with this model as well now (with some unnecessary recomputation)
        - Issue for converting the internal representation to a DAG [here](https://github.com/racklet/electronics-prototyping/issues/26)

# July 20, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** July 20, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Lucas Käldström, @luxas
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Recap of the week
    1. High-level goals for July and August
    1. Backplane MCU: ATMega vs STM32?
:::

## Notes

- Recap of the week
    - twelho:
        - More FreeCAD practice
        - Fixing up the KiCad evaluator now that Verneri is trying to use it
        - Found a special branch of FreeCAD (nicknamed LinkStage3) that fixes topological naming problem: https://github.com/realthunder/FreeCAD_assembly3/releases
            - Relevant issue for Flatpak packaging: https://github.com/realthunder/FreeCAD_assembly3/issues/671
            - LinkStage3 flatpak: https://github.com/astralaster/org.freecadweb.FreeCAD
            - Having used this for a bit I can say it is awesome
                - Enables really intuitive use of the Part Design workbench
                - Comes with a bunch of quality of life improvements
            - **But**: I won't use this for racklet, since it's not "official", a bit unknown and is pretty demanding to compile from source (6h with 12 cores)
                - These features are expected to land upstream though in the next couple of revisions of FreeCAD, so we'll stay tuned for that
    - chiplet:
        - USB hub schematic design with 4 downstream ports is mostly done
        - 3D printer is now also in usable condition for racklet prototyping
- High-level goals for July and August
    - twelho:
        - Get a basic rack modeled in FreeCAD
        - Finish the KiCad tooling to be usable for the backplane/BMC
        - Get the BMC firmware up so it can boot a compute unit
    - chiplet:
        - USB HUB on backplane + BMC prototype ready
        - Backplane header to USB port adapter (for debugging)
        - Port over the layer architecture and other graphics to DrawIO (diagrams.net) and use the CI tooling
    - luxas:
        - Bootloader environment MVP for the compute ready
        - YAML/JSON framing and content reading framework MVP
    - Order all electronics prototypes
    - Build automation / CI for the bootloader environment
    - Finish (at least) one more RFC
- Backplane MCU: ATMega vs STM32?
    - Pricing:
        - Firmware development is the most time consuming part of a microcontroller design
        - STM32 microcontrollers seem to be out of stock everywhere
            - STM32F103 is Cortex-M3
            - ATSAMD21G pretty much in the same situation, but only Cortex-M0+
            - [RP2040 Rust support](https://crates.io/search?q=rp2040) still questionable
            - NXP [i.MX RT MCUs](https://www.nxp.com/products/processors-and-microcontrollers/arm-microcontrollers/i-mx-rt-crossover-mcus:IMX-RT-SERIES) could techincall also be an option: https://crates.io/keywords/imxrt
        - One option is to buy Chinese development boards and transplant the chips into the prototype boards
            - Even the boards have gone up in price
            - Bulk orders from China seem to be in stock (e.g. from [alibaba.com](https://www.alibaba.com/product-detail/STM32F103C8T6-ARM-STM32-LQFP48-Microcontroller-STM32F103_62343619437.html?spm=a2700.galleryofferlist.normal_offer.d_title.7ea3565dIFT5mq))
- Or even an FPGA?
    - FPGAs seem to be much better stocked and low end ones are [reasonably priced]
    - It's possible to piece together Register Transfer Level (RTL) description for purpose build SoC for the BMC
    - Open toolchains ([yosys]) and gateware already exist
        - USB -- [LUNA]()
        - RISC-V Cores -- [picorv32], [rocket], ...
        - Cryptographic accelerators?
- [OpenTitan](https://opentitan.org/) open silicon root of trust
    - [GitHub](https://github.com/lowRISC/opentitan)

[reasonably priced]: https://www.digikey.fi/products/en/integrated-circuits-ics/embedded-fpgas-field-programmable-gate-array/696?k=ice40&k=&pkeyword=ice40&sv=0&pv7=2&pv7=17&sf=0&FV=-8%7C696&quantity=&ColumnSort=1000011&page=1&stock=1&nstock=1&pageSize=25
[yosys]: http://www.clifford.at/yosys/
[LUNA]: https://github.com/greatscottgadgets/luna
[picorv32]: https://github.com/cliffordwolf/picorv32
[rocket]: https://github.com/chipsalliance/rocket-chip


# July 13, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** July 13, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @cyrevolt
    - Lucas Käldström, @luxas
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Recap of the week
    1. SBC mainline support
    1. FreeCAD adventures
    1. Update from Lucas
:::

## Notes

- Recap of the week
    - Daniel:
        - Received Rock64
        - Golang WebAssembly experiments are already working with UTK
    - More exploration into the functionality FreeCAD
- SBC mainline support
    - Allwinner and potentially other SoC vendors don't have a great track record
        - Mainlining requires community reverse engineering
        - See [SBCs in 2021: The State of Play](https://www.youtube.com/watch?v=RcvMxC81r_g)
    - Limited resources to ensure Racklet compatibility of different boards
        - Focus on having a smaller subset working reliably instead of trying to maintain a subpar level of support for the widest variety possible
- FreeCAD adventures
    - Rack is getting assembled using the Assembly 4 workbench
        - Simpler than Assembly 3
        - Most importantly comes as a one-click install from the addon workbenches installer and doesn't require the external SolveSpace solver like Assembly 3
        - Assembly 4 is actually very versatile, and well enough for our needs
    - Figured out how to fuse objects across FreeCAD App::Link
        - Can now e.g. separate the tray and rails and fuse them together
        - Unblocks work on modeling the rest of the rack
    - Sidenote: Tried out FEM (finite element modeling) in FreeCAD
        - Works quite well, but still a bit of a learning curve and lack of tutorials
- Update from Lucas
    - Better YAML encode/decode work
        - No good "user interface" yet
        - "Rust revolution" of serializing/parsing due
        - Current methods don't adapt well to the cloud native landscape
    - UNIX pipe-style YAML/JSON processing pipeline
        - Enables e.g. easy defaulting
        - Automatic conversions and upgrades of objects
        - Flattening of nested lists
    - Framing stuff: https://github.com/weaveworks/libgitops/pull/48
    - Tracing included for free
    - People shouldn't be YAML engineers, most of the YAML should be declarative and automated

# July 6, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** July 6, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Lucas Käldström, @luxas
    - Daniel Maslowski, @cyrevolt
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Recap of the week
    1. YAML parsing is hard
    1. Unified tracing/logging platform
    1. Plans for the next weeks
    1. Go-based in-browser back-ends for web UIs (WASM)
    1. Modeling the rack with FreeCAD
    1. KiCad Rust tooling close to ready
    1. PCB design updates
:::

## Notes

- Recap of the week
    - Daniel: [Nezha](https://www.indiegogo.com/projects/nezha-your-first-64bit-risc-v-linux-sbc-for-iot#/) [board](https://bbs.huaweicloud.com/blogs/280384) [ordered/shipped](https://www.aliexpress.com/item/1005002668194142.html)
    - twelho: 3D printer kind of running now
    - Cheap bulk injection molding for Racklet racks in the future?
- YAML parsing is hard
    - No parser implementation is really spec-compliant (no matter if it's in Go or Rust)
    - Frame readers/writers in libgitops will help with this in Racklet
- Unified tracing/logging platform
    - Traditionally logging not consistent
    - Logs not machine-readable/friendly
    - Durations of logged events are missing (e.g. how long did parsing take)
    - [OpenTelemetry](https://opentelemetry.io/) aims to solve this
        - Core elements are [traces](https://opentelemetry.io/docs/concepts/data-sources/)
        - Visualization easy with [Jaeger](https://www.jaegertracing.io/)
        - [OpenTracing](https://opentracing.io/) enables reporting spans to Jaeger in a standardized and language independent way
- Plans for the next weeks
    - Get the CUE support for the KiCad tooling integrated
    - Initial CAD design of the rack to get the correct PCB measurements
    - Initial BMC firmware implementation (on STM32)
    - Start looking at automation for building the bootstrap environment firmware (for Raspberry Pis)
- Go-based in-browser back-ends for web UIs (WASM)
    - [webpack loader](https://github.com/orangecms/webpack-golang-wasm-async-loader)
    - example: [fmap](https://hostile.education/fmap) (adapted from [Fiano](https://github.com/linuxboot/fiano/tree/master/cmds/fmap))
- Modeling the rack with FreeCAD
    - [Topological Naming Problem](https://wiki.freecadweb.org/Topological_naming_problem)
    - [Assembly3](https://wiki.freecadweb.org/Assembly3_Workbench) vs. [Assembly4](https://wiki.freecadweb.org/Assembly4_Workbench)
    - Basic model targets good enough precision for figuring out the PCB sizes
- KiCad Rust tooling close to ready
    - Almost ready, one more feature needed for the evaluator (globals)
    - CUE support work-in-progress, will be merged soon
    - Bugs in libraries, need to fix field escaping in [kicad_parse_gen](https://github.com/productize/kicad-parse-gen/tree/kicad5)
- PCB design updates
    - Backplane ventilation holes added
        - Still missing USB hub, space is a bit constrained
    - USB MUX needed to switch between backplane/Pi boot USB on the BMC
        - Alternatively try to fetch a microcontroller with dual USB peripherals, but those are kind of expensive

# June 29, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** June 29, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Lucas Käldström, @luxas
    - Daniel Maslowski, @cyrevolt
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Recap of the week
    1. Racklet physical prototypes
    1. MaaXBoard, Nehza
    1. RFC-0002 final draft
    1. KiCad Rust tooling almost ready to roll
:::

## Notes

- Recap of the week
    - Midsummer week, a bit of a relaxation week for everyone
    - twelho: working on fixing the 3D printer and learning FreeCAD
- Racklet physical prototypes
    - HAT need to be longer to accommodate the 10 cm SSD
        - From around 6.5 cm to roughly 9.5~10cm
    - Backplane needs cooling holes for the fans to pull air through
    - Will do some rough CAD sketches of the rack this week to check fit and dimensions
    - Prototype order in when these changes have been implemented
- [MaaXBoard](https://www.avnet.com/wps/portal/us/products/new-product-introductions/npi/avnet-maaxboard/), [Nezha](https://www.indiegogo.com/projects/nezha-your-first-64bit-risc-v-linux-sbc-for-iot#/)
    - MaaXBoard SoC: [i.MX 8MQ](https://www.nxp.com/products/processors-and-microcontrollers/arm-processors/i-mx-applications-processors/i-mx-8-processors/i-mx-8m-family-armcortex-a53-cortex-m4-audio-voice-video:i.MX8M), 1.5 GHz Quad Arm Cortex-A53; Cortex-M4F, 2GB DDR4 SDRAM
    - Nezha SoC: [Allwinner D1](https://d1.docs.allwinnertech.com/d1_dev_en/), 1GHz RISC-V (XuanTie C906), DDR3 1GB/2GB
- RFC-0002 final draft
    - Racklet concepts explained: https://hackmd.io/HYXkw0gMTSia7-Da14GLpQ#Guide-level-explanation
    - Hope to get merged this week
- KiCad Rust tooling almost ready to roll
    - [CUElang](https://cuelang.org/) support coming
    - Two more things needed:
        - Saving to a Eeschema (KiCad schematic) file
        - "Global" variable support from component
    - "Tracking" issue: https://github.com/racklet/electronics-prototyping/pull/14

# June 22, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** June 22, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @cyrevolt
- **Agenda:**
    1. Recap of the week
    1. Khadas VIM1 board
    1. Learning/developing in Rust
    1. Short LFX update
    1. U-Boot Falcon Mode
:::

## Notes

- Recap of the week
    - It's warm in Finland, 30-35 degrees
        - Germany is cooling down
    - The team has mostly been working on the electronics tooling in Rust
- [Khadas VIM1](https://www.khadas.com/vim1) board
    - It has arrived :)
    - Browsing the [docs](https://docs.khadas.com/vim1)
    - Advertised for cluster use, but specs don't quite align
        - Not a lot of RAM
        - 100 Mbps ethernet
        - WiFi and GPU a bit unnecessary
    - There are quite some [other Amlogic SoCs](https://www.amlogic.com/Product/Stencil.html)
        - [Wikipedia](https://en.wikipedia.org/wiki/Amlogic) has a bit better formatted list
    - Test if kexec and TF-A work when time allows
        - Michael Stapelberg is [using it successfully on a router](https://twitter.com/zekjur/status/1406864269917003778)
- Learning/developing in Rust
    - twelho: Finally getting the hang of ownership and lifetimes
        - Still searching for the "idiomatic way" to do different things
        - Also still discovering new fancy language features
- Short LFX update
    - Synced with mentors last week (for the first time)
        - Everything is looking good, work continues
        - Did a little showcase of what we have right now and a rough plan for this and next week
- U-Boot [Falcon Mode](https://source.denx.de/u-boot/u-boot/-/blob/master/doc/README.falcon)
    - Got a [reply on U-Boot mailing list](https://lists.denx.de/pipermail/u-boot/2021-June/452036.html)
        - ARM64 boards not really designed for Linux in flash
        - But we have this graphic on LinuxBoot.org saying you can run Linux from U-Boot SPL
        - Should be technically possible through Falcon Mode, which just loads anything from SPL directly
        - Not yet implemented for Allwinner SoCs
            - No precise list of SoCs that are "Falcon aware"
        - Starting experiments

# June 15, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** June 15, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Lucas Käldström, @luxas
    - Daniel Maslowski, @cyrevolt
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Recap of the week
    1. Racklet hardware support
    1. RFC-0002 functionally ready
    1. Work on KiCad tooling in Rust
    1. OLinuXino/other boards updates
    1. LFX update
:::

## Notes

- Recap of the week
    - Need for handling component shortage
    - Writing more documentation, repository and organization management
    - Lucas did the ODSC presentation last Weds, went well
    - Draw.io integration is now working (good enough)
    - Writing tooling for KiCad and electronics orders
        - Need to get the first order for the prototype out this week
- Racklet hardware support/how to deal with different boards
    - ROCKPro64 (RK3399) for upstream coreboot experiments
    - TF-A assumed to be (mostly) available across the board
    - coreboot, U-Boot, RPi firmware etc. -> fragmentation
    - Use the u-root/[GoTEE](https://github.com/f-secure-foundry/GoTEE/wiki) trusted environment as the lowest common denominator
    - ROCK64 (non-Pro, RK3328) coreboot/U-Boot support?
        - U-Boot [has it](https://u-boot.readthedocs.io/en/latest/board/rockchip/rockchip.html)
        - coreboot [doesn't as of now](https://github.com/coreboot/coreboot/tree/master/src/soc/rockchip)
- RFC-0002 functionally ready
    - https://hackmd.io/HYXkw0gMTSia7-Da14GLpQ
    - Requires additional graphics to clarify the layers and holistic picture
    - Recommended reading page (links to good references)
- Work on KiCad tooling in Rust
    - https://github.com/racklet/electronics-prototyping/pulls
    - "Declarative electronics"?
    - https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md
    - BOM creation is error-prone and tedious, could that be automated?
    - KiCad uses S-EX for schematics
        - Extract component data to YAML
        - Query a distributor (e.g. DigiKey) using that data
        - Output an easy-to-understand Markdown document
    - Help users/developers unfamiliar with electronics
    - Speccing out components is very manual:
        - Traditionally: read a datasheet, compute a bunch of equations to figure out compnent values by hand
        - With the KiCad evaluator (ICCC): define expressions declaratively using the datasheet, the Rust binary will compute everything for you straight in your KiCad file
        - Pipe the results into the YAML parser etc.
    - KiCad doesn't really have APIs to process schematics nicely...
    - The Amp Hour episodes on component libraries and management thereof
        - https://theamphour.com/543-cassette-decks-have-browsers/
        - https://theamphour.com/542-component-management-with-jan-rychter/
- OLinuXino/other boards updates
    - Tracking notes and development at https://github.com/orangecms/arm-cpu/tree/main/olinuxino-a64
    - Signed up to the U-Boot mailing list, sent an issue report regarding build issues with the OLinuXino-A64, pending approval
        - Their GitLab does not allow public sign-up
    - Some U-Boot PRs on GitHub, which doesn't accept PRs...
    - Bought other boards:
        - [Khadas VIM1, Amlogic S905X](https://www.khadas.com/vim1)
        - [ROCK64, RK3328]( https://www.pine64.org/devices/single-board-computers/rock64/)
    - Looking at different platforms
        - Rockchip, Amlogic, Allwinner, Broadcom, STM32MP15x, i.MX?
        - Ron Minnich is working on GoTEE on i.MX6ULL
    - Figuring out which platform works best for Racklet
- LFX update
    - Three core (full-time) contributors this summer: Dennis, Lucas, Verneri
    - LFX meeting last week wasn't possible, so scheduled for this week
    - Right now working on what needs to be done in the short term, next week we might have already established some more long-term goals as well

# June 8, 2021 3:30 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** June 8, 2021 3:30 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Lucas Käldström, @luxas
    - Daniel Maslowski, @cyrevolt
    - Verneri Hirvonen, @chiplet
- **Agenda:**
    1. Recap of the week
    1. ODSC talk tomorrow (Lucas)
    1. PCB design update
    1. OLinuXino-A64 update
    1. LFX Update
    1. RFC-0002 work
:::

## Notes

- Recap of the week
    - Lucas & Dennis done with school :)
    - Lucas working on the ODSC talk
    - RFC-0001 updates: https://github.com/racklet/racklet/pull/22, https://github.com/racklet/racklet/pull/31
    - Found a way to make a HackMD team so that any Racklet team member can sync
    - RFC-0002 work: https://github.com/racklet/racklet/pull/20
    - Daniel played with the OLinuXino-A64
- ODSC (Open Data Science Conference) talk tomorrow
    - Lucas is about to [present at ODSC](https://odsc.com/speakers/exploring-modern-and-secure-operations-of-kubernetes-clusters-on-the-edge/)
    - [Link to slides](https://docs.google.com/presentation/d/1LIUEafHDGfKcCRMLo_6pHx4YDQ5RkmG8ufNrdZnq6bM/edit#slide=id.gdf274658c2_0_66)
- Architecture design
    - When talking about "GPIO pinout compatible", does it also support powering from GPIO?
    - We'll convert the preliminary architecture designs we had in Google Draw to draw.io/diagrams.net soon
    - Walkthrough of the architecture design a bit
- PCB design update
    - Verneri did a demo of the current state of the PCB schematics
    - Backplane prototype implemented (power only for now)
    - Backplane spaced using 1U for now, it fits well
    - PCB design is targeted at the HAT specification: https://github.com/raspberrypi/hats
    - Might be able to make "shims" in between for boards that don't implement the HAT standard
- OLinuXino-A64 update
    - Got TF-A working on it. Built u-boot for it. Managed to get to SPL working? Couldn't get SPI working in U-Boot after SPL, however.
    - Kconfig doesn't give guarantees for successful compilation
    - Will see if the upstream SPI support working, now fails compilation. There is a fork that is old and might work, but that is not optimal.
    - Live demo (interrupted by Phoenix/AMD quirks)
    - On-chip ROM init code (BL1 at EL3) -> u-boot SPL (BL2 at EL1) -> TF-A armstub (BL31 at EL3)
        - -> OP-TEE (or Rust/Go alternative) (BL32 at EL1)
        - -> "Raw" binary executable loaded to 0x0: u-boot "proper" (BL33 at EL2) -> Linux kernel (EL1)
    - https://wiki.st.com/stm32mpu/wiki/TF-A_overview#Architecture
    - If Linux is Bl33 directly, does it go up to EL1 by itself?
    - TPL is just SPL being divided into two parts due to space constraints on some powerpc platform. Not very relevant for us.
    - https://wiki.amarulasolutions.com/bsp/sunxi/a64/a64-oli.html
    - https://trustedfirmware-a.readthedocs.io/en/latest/plat/allwinner.html
    - https://trustedfirmware-a.readthedocs.io/en/latest/plat/index.html
    - Coreboot cache-as-ram: https://www.coreboot.org/images/6/6c/LBCar.pdf
    - https://qr.ae/pGYlxc
    - https://source.denx.de/u-boot/u-boot/-/blob/master/board/sunxi/README.sunxi64
    - https://www.denx.de/wiki/pub/U-Boot/MiniSummitELCE2013/tpl-presentation.pdf
    - https://stackoverflow.com/questions/31244862/what-is-the-use-of-spl-secondary-program-loader
    - https://github.com/ARM-software/u-boot/blob/master/doc/README.TPL
    - https://community.arm.com/developer/research/b/articles/posts/running-trusted-firmware-a-on-gem5
    - https://wiki.st.com/stm32mpu/wiki/STM32MPU_Embedded_Software_architecture_overview
    - https://stackoverflow.com/a/31252989 (what is what, terminology of stages from different views)
- LFX Update
    - The 4 "founders" Lucas, Dennis, Verneri and Jaakko will start this project this summer using the LFX program, and then following summers we'll see what the status is. The plan is to then involve many other people through LFX if possible next summer instead.
    - Motivated by that at the moment there is no clear "project" an external mentee could work on
    - Meeting with LFX mentors every Thursday
- RFC-0002 work
    - RFC-0001 improvements, ability to link to invididual values and user goals
    - RFC-0002 (component requirements) is taking shape, moving on to RFC-0004 (electical stuff) after that

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

# Racklet Community Meeting Notes 2022

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Monday at [4 PM UTC](https://dateful.com/convert/utc?t=4pm) (on odd weeks). Check the the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/GNrvcfXqSUaHeehBgfzTAg/badge)](https://hackmd.io/GNrvcfXqSUaHeehBgfzTAg)

[TOC]

# January 31, 2022 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** January 31, 2022 4:00 PM UTC
- **Host:**
- **Participants:**
:::

# January 17, 2022 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** January 17, 2022 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho 
    - Marvin Drees, @MDr164
    - Verneri Hirvonen, @chiplet
    - Daniel Maslowski, @orangecms
    - Michael Engel
:::

## Agenda/Notes

### Biweekly recap

- Michael Engel joined, welcome! :wave:
- More research into how to implement the SD protocol with PIO
- Marvin: Started working with the EOS S3 eFPGA and refreshed my VHDL/Verilog knowledge
- Marvin: Used the same custom tooling to provision a fleet of servers (system-transparency) for u-bmc (aka progress in tooling)
- Marvin: Continued working on baremetal Golang for ASpeed (finally got a NDA with ASpeed as well)
- Daniel: WIP porting D1 SPL0 to Rust :crab:

### Refactoring the repositories

- Cleaning up unneeded forks and other cruft in the [Racklet GH organization](https://github.com/racklet/)
- Preparing to move to separate tagged repositories for hardware, electronics, firmware etc.
    - All of these will be joined in tagged releases under the main repo
- ETA for refactor completion is this week

### Progress with SD emulation

- SD_Device needs some changes to support synthesis for ICE40
    - Initially designed for Xilinx FPGAs
    - Original code has Xilinx primitive instantiations e.g. for IO buffers
    - ICE40 doesn't support asynchronous resets
- Figured out bidirectional pins
    - SB_IO is a bit different than IOBUF (seems like output enable is inverted)
    - Inference works well on yosys
- Can already see valid command/response traffic on logic analyzer
- There are some timing violations on internal paths (40MHz instead of 50MHz)
- Shows up as a block device on linux!
    - ... but reads fail with an IO error

### Nezha News

- support for LinuxBoot with RustSBI now in upstream `main`
- can boot from SD card using the stock SPL/Boot0, though it makes some changes keeping Linux from coming up - reworking thusly
    - https://github.com/smaeul/sun20i_d1_spl
- got two RV Docks from Sipeed and the 86 Panel
    - https://linux-sunxi.org/Sipeed_Lichee_RV

### Misc

- KiCad 6 was released, looks very nice!
    - A lot of papercuts were fixed
    - We need to update the [`kicad-rs`](https://github.com/racklet/kicad-rs) tooling now
- Daniel: got a TV box based on the S905X4 - dev board now, UART is accessible :)
- Daniel: ordered a [MakerCase v2](https://www.conrad.de/de/p/joy-it-makercase-v2-sbc-gehaeuse-passend-fuer-entwicklungskits-raspberry-pi-acrylglas-klar-1720608.html) (multi-SBC stacking thingy) (variants for 2 and 8 boards exist, looks like they can be combined arbitrarily)
    - ![MakerCase V2](https://asset.conrad.com/media10/isa/160267/c1/-/de/1720608_AB_00_FB/joy-it-makercase-v2-sbc-gehaeuse-passend-fuer-entwicklungskits-raspberry-pi-acrylglas-klar.jpg?x=500&y=500&format=jpg&ex=500&ey=500&align=center)

# January 3, 2022 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** January 3, 2022 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho 
    - Marvin Drees, @MDr164
    - Verneri Hirvonen, @chiplet
    - Daniel Maslowski, @orangecms
:::

## Agenda/Notes

### Biweekly recap

- It's a new year, and with that comes an increase in the effort put into Racklet
    - twelho: Multiple professors at Aalto are interested in Racklet, I've had and have upcoming meetings then to give a more in-depth perspective
        - One of the professors recommended starting a European/Finnish equivalent of a Foundation, this would help with e.g. companies wanting to fund resources/hardware
            - This is going to probably happen a bit later in the year
            - Daniel: [Michael Engel](https://www.ntnu.edu/employees/michael.engel) might be intersted as well, got to know him through D1/Nezha community, will reach out and ask
        - Work on establishing the OSF foundation deals with the same legal stuff https://opensourcefirmware.foundation/
- Holidays and vacationing
- In the coming weeks: upstreaming u-bmc work and writing documentation
- Progress on FPGA-based SD card emulation
- Docker without the daemon: https://github.com/ghedo/pflask
    - Go implementation: https://github.com/u-root/u-root/tree/main/cmds/exp/pflask

### Logic analyzer update

- twelho: Have been playing around with the DSLogic Plus, got it to successfully decode WS2812B (neopixel) commands right away
    - It feels reliable enough for debugging SD cards, and also the sampling does actually go up to the required sample rates even with the kind of cheap integrated probes
    - Will test more protocols and actually attempt to debug an SD card in the coming weeks
- Verneri:
    - SPI flash probing and decoding also works

### Nezha News

- LinuxBoot up and running
- can boot into OpenWrt and openSUSE
- oreboot used in [advertisements](https://twitter.com/OrangeCMS/status/1477670720742334476) already
- [`cpu`](https://linuxboot.github.io/book/cpu/) works
- little [testing tool](https://github.com/orangecms/hello-linux-rv64-rs) to print CPU, memory etc info
- OpenXiangShan will have the second high-performance RISC-V core soon (Nanhu) 
    - https://github.com/OpenXiangShan/XiangShan
    - https://www.youtube.com/watch?v=LSiBKxoszz4

### New SBCs and embedded boards

- Daniel: more Lichee RV D1 modules and carrier boards, [NanoPi NEO (Allwinner H3)](http://nanopi.io/nanopi-neo.html) / https://www.friendlyarm.com/index.php?route=product/product&path=69&product_id=132
- Marvin: [QuickLogic ThingPlus EOS S3](https://www.sparkfun.com/products/17273) for use with [Amaranth](https://github.com/amaranth-lang/amaranth), [USB armory](https://github.com/f-secure-foundry/usbarmory) for [Tamago](https://github.com/f-secure-foundry/tamago)
    - More about the [EOS S3](https://www.quicklogic.com/products/soc/) in the Thing Plus
- Dennis & Verneri: 3x [Odroid-C4](https://www.hardkernel.com/shop/odroid-c4/)
    - Amlogic S905X3 (12 nm)
        - Boots by loading `u-boot.bin`
    - Raspberry Pi form factor
    - DDR4 memory (4 GiB)
    - Daniel has a Khadas VIM1 (S905X)
        - https://www.khadas.com/vim
        - https://gadgetversus.com/processor/amlogic-s905x-vs-amlogic-s905x3/

### Progress on FPGA-based SD card emulation
  - Tested to be working minimal "boot" image (820 kB)
    - Only boots to the rainbow screen, but sufficient for testing SD emulation
  - Breadboard prototype with:
    - [TinyFPGA BX](https://tinyfpga.com/)
    - SPI flash with boot image flashed
    - connector to SD card break
  - [FuseSoC](http://fusesoc.net/)+[Yosys](https://yosyshq.net/yosys/)+[nextpnr](https://github.com/YosysHQ/nextpnr) based flow for simulation, and synthesis and pnr (place'n'route)
    - Working
      - ad-hoc SPI flash controller that can already read the boot blob from SPI flash (verified on hardware with pulseview and DSLogic Plus)
    - TODO:
      - [Wishbone](https://en.wikipedia.org/wiki/Wishbone_(computer_bus)) interfaces between project value SD emulator and SPI flash

### Racklet roadmap 2022

- Target: working BMC enabled boot of a compute unit for OSFC 2022
    - Research and develop firmware for Allwinner, Amlogic
        - Currently most suitable SBCs:
            - Odroid [Odroid-C4](https://www.hardkernel.com/shop/odroid-c4/)
            - Pine64 [Quartz64 Model B](https://www.pine64.org/quartz64b/)
            - Pine64 [Rock64](https://www.pine64.org/devices/single-board-computers/rock64/)
            - Khadas [VIM1](https://www.khadas.com/vim1)
            - FriendlyARM [NanoPi NEO](http://nanopi.io/nanopi-neo.html)
            - Radxa [ROCK3](https://wiki.radxa.com/Rock3)
- During spring: Get SD card emulation working (at least reads)
- Refactor website, documentation and repositories
    - Move from an RFC-based model to tagged releases
    - More automation and easier maintenance
- Establish a foundation for Racklet
    - This will be done a bit later in the year

# Racklet Community Meeting Notes 2022

This document contains the notes of the [Racklet](https://github.com/racklet/) community meeting. The meeting occurs every other Monday at [3 PM UTC](https://dateful.com/convert/utc?t=3pm) (on odd weeks). Check the the [#racklet](https://osfw.slack.com/messages/racklet/) channel on the OSFW Slack for more info.

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/GNrvcfXqSUaHeehBgfzTAg/badge)](https://hackmd.io/GNrvcfXqSUaHeehBgfzTAg)

[TOC]

# August 1, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** August 1, 2022 3:00 PM UTC
- **Host:**
- **Participants:**
:::

# July 18, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** July 18, 2022 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
    - Verneri Hirvonen, @chiplet
:::

## Agenda/Notes

### Biweekly recap

- Dennis:
    - Still working on DHCP fixes for Talos Linux, but it's finally getting there in [PR #5897](https://github.com/siderolabs/talos/pull/5897)
        - Depends on some fixes to the underlying DHCP library
        - Different DHCP server implementations are quite quirky, coming up with a generic solution for bidirectional hostname passing was not easy
    - That de-facto DHCP library used by Go projects is not the most elegant to work with, see lengthy discussion with the maintainer in [PR #469](https://github.com/insomniacslk/dhcp/pull/469)
        - There are a bunch of PRs just lying around
        - Will need to submit another one still that implements easy access to DHCPINFORM
    - Other misc. things: Fixed a 1541 floppy disk drive over the weekend, it had a broken 6502 CPU :D Wrote some debug tooling:
        - https://github.com/twelho/sram-tester
        - https://github.com/twelho/rom-tester
    - [Kestrel SoftBMC](https://gitlab.raptorengineering.com/kestrel-collaboration/kestrel-litex/litex-boards/-/tree/master)
- Marvin:
    - Successfully installed a Talos Cluster with last time discussed fixes
    - Got hold of even more BMC cards (including AST2600) to try software on
    - Continued general development on u-bmc
        - Currently making more things more configurable (e.g. no `sh` mode like Talos has)
        - Integrated Generics and Structs into the RPC API in the [schema repo](https://github.com/u-bmc/schema/blob/988dc77fe18541e8eca79364cedc0147d32c8f8b/bmcapis/management/v1alpha1/management.proto)
        - OpenTelemetry on hold until their metrics interface is out of alpha (for now the full OpenMetrics implementation works)
- Daniel
    - Got memory mapping to work on the [JH7100](https://starfivetech.com/en/site/soc) (VisionFive) \o/
        - [Documented setup for booting via mask ROM from SRAM + continue via flash up into U-Boot](https://github.com/oreboot/oreboot/pull/587)
        - Hoping that we can reuse code for the upcoming JH7110 (not many details public yet, marketing calls it a "quad core upgrade" compare to JH7100)
    - Started [u-apps for platform exploration](https://github.com/platform-system-interface/u-apps) based on [Bubble Tea 🧋](https://github.com/charmbracelet/bubbletea)
        - Drafts: `msrexplore` for x86 MSRs, `teaboot` as a fancier boot menu
        - TBD: `csrexplore` and ARM counterparts
    - Getting [Wi-Fi to work with the D1](https://github.com/lwfinger/rtl8723ds) (MangoPi and Lichee RV Dock), will attend a D1 hack week next week

# July 4, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** July 4, 2022 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
:::

## Agenda/Notes

### Biweekly recap

- Pine64 [launches in the EU](https://pine64eu.com/)! :tada:
    - Prices are a bit high though, we'll see how this develops
- Marvin
    - Installed a Talos Cluster in Hetzner
        - Ephemeral encryption seems broken...
    - Researched different ways to publish BMC interfaces via gRPC
        - [Protobuf Structs](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Struct)
        - [Protobuf Generics](https://developers.google.com/protocol-buffers/docs/reference/google.protobuf#google.protobuf.Any)
    - Got new BMC cards with AST2400, AST2600 and AST2150 in addition to the AST2500
- Daniel
    - WIP oreboot build system changes for full port build with all stages
    - Based on Luo Jia's work, already got [bt0+main for sunxi/nezha](https://github.com/oreboot/oreboot/pull/590)
    - Got so many D1 boards now, lost track of what I already shared
    - 4 [D1 board pages in the wiki](https://linux-sunxi.org/Category:D1_Boards) now (ClockworkPi CM tbd)
    - People from the [Anachro community](https://anachro.computer/) are planning a D1 hack week in Berlin July 25-31
    - Embedded World was fun \o/
    - First Rust :crab: code running on the JH7100 / VisionFive
- Dennis
    - Bugfixing and improving Talos at work, PRs coming soon!
        - Implemented DHCP hostname registration
        - Fixed setting `talos.platform` via `extraKernelArgs`
    - Worked on a new tool last week for resetting Epson printers, will be published this week
    - [Raspberry Pi Pico W](https://www.raspberrypi.com/news/raspberry-pi-pico-w-your-6-iot-platform/) released, might be interesting with its WiFi+BT+BLE capabilities
        - ESP32 rival with essentially a much better software ecosystem
    - Pico (RP2040) as a logic analyzer: https://hackaday.com/2022/03/02/need-a-logic-analyzer-use-your-pico/
        - Need to research the code to see how synchronization and data streaming is implemented

# June 20, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** June 20, 2022 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
    - Verneri Hirvonen, @chiplet
:::

## Agenda/Notes

### Biweekly recap

- Dennis
    - Discovering new K8s distros for Racklet
        - Originally intended to use k3s + k3OS for "immutability" and declarative configuration/upgrades
        - Now discovered [Talos Linux](https://www.talos.dev/), which basically does everything that k3OS does, but better
- Daniel
    - Got [boot from (NOR) flash + DRAM init + load from flash to RAM](https://github.com/orangecms/test-d1-flash-bare/tree/dram-rerere) all set on D1
    - See an [awesome demo](https://asciinema.org/a/502138) :tada: 
    - https://github.com/oreboot/oreboot/pull/583 documents the boot flow
    - https://github.com/oreboot/oreboot/pull/585 by Luo Jia introduces a build system for a full image
    - Distinguishing feature: First firmware with emoji on serial :crab: 
    - Booting into LinuxBoot with u-root fine so far, but hitting space limit
    - WIP decompression in oreboot; kernel+initramfs can be shrunk from ~16MB to ~6MB
    - Will be at [**Embedded World**](https://www.embedded-world.de/en) :wave:
- Verneri
    - PMOD boards arrived
        - Probing board works very well
        - Heisenbug!
            - SD traffic works reliably *only* while being probed
            - Hypothesis: the driver has very fast rise times, and the probe loading limits the rise time.
            - TODO: Make a SD to PMOD breakout with series resistors to experiment with rise times
    - SD emulation
        - Got the project vault SD card to SPI flash configuration working on the [Zybo Z7-10](https://digilent.com/reference/programmable-logic/zybo-z7/start)
        - Reads from SD card work fine over a USB adapter with test data
            - However, with real data (FAT partition table) the host flips out, resets and locks up the design :thinking_face:
- Marvin
    - Finished u-bmc bootloader for RPi
    - Advanced further on the u-bmc project in general
    - Handed in a CFP for this years OCP Global Summit
    - Started on a RISC-V design for a RMC card (larger scale than Racklet RMC but likely similar functionality minus SD emulation etc.)

### Talos Linux

- Racklet needs a lightweight primary K8s operating system to run on the nodes
- Talos Linux delivers:
    - amd64 and aarch64 support
    - Full immutability
    - Runs from RAM
    - Support for node-attached disks using Rook (Cephfs)
    - **[WireGuard](https://www.wireguard.com/) VPN between nodes** with automatic setup
    - (Relatively) [low system requirements](https://www.talos.dev/v1.0/introduction/system-requirements/)
    - Easy [kernel](https://www.talos.dev/v1.0/advanced/customizing-the-kernel/) and [rootfs](https://www.talos.dev/v1.0/advanced/customizing-the-root-filesystem/) customization using containerized builds
    - `talosctl` to easily provision and upgrade clusters
    - Full mTLS GRPC between the cluster and `talosctl`
    - Separate kernel + `vmlinuz` builds
    - No `systemd`, `OpenRC` or anything else, just a lightweight Golang-based init
- This checks a lot of the boxes for the security, ease of use and customizability of Racklet

# June 6, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** June 6, 2022 3:00 PM UTC
- **Host:** N/A
- **Participants:** N/A
:::

## Agenda/Notes

### Biweekly recap

- Marvin
    - Assembled RPi test fixture (3B+ connect to Pico via Grove cables)
    - Switched over from protoc and gRPC to [Connect](https://connect.build) and [Buf](https://buf.build)
        - Connect reduced dependencies and is easier to use than gRPC directly (gRPC compatibility given)
        - Downside: Only Go support as of now (TS/JS planned)
    - Continued advancing code-base in preparation of next Fridays Hackathon
- Verneri
    - Received SD to PMOD and Pmod probe PCBs
    - Did some more experimentation with the Pi 4 SD interface
        - Looks like boot sequence frequency also depends on Pi 4 bootloader firmware version
        - bootloader version `507b2360 2022/04/26` clocks SD card at:
            - 160kHz enumeration, **33.33 MHz** operation independent of the value of `TRAN_SPEED` in the CSD register. :(

# May 23, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** May 23, 2022 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
:::

## Agenda/Notes

### Biweekly recap

- Verneri
  - Managed to find a PMOD port on ULX3S that has clock capable input on same pin as on Zybo Z7-10
  - Ordered test boards, should arrive in about a week
    - PMOD logic analyzer breakout
    - microSD to PMOD adapter
- Dennis
    - Rich answered after many months to my question about licensing, we're now discussing future outlooks and use cases to determine the proper license for the repo refactor
    - Found time to dig deeper into SD emulation, figured out where the latency comes from
- Marvin
    - Got Seeed Grove HATs for Raspberry Pi test fixture
    - Still busy with moving but continued on ubmc work on the Pi (spoilers ahead: Made a CFP for the next OSFC about it)
- Daniel
    - Got a VisionFive from RISC-V Foundation eventually
    - Got even more D1 boards \o/
        - https://github.com/riktw/jeadock
        - https://www.analoglamb.com/product/dev-lnx0004-allwinner-d1-risc-v64-development-board-dongshan-neza-stu/
    - D1 DRAM init slooowly progressing :snail: 

### SD emulation updates

- Tried different PIO programs to cut down on the ~5 clock cycle latency
    - This didn't help, manual JMPs are just as slow as the WAIT instruction
    - (There also is no one-instruction move from an input to an output)
- Schmitt triggers and slew control don't do anything for PIO controlled pins
- Overclocking adventures
    - Overclocking to 250 MHz works out of the box by adjusting clock dividers, but any faster will cause panics and flash read errors
        - Core voltage needs to be bumped and flash clock divider upped from 2 to 4
- **Latency is directly correlated with clock speed**
    - This means that the signal quality is likely sufficiently good and there is no major impedance mismatch etc.
    - The source of the latency is the *APB Bridge* between the I/O and PIO, which does buffering to prioritize bandwidth over latency. From Chapter *2.1.3. APB Bridge* in the RP2040 Datasheet:
        - APB bus accesses take two cycles minimum (setup phase and access phase)
        - The bridge adds an additional cycle to read accesses, as the bus request and response are registered
        - The bridge adds two additional cycles to write accesses, as the APB setup phase can not begin until the AHB-Lite write data is valid
- The latency can be compensated for by storing and looking up the sampled values 5 cycles ago
- Now that the PIO approach is proven possible, I'll move onto implementing the actual SD state machine and functionality

# May 9, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** May 9, 2022 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
    - Verneri Hirvonen, @chiplet
:::

## Agenda/Notes

### Biweekly recap

- Dennis:
    - Received an apartment on-campus and moving has taken a lot of time, no progress on Racklet stuff yet
    - Starting my summer job tomorrow, Kubernetes stuff :)
- Marvin:
    - Moving as well, so equally less time to continue hacking
    - Continued a bit work on u-bmcs main operator process
- Daniel:
    - Got a [MangoPi MQ-Pro](https://mangopi.cc/mangopi_mqpro)
    - We're removing FSP stuff from oreboot
    - D1 will soon be the first and only fully open oreboot + Linux SoC
    - ESP32 input turned out to be an upstream issue, errata were applied to the HAL, works fine now :)

### Verneri's PMOD probing board

- PMOD [page](https://digilent.com/reference/pmod/start) and [specification](https://digilent.com/reference/_media/reference/pmod/pmod-interface-specification-1_3_1.pdf)
- https://github.com/chiplet/pmod-probe
- In his own words: "SD card to PMOD adapter for FPGA emulation"
- Goals:
    - Better signal integrity
    - Cleaner probe wiring for logic analyzers etc.
- Existing solutions from e.g. [Digilent](https://digilent.com/shop/pmod-tph2-12-pin-test-point-header/) don't have enough ground connections for logic analyzer inputs

### Nezha News

We can boot from SPI flash \o/
- GPIOs work
- Serial works; we can print "Rust :crab:"
- DRAM init is WIP
- Reading from flash to RAM is also WIP

# April 25, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** April 25, 2022 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
    - Verneri Hirvonen, @chiplet
:::

## Agenda/Notes

### Biweekly recap

- Dennis: Final course work still preventing larger work on Racklet, but managed to do some more SD card emulation tests and discoveries for my BSc thesis
- Daniel: Got more Lichee RV (D1) boards, added SPI flash chips, went further with Rust embedded book, PAC and HAL, interrupts, and ESP32 (not only for exercise)
- Marvin:
    - Continued work on RPi Pico hardware sim in u-bmc org (repo will be called u-sim)
    - Work on u-bmc itself continued. Base architecture redesign finished. Hardware target will first be all regular Raspberry Pis after ASPeed SoCs and Nuvoton will be (re)added.

### SD card emulation discoveries

- The Raspberry Pi bootloader does **not** clock the SD bus at the maximum speed of 25 MHz
    - Pi 3B+ clocks: 128 kHz enumeration, 10 MHz operation
    - Pi 4 clocks: 200 kHz enumeration, **13.33 MHz** operation
- With the simple wait-set loop in PIO assembly on the RP2040 10 MHz is barely sustainable, but 13.33 MHz is simply too fast (at least at the stock core clock of 125 MHz)
    - I really need an oscilloscope to inspect the analog signal quality/integrity, might be an impedance issue
- Next steps for exploration:
    - Overclocking (pretty trivial, but the signal degradation might be too significant)
    - Faster polling: would it be possible to unconditionally sample on each cycle without waiting?

### Nezha News

- Work on D1 init is kicked off now
    - https://github.com/luojia65/test-d1-flash-bare/
    - https://github.com/luojia65/xuantie
    - https://github.com/luojia65/D1-D1s-ROM-reverse
- Secure environments for RISC-V / RustSBI
    - Hypervisor: https://github.com/luojia65/zihai
    - Enclave for S and M mode is planned
- HDMI / Linux 5.18 working :)
    - gist updated https://gist.github.com/orangecms/df0d3c15f9e32b5c708c021c4be8857b
- Another Lichee RV carrier board
    - https://github.com/riktw/jeadock

### oreboot

- Now based on a Cargo workspace layout
    - Increased build speed, ca 40%
- Intention to rework towards PAC and HAL structure
    - PAC: yay!
    - HAL: nay? Some issues with memory management, gotta check

# April 11, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** April 11, 2022 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
:::

## Agenda/Notes

### Biweekly recap

- Dennis: School work keeping me busy, but should be able to proceed with the SD card emulation and repository refactor next week
- Marvin: Received a bunch of Pi Picos

### RISC-V / SBC News

- New boards will follow soon
    - [MangoPi](https://mangopi.cc/) ([GitHub](https://github.com/mangopi-sbc/MQ-Pro)) ([review](https://www.cnx-software.com/2022/04/09/mangopi-mq-pro-a-20-risc-v-alternative-to-raspberry-pi-zero-w/)), clone of the RPi Zero 2 W, but with D1 and USB-C
    - Sipeed with a new chip, maybe shipping with oreboot already
    - D1 HDMI now works on 5.18 RC, smaeul's mainline WIP branch
        - https://github.com/smaeul/linux/tree/riscv/d1-wip
        - Daniel is trying to get it to work again with oreboot :grimacing: 
            - https://github.com/orangecms/linux/tree/5.18-riscv-debug

### oreboot

We're getting some structure, closer to other projects, with a new module/crate structure and Cargo workspace setup.

# March 28, 2022 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** March 28, 2022 3:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
    - Verneri Hirvonen, @chiplet
:::

## Agenda/Notes

### Biweekly recap

- Dennis: Finally received the 10" rack casing I ordered half a year ago, yay!
    - Already mesured a bunch of dimensions, looks like solid choice
- Daniel:
    - Got a WizNet W5500 controller working on the Lichee RV Dock
        - [added DTS to Linux fork](https://github.com/orangecms/linux/commit/c180cf125a825fd3d03b048640403d2c4e968134)
    - oreboot work: [RustSBI author contributing to move to using features over many crates and clean up structure and tooling](https://github.com/oreboot/oreboot/pull/553)
    - New [Fiedka release v1.3.4](https://github.com/fiedka/fiedka/releases/tag/v1.3.4)
        - CBFS (coreboot filesystem) support
        - Support for DXE removal and BdsDxe replacement (e.g. inserting LinuxBoot)
        - nicer layout with a feedback section and outline (currently for AMD only)
        - someone already used it to deploy LinuxBoot on real hardware
- Marvin:
    - u-bmc log rotation and TOML
- Verneri:
    - Vacationing and computer upgrading
    - Probing the SD signal to see the signal quality
    - Received a ULX3S, will continue SD emulation with that

### The Flyht 10 inch Rack Casing

- Dennis: I finally received the [Flyht Pro Stage Rack 9,5" 6U Double Door](https://www.thomann.de/fi/flyht_pro_stage_rack_95_6u_double_door.htm) after half a year of shortage delays
- 6U as tall as they go at least in this series but it should be enough
- The price, dimensions, and robustness are quite well suited for use as a Racklet rack
- Here's a picture of what it looks like: ![The Flyht Pro Stage Rack 9,5" 6U Double Door](https://thumbs.static-thomann.de/thumb/padthumb600x600/pics/bdb/429743/13275761_800.jpg)
- The 10 inch for factor dimensions are finally clear:
    - The rack unit is **1.75 inches** (the same as for 19 inch racks)
    - 9.5 inches is the **maximum device width** that will fit in the rack
    - 10 inches is the distance between the **mounting screws/holes**
        - While the 19 inch rack width includes the ears and vertical rails, 10 inch rack equipment is actually wider than 10 inches
- This rack has detachable doors on the front and back, they are approx. 6 cm deep and symmetrical (with the exception of the handle)
- The main body is approx. 25 cm deep and has 2 cm inset vertical rails on the front and back: ![Doors open](https://thumbs.static-thomann.de/thumb/padthumb600x600/pics/bdb/429743/13275791_800.jpg)
    - Usable depth for equipment is specified as 20.8 cm

# March 14, 2022 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** March 14, 2022 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
    - Marvin Drees, @MDr164
:::

## Agenda/Notes

### Biweekly recap

- Dennis:
    - Contacted Rich wrt. relicensing, do want to hear his feedback
    - RP2040 software SD probing quite reliable, moving on to data capture and state machine implementation next
- Verneri is having signal integrity issues with the FPGA SD emulation
    - Will make a new adapter and probing boards for more reliable results
- Marvin: TinyGo project for RP2040
    - Need to emulate some devices: PWM, I2C, SM/PMBus
    - E.g. fan emulation
    - TinyGo RP2040 support: PWM and I2C works, PIO support seems to also be in [place](https://github.com/kenbell/tinygo-rp2040-pio-example/blob/main/main.go)
- Daniel: Gave a talk on driver development via `cpu` at CLT
    - https://github.com/u-root/cpu
    - https://metaspora.org/drivers-from-outer-space.pdf
    - https://chemnitzer.linux-tage.de/2022/de/programm/beitrag/226
    - we're working on a second implementation in Rust :)
    - `cpu` is getting some more fixes in addition
      - e.g. so you can `echo hello | cpu remote tee /foo/bar`
      - an improvement over the original!
    - and port to FreeBSD is also WIP; maybe Redox will be next then

### Raspberry Pi 4 EEPROM Write Protection

- It's possible to officially enable write protection for all EEPROMs (bootloader and USB firmware): https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#eeprom-write-protect
    - Requires the hardware write protect pin to be pulled low, this is achieved using TP5 on the bottom of the board, which is quite easy to solder to
    - Newer Pi 4 models have the USB firmware embedded in the VLI controller, but should still be write protectable
- There's a debug bootloader one can flash to the EEPROM to debug the VideoCore over UART, it outputs a lot of information

# February 28, 2022 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** February 28, 2022 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
    - Marvin Drees, @MDr164
:::

## Agenda/Notes

### Biweekly recap

- ODROID-C4 runs a U-Boot fork form 2015...
    - https://wiki.odroid.com/odroid-c4/software/building_u-boot
    - https://github.com/hardkernel/u-boot/tree/odroidg12-v2015.01
    - Might try to upstream this at some point
        - Oreboot support?
        - 26k commit diff is painful https://github.com/hardkernel/linux
- Dennis: Will contact Rich regarding MPL-2.0 relicensing this week
    - Repo refactor starts after that
- Daniel working a bit more on LinuxBoot, cpu and Fiedka at the moment
- Marvin working a bit more on u-bmc
- Marvin also got hold on two RP2040s to play around with

### Pi "netboot" (SD card remote flash)

- [Official beta feature](https://www.raspberrypi.com/news/network-install-beta-test-your-help-required/)
- Can we abuse it to load our own tools?
- Seems to be embedded into the RPi 4 EEPROM
    - Loads the imager from RPi servers using HTTPS (which TLS version?)
    - Puts the imager into RAM and executes it (without additional validation or authorization)
    - The imager downloads Raspbian and puts it on the SD card
- Can the imager source URL be changed?
- Can the HTTPS certificate store be changed?

### u-bmc migration

- BSD-3 licensed
- Move out of u-root org to new home: https://github.com/u-bmc
    - Split out the monorepo
    - Code spring cleaning/rewrite
    - Granular CI
    - Will use https://github.com/bufbuild/buf to provide protobuf files then
    - Thorough documentation coming

# February 14, 2022 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** February 14, 2022 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Daniel Maslowski, @orangecms
    - Verneri Hirvonen, @chiplet
:::

## Agenda/Notes

### Biweekly recap

- vPub v4 is coming up
    - https://vpub.dasharo.com/
    - The new repo structure will be in place before that
    - Little presentation on Racklet :)
- Repository discussion
    - Using separate .gitignore and .gitattributes per repository
    - Licensing: Apache 2.0 vs MPL 2.0 vs others?
        - Ask @stacktrust at vPub
        - https://gist.github.com/nicolasdao/a7adda51f2f185e8d2700e1573d8a633
        - https://www.kernel.org/doc/html/latest/process/license-rules.html
        - https://users.rust-lang.org/t/licensing-mit-apache-2-vs-mpl-2-0/46250
- twelho: Received the ODROID-C4 SBCs! Will take them for a spin tomorrow and see how they perform and what the boot flow looks like
- orangecms: D1 DRAM init C implementation is done and working now, translating to Rust is sort of started

# January 31, 2022 4:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-community
- **Date:** January 31, 2022 4:00 PM UTC
- **Host:** @twelho
- **Participants:**
    - Dennis Marttinen, @twelho
    - Marvin Drees, @MDr164
    - Daniel Maslowski, @orangecms
    - Michael Engel
:::

## Agenda/Notes

### Biweekly recap

- Still updating/cleaning up the GH organization
    - Planning on making a new release of https://github.com/racklet/render-drawio-action today
    - Still more repos to clean up
    - Unsolved issue: keeping template descendants up to date with upstream updates
    - Figured out that .gitignore can't be a symlink
- Marvin: Worked more on Kubernetes on ARM64
    - Mixed arch cluster experiments (x86 and ARM)
- D1 DRAM code is being ported
    - Paul is doing initial work: https://gitlab.com/pnru/boot0
    - Daniel is documenting and extracting register names https://gitlab.com/cyrevolt/boot0/-/tree/doc
- Daniel: Started looking into Rockchip
    - not yet as promising in terms of firmware customization
    - see https://github.com/oreboot/oreboot/issues/533

### Probing the SD interface with the RP2040

- twelho: Wired up the Raspberry Pi Pico to the SD slot of a RPi 3
    - Wrote some Rust code for the RP2040, [rp-rs](https://github.com/rp-rs/rp-hal) is very nice and quite feature complete
    - The PIO state machines are programmable with [pio-rs](https://github.com/rp-rs/pio-rs)
    - Got the core up to 125 MHz (within spec, not overclocked) and captured the SD clock signal
        - RP2040 is fast enough :100:
        - There's about 35 ns of latency in a wait-set loop
        - Going through the SM I/O mapping is about 10 ns slower

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

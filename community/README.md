# Racklet Community Meeting Notes

This document is best viewed and edited online: [![hackmd-github-sync-badge](https://hackmd.io/P7WKiyZSTpCeyfpj3Lm2Fw/badge)](https://hackmd.io/P7WKiyZSTpCeyfpj3Lm2Fw)

## Note: Meeting day/time has been updated! Starting from week 37 the community meeting will be on Mondays at 3 PM UTC (already on September 13th)!

## Another note: Meetings will be bi-weekly starting September 13 (i.e. next one on September 27) as core team continues university studies

[TOC]

# September 27, 2021 3:00 PM UTC

:::info
- **Location:** https://meet.jit.si/racklet-weekly
- **Date:** September 27, 2021 3:00 PM UTC
- **Host:**
- **Participants:**
- **Agenda:**
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

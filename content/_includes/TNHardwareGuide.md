---
---

{{< toc >}}

From repurposed systems to highly-custom builds, the fundamental freedom of TrueNAS is the ability to run it on almost any x86 computer.

## Minimum Hardware Requirements

The recommended system requirements to install TrueNAS:

| Processor | Memory | Boot Device | Storage |
|-----------|--------|-------------|---------|
| 2-Core Intel 64-Bit or AMD x86_64 Processor | 16 GiB Memory | 16 GB SSD Boot Device | 2 identically-sized devices for a single storage pool |

The TrueNAS CORE installer recommends 8 GB of RAM. TrueNAS CORE installs, runs, operates a jail, hosts a SMB share, and replicates TBs of data with less. iXsystem recommends the above for better performance and fewer issues.

A SSD isn't required as a boot device. iXsystems discourages using either a spinner or a USB stick for obvious reasons.
iXsystems discourages installing TrueNAS CORE on a single disk or striped pool unless there is a good reason to do so. TrueNAS CORE can install and run without any data device but this is also discouraged.

Two cores aren't required as most halfway-modern 64-bit CPUs likely already have at least two cores.

For help building a system according to your unique performance, storage, and networking requirements, read on!

## Storage Considerations

At the heart of any storage system is the symbiotic pairing of its file system and its physical storage devices.
The ZFS file system in TrueNAS provides the [best available data protection of any file system at any cost](https://www.ixsystems.com/blog/openzfs-vs-the-competition/) and makes very effective use of both spinning-disk and all-flash storage, or a mix of the two.
ZFS is completely prepared for the eventual failure of storage devices. It is highly-configurable to achieve the perfect balance of redundancy and performance to meet any storage goal.
A properly-configured TrueNAS system can tolerate the failure of multiple storage devices and even recreate its boot media with a copy of the [configuration file]({{< relref "ConfigBackup.md" >}}).

### Storage Media

Choosing storage media is the first step in designing the storage system to meet immediate objectives and prepare for future capacity expansion.

{{< tabs "Storage Device Options">}}
{{< tab "Spinning Disks" >}}
Until the next scientific breakthrough in storage media, spinning hard disks are here to stay, thanks to their balance of capacity and cost.
The arrival of double-digit terabyte consumer and enterprise drives provides more choices to TrueNAS users than ever.
TrueNAS Mini systems ship with Western Digital NAS and NL-SAS for good reason, and understanding the alternatives explains this decision.
{{< /tab >}}
{{< tab "SATA NAS Disks" >}}
Serial Advanced Technology Attachment (SATA) is still the de facto standard disk interface found in many desktop/laptop computers, servers, and some non-enterprise storage arrays.
SATA disks first arrived offering double-digit gigabyte capacities and are now produced to meet a myriad of capacity, reliability, and performance goals.
While consumer desktop SATA disks don't have the problematic overall reliability issues they once had, they are still not designed or warrantied for continuous operation or use in RAID groups.
Enterprise SATA disks address the always-on factor, vibration tolerance, and drive error handling required in storage systems, but the price delta between desktop and enterprise SATA drives is vast enough that it drives users to push their consumer drives into 24/7 service in pursuit of cost savings.

Drive vendors, likely tired of honoring warranties for failed desktop drives used in incorrect applications, responded to this gap in the market by producing NAS drives, made famous by the original Western Digital (WD) Red™ drives with CMR/PMR technology (now called WD Red Plus).
Western Digital Designed the WD Red™ Plus NAS drives (non-SMR) for use in systems with up to eight hard drives, or for up to 16 drives the [WD Red™ Pro](https://www.westerndigital.com/products/internal-drives/wd-red-pro-sata-hdd) drives, and the [WD UltraStar™](https://www.westerndigital.com/products/data-center-platforms) drives for systems beyond 16 drives.

WD drives are known among the iXsystems Community Forum as the preferred hard drives for TrueNAS builds due to their exceptional quality and reliability.
All TrueNAS Minis ship with WD Red™ Plus drives unless requested otherwise.
{{< /tab >}}
{{< tab "Nearline SAS Disks" >}}
Nearline SAS (NL-SAS) disks are 7200 RPM enterprise SATA disks with the industry-standard SAS interface found in the majority of enterprise storage systems.
SAS stands for **Serial Attached SCSI**, with the traditional SCSI disk interface in serial form.
SAS systems, designed for data center storage applications, have accurate, verbose error handling, predictable failure behavior, reliable hot swapping, and the added feature of multipath support.
Multipath access means that each drive has two interfaces and can connect to either two storage controllers, or one controller over two cables.
This redundancy protects against cable, controller card, or complete system failure in the case of the TrueNAS high-availability architecture in which each controller is an independent server that accesses the same set of NL-SAS drives.
NL-SAS drives are also robust enough to handle the rigors of systems with more than 16 disks.
So, capacity-oriented TrueNAS systems ship with [Western Digital UltraStar](https://www.westerndigital.com/products/data-center-platforms) NL-SAS disks thanks to the all-around perfect balance of capacity, reliability, performance, and flexibility that NL-SAS drives offer.
{{< /tab >}}
{{< tab "SAS Disks" >}}
Enterprise SAS disks, built for maximum performance and the reliability that a spinning platter can provide, are the traditional heavy-lifters of the enterprise storage industry .
SAS disk capacities are low compared to NL-SAS or NAS drives due to the speed at which the platters spin, reaching as high as 15,000 RPMs.
While SAS drives may sound like the ultimate answer for high-performance storage, many consumer and enterprise flash-based options have come onto the market and significantly-reduced the competitiveness of SAS drives.
For example, enterprise SAS drives discontinued from the TrueNAS product lines were almost completely replaced by flash drives (SSDs or NVMe) in 2016 due to their superior performance/cost ratio.
{{< /tab >}}
{{< tab "SATA and SAS Flash Storage SSDs" >}}
Significant progress in flash storage technology in recent years has enabled a revolution in mobile devices and the rise of flash storage in general-purpose PCs and servers.
Unlike hard disks, flash storage is not sensitive to vibration and can be much faster with comparable reliability.
Flash storage remains more expensive per gigabyte but is finding many ways into TrueNAS systems as that price gap narrows.

The shortest path for the introduction of flash storage into the mainstream market was for vendors to use standard SATA/SAS hard disk interfaces and form factors that emulate standard hard disks but without moving parts.
For this reason, flash storage Solid State Disks (SSDs) have SATA interfaces and are the size of 2.5” laptop hard disks, allowing them to be drop-in replacements for traditional hard disks.
Flash storage SSDs can replace HDDs for primary storage on a TrueNAS system, resulting in a faster, though either a smaller or more expensive storage solution.
If going all-flash, buy the highest-quality flash storage SSDs the budget allows with a focus on power safety and write endurance that matches your expected write workload.
{{< /tab >}}
{{< tab "NVMe" >}}
While it made sense for SSDs to pretend they were HDDs for rapid adoption, the Non-Volatile Memory Express (NVMe) standard is a native flash protocol that takes full advantage of the non-linear and parallel nature of flash storage.
The key advantage of NVMe is generally its low-latency performance and it’s becoming a mainstream option for boot and other tasks.
While originally limited to expansion-card form factors such as PCIe and M.2, the new U.2 interface offers a rather universal solution that includes the 2.5” drive form factor and an externally-accessible (but generally not hot-swappable) NVMe interface.
Note that NVMe devices can run quite hot and might need dedicated heat sinks.
{{< /tab >}}
{{< tab "USB Hard Disks" >}}
Avoid using USB-connected hard disks for primary storage with TrueNAS but you can use them for very basic backups in a pinch.
While TrueNAS does not automate this process, you can connect a USB HDD, replicated at the command line, and then take it off-site for safe keeping.
{{< /tab >}}
{{< /tabs >}}

These storage device media arrange together to create powerful storage solutions.

### Storage Solutions

{{< tabs "Storage Solutions" >}}
{{< tab "Hybrid Storage & Flash Cache (SLOG/ZIL/L2ARC)" >}}
With hard disks providing double-digit terabyte capacities and flash-based options providing even higher performance, a best of both worlds option is available.
With TrueNAS and OpenZFS, you can merge both flash and disk to create hybrid storage that makes the most of both storage types.
In a hybrid configuration, large-capacity spinning disks store the data, while DRAM and flash act as hyper-fast read and write caching.
The technologies work in conjunction with a flash-based separate write log (SLOG), think of it as a write cache keeping what’s called the ZFS-intent log (ZIL), used to speed up writes.
On the read side, flash is a level two adaptive replacement (read) cache (L2ARC) to keep the hottest data sets on the faster flash media.
Workloads with synchronous writes such as NFS and databases benefit from SLOG devices, while workloads with frequently-accessed data might benefit from an L2ARC device.
An L2ARC device is not always the best choice because the level one ARC in RAM [always provide a faster cache](https://www.ixsystems.com/blog/visualizing-zfs-performance/), and the L2ARC table uses some RAM.

A SLOG device doesn't need to be large. It only needs to service five seconds of data writes delivered by the network or a local application.
A high-endurance, low-latency device between 8 GB and 32 GB in size is adequate for most modern networks, and you can strip or mirror several devices for either performance or redundancy.
Paying attention to the published endurance claims of the device is imperative, since a SLOG acts as the funnel point for a majority of the writes made to the system.

It is also vital that a SLOG device has power protection.
The purpose of the ZFS intent log (ZIL), and thus the SLOG, is to keep sync writes safe in the event of a crash or power failure.
If the SLOG isn’t power-protected and loses data after a power failure, it defeats the purpose of using a SLOG in the first place!
Check the manufacturer’s specifications to ensure the SLOG device is power safe or has power loss/failure protection.

Random read performance is the most important quality to look for in an L2ARC device. It needs to support more IOPS than the primary storage media it caches.
For example, using a single SSD as an L2ARC is ineffective in front of a pool of 40 SSDs as the 40 SSDs are able handle far more IOPS than the single L2ARC drive.
As for capacity, 5x to 20x larger than RAM size is a good guideline.
High-end TrueNAS systems can have NVMe-based L2ARC in the double digit terabytes in size, as an example.

Keep in mind that for every data block in the L2ARC, the primary ARC needs an 88 byte entry. This could cause an unexpected fill-up in the ARC and reduce performance in a poorly-designed system.
For example, a 480GB L2ARC filled with 4KiB blocks needs more than 10GiB of metadata storage in the primary ARC.
{{< /tab >}}
{{< tab "Self Encrypting Drives" >}}
TrueNAS supports two forms of data encryption at rest to achieve privacy and compliance objectives: [Native ZFS encryption]({{< relref "StorageEncryption.md" >}}) and [Self Encrypting Drives (SEDs)]({{< relref "SED.md" >}}).
SEDs do not experience the performance overhead introduced by software partition encryption but aren’t as readily-available as non-SED drives (and thus can cost a little bit more).
{{< /tab >}}
{{< tab "Boot Devices" >}}
It was once very popular to boot legacy FreeNAS systems from 8 GB or larger USB flash drives, but with the wide variance in USB drive quality and the increased drive writes done to the boot pool by modern TrueNAS versions, it’s advisable to look at other options.
For this reason, all pre-built [TrueNAS Systems]({{< relref "/hardware/_index.md" >}}) ship with either M.2 drives or SATA DOMs.

SATA DOMs, or disk-on-modules, offer reliability close to that of consumer 2.5” SSDs with a smaller form factor that mounts to an internal SATA port so doesn’t consume a drive bay.
Because SATA DOMs and motherboards with m.2 slots are not as common as the other storage devices mentioned here, it is popular to boot TrueNAS systems from 2.5” SSDs and HDDs (often mirrored for added redundancy).
The recommended size for the TrueNAS boot volume is 8 GB, but using 16 or 32 GB (or a 120 GB 2.5” SATA SSD) provides room for more boot environments.
{{< /tab >}}
{{< tab "Hot Swapability" >}}
TrueNAS CORE systems come in all shapes and sizes. It is very desirable to have external access to all storage devices for efficient replacement if issues occur.
Most hot swap drive bays need a proprietary drive tray into which you install each drive.
These bay and tray combinations often include convenient features like activity and identification lights to visualize activity and illuminate a failed drive with sesutil(8) (https://www.freebsd.org/cgi/man.cgi?query=sesutil&sektion=8 for CORE, https://manpages.debian.org/testing/sg3-utils/sg3_utils.8.en.html for SCALE).
TrueNAS Mini systems ship with four or more hot swap bays.  TrueNAS R-Series systems can support dozens of drives in their head units and external expansion shelves.
Pre-owned or repurposed hardware is popular among TrueNAS users.  Pay attention to the maximum performance offered by the hot swap backplanes of a given system. Watch for at least 6 Gbps SATA III support.
Note that hot-swapping PCIe NVMe devices is not currently supported.
{{< /tab >}}
{{< /tabs >}}

### Storage Device Sizing

[Zpool layout]({{< relref "PoolCreate.md#vdev-layout" >}}) (the organization of LUNs and volumes, in TrueNAS/ZFS parlance) is outside of the scope of this guide. The availability of double-digit terabyte drives raises a question TrueNAS users now have the luxury of asking. How many drives should I use to achieve my desired capacity?
While you can mirror two 16TB drives to achieve 16TB of available capacity it doesn’t mean that you should.
Mirroring two large drives offers the advantage of redundancy and balancing reads between the two devices which could lower power draw, but little else.
The write performance of two large drives at most is that of a single drive.
By contrast, an array of eight 4TB drives offers a wide range of configurations to optimize performance and redundancy at a lower cost.
If configured as striped mirrors, eight drives could yield four times greater write performance with similar total capacity.
You might also consider adding a hot spare drive with any zpool configuration to allow the automatic rebuild of  zpool by itself if a primary drive fails in the zpool.

### Storage Device Burn-In

Spinning disk hard drives have moving parts, by definition. These parts are highly-sensitive to shock and vibration, and wear out with use. Consider pre-flighting every storage device before putting it into production, paying attention to:

* Start a long HDD self test (`smartctl -t long /dev/`), and after the test completes (could take 12+ hrs)
* Check the results (`smartctl -a /dev/`)
* Check pending sector reallocations (`smartctl -a /dev/ | grep Current_Pending_Sector`)
* Check reallocated sector count (`smartctl -a /dev/ | grep Reallocated_Sector_Ct`)
* Check the UDMA CRC errors (`smartctl -a /dev/ | grep UDMA_CRC_Error_Count`)
* Check HDD and SSD write latency consistency (`diskinfo -wS `) *Unformatted drives only!*
* Check HDD and SSD hours (`smartctl -a /dev/ | grep Power_On_Hours`)
* Check NVMe percentage used (`nvmecontrol logpage -p 2 nvme0 | grep “Percentage used”`)

Take time before deploying the system to create a pool. Subject it to as close to real-world workload as possible to reveal individual drive issues and help determine if an alternative pool layout is better suited to that workload.
Be cautious of used drives as vendors may not be honest or informed about the age and health of any given drive.
Check the number of hours on all new drives using `smartctl(8)` to verify they aren't recertified.
A drive vendor could also zero the hours of a drive during recertification, masking its true age.
iXsystems tests all storage devices it sells for at least 48 hours before shipment.

### Storage Controllers

The uncontested most popular storage controllers used with TrueNAS are the 6 and 12 Gbps (Gigabits per second, sometimes expressed as Gb/s) Broadcom (formerly Avago, formerly LSI) SAS host bus adapters (HBA).
These ship as embedded controllers on some motherboards but are generally PCIe cards with four or more internal or external SATA/SAS ports.
The 6 Gbps LSI 9211 and its rebranded siblings that also use the LSI SAS2008 chip, such as the IBM M1015 and Dell H200, are legendary among TrueNAS users who build systems using parts from the second-hand market. 
Flash using the latest IT or Target Mode firmware to disable the optional RAID functionality found in the IR firmware on Broadcom controllers.
For those with the budget, newer models like the Broadcom 9300/9400 series give 12 Gbps SAS capabilities and even NVMe to SAS translation abilities with the 9400 series.
TrueNAS includes the `sas2flash`, `sas3flash`, and `storcli` commands to flash or to perform re-flashing operations on 9200, 9300, and 9400 series cards.

Onboard SATA controllers are popular with smaller builds, but motherboard vendors are better at catering to the needs of NAS users by including more than the traditional four SATA interfaces.
Be aware that many motherboards ship with a mix of 3 Gbps and 6 Gbps onboard SATA interfaces and that choosing the wrong one could impact performance.
If a motherboard includes hardware RAID functionality, do not use or configure it, but note that disabling it in the BIOS might remove some SATA functionality depending on the motherboard.
Most SATA compatibility-related issues are immediately obvious.

There are countless warnings against using hardware RAID cards with TrueNAS. 
ZFS and TrueNAS provide built-in RAID that protects your data better than any hardware RAID card eliminating the need for one.
Use a hardware RAID card if it’s all you have, but there are limitations.
First and most important, do not use their RAID facility (there is one caveat in the bullets below).
If the chosen hardware RAID card supports HBA mode, also known as passthrough or JBOD mode. When used it allows it to perform indistinguishable from a standard HBA.
If your RAID card does not have this mode, you can configure a RAID0 for every single disk in your system.
It’s not ideal, but it works in a pinch.
If repurposing hardware RAID cards with TrueNAS, be aware that some hardware RAID cards:

* Could mask disk serial number and S.M.A.R.T. health information
* Could perform slower than their HBA equivalents
* Could cause data loss if using a write cache with a dead battery backup unit (BBU))

### SAS Expanders

A direct-attached system, where every disk connects to an interface on the controller card, is optimal but not always possible.
A SAS expander, which is a port multiplier or splitter, enables each SAS port on a controller card to service many disks.
You find SAS expanders only on the drive backplane of servers or JBODs with  more than twelve drive bays.
For example, a [TrueNAS JBODs that eclipses 90 drives]({{< relref "ES102BSG.md" >}}) in only four rack units of space. 
This wouldn’t be possible without the miracle of SAS expanders.
Otherwise, imagine how many eight port HBAs you would required to access 90 drives!
While SAS expanders, designed for SAS disks, can often support SATA disks via the SATA Tunneling Protocol or STP.
SAS disks are still preferred for reasons mentioned in the NL-SAS section above but SATA disks function on a SAS-based backplane.
Note that the opposite is not true: you can’t use a SAS drive in a port designed for SATA drives.

### Storage Device Cooling

A much-cited study floating around the Internet asserts that drive temperature has little impact on drive reliability.
This study makes for a great headline or conversation starter but a careful reading of the report indicates that the drives were all tested under optimal environmental conditions.
The average temperature that a well-cooled spinning hard disk reaches in production is around 28 °C and [one study](https://en.wikibooks.org/wiki/Minimizing_Hard_Disk_Drive_Failure_and_Data_Loss/Environmental_Control) found that disks experience twice the number of failures for every 12 °C increase in temperature.
Before adding drive cooling that often comes with added noise, especially on older systems, recognize there is always a risk of throwing money away by running a server in a data center or closet without noticing that the internal cooling fans are set to their lowest setting.
Pay close attention to drive temperature in any chassis that supports 16 or more drives, especially if they are exotic, high-density designs.
Every chassis has certain areas that are warmer for whatever reason. Watch for fan failures and the tendency for some models of 8TB drives to run hotter than other drive capacities.
In general, try to keep drive temperatures below the drive vendor’s specification.

## Memory, CPU, and Network Considerations

### Memory Sizing

TrueNAS has higher memory requirements than many Network Attached Storage solutions for good reason: it shares [dynamic random-access memory](https://en.wikipedia.org/wiki/Dynamic_random-access_memory) (DRAM or simply RAM) between sharing services, add-on plugins, jails, and virtual machines, and sophisticated read caching.
RAM rarely goes unused on a TrueNAS system and enough RAM is key to maintaining peak performance.
You should have at least 8 GB of RAM for basic TrueNAS operations with up to eight drives. Other use cases each have distinct RAM requirements:

* Add 1GB for each drive added after eight to benefit most use cases.
* Add extra RAM (in general) if there are more clients connecting to the TrueNAS system. A 20 TB pool backing lots of high-performance VMs over iSCSI might need more RAM than a 200 TB pool storing archival data. If using iSCSI to back VMs, plan to use at least 16 GB of RAM for reasonable performance and 32 GB or more for optimal performance.
* Add 2 GB of RAM for directory services for the winbind internal cache.
* Add more RAM as required for plugins and jails as each have specific application RAM requirements.
* Add more RAM for virtual machines with guest operating system and application RAM requirements.
* Add the suggested 5 GB per TB of storage for deduplication that depends on an in-RAM deduplication table.
* Add approximately 1 GB of RAM (conservative estimate) for every 50 GB of L2ARC in your pool. Attaching an L2ARC drive to a pool actually uses some RAM, too. ZFS needs metadata in ARC to know what data is in L2ARC.  

### Error Correcting Code Memory

Electrical or magnetic interference inside a computer system can cause a spontaneous flip of a single bit of RAM to the opposite state, resulting in what’s known as a memory error.
Memory errors can cause security vulnerabilities, crashes, transcription errors, lost transactions, and corrupted or lost data.
So RAM, the temporary data storage location , is one of the most vital areas for preventing data loss.

Error correcting code or ECC RAM detects and corrects in-memory bit errors as they occur.
If errors are severe enough to be uncorrectable, ECC memory causes the system to hang (become unresponsive) rather than continue with errored bits.
For ZFS and TrueNAS, this behavior virtually eliminates any chances that RAM errors pass to the drives to cause corruption of the ZFS pools or errors in the files.

The lengthy, Internet-wide debate on whether to use error correcting code (ECC) system memory with OpenZFS and TrueNAS summarizes as:

* ECC RAM is *strongly* recommended as another data integrity defense

However:

* Some CPUs or motherboards support ECC RAM but not all
* Many TrueNAS systems operate every day without ECC RAM
* RAM of any type or grade can fail and cause data loss
* RAM is most likely to fail in the [first three months](https://media.kingston.com/images/usb/pdf/Server_Burn-in.pdf) so test all RAM before deployment.  

### Central Processing Unit (CPU) Selection

Choosing ECC RAM significantly reduces the available CPU and motherboard options, but that is actually a good thing.
Intel<sup>®</sup> makes a point of limiting ECC RAM support to their lowest and highest-end CPUs, cutting out the mid-range i5 and i7 models.

Exactly what CPU to choose can come down to a short list of key factors:

* An under-powered CPU can create a performance bottleneck because of the way OpenZFS does checksums, compresses and (optional) encrypts data.
* A higher-frequency CPU with fewer cores usually performs best for SMB only workloads because of Samba, the lightly-threaded TrueNAS SMB daemon.
* A higher-core-count CPU is better suited for parallel encryption and virtualization.
* A CPU with AES-NI encryption acceleration support improves the speed of file system and network encryption.
* A server-class CPU is best and recommended for their power and ECC memory support .
* A Xeon E5 CPU (or similar) is best and recommended for use with software-encrypted pools.
* An Intel Ivy Bridge CPU or later is best and recommended for virtual machine use.

Watch for VT-d/AMD-Vi device virtualization support on the CPU and motherboard to pass PCIe devices to virtual machines.
Be aware if a given CPU contains a GPU or requires an external one. Also, note that many server motherboards include a BMC chip with a built-in GPU. See below for more details on BMCs.

AMD CPUs are making a comeback thanks to the Ryzen and EPYC (Naples/Rome) lines. Support for these platforms is rather limited on FreeBSD and, by extension, TrueNAS CORE. But there is significant Linux support and TrueNAS SCALE should work with AMD CPUs without issue.

### Remote Management: IPMI

As a courtesy to further limit the motherboard choices, consider the Intelligent Platform Management Interface or IPMI, a.k.a. baseboard management controller (or BMC, iLo, iDrac, and other names depending on the vendor) if you need:

* Remote power control and monitoring of remote systems
* Remote console shell access for configuration or data recovery
* Remote virtual media for TrueNAS installation or reinstallation

TrueNAS relies on its web-based user interface (UI). But you might on occasion, need console access to make network configuration changes.
TrueNAS administration and sharing default to a single network interface. This  becomes a challenge when it comes time to upgrade, for example, LACP aggregated networking.
The ideal solution is to have a dedicated subnet for access to the TrueNAS web UI, but this luxury is not available to all users. So the occasional visit to the hardware console is necessary for global configuration and even for system recovery.
The latest TrueNAS Mini and R-Series systems ship with full-featured, HTML5-based IPMI support on a dedicated gigabit network interface.

### Power Supply Units

The top criteria to consider for a power supply unit (or PSU) on a TrueNAS system are its:

* Power capacity (in watts) for the motherboard and number of drives it must support
* Reliability
* Efficiency rating
* Relative noise
* Optional redundancy to keep an important system running if one power supply fails

Select a PSU rated for the initial and a future load placed on it.
Have a PSU with adequate power to handle a future plan to migrate from a chassis of large storage to a fully-populated chassis.
Also, consider a hot-swappable redundant PSU to help guarantee uptime.
Users on a budget can keep a cold spare PSU to limit their potential downtime to hours, rather than days.
A good, modern PSU is efficient and completely integrates into the IPMI management system to provide real-time fan, temperature, and load information. 

Most power supplies carry a certified efficiency rating known as an [80 Plus](https://en.wikipedia.org/wiki/80_Plus) rating.
The 80 plus rating indicates the power drawn from the wall lost as heat, noise, and vibrations, instead of doing useful work like powering your components.
If a power supply needs to draw 600 watts from the wall to provide 500 watts of power to your components, it’s operating at 500/600 = \~83% efficiency.
The other 100 watts gets lost as heat, noise, and vibration.
Power supplies with higher ratings are more efficient, but also likely far more expensive.
Do some return-on-investment calculations if you’re unsure what efficiency to buy.
For example, if an 80 Plus Platinum PSU costs $50 more than the comparable 80 Plus Gold, it should save you at least $10 per year on your power bill for that investment to pay off over 5 years.
You can read more about 80 Plus ratings in [this post](https://www.tomshardware.com/news/what-80-plus-levels-mean,36721.html).

### Uninterruptible Power Supplies

TrueNAS provides the ability to communicate with a battery-backed, uninterruptible power supply (UPS) over a traditional serial or USB connection to coordinate a graceful shutdown in the case of power loss.
TrueNAS works well with APC brand UPSs, followed by CyberPower. Consider budgeting for a UPS with pure sine wave output.
Some models of SSD can experience data corruption on power loss.
If several SSDs experience simultaneous power loss, it could cause total pool failure, making a UPS a critical investment.

### Ethernet Networking

The network in Network Attached Storage is as important as storage but the topic reduces to a few key points:

* Simplicity - Simplicity is often the secret to reliability with network configurations.
* Individual interfaces - Faster individual interfaces such as 10/25/40/100GbE are preferable to aggregating slower interfaces.
* Interface support - Intel and Chelsio interfaces are the best supported options.
* Packet fragmentation - Only consider a *jumbo frames* [MTU](https://en.wikipedia.org/wiki/Maximum_transmission_unit) with dedicated connections such as between servers or video editors and TrueNAS that are unlikely to experience packet fragmentation.
* LRO/LSO offload features - Interfaces with [LRO](https://en.wikipedia.org/wiki/Large_receive_offload) and [LSO](https://en.wikipedia.org/wiki/Large_send_offload) offload features generally alleviates the need for jumbo frames and their use can result in lower CPU overhead.

### High Speed Interconnects

Higher band hardware is becoming more accessible as the pace of hardware development increases and enterprises upgrade at a faster pace.
Home labs can now deploy and use 40GB and higher networking components. Home users are now discovering the same issues and problems with these higher speeds found by Enterprise customers.

iXsystems recommends using optical fiber over *direct attached copper* (DAC) cables for the high speed interconnects listed below:

* 10Gb NICs: SFP+ connectors
* 25Gb NICs: SFP28 connectors
* 40Gb NICs: QSFP+ connectors
* 100Gb NICs: QSFP28 connectors
* 200Gb NICs: QSFP56 connectors
* 400Gb NICs: QSFP-DD connectors

iXsystems also recommends using optical fiber for any of the transceiver form factors mentioned when using fiber channel.
Direct attached copper (DAC) cables could create interoperability issues between the NIC, cable, and switch.

## Virtualized TrueNAS CORE

Finally, the ultimate TrueNAS hardware question is whether to use actual hardware at all or go with a virtualization solution.
TrueNAS developers [virtualize TrueNAS every day](https://www.ixsystems.com/blog/yes-you-can-virtualize-freenas/) as part of their work, and cloud services are popular among users of all sizes.
The design of TrueNAS has OpenZFS at its heart.  The design, from day one to works with physical storage devices. It is completely aware of their strengths and compensates for their weaknesses.
When the need arises to virtualize TrueNAS:

* Pass hardware disks or the entire storage controller to the TrueNAS VM if possible (requires VT-d/AMD-Vi support).
* Disable automatic scrub pools on virtualized storage such as VMFS, and never scrub a pool while storage repair tasks are taking place on another layer.
* Use a least three vdevs to provide adequate metadata redundancy, even with a striped pool.
* Provide one or more 8GB or larger boot devices. 
* Provide the TrueNAS VM with adequate RAM, as per its usual requirements.
* Consider jumbo frame networking if supported by all devices.
* Understand that the guest tools in FreeBSD might lack features found in other guest operating systems.
* Enable MAC address spoofing on virtual interfaces and enable promiscuous mode to use VNET jail and plugins.

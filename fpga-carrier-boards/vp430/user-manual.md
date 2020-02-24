# User Manual

## About This Manual 

### Conventions 

#### Numbers 

All numbers are expressed in decimal, except addresses and memory or register data, which are expressed in hexadecimal. Where confusion may occur, decimal numbers have a “D” subscript and binary numbers have a “b” subscript. The prefix “0x” shows a hexadecimal number, following the ‘C’ programming language convention. Thus: 

One dozen = 12D = 0x0C = 1100b 

The multipliers “k”, “M” and “G” have their conventional scientific and engineering meanings of x103, x106 and x109, respectively, and can be used to define a transfer rate. The only exception to this is in the description of the size of memory areas, when “K”, “M” and “G” mean 2^10, 2^20 and 2^30 respectively. 

In PowerPC terminology, multiple bit fields are numbered from 0 to n where 0 is the MSB and n is the LSB. PCI terminology follows the more familiar convention that bit 0 is the LSB and n is the MSB. 

#### Text 

Signal names ending with a tilde \(“~”\) denote active low signals; all other signals are active high. “N” and “P” denote the low and high components of a differential signal respectively. 

### Further Information 

Abaco Systems Serial BLAST Design Specification \(BLAST UM050r1.1\) 

#### Third-party Documents 

ANSI/VITA 46.0-2007 \(R2013\) VPX Baseline Standard   
ANSI/VITA 46.4 PCI Express® on the VPX Fabric Connector   
ANSI/VITA 46.6 Gigabit Ethernet Control Plane on VPX   
ANSI/VITA 46.11-2015 System Management on VPX   
ANSI/VITA 48.1 Specification for Using REDI Air Cooling   
ANSI/VITA 48.2 Specification for Using REDI Conduction Cooling   
ANSI/VITA 65-2010 \(R2012\) OpenVPX™ System Specification   
ANSI/VITA 66.4 Optical Interconnect on VPX – Half Width MT Variant   
ANSI/VITA 67.1 Coaxial Interconnect on VPX, 3U, 4 Position SMPM Configuration   
Xilinx Zynq Ultrascale Plus Datasheet \(DS925\)   
Xilinx Zynq US+ RFSoC Datasheets \(DS926\)

{% hint style="info" %}
Technical literature describing components used on the VP430 is available from the manufacturers’ websites.
{% endhint %}

### Errata 

#### PCB revision 1.0 

Non-disclosed 

#### PBC revision 1.1 

The Zynq RFSoC cannot communicate with the MMC or backplane using the VPX IPMB-A I2C interface. Boards with S/N REMZ000005, 7, 9,10, 11, 14, 15, and 16 may exhibit IPMB-A bus communications errors with more than one VP430 in a system. 

### Technical Support Contact Information 

You can find technical assistance contact details on the website [Support](http://www.abaco.com/support%20) page.

Abaco will log your query in the Technical Support database and allocate it a unique Case number for use in any future correspondence. 

Alternatively, you may also [contact Abaco’s Technical Support](mailto:support@abaco.com).

### Returns 

If you need to return a product, there is a Return Materials Authorization \(RMA\) form available via the website [Support ](http://www.abaco.com/support)page.  

Do not return products without first contacting the Abaco Repairs facility. 

### Requirements and Handling Instructions 

* The convection-cooled VP430 must be installed in a 1-in pitch VITA 65-compliant, 3U VPX slot. Conduction-cooled VP430 boards also require a 1-in pitch. VITA 46.0 slots do not provide adequate clearance for backside components mounted on the VP430 module. 
* The OpenVPX or VPX chassis slot must support differential backplane connectors for J1 and J2A, and be able to optionally support optical or coax backplane connectors for J2B. P0 is the utility connector for power, resets and board management. 
* The sBLAST site is populated at the factory and must be specified when ordering. 
* Prevent electrostatic discharge by observing ESD precautions when handling the card. Safety This module presents no hazard to the user.

### Safety 

{% hint style="info" %}
This module presents no hazard to the user.
{% endhint %}

## 1. General Description 

The VP430 is a 3U OpenVPX payload module that employs Xilinx’s Zynq® Ultrascale+™ Radio Frequency System on Chip device to provide high-speed, low latency, Digital to Analog and Analog to Digital conversion. The RFSoC device accomplishes this by combining a Zynq US+ SoC/FPGA with eight ADC channels and eight DAC channels. Each ADC and DAC channel is capable of 4 Gs/s and 6.4 Gs/s, respectively. The VP430 module includes two 64-bit DDR4 interfaces capable of 2400 Mbit/s. Eight of the 16 GTY channels available on the device are used to support PCIe Gen2/3/4 over the subsystem’s backplane. The remaining 8 GTY are used to support Abaco’s proprietary sBLAST site. This flexible board-on-board form factor can support many forms of static and dynamic memory with its GPIO, or it can support fiber optics over backplane using Samtec’s FireFly™ line of transceiver modules. 

The VP430 SoC contains many standard hardware interfaces that are supported using Abaco’s RTM311, a 3U VPX RTM module. The RTM311 supports one DisplayPort, one M.2 SATA SSD, two 1000Base-T Ethernet ports, two USB ports, one Serial port, and a JTAG port for firmware debug. The VP430 device boots from onboard QSPI FLASH or from the removable SD card on the RTM \(Rear Transition Module\) or the onboard SDC slot. The global architecture of the VP430 is shown in Figure 1-2.

## 2. Design 

This section describes the design details of the VP430. Design aspects such as the mechanical specifications, power provision, clock tree and electrical interfaces are described in this section.

### 2.1 Board Dimensions

The VP430 is a ruggedized 3U \(160 x 100 mm\) OpenVPX plug-in module designed to work in 1-in pitch systems, supporting both convection- and conduction-cooled variants.  Figure 2-1, shown below, is the mechanical reference for conduction-cooled OpenVPX 3U plug-in modules, including board dimensions, component envelopes, and mechanical keep-outs.

### 2.2 Component Placement 

Component height restrictions are compliant to the VITA 48.1 and VITA 48.2 standards for 1-in modules. The maximum component height allowed on the bottom side of the board is 7.87 mm, from primary side of PCB, and including thermal enclosure. The maximum allowable component height on the top side of the board this is 16.76 mm, including heatsink or clamshell enclosures. 

### 2.3 Cooling Options 

The VP430 is available in two cooling options, convection and conduction. For convection-cooled applications, a board-level designed heatsink is provided to deliver optimal thermal performance by dissipating heat from critical components. For convection cooling, a minimum air flow is required across the heat sink when power is applied, or the FPGA and board may be damaged. Refer to Section 11.3, “Convection Cooling” for more details on minimum airflow.

### 2.4 Component Selection 

The VP430 employs Xilinx’s Zynq UltraScale Plus FPGA/SoC with ADC/DAC capabilities, the XCZU27DR RFSoC.  The device is an FPGA with a quad core Arm® Cortex processor system and peripherals, combined with eight high-speed ADC and DAC channels. The FFVG1517 package used on the VP430 supports three component migrations in all speed and temperature grades. The device integrates many features and interfaces typically associated with PCs \(Personal Computers\), allowing users to limit or eliminate the need for other costly VPX hardware such as SBCs \(Single Board Computers\). The VP430 supports some of these features onboard and uses on an RTM to provide more I/O panel space and component space for features like USB, SATA, and Display Port. Table 2-4 describes the FPGA logic metrics and the Zynq features and peripherals. Figure 2-4 contains more details on the RFSoC processor system.     

|  | XCZU25DR | XCZU27DR | XCZU28DR |
| :--- | :--- | :--- | :--- |
| 12-bit, 4 GS/s RF-ADC w/DDC | 8 | 8 | 8 |
| 12-bit, 2 GS/s RF-ADC w/DDC | 0 | 0 | 0 |
| 14-bit, 6.4 GS/s RF-DAC w/DUC | 8 | 8 | 8 |
| SD-FEC | 0 | 0 | 0 |
| System Logic Cells | 680,400 | 930,300 | 930,300 |
| CLB Flip-Flops | 622,080 | 850,560 | 850,560 |
| CLB LUTs | 311,040 | 425,280 | 425,280 |
| Distributed RAM \(Mbit\) | 9.6 | 13.0 | 13.0 |
| Block RAM Blocks | 792 | 1,080 | 1,080 |
| Block RAM \(Mbit\) | 27.8 | 38.0 | 38.0 |
| UltraRAM Blocks | 48 | 80 | 80 |
| UltraRAM \(Mbit\) | 13.5 | 22.5 | 22.5 |
| DSP Slices | 3,168 | 4,272 | 4,272 |
| CMTs | 6 | 8 | 8 |
| Maximum HP I/O | 299 | 299 | 299 |
| Maximum HD I/O | 48 | 48 | 48 |
| System Monitor | 1 | 1 | 1 |
| GTY Transceivers | 8 | 16 | 16 |
| Transceivers Fractional PLLs | 4 | 8 | 8 |
| PCIe Gen3 x16 and Gen4 x8 | 1 | 2 | 2 |
| 150G Interlaken | 1 | 1 | 1 |
| 100G Ethernet w/RS-FEC | 1 | 2 | 2 |

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">
        <p></p>
        <p>XCZU25DR, XCZU27DR, XCZU28DR</p>
        <p></p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Application Processing Unit</td>
      <td style="text-align:left">
        <ul>
          <li>Quad-core Arm Cortex-A53 MPCore with CoreSight&#x2122;</li>
          <li>NEON and Single/Double Precision Floating Point</li>
          <li>32KByte/32KByte L1 Cache</li>
          <li>1 MByte L2 Cache</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Real-Time Processing Unit</td>
      <td style="text-align:left">
        <ul>
          <li>Dual-core Arm Cortex-R5 with CoreSight</li>
          <li>Single/Double Precision Floating Point</li>
          <li>32KByte/32KByte L1 Cache</li>
          <li>TCM</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Embedded and External Memory</td>
      <td style="text-align:left">
        <ul>
          <li>256 KByte On-Chip Memory w/ECC</li>
          <li>External DDR4</li>
          <li>DDR3</li>
          <li>DDR3L</li>
          <li>LPDDR4</li>
          <li>LPDDR3</li>
          <li>External Quad-SPI</li>
          <li>NAND</li>
          <li>eMMC</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">General Connectivity</td>
      <td style="text-align:left">
        <ul>
          <li>4 PS-GTR; PCIe Gen1/2</li>
          <li>Serial ATA 3.1</li>
          <li>DisplayPort 1.2a</li>
          <li>USB 3.0</li>
          <li>SGMII</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>### 2.5 Component Migrations 

While the primary device targeted on the VP430 is the XCZU27DR, two other pin-compatible RFSoC device migrations are supported by the VP430, the XCZU25DR and the XCZU28DR. The XCZU25DR has fewer FPGA logic and associated FPGA IP blocks, as well as fewer GTY lanes. The XCZU28DR device is the same as the XCZU27DR, except it includes eight Forward Error Correction \(FEC\) IP blocks, for telecom applications like massive MIMO. Other than the XCVZU25’s eight fewer GTY lanes, all three devices have the same pin count shown in Table 2-2.

| Package | Dimensions | Resource | XCZU25DR | XCZU27DR | XCZU28DR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| FFVG1517 | 40x40 | PSIO | 214 | 214 | 214 |
|  |  | HDIO | 48 | 48 | 48 |
|  |  | HPIO | 299 | 299 | 299 |
|  |  | PS-GTR | 4 | 4 | 4 |
|  |  | GTY | 8 | 16 | 16 |
|  |  | RF-ADC | 8 | 8 | 8 |
|  |  | RF-DAC | 8 | 8 | 8 |

### 2.6 Onboard DDR4 SDRAM Interfaces 

There are two onboard SDRAM interfaces. One interface is managed by the Zynq, using a hard IP memory controller. The other is implemented in the programmable logic, using Xilinx’s MIG soft IP. Both interfaces can support DMA using AXI interconnect buses. 

Each DDR4 interface can support a single rank of 1Gx16bit Twin-Die DDR4 ICs, or Single-Die 512Mx16bit DDR4 ICs. With 4 devices per bus, each of the DDR4 interface can be used as a standard 64bit memory interface. At the time of design, the Twin-Die product is only supported by Micron, and is only offered in extended temperature grades. The lower density single die parts are offered in industrial temperature grade.

The maximum operating frequency is 1200 MHz, for a bit rate of 2400 Mbit/s. Per byte data masking is supported for each lane. The DDR4 standard also provisions for up to 8 stacked silicon devices in the DDR4 package. When these products become available, up to 64 GBytes of DDR4 may be supported on the VP430 VPX module. 

The VP430 standard memory configuration uses 512Mx16b devices from Micron, part number MT40A512M16JY-083E IT:B or equivalent.  

### 2.7 First Stage Boot Loader NOR Flash 

By default, the VP430 relies on QPSI serial flash for loading the FPGA bit images and for loading the Zynq software after reset. The VP430 provides a 1 Gbit serial NOR flash device for this purpose. The QSPI flash interface can operate up to 

100 MHz x 4bit DDR. The part number used is Micron’s MT25QL01GBBB8E12-0SIT. 

\(1 Gbit – 108 MHz 3.3V Industrial Temperature TPBGA-24\) 

Users may also boot directly from either, the onboard µSD Card slot, or the µSD microSD Card slot available on the RTM311. JTAG boot mode is also supported, and useful for FW and SW debug.  No other boot modes are currently supported in Vivado® tools, but USB 2.0 booting is supported in HW on the VP430. Please consult Xilinx for more information on USB 2.0 tool support.   

The boot device is selected using BOOT MODE pins on the Zynq configuration bank. These pins are sampled at reset to determine which boot method, from Table 2-3, is used to load the device. These pins can be controlled by the board switch array described in Figure 2-5. Not all boot modes in Table 2-3 are supported on the VP430. Please refer to Table 2-4 for supported boot modes.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Boot Mode</th>
      <th style="text-align:left">Mode Pins [3:0]</th>
      <th style="text-align:left">
        <p>MIO</p>
        <p>Location</p>
      </th>
      <th style="text-align:left">Non-Secure</th>
      <th style="text-align:left">Secure</th>
      <th style="text-align:left">Signed</th>
      <th style="text-align:left">Mode</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">JTAG</td>
      <td style="text-align:left">0x0</td>
      <td style="text-align:left">JTAG</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Slave</td>
      <td style="text-align:left">Dedicated PS Interface</td>
    </tr>
    <tr>
      <td style="text-align:left">QSPI24</td>
      <td style="text-align:left">0x1</td>
      <td style="text-align:left">MIO[12:0]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Master</td>
      <td style="text-align:left">
        <ul>
          <li>24-bit addressing
            <br />
          </li>
          <li>Supports single and dual parallel configurations
            <br />
          </li>
          <li>Stack and dual stack is not supported
            <br />
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">QSPI32</td>
      <td style="text-align:left">0x2</td>
      <td style="text-align:left">MIO[12:0]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Master</td>
      <td style="text-align:left">
        <ul>
          <li>32-bit addressing
            <br />
          </li>
          <li>Supports single and dual parallel configurations
            <br />
          </li>
          <li>Stack and dual stack is not supported
            <br />
          </li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SD0</td>
      <td style="text-align:left">0x3</td>
      <td style="text-align:left">MIO[25:13]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Master</td>
      <td style="text-align:left">Supports SD 2.0</td>
    </tr>
    <tr>
      <td style="text-align:left">NAND</td>
      <td style="text-align:left">0x4</td>
      <td style="text-align:left">MIO[25:09]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Master</td>
      <td style="text-align:left">Requires 8-bit data bus width</td>
    </tr>
    <tr>
      <td style="text-align:left">SD1</td>
      <td style="text-align:left">0x5</td>
      <td style="text-align:left">MIO[51:38]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Master</td>
      <td style="text-align:left">Supports SD 2.0</td>
    </tr>
    <tr>
      <td style="text-align:left">eMMC_18</td>
      <td style="text-align:left">0x6</td>
      <td style="text-align:left">MIO[22:13]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Master</td>
      <td style="text-align:left">Supports eMMC 4.5 at 1.8V</td>
    </tr>
    <tr>
      <td style="text-align:left">USB 0</td>
      <td style="text-align:left">0111b</td>
      <td style="text-align:left">MIO[52:63]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Slave</td>
      <td style="text-align:left">Supports USB 2.0 and USB 3.0</td>
    </tr>
    <tr>
      <td style="text-align:left">PJTAG_0</td>
      <td style="text-align:left">0x8</td>
      <td style="text-align:left">MIO[29:26]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Slave</td>
      <td style="text-align:left">PS JTAG connection 0 option</td>
    </tr>
    <tr>
      <td style="text-align:left">PJTAG_1</td>
      <td style="text-align:left">0x9</td>
      <td style="text-align:left">MIO[15:12]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">No</td>
      <td style="text-align:left">Slave</td>
      <td style="text-align:left">PS JTAG connection 1 option</td>
    </tr>
    <tr>
      <td style="text-align:left">SD1-LS</td>
      <td style="text-align:left">0xE</td>
      <td style="text-align:left">MIO[51:39]</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Yes</td>
      <td style="text-align:left">Master</td>
      <td style="text-align:left">Supports SD 3.0 with a required SD 3.0 compliant level shifter</td>
    </tr>
  </tbody>
</table>| Boot Mode Switches \(4:1\) | Boot Mode Pins\(\*3:0\) | Boot Mode Selection |
| :--- | :--- | :--- |
| 0000 | 0x02 | QSPI32 "This is the Default Mode" |
| 0011 | 0x03 | QSPI32 or Onboard SD Card 0, Auto switching w/CD |
| 0100 | 0x00 | JTAG Boot Mode, using Dedicated Pins |
| 0110 | 0x01 | QSPI24 for Devices w/out 32 Addressing |
| 1010 | 0x07 | USB 0 Boot Mode PSIO pins \(63:52\) |
| 1110 | 0x05 | RTM SDCARD 1, Forced RTM Boot Mode |

{% hint style="info" %}
All combinations not listed here are considered invalid and may result in unpredictable behavior. 
{% endhint %}

{% hint style="info" %}
\*Boot Mode Pin 3 is always 0, and has no switch. SW1 switch elements are labeled 1 through 8.
{% endhint %}

### 2.8 Zynq PS Peripheral Interfaces 

The device’s Processing System \(PS\) has 214 dedicated pins used to support one 

64-bit DDR4 interface and majority of Arm peripherals, including general purpose I/O. These pins have multiplexed functions selected using an internal crossbar configured by the First Stage Boot Loader \(FSBL\) during configuration.

The VP430 supports one USB 2.0 port to the backplane connector P1. The VP430 supports one SD Card interface using a MicroSD slot card onboard and another through the P1 connections to the RTM311. The VP430 can be configured to boot from either SD Card slot, or from the onboard QSPI flash device. This QSPI flash is the primary boot media on the VP430 for all variants. Also connected to the PSIO banks are 1 GbE MACs, two UARTs, and two I2C buses for communication, debug, and device control.

The two SPI controllers located in PLIO banks 84 and 87 and are used to control and configure the LMX and HMC clock generation devices. These banks share clock domains with the ADC and DAC outer banks. To reduce noise, banks 84 and 87 also include power filtering and do not cross into the digital domain without line filtering.

Figure 2-6 contains information on the processor and internal peripheral controllers of the Zynq devices. Peripheral devices with a check mark are implemented in HW and in the Board Support Package \(BSP\). 

#### 2.8.1 Secure Digital Storage

The VP430 supports a MicroSD \(Secure Digital\) card slot onboard with limited access. The VP430 can detect the presence of an SD card here and automatically change the boot mode pins on the Zynq. This allows the user to boot from the onboard Quad SPI device or the pluggable SD card when installed. Refer to Table 2-4 for details on selecting the boot mode using the switch array.  Figure 2-5 shows how the VP430 boot devices are implemented on the VP430 carrier card. 

#### 2.8.2 USB 2.0 

The Zynq US+ device supports two USB 2.0 ports with ULPI interfaces. The VP430 only implements one USB port. This port connects through the SE pins of P1 to the RTM311, where a hub is used to expand to two USB ports with rear bezel access. The VP430 supports USB 2.0, and the Zynq is capable of booting from this interface if selected using the switch array. The VP430 uses Microchip Technologies P/N USB3320C-EZK USB PHY and includes power control and device protection circuitry on the VP430 main board. The RTM311 is not required for USB operation.  


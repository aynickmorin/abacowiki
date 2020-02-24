# Reference Firmware

**AXI FMC432**

**10GbE Ethernet Star**

**Revision History**

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>Document</b>
        </p>
        <p><b>Revision</b>
        </p>
      </th>
      <th style="text-align:left"><b>Changes</b>
      </th>
      <th style="text-align:left"><b>Author</b>
      </th>
      <th style="text-align:left"><b>Peer Review</b>
      </th>
      <th style="text-align:left"><b>Quality Approval</b>
      </th>
      <th style="text-align:left"><b>Date</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">r1.0</td>
      <td style="text-align:left">Initial Release</td>
      <td style="text-align:left">RZA</td>
      <td style="text-align:left">Ivk</td>
      <td style="text-align:left">Ivk</td>
      <td style="text-align:left">06/08/2017</td>
    </tr>
    <tr>
      <td style="text-align:left">r1.1</td>
      <td style="text-align:left">Added VP868 Support</td>
      <td style="text-align:left">ANE</td>
      <td style="text-align:left">DAL</td>
      <td style="text-align:left">Pko</td>
      <td style="text-align:left">12/14/2018</td>
    </tr>
  </tbody>
</table>Abaco Systems.

This document is the property of Abaco Systems and may not be copied nor communicated to a third party without the written permission of Abaco Systems.

## Introduction

This document gives the description of the FMC432 Ethernet firmware star. This star features two MAC engines to transfer the Ethernet data from/to the two Ethernet ports of the FMC432. The main features are:

* XFI Interface \(10Gbps\)
* Receiving and transmitting Ethernet frames at 10Gbps
* Variable payload
* Configurable offload size
* Configurable inter packet frame gap
* Ramp generator and checker for analyzing the integrity of the link
* MDIO controller to control the Ethernet Transceiver PHY on the FMC432.

## Firmware Architecture

This star makes use of two XFI IP modules from Xilinx that take care of transmitting and receiving Ethernet data to/from the XFI transceiver interfaces. The Ethernet frames are passed to/from the MAC engines via 64 bit XGMII interfaces on a 156.25MHz clock deriving from the XFI IP. The frames are constructed and destructed with the modules marked red in Figure 1. The Ethernet payload data is transferred from the XFI clock domain to the AXI-S \(AXI4-Streaming\) inside the **axi\_stream\_conv** module.

![](../../.gitbook/assets/0%20%281%29.png)

Figure 1: Firmware architecture

The modules inside the receive and transmit path do have the following function:

**Receive Path**

* xgmii\_eth\_rx\_stream

Removes the XGMII characters from the incoming data stream and left aligns the data to guarantee that the first byte always starts at byte-lane 0.

* eth\_filter

Filters the incoming MAC frames. The frame is dropped if the MAC address inside the destination MAC address field of the received frame does not match with the MAC address of the MAC engine.

* eth\_rx\_crc

Calculates the CRC of the received MAC frame. The frame is dropped if the calculated CRC does not match with the CRC value inside the received MAC frame.

* eth\_rx\_detach\_payload

Removes the header, CRC, and the 6 bytes of padding from the received frame. The payload is placed on the m00\_axis bus via the **axi\_stream\_conv** module.

**Transmit Path**

* eth\_tx\_atach\_payload

This module constructs the MAC frame, adds the MAC header, the payload, 6 bytes of padding, calculates the CRC, and places the outcome in the CRC field.

* xgmii\_eth\_tx\_stream

This module converts the outgoing Ethernet stream to the XGMII format. XGMII idle characters are output by this module when no frame is transmitted.

## Using the IP

The following steps need to be taken to configure the MAC engine to start with transmitting or receiving frames. For a register description, refer to section 6.

**Note:** Please refer to the user manual of the FMC432 for information on configuring the hardware. The FMC432 must be fully operational before starting data transmission/reception.

**Setting-up the MAC engine to transmit/receive data:**

1. Reset the MAC engine
2. Configure the destination MAC address
3. Configure the payload size

_The payload size ranges from 1024 to 8192 bytes. The data that is attached to the payload field needs to be a multiple of 8 bytes. The size to be filled in as payload, is the number of data bytes plus 6. \(See_ Figure 2_.\)_

1. Configure the inter packet frame gap

_The interpacket frame gap is the delay between the MAC frames. This delay needs to be increased when the receiving side \(for example a PC\) is not able to cope with the speed of the incoming data stream._

1. Configure the offload size

_The offload size is the number of frames to be offloaded by the MAC engine._

1. Start the offload

**Setting-up the MAC engine to receive data:**

1. Reset the MAC engine
2. Configure the destination MAC address

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>xx xx xx xx xx xx</b>
        </p>
        <p><b>Destination MAC Address</b>
        </p>
      </th>
      <th style="text-align:left">
        <p><b>34 44 53 50 30 31</b>
        </p>
        <p><b>Source MAC Address</b>
        </p>
      </th>
      <th style="text-align:left">
        <p><b>90 00 00 00 00</b>
        </p>
        <p><b>Ethernet Type</b>
        </p>
      </th>
      <th style="text-align:left">
        <p><b>xx xx</b>
        </p>
        <p><b>Size</b>
        </p>
        <p><b>(Data size + 6)</b>
        </p>
      </th>
      <th style="text-align:left">
        <p><b>00 00 00 00 00 00</b>
        </p>
        <p><b>Padding</b>
        </p>
      </th>
      <th style="text-align:left"><b>Data</b>
      </th>
      <th style="text-align:left">
        <p><b>XX XX XX XX</b>
        </p>
        <p><b>CRC</b>
        </p>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>MAC Header</b>
        </p>
        <p><b>(14 Bytes)</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><b>Payload</b>
        </p>
        <p><b>(1024 to 8120 +6 Bytes)</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><b>CRC</b>
        </p>
        <p><b>(4 Bytes)</b>
        </p>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>IEEE 802.3 Ethernet Frame</b>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>Figure 2: Ethernet frame

## Star instantiation options

Depending on the target hardware configuration, it may be required to use different source and constraint files. This is supported by StellarIP through the .lst file. In most cases, a different FPGA family requires a different FPGA type. Below is a list of the current lst files and their intended usage.

 Table 1: Available lst files

| **File name** | **Target FPGA** | **Description** |
| :--- | :--- | :--- |
| axi\_fmc432\_7series.lst | 7 Series | This lst file is required when targeting a Xilinx 7 Series FPGA |
| axi\_fmc432\_ultrascale.lst | UltraScale | This lst file is required when targeting a Xilinx UltraScale FPGA |

In StellarIP, the star index is used to target a specific XDC file for the star. StellarIP always searches for the XDC file that starts with the constellation name followed by target board name and then the star index. Not all possible combinations are available, but it does not necessarily mean a specific board/FPGA type cannot be supported.

Table 2: Available XDC files

| **File name** | **Target board** | **Target FMC Site** | **Target FPGA type** |
| :--- | :--- | :--- | :--- |
| axi\_fmc432\_vc707\_0.xdc | VC707 | HPC1 | XC7V485T |
| axi\_fmc432\_kcu105\_0.xdc | KCU105 | HPC | XCKU040 |
| axi\_fmc432\_VP868\_0.xdc | VP868 | HSPC | VP868 UltraScale |

## Wormhole descriptions

This section gives a description of the wormholes of the 10GbE MAC Engine star.

### s00\_axi\_clk\_aclk wormhole of type axi\_clk\_in

| **Signal Name** | **Direction** | **Width** | **Description** |
| :--- | :--- | :--- | :--- |
| s00\_axi\_clk\_aclk | Input | 1 | The AXI4-Lite clock. The command interface is synchronous to this clock. |

### s00\_aresetn wormhole of type axi\_reset\_in

| **Signal Name** | **Direction** | **Width** | **Description** |
| :--- | :--- | :--- | :--- |
| s00\_aresetn\_aresetn | Input | 1 | The global reset signal. Active-LOW. |

### s00\_axis\_clk wormhole of type axi\_clk\_in

| **Signal Name** | **Direction** | **Width** | **Description** |
| :--- | :--- | :--- | :--- |
| s00\_axis\_clk | Input | 1 | The AXI4-Stream interfaces are synchronous to this clock. |

### s00\_axi wormhole of type axi\_in

| **Signal Name** | **Direction** | **Width** | **Description** |
| :--- | :--- | :--- | :--- |
| s00\_axi\_awaddr | Input | 32 | Write address |
| s00\_axi\_awvalid | Input | 1 | Write address valid |
| s00\_axi\_wdata | Input | 32 | Write data |
| s00\_axi\_wstrb | Input | 4 | Write strobes |
| s00\_axi\_wvalid | Input | 1 | Write valid |
| s00\_axi\_bready | Input | 1 | Response ready |
| s00\_axi\_araddr | Input | 32 | Read address |
| s00\_axi\_arvalid | Input | 1 | Read address valid |
| s00\_axi\_rready | Input | 1 | Read ready |
| s00\_axi\_awready | Output | 1 | Write address ready |
| s00\_axi\_wready | Output | 1 | Write ready |
| s00\_axi\_bresp | Output | 2 | Write response |
| s00\_axi\_bvalid | Output | 1 | Write response valid |
| s00\_axi\_arready | Output | 1 | Read address ready |
| s00\_axi\_rdata | Output | 32 | Read data |
| s00\_axi\_rresp | Output | 2 | Read response |
| s00\_axi\_rvalid | Output | 1 | Read valid |

### m0X\_axis wormhole of type axis\_256b\_out

| **Signal Name** | **Direction** | **Width** | **Description** |
| :--- | :--- | :--- | :--- |
| m0X\_axis\_tdata | Output | 256 | TDATA is the primary payload that is used to provide the data that is passing across the interface. The width of the data payload is an integer number of bytes. |
| m0X\_axis\_tstrb | Output | 32 | Not Used. \(Tied to x”00000000”\) |
| m0X\_axis\_tuser | Output | 32 | Not Used. \(Tied to x”00000000”\) |
| m0X\_axis\_tvalid | Output | 1 | TVALID indicates that the master is driving a valid transfer. A transfer takes place when both TVALID and TREADY are asserted. |
| m0X\_axis\_tlast | Output | 1 | Not Used. \(Tied to ‘0’\) |
| m0X\_axis\_tready | Input | 1 | TREADY indicates that the slave can accept a transfer in the current cycle. |

### s0X\_axis wormhole of type axis\_256b\_in

| **Signal Name** | **Direction** | **Width** | **Description** |
| :--- | :--- | :--- | :--- |
| s0X\_axis\_tdata | Input | 256 | TDATA is the primary payload that is used to provide the data that is passing across the interface. The width of the data payload is an integer number of bytes. |
| s0X\_axis\_tstrb | Input | 32 | Not Used. |
| s0X\_axis\_tuser | Input | 32 | Not Used. |
| s0X\_axis\_tvalid | Input | 1 | TVALID indicates that the master is driving a valid transfer. A transfer takes place when both TVALID and TREADY are asserted. |
| s0X\_axis\_tlast | Input | 1 | Not Used. |
| s0X\_axis\_tready | Output | 1 | TREADY indicates that the slave can accept a transfer in the current cycle. |

### axi\_fmc432\_ext of type axi\_fmc432\_ext

| **Signal Name** | **Direction** | **Width** | **Description** |
| :--- | :--- | :--- | :--- |
| clkrefout\_n/p | In | 1 | Reference clock used by the transceivers |
| xfi\_out\_p0\_n/p | Out | 1 | RX transceiver data for p0 |
| xfi\_out\_p1\_n/p | Out | 1 | RX transceiver data for p1 |
| xfi\_in\_p0\_n/p | In | 1 | TX transceiver data for p0 |
| xfi\_in\_p1\_n/p | In | 1 | TX transceiver data for p1 |
| lasin\_p0 | In | 1 | Interrupt output of port 0 from the Ethernet Transceiver |
| lasin\_p1 | In | 1 | Interrupt output of port 1 from the Ethernet Transceiver |
| mdc\_p0 | Out | 1 | Clock for the MDIO communication interface of port 0 |
| mdc\_p1 |  |  | Clock for the MDIO communication interface of port 1 |
| mdio\_p0 | In/Out | 1 | Data line for the MDIO communication interface of port 0 |
| mdio\_p1 | In/Out | 1 | Data line for the MDIO communication interface of port 1 |
| mdio\_sel | Out | 1 | Needs to be tied to ‘0’ |
| resetn\_p0 | Out | 1 | Reset signal connecting to the reset input of port 0 from the Ethernet transceiver |
| resetn\_p1 | Out | 1 | Reset signal connecting to the reset input of port 1 from the Ethernet transceiver |

### generic wormhole of type generic\_def

| **Generic Name** | **Direction** | **Width** | **Description** |
| :--- | :--- | :--- | :--- |
| global\_start\_addr\_gen | N.A. | 28 | Start of the global address range of the constellation. All stars may act on a register access to the global address range. |
| global\_stop\_addr\_gen | N.A. | 28 | End of the global address range of the constellation. All stars may act on a register access to the global address range. |
| private\_start\_addr\_gen | N.A. | 28 | Start of the private address range of the star. The star may act on a register access to the private address range. |
| private\_stop\_addr\_gen | N.A. | 28 | End of the private address range of the star. The star may act on a register access to the private address range. |

## Register Map

### Star Reserved Registers \(0x0-0x7\)

The first 8 registers of any AXI 4DSP IP Block are always the same. In this case, they are not used and are all returning 0x0.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Address Offset</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
      <th style="text-align:left"><b>Read/Write</b>
      </th>
      <th style="text-align:left"><b>Bit Map</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><em>0x00</em>
      </td>
      <td style="text-align:left">Reserved</td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">Reserved</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>0x01</em>
      </td>
      <td style="text-align:left">Reserved</td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">Reserved</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>0x02</em>
      </td>
      <td style="text-align:left">Storage Space Register</td>
      <td style="text-align:left">R</td>
      <td style="text-align:left">
        <p><b>BIT [2:0]</b>
        </p>
        <p>000 - Byte</p>
        <p>001 - Kilobyte</p>
        <p>010 - Megabyte</p>
        <p>011 - Gigabyte</p>
        <p>100 - Terabyte</p>
        <p><b>BIT [13:3]</b>
        </p>
        <p>Input Storage Size</p>
        <p><b>BIT [24:14]</b>
        </p>
        <p>Output Storage Size</p>
        <p><b>BIT [32:15]</b>
        </p>
        <p>Reserved</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em>0x03</em>
      </td>
      <td style="text-align:left">Reserved</td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">Reserved</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>0x04</em>
      </td>
      <td style="text-align:left">Reserved</td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">Reserved</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>0x05</em>
      </td>
      <td style="text-align:left">Reserved</td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">Reserved</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>0x06</em>
      </td>
      <td style="text-align:left">Reserved</td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">Reserved</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>0x07</em>
      </td>
      <td style="text-align:left">Reserved</td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">Reserved</td>
    </tr>
  </tbody>
</table>### FMC432 Control Registers \(0x08-0x1F\)

Addresses 0x08-0x1F are used to control the FMC432 star.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Address Offset</b>
      </th>
      <th style="text-align:left"><b>Register Name</b>
      </th>
      <th style="text-align:left"><b>Read/Write</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><em><b>0x08</b></em>
      </td>
      <td style="text-align:left">Reserved</td>
      <td style="text-align:left">-</td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x09</b></em>
      </td>
      <td style="text-align:left">Command</td>
      <td style="text-align:left">W (self-clearing)</td>
      <td style="text-align:left">
        <p><b>BIT0 =</b> Writing a &#x2018;1&#x2019; to this bit resets the XAUI interface
          and the MAC engine.</p>
        <p><b>BIT1 =</b> Writing a &#x2018;1&#x2019; to this bit injects an error
          word in the RAMP data transmitted on port 0.</p>
        <p><b>BIT2 =</b> Writing a &#x2018;1&#x2019; to this bit injects an error
          word in the RAMP data transmitted on port 1.</p>
        <p><b>BIT3 =</b> The MAC engine starts offloading data on port 0 after a &#x2018;1&#x2019;
          is written to this bit.</p>
        <p><b>BIT4 =</b> The MAC engine starts offloading data on port 0 after a &#x2018;1&#x2019;
          is written to this bit.</p>
        <p><b>BIT5 =</b> The MAC engine stops offloading data on port 0 after a &#x2018;1&#x2019;
          is written to this bit.</p>
        <p><b>BIT6 =</b> The MAC engine stops offloading data on port 1 after a &#x2018;1&#x2019;
          is written to this bit.</p>
        <p><b>BIT7 =</b> Port 0 of the Ethernet transceiver is being reset when a
          &#x2018;1&#x2019; is written to this bit.</p>
        <p>(This functionality first needs to be enabled in the CPLD)</p>
        <p><b>BIT8 =</b> Port 1 of the Ethernet transceiver is being reset when a
          &#x2018;1&#x2019; is written to this bit.</p>
        <p>(This functionality first needs to be enabled in the CPLD)</p>
        <p><b>BIT9 =</b> Writing a &#x2018;1&#x2019; to this bit resets the BUFG_GT
          used to generate the txusrclk and txusrclk2 clocks that are inputs to the
          PCS/PMA IP core.</p>
        <p><b>BIT10 =</b> Writing a &#x2018;1&#x2019; to this bit resets the UltraScale
          MGT QPLL used by the PCS/PMA IP core.</p>
        <p>(Other bits are reserved)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x0A</b></em>
      </td>
      <td style="text-align:left">Control</td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">
        <p><b>BIT0 =</b> Setting this bit to &#x2018;1&#x2019; enables the ramp checker
          on port 0.</p>
        <p><b>BIT1 =</b> Setting this bit to &#x2018;1&#x2019; enables the ramp checker
          on port 1.</p>
        <p><b>BIT2 =</b> Setting this bit to &#x2018;1&#x2019; enables the ramp generator
          on port 0.</p>
        <p><b>BIT3 =</b> Setting this bit to &#x2018;1&#x2019; enables the ramp generator
          on port 1.</p>
        <p>(Other bits are reserved)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x0B</b></em>
      </td>
      <td style="text-align:left">
        <p>Offload Size</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The number of frames to be offloaded by the MAC engine on port 0. (0 is
        unlimited offload)</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x0C</b></em>
      </td>
      <td style="text-align:left">
        <p>Offload Size</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The number of frames to be offloaded by the MAC engine on port 1. (0 is
        unlimited offload)</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x0D</b></em>
      </td>
      <td style="text-align:left">Link Info</td>
      <td style="text-align:left">R</td>
      <td style="text-align:left">
        <p><b>BIT0 =</b> Link on port 0 is ready when set to &#x2018;1&#x2019;.</p>
        <p><b>BIT1 =</b> Link on port 1 is ready when set to &#x2018;1&#x2019;.</p>
        <p>(Other bits are reserved)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x0E</b></em>
      </td>
      <td style="text-align:left">
        <p>Invalid Words</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">R</td>
      <td style="text-align:left">The number of invalid words detected by the ramp checker on port 0. This
        counter is reset to 0 when disabling ramp checker.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x0F</b></em>
      </td>
      <td style="text-align:left">
        <p>Invalid Words</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">R</td>
      <td style="text-align:left">The number of invalid words detected by the ramp checker on port 1. This
        counter is reset to 0 when disabling ramp checker.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x10</b></em>
      </td>
      <td style="text-align:left">Interrupt</td>
      <td style="text-align:left">R</td>
      <td style="text-align:left">
        <p><b>BIT0 =</b> An interrupt is being received from port 0 when set to &#x2018;1&#x2019;.</p>
        <p><b>BIT1 =</b> An interrupt is being received from port 1 when set to &#x2018;1&#x2019;.</p>
        <p>This register is automatically cleared after reading.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x11</b></em>
      </td>
      <td style="text-align:left">
        <p>Valid Words</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">R</td>
      <td style="text-align:left">The number of valid words detected by the ramp checker on port 0. This
        counter is reset to 0 when disabling ramp checker.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x12</b></em>
      </td>
      <td style="text-align:left">
        <p>Valid Words</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">R</td>
      <td style="text-align:left">The number of valid words detected by the ramp checker on port 1. This
        counter is reset to 0 when disabling ramp checker.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x13</b></em>
      </td>
      <td style="text-align:left">
        <p>Inter Packet Frame Gap</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The value inside this register represents the number of 64bit idle transfers
        on the XGMII bus between the frames.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x14</b></em>
      </td>
      <td style="text-align:left">
        <p>Inter Packet Frame Gap</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The value inside this register represents the number of 64bit idle transfers
        on the XGMII bus between the frames.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x15</b></em>
      </td>
      <td style="text-align:left">
        <p>Destination MAC Low</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The 4 lower bytes of the destination MAC address.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x16</b></em>
      </td>
      <td style="text-align:left">
        <p>Destination MAC High</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The 2 upper bytes of the destination MAC address.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x17</b></em>
      </td>
      <td style="text-align:left">
        <p>Destination MAC Low</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The 4 lower bytes of the destination MAC address.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x18</b></em>
      </td>
      <td style="text-align:left">
        <p>Destination MAC High</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The 2 upper bytes of the destination MAC address.</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x19</b></em>
      </td>
      <td style="text-align:left">
        <p>Source MAC Low</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The 4 lower bytes of the source MAC address. (Reset value = 0x53503031)</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x1A</b></em>
      </td>
      <td style="text-align:left">
        <p>Source MAC High</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The 2 upper bytes of the source MAC address. (Reset value = 0x3444)</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x1B</b></em>
      </td>
      <td style="text-align:left">
        <p>Source MAC Low</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The 4 lower bytes of the source MAC address. (Reset value = 0x53503030)</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x1C</b></em>
      </td>
      <td style="text-align:left">
        <p>Source MAC High</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The 2 upper bytes of the source MAC address. (Reset value = 0x3444)</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x1D</b></em>
      </td>
      <td style="text-align:left">
        <p>Payload Size Byte</p>
        <p>Port 0</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The value to be configured is data bytes that is added to the payload
        field of the Ethernet frame plus 6 (padding).</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x1E</b></em>
      </td>
      <td style="text-align:left">
        <p>Payload Size Byte</p>
        <p>Port 1</p>
      </td>
      <td style="text-align:left">W/R</td>
      <td style="text-align:left">The value to be configured is data bytes that is added to the payload
        field of the Ethernet frame plus 6 (padding).</td>
    </tr>
  </tbody>
</table>### MDIO Controller Registers \(0x28-0x37\)

Addresses 0x28-0x37 are used to control the MDIO controller inside the FMC432 star.

**Note:** the CPLD can also drive the MDIO interface to the phy \(refer to the FMC432 User Manual\). The CPLD is not actively driving the MDIO interface unless it is instructed to do so. Concurrent writes to the CPLD and from the FPGA MDIO controller must be avoided to prevent the interface bus contention.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Address Offset</b>
      </th>
      <th style="text-align:left"><b>Register Name</b>
      </th>
      <th style="text-align:left"><b>Read/Write</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><em><b>0x20</b></em>
      </td>
      <td style="text-align:left">Register Address</td>
      <td style="text-align:left">W</td>
      <td style="text-align:left"><b>BIT15-0 =</b> MDIO register address</td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x21</b></em>
      </td>
      <td style="text-align:left">MDIO Control</td>
      <td style="text-align:left">R/W</td>
      <td style="text-align:left">
        <p><b>See Register Description</b>
        </p>
        <p>(A write to this register triggers the MDIO transfer, the register must
          be written after setting the Register Address)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><em><b>0x22</b></em>
      </td>
      <td style="text-align:left">Read Data</td>
      <td style="text-align:left">R</td>
      <td style="text-align:left"><b>BIT15-0 =</b> Data read from the MDIO interface.</td>
    </tr>
  </tbody>
</table>| **Bit nr.** | **Bit 7** | **Bit 6** | **Bit 5** | **Bit 4** | **Bit 3** | **Bit 2** | **Bit 1** | **Bit 0** |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Name** | WR\_DATA |  |  |  |  |  |  |  |
| **Bit nr.** | **Bit 15** | **Bit 14** | **Bit 13** | **Bit 12** | **Bit 11** | **Bit 10** | **Bit 9** | **Bit 8** |
| **Name** | WR\_DATA |  |  |  |  |  |  |  |
| **Bit nr.** | **Bit 23** | **Bit 22** | **Bit 21** | **Bit 20** | **Bit 19** | **Bit 18** | **Bit 17** | **Bit 16** |
| **Name** | RW | PORT\_SEL | DEVICE\_ADDRESS |  |  |  |  |  |
| **Bit nr.** | **Bit 31** | **Bit 30** | **Bit 29** | **Bit 28** | **Bit 27** | **Bit 26** | **Bit 25** | **Bit 24** |
| **Name** | Rsvd |  |  |  |  |  |  |  |

Table 3: MDIO Control Register Definition

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Field</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>WR_DATA</b>
      </td>
      <td style="text-align:left">The value in this register represent the data that is written to the MDIO
        register.</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>DEVICE_ADDRESS</b>
      </td>
      <td style="text-align:left">
        <p>Manageable device address</p>
        <p><em>(Not to be confused with the MDIO port address. This is the address of the PCS, PMA, etc. controller inside the PHY. The port address is set automatically by the MDIO controller.)</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>PORT_SEL</b>
      </td>
      <td style="text-align:left">10GBASE-T port selection (Port 0 = &#x201C;10&#x201D; Port 1 = &#x201C;11&#x201D;)</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>RW</b>
      </td>
      <td style="text-align:left">Setting this bit to &#x2018;1&#x2019; triggers a read operation, setting
        this bit to &#x2018;0&#x2019; triggers a write operation.</td>
    </tr>
  </tbody>
</table>Table 4 Register MDIO Control Register Description


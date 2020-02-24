# Reference Firmware

## 1 Related Documents 

#### Abaco Systems 

[SD521 - Blast-O Control](https://abaco.gitbook.io/star-documentation/blast-o-control)  
[SD522 - Waveform Capture](https://abaco.gitbook.io/star-documentation/ipi-waveform-capture)  
[SD523 - VP430 Control](https://abaco.gitbook.io/star-documentation/vp430-control)  
[SD524 - VP430 DDR4 FIFO](https://abaco.gitbook.io/star-documentation/vp430-ddr4-fifo)  
[SD525 - AXI I2C Master](https://abaco.gitbook.io/star-documentation/axi-i2c-master)  
[SD526 - VP430 Info](https://abaco.gitbook.io/star-documentation/vp430-info)  
[SD527 - VP430 8Lane PCIe](https://abaco.gitbook.io/star-documentation/vp430-8-lane-pcie)  
[SD528 - Waveform Repeat](https://abaco.gitbook.io/star-documentation/ipi-waveform-repeat)  
[SD529 - Channel Router](https://abaco.gitbook.io/star-documentation/channel-router)

#### Xilinx

[PG021 – AXI DMA](https://www.xilinx.com/support/documentation/ip_documentation/axi_dma/v7_1/pg021_axi_dma.pdf)  
[PG059 – AXI Interconnect](https://www.xilinx.com/support/documentation/ip_documentation/axi_interconnect/v2_1/pg059-axi-interconnect.pdf)  
[PG065 – Clocking Wizard](https://www.xilinx.com/support/documentation/ip_documentation/clk_wiz/v6_0/pg065-clk-wiz.pdf)  
[PG074 – Aurora 64B/66B](https://www.xilinx.com/support/documentation/ip_documentation/aurora_64b66b/v10_0/pg074-aurora-64b66b.pdf)  
[PG085 – AXI4-Stream Infrastructure IP Suite](https://www.xilinx.com/support/documentation/ip_documentation/axis_infrastructure_ip_suite/v1_1/pg085-axi4stream-infrastructure.pdf)  
[PG144 – AXI GPIO](https://www.xilinx.com/support/documentation/ip_documentation/axi_gpio/v2_0/pg144-axi-gpio.pdf)  
[PG164 – Processor System Reset Module](https://www.xilinx.com/support/documentation/ip_documentation/proc_sys_reset/v5_0/pg164-proc-sys-reset.pdf)  
[PG185 – System Management Wizard](https://www.xilinx.com/support/documentation/ip_documentation/system_management_wiz/v1_3/pg185-system-management-wiz.pdf)  
[PG201 – Zynq UltraScale+ MPSoC Processing System](https://www.xilinx.com/support/documentation/ip_documentation/zynq_ultra_ps_e/v2_0/pg201-zynq-ultrascale-plus-processing-system.pdf)  
[PG269 – Zynq UltraScale+ RFSoC RF Data Converter](https://www.xilinx.com/support/documentation/ip_documentation/usp_rf_data_converter/v2_0/pg269-rf-data-converter.pdf)  
[UG994 – Vivado Design Suite User Guide](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2017_1/ug994-vivado-ip-subsystems.pdf)

## 2 Introduction 

This document describes the Reference Firmware for the Programmable Logic \(PL\) on the VP430. This design flow is based on the Xilinx Vivado IP Integrator \(IPI\) flow using Vivado 2018.2.1. A description of the Vivado IPI flow is beyond the scope of this document, but can be found in the Xilinx User Guide UG994: Designing IP Subsystems Using IP Integrator. This document only gives a global description of each custom IP included in the design. Detailed information can be found in the Star Document \(SD\#\#\#\) available for each custom IP.

## 3 Reference Firmware Architecture 

The VP430 is Abaco Systems’ 3U OpenVPX payload module that employs Xilinx’s Zynq UltraScale+ Radio Frequency System on Chip \(RFSoC\) device to provide high-speed, low-latency, Digital-to-Analog and Analog-to-Digital conversion. The Reference Firmware for the PL on the VP430 is architected to provide a reference for getting started with all of the interfaces that are exposed on the VP430 through the PL on the RFSoC device.

The RFSoC device is a combination of embedded micro-processors, FPGA fabric, and RF Converters. Due to the way that Xilinx integrates all of this functionality into the Vivado, SDK, and PetaLinux tools, the VP430 Reference Firmware is designed entirely inside of a Vivado IPI Block Design. It is a combination of standard IP cores from the Xilinx IP Catalog installed along with Vivado and custom IP cores designed by Abaco Systems to meet the requirements of the VP430 Reference Firmware.

The top-level block diagram for the VP430 Reference Firmware architecture can be seen in Figure 1 below:

## 4 Vivado IPI Block Design Description 

Several sections of the design are wrapped inside of hierarchical blocks, or sub-blocks, to make the Vivado IPI Block Design easier to view and easier to understand. This section will start at the top level of the Block Design, then expand into each sub-block in order to provide a brief description of each of the IP cores included in the design.

### 4.1 Top-level Block Design Description 

The top-level of the VP430 Vivado IPI Block Design is depicted in Figure 2 below. It has the following IP cores:

1. VP430 Info 
2. VP430 8-Lane PCIe 
3. VP430 DDR4 FIFO 
4. AXI Interconnect 
5. AXI4-Stream Interconnect 
6. AXI I2C Master 

#### 4.1.1 VP430 Info 

The VP430 Info IP core holds information about the design \(design ID, design revision, etc.\). Please refer to SD526 for more information on the VP430 Info IP.

#### 4.1.2 VP430 8-Lane PCIe

The VP430 8-Lane PCIe IP core distributes register read and register write commands coming from the PCIe interface on the VPX backplane. In addition, it also transfers DMA data to/from the host PC through the PCIe interface. Please refer to SD527 for more information on the VP430 8-Lane PCIe IP.

#### 4.1.3 VP430 DDR4 FIFO 

The VP430 DDR4 FIFO IP core acts as two separate FIFO buffers, each with 2GB of storage. All of the data is stored in the external DDR4 memory in blocks of configurable size. Buffer data is made available on the AXI4-Stream Master ports after one or more blocks are available in the external memory. Please refer to SD524 for more information on the VP430 DDR4 FIFO IP.

#### 4.1.4 AXI Interconnect 

From Xilinx PG059: “The Xilinx LogiCORE IP AXI Interconnect core connects one or more AXI memory-mapped master devices to one or more memory-mapped slave devices.” In this design, the AXI Interconnect has two master devices: the MPSoC Processor System \(PS\) AXI master and the VP430 8-Lane PCIe AXI master. The AXI Interconnect is used to connect both master devices to all of the memory-mapped slave devices in the design. Please refer to Xilinx PG059 for more information on the AXI Interconnect IP.

#### 4.1.5 AXI4-Stream Interconnect 

From Xilinx PG085: “Allows masters and slaves with differing AXI4-Stream characteristics to exchange AXI4-Stream transfers.” In this design, the AXI4-Stream Interconnect is used to connect seven pairs of AXI4-Stream masters and slaves: the MPSoC PS DMA Engine, the VP430 8-Lane PCIe, the VP430 DDR4 FIFO \(x2\), the Converter Routing sub-block, the BLAST-O sub-block, and the BRAM sub-block. Please refer to Xilinx PG085 for more information on the AXI4-Stream Interconnect IP.

#### 4.1.6 AXI I2C Master 

The AXI I2C Master IP core acts as the master on an I2C bus. This instance of the IP in this design is connected to the VPX P0 I2C bus. Please refer to SD525 for more information on the AXI I2C Master IP.

### 4.2 MPSoC Sub-Block Design Description 

The “mpsoc” hierarchy of the VP430 Vivado IPI Block Design is depicted in Figure 3 below. It has the following IP cores:

1. Zynq Ultrascale+ MPSoC 
2. Processor System Reset 
3. Clocking Wizard 
4. System Management Wizard 

#### 4.2.1 Zynq Ultrascale+ MPSoC 

From Xilinx PG201: “The Xilinx Zynq UltraScale+ Processing System LogiCORE IP core is the software interface around the Zynq UltraScale+ Processing System.” This IP core is used to configure all of the PS peripherals on the VP430 RFSoC device and the PS-PL interfaces. Please refer to Xilinx PG201 for more information on the Zynq UltraScale+ MPSoC IP.

#### 4.2.2 Processor System Reset 

From Xilinx PG164: “The Xilinx LogiCORE IP Processor System Reset Module core provides customized resets for an entire processor system, including the processor, the interconnect and peripherals.” There are two possible sources for this reset module in this design: the pl\_resetn0 from the MPSoC or the pcie\_sw\_aresetn from the VP430 8-Lane PCIe. The reset output is gated by the Clocking Wizard’s clocks being locked and synchronized to the 125MHz register bus clock. The reset generated by this module is used throughout the design by all of the AXI and AXI4-Stream interconnect peripherals. Please refer to Xilinx PG164 for more information on the Processor System Reset IP.

#### 4.2.3 Clocking Wizard 

From Xilinx PG065: “The LogiCORE IP Clocking Wizard core simplifies the creation of HDL source code wrappers for clock circuits customized to your clocking requirements.” This IP core expects a 125MHz source clock from the MPSoC pl\_clk0 and generates a 125MHz register bus clock and a 250MHz AXI4-Stream bus clock. Please refer to Xilinx PG065 for more information on the Clocking Wizard IP.

#### 4.2.4 System Management Wizard 

From Xilinx PG185: “The LogiCORE IP System Management Wizard provides a complete solution for system-monitoring Xilinx UltraScale devices. This IP generates an HDL wrapper to configure the SYSMON for user-specified external channels, internal sensor channels, modes of operation and alarms.” The On-Chip Sensors enabled in this design are the Temperature, Vccint, Vccaux, and Vccbram. The four VUSER0-VUSER3 supplies are also enabled to monitor the ADC AVCC, ADC AVCCAUX, DAC AVCC, and DAC AVCCAUX supplies, respectively. Please refer to Xilinx PG185 for more information on the System Management Wizard IP.

### 4.3 DMA Engine Sub-Block Design Description 

The “dma\_engine” hierarchy of the VP430 Vivado IPI Block Design is depicted in Figure 4 below. It has the following IP cores:

1. AXI Direct Memory Access \(AXI DMA\) 
2. AXI4-Stream Data FIFO 
3. AXI Interconnect 
4. AXI GPIO 
5. axi4stream\_1kb\_tlast\_gen 

#### 4.3.1 AXI Direct Memory Access \(AXI DMA\) 

From Xilinx PG021: “The AXI DMA provides high-bandwidth direct memory access between memory and AXI4-Stream target peripherals.” There are two instances of this IP in the design to achieve DMA between the PS and the PL: one for DMA reads \(DMA transmit\) and one for DMA writes \(DMA receive\). The AXI4-Stream ports of these AXI DMA IP cores are connected to AXI4-Stream Data FIFOs before connecting to the rest of the PL design. The AXI Memory Mapped ports of these AXI DMA IP cores are connected to the PS memory through an AXI Interconnect. Please refer to Xilinx PG021 for more information on the AXI DMA IP.

#### 4.3.2 AXI4-Stream Data FIFO 

From Xilinx PG085: “The FIFO module is capable of providing temporary storage \(a buffer\) of the AXI4-Stream data.” There are two instances of this IP in the design: one as a buffer for DMA reads \(DMA transmit\) and one as a buffer for DMA writes \(DMA receive\). The axis\_data\_count ports of these IP cores are connected to an AXI GPIO to report how much data is available in each. Please refer to Xilinx PG085 for more information on the AXI4-Stream Data FIFO IP.

#### 4.3.3 AXI Interconnect 

In this hierarchy, the AXI Interconnect has two master devices: the AXI DMA transmit and the AXI DMA receive. The AXI Interconnect is used to connect both master devices to the PS memory.

#### 4.3.4 AXI GPIO 

From Xilinx PG144: “The Xilinx LogiCORE IP AXI General Purpose Input/Output \(GPIO\) core provides a general purpose input/output interface to the AXI interface.” This instance of the IP core is configured in dual channel mode to input the axis\_data\_count from both AXI4-Stream Data FIFOs. Please refer to Xilinx PG144 for more information on the AXI GPIO IP.

#### 4.3.5 AXI4-Stream 1KB TLAST Generator 

The AXI4-Stream 1KB TLAST Generator IP core is a simple component which takes an AXI4-Stream slave bus without a TLAST signal and generates a TLAST on the AXI4-Stream master bus for every 1KB of data transferred. This is necessary because the AXI4-Stream Interconnect in the design doesn’t use the TLAST signal but the AXI DMA in Write mode requires the assertion of TLAST to function properly.

### 4.4 RF Converters Sub-Block Design Description 

The “rf\_converters” hierarchy of the VP430 Vivado IPI Block Design is depicted in Figure 5 below. It has the following IP cores:

1. Zynq Ultrascale+ RF Data Converter 
2. I/Q Interleaver 

#### 4.4.1 Zynq Ultrascale+ RF Data Converter 

From Xilinx PG269: “The Xilinx LogiCORE IP Zynq UltraScale+ RFSoC RF Data Converter IP core provides a configurable wrapper to allow the RF-DAC and RF-ADC blocks to be used in IP integrator designs.” This IP core is how the RFSoC device’s RF Converters are exposed to the rest of the design. There are many configuration settings in this IP core, some of which are static, pre-configured options that impact the architecture of the Reference Firmware design while others are just the default settings on start-up which can be dynamically reconfigured through the Xilinx RFdc Driver API.

In this design, all 8 ADCs and 8 DACs are configured to run at the maximum sample rate of 4GHz and 6.4GHz, respectively. None of the internal PLLs are enabled by default requiring the external analog clock tree to provide the necessary sampling clocks. All of the Decimation and Interpolation Modes are enabled with an 8x factor to support a single-channel streaming example via PCIe DMA. Because the Digital Down Converters \(DDCs\) and Digital Up Converters \(DUCs\) are used, the AXI4-Stream digital data streams are configured to be I/Q complex data. Screen shots of the ADC and DAC Tile configuration settings are included below for reference. Please refer to Xilinx PG269 for more information on the Zynq UltraScale+ RF Data Converter IP.

#### ADC Configuration 

#### DAC Configuration 

#### 4.4.2 I/Q Interleaver 

The I/Q Interleaver IP core is a simple component which is designed to take a 128-bit AXI4-Stream bus for I data and one for Q data and generate an I/Q interleaved 256-bit AXI4-Stream bus. This allows for the complex ADC AXI4-Stream data to be formatted the same as the complex DAC AXI4-Stream data in the design with a 256-bit bus comprised of 16 total samples \(8x I samples, 8x Q samples\): \[Q7,I7…Q0,I0\].

### 4.5 Converter Routing Sub-Block Design Description 

The “converter\_routing” hierarchy of the VP430 Vivado IPI Block Design is depicted in Figure 6 below. This hierarchy is considered the “Signal Processing Placeholder” of the design as it could be replaced with an end user’s custom DSP application logic. It has the following IP cores:

1. Channel Router 
2. Waveform Capture 
3. Waveform Repeat 
4. AXI Interconnect 

#### 4.5.1 Channel Router 

The Channel Router IP core is used to route AXI4-Stream channel data in a more custom way than the Xilinx AXI4-Stream Interconnect IP. The main features included in this IP are individual converter channel selection for both ADC and DAC streams between the RF Data Converter IP and the AXI4-Stream Interconnect in the design and ADC-to-DAC channel loopback. Please refer to SD529 for more information on the Channel Router IP.

#### 4.5.2 Waveform Capture 

The Waveform Capture IP core is used to capture and store AXI4-Stream data. The main features included in this IP are capture and burst control, triggering, and a zero-delay bypass path. The design includes one Waveform Capture IP per ADC channel. Please refer to SD522 for more information on the Waveform Capture IP.

#### 4.5.3 Waveform Repeat 

The Waveform Repeat IP core is used to store and continuously repeat play-back of AXI4-Stream data. The main features included in this IP are waveform storage and repeat control, triggering, and a zero-delay bypass path. The design includes one Waveform Repeat IP per DAC channel. Please refer to SD528 for more information on the Waveform Repeat IP.

#### 4.5.4 AXI Interconnect 

In this hierarchy, the AXI Interconnect has one master device which is connected to the Top-Level AXI Interconnect for the main system control. There are 17 slaves which are connected to the 8 Waveform Capture IP cores, 8 Waveform Repeat IP cores, and the Channel Router IP core.

### 4.6 Front End Control Sub-Block Design Description 

The “front\_end\_control” hierarchy of the VP430 Vivado IPI Block Design is depicted in Figure 7 below. It has the following IP cores:

1. Processor System Reset 
2. Clocking Wizard
3. AXI Interconnect 
4. VP430 Control 

#### 4.6.1 Processor System Reset 

In this hierarchy, there are two instances of the Processor System Reset IP core: one for the ADC AXI4-Stream clock and one for the DAC AXI4-Stream clock. There are two possible sources for these two reset modules: the interconnect reset from the Top-Level Block Design or the reset\_data\_paths signal from the VP430 Control. The resets generated by these modules are used as the AXI4-Stream resets on the RF Data Converter IP core’s ADC and DAC channels.

#### 4.6.2 Clocking Wizard 

In this hierarchy, there are two instances of the Clocking Wizard IP core: one for the ADC AXI4-Stream clock and one for the DAC AXI4-Stream clock. The input clock source for both of these IP cores is the pl\_clock\_out clock from the VP430 Control. The clocks generated by these modules are used as the ADC and DAC AXI4-Stream clocks to the RF Converters sub-block, Converter Routing sub-block, and as the sysref\_sample\_clk and trig\_clk inputs to the VP430 Control. The ADC clock is configured as 62.5MHz by default and the DAC clock is configured as 100MHz by default to match with the default RF Data Converter requirements. These Clocking Wizard IP cores are also configured with the AXI4-Lite Dynamic Reconfig option to allow for additional flexibility in the clocking architecture.

#### 4.6.3 AXI Interconnect 

In this hierarchy, the AXI Interconnect has one master device which is connected to the Top-Level AXI Interconnect for the main system control. There are three slaves which are connected to the two Clocking Wizard IP cores and the VP430 Control IP core.

#### 4.6.4 VP430 Control 

The VP430 Control IP core is used for configuring the analog clock tree on the VP430. The main features included in this IP are SPI bus controllers, external trigger sampling, PL\_SYSREF sampling, and frequency counters for internal FPGA clocks. Please refer to SD523 for more information on the VP430 Control IP.

### 4.7 P2 GPIO Sub-Block Design Description

The “p2\_gpio” hierarchy of the VP430 Vivado IPI Block Design is depicted in Figure 8 below. It has the following IP cores:

1. AXI GPIO 

#### 4.7.1 AXI GPIO 

In this hierarchy, there are two instances of the AXI GPIO IP core. One is used for individual control of the 4x Single-Ended \(SE\) VPX P2 connections and one is used for individual control of the 16x LVDS VPX P2 connections \(pinned out as SE for a total of 32 I/O\).

### 4.8 BLAST-O Sub-Block Design Description 

The “blast\_o” hierarchy of the VP430 Vivado IPI Block Design is depicted in Figure 9 below. It has the following IP cores:

1. Aurora 64B66B
2. BLAST-O Control 
3. AXI I2C Master 

#### 4.8.1 Aurora 64B66B 

From Xilinx PG074: “The Xilinx LogiCORE IP Aurora 64B/66B core is a scalable, lightweight, high data rate, link-layer protocol for high-speed serial communication.” This IP core is used to transmit high-speed serial data to the BLAST-O site on the VP430. Please refer to Xilinx PG074 for more information on the Aurora 64B66B IP.

#### 4.8.2 BLAST-O Control 

The BLAST-O Control IP core is used to complement the Xilinx Aurora 64B66B IP core in interfacing to the BLAST-O site on the VP430. The main features included in this IP are data payload AXI4-Streams, User Flow Control, and general status and control to the Aurora 64B66B IP core. It also includes control of the BLAST-O FireFly sideband signals. Please refer to SD521 for more information on the BLAST-O Control IP.

#### 4.8.3 AXI I2C Master 

In this hierarchy, the AXI I2C Master is connected to the BLAST-O FireFly I2C bus.

### 4.9 BRAM Sub-Block Design Description 

The “bram\_block” hierarchy of the VP430 Vivado IPI Block Design is depicted in Figure 10 below. It has the following IP cores:

1. AXI Interconnect 
2. AXI GPIO 
3. AXI4-Stream Data FIFO 

#### 4.9.1 AXI Interconnect 

In this hierarchy, the AXI Interconnect has one master device which is connected to the Top-Level AXI Interconnect for the main system control. There are two slaves which are connected to the two AXI GPIO IP cores.

#### 4.9.2 AXI GPIO 

In this hierarchy, there are two instances of the AXI GPIO IP core. One is configured for a single output signal with no interrupt and the other is configured for a single input signal with an interrupt to detect a change in state on the input. These two AXI GPIO IP cores are used to assert an interrupt to the PS whenever data is available in the AXI4-Stream Data FIFO.

#### 4.9.3 AXI4-Stream Data FIFO 

In this hierarchy, the AXI4-Stream Data FIFO is used as a generic 4KB block of memory that can be used to pass data between the PL and the PS. The Abaco Systems RFAPI Open Source Library \(OSL\) software and “hostif” PetaLinux application use this generic 4KB block of memory to exchange information. A primary function for this information exchange is to initiate Xilinx XRFdc driver calls in the hostif application running in the PS from application software controlling the VP430 through PCIe in the PL.

## 5 Register Map 

The individual register maps for each of the IP offsets below can be found in the corresponding IP-level documentation. This register map is only intended to document how all of the IP with register spaces in the Reference Firmware get mapped in the Vivado IPI Address Editor.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Offset Address</th>
      <th style="text-align:left">IP Name</th>
      <th style="text-align:left">Offset Range</th>
      <th style="text-align:left">Register Map Documentation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">0x8000_0000</td>
      <td style="text-align:left">AXI GPIO
        <br />(axi_gpio_bram_intr_out)</td>
      <td style="text-align:left">
        <p>4KB
          <br />
        </p>
        <p>0x8000_0000 to 0x8000_0FFF</p>
      </td>
      <td style="text-align:left"><a href="https://www.xilinx.com/support/documentation/ip_documentation/axi_gpio/v2_0/pg144-axi-gpio.pdf">Xilinx PG144</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">0x8000_1000</td>
      <td style="text-align:left">
        <p>AXI GPIO</p>
        <p>(axi_gpio_bram_intr_in)</p>
      </td>
      <td style="text-align:left">
        <p>4KB
          <br />
        </p>
        <p>0x8000_1000 to</p>
        <p>0x8000_1FFF</p>
      </td>
      <td style="text-align:left"><a href="https://www.xilinx.com/support/documentation/ip_documentation/axi_gpio/v2_0/pg144-axi-gpio.pdf">Xilinx PG144</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">0x8000_2000</td>
      <td style="text-align:left">VP430 Info</td>
      <td style="text-align:left">4KB
        <br />
        <br />0x8000_2000 to 0x8000_2FFF</td>
      <td style="text-align:left"><a href>Abaco Systems SD526</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">0x8004_0000</td>
      <td style="text-align:left">AXI DMA
        <br />(axi_dma_receive)</td>
      <td style="text-align:left">
        <p>64KB
          <br />
        </p>
        <p>0x8004_0000 to 0x8004_FFFF</p>
      </td>
      <td style="text-align:left"><a href="https://www.xilinx.com/support/documentation/ip_documentation/axi_dma/v7_1/pg021_axi_dma.pdf">Xilinx PG021 </a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">0x8005_0000</td>
      <td style="text-align:left">AXI DMA
        <br />(axi_dma_transmit)</td>
      <td style="text-align:left">
        <p>64KB</p>
        <p></p>
        <p>0x8005_0000 to 0x8005_FFFF</p>
      </td>
      <td style="text-align:left"><a href="https://www.xilinx.com/support/documentation/ip_documentation/axi_dma/v7_1/pg021_axi_dma.pdf">Xilinx PG021</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">0x8006_0000</td>
      <td style="text-align:left">
        <p>AXI GPIO</p>
        <p>(axi_gpio_rx_tx_data_count)</p>
      </td>
      <td style="text-align:left">
        <p>4KB</p>
        <p></p>
        <p>0x8006_0000 to 0x8006_0FFF</p>
      </td>
      <td style="text-align:left"><a href="https://www.xilinx.com/support/documentation/ip_documentation/axi_gpio/v2_0/pg144-axi-gpio.pdf">Xilinx PG144</a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">0x8006_1000</td>
      <td style="text-align:left">AXI4-Stream Interconnect</td>
      <td style="text-align:left">
        <p>4KB</p>
        <p></p>
        <p>0x8006_1000 to 0x8006_1FFF</p>
      </td>
      <td style="text-align:left"><a href="https://www.xilinx.com/support/documentation/ip_documentation/axis_infrastructure_ip_suite/v1_1/pg085-axi4stream-infrastructure.pdf">Xilinx PG085 </a>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">0x8006_2000</td>
      <td style="text-align:left">
        <p>AXI GPIO</p>
        <p>(axi_gpio_p2_lvds)</p>
      </td>
      <td style="text-align:left">
        <p>4KB</p>
        <p></p>
        <p>0x8006_2000 to 0x8006_2FFF</p>
      </td>
      <td style="text-align:left"><a href="https://www.xilinx.com/support/documentation/ip_documentation/axi_gpio/v2_0/pg144-axi-gpio.pdf">Xilinx PG144</a>
      </td>
    </tr>
  </tbody>
</table>## 6 Implementing the Reference Firmware

The Reference Firmware for the PL on the VP430 comes with a Tool Command Language \(TCL\) script that can be run in Vivado 2018.2.1 to generate the complete Vivado project for the VP430. The “755\_vp430\_axi\_prj.tcl” script must be run from the root directory of the Reference Firmware project directory. Vivado 2018.2.1 must be installed, licensed, and launched before it is possible to run the TCL script. You can source the TCL script in the Vivado GUI by selecting “Tools-&gt;Run Tcl Script…” or by entering the following command into the Tcl Console:

`source <Install Dir>/755_vp430_axi/755_vp430_axi_prj.tcl`

The result of running this TCL script will be a directory called “vp430\_axi” which includes the Vivado project with all of its local project directories. The IP Repository path for the generated project will be pointing to the original IP install directory. Below is a list of the project installation directories and what they contain:

* **755\_vp430\_axi**: Root project installation directory
  * **ip:** Custom IP Repository directory for VP430 Vivado project
    * **&lt;IP Name&gt;**: Custom IP directory for all IP-level files 
      * **doc**: IP-Level documentation 
      * **src**: All VHDL and XCI source files for IP 
      * **xgui**: Custom IP TCL script 
      * **component.xml**: Vivado IP definition file 
  * **src**: Top-level source files for VP430 Vivado project 
    * **vp430\_top.vhd**: Top-level VHDL for the project 
    * **vp430\_top.xdc**: Top-level XDC constraints for the project 
    * **vp430\_bd\_wrapper.vhd**: Block Design wrapper file instantiated by vp430\_top 
  * **755\_vp430\_axi\_proj.tcl**: TCL script for generation the VP430 Vivado project 

After the Vivado project has been successfully generated, the VP430 Reference Firmware can be compiled into a bitfile by selecting the “Generate Bitstream” option in the Vivado Flow Navigator. After the programming file has been successfully generated with no errors, the resulting bitfile “vp430\_top.bit” can be found in:

`<Install Dir>\755_vp430_axi\vp430_axi\vp430_axi.runs\impl_1\`

The Xilinx SDK and PetaLinux tools will also require a Hardware Description File \(HDF\) for this Reference Firmware design which can be generated by Vivado by selecting “File-&gt;Export-&gt;Export Hardware” and enabling the “Include bitstream” option. If you keep the default &lt;Local to Project&gt; option, then the resulting HDF file “vp430\_top.hdf” can be found in:

`<Install Dir>\755_vp430_axi\vp430_axi\vp430_axi.sdk\`

## 7 Resource Utilization 

This section shows the resource utilization of the full design and per individual IP. Note that these numbers should be used as ballpark numbers since they might vary a bit once a designer puts additional logic in the design. It should also be noted that a significant percentage of the overall utilization is contained within the “Signal Processing Placeholder” block \(Converter Routing\). The figure below shows the overall resource utilization in the design:

The Vivado Device view of the implemented design was used with highlighting leaf cells to provide more context for how the major blocks in the design get placed and routed along with their individual resource utilization:.

## 8 Vivado Long Path Names Error 

Running Vivado in Windows will often result in a maximum path length limitation \(of 260 characters\) which requires the creation of a virtual drive mapping to the project folder. You can map a folder to a specific drive using the “subst” command to work around the path length limitation:

1. Open a command prompt 
2. Type subst, a space and then the drive letter you want to use 
3. Type the path you want to substitute 
4. You now have a new drive in your Windows Explorer. Open your Vivado Project from this drive 

Example to map drive V: 

```text
> subst V: C:\Projects\755_vp430_axi 
```

To delete the drive, you should type the following command: 

```text
> subst V: /D
```

\`\`


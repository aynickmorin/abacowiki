# Petalinux BSP

## General Description <a id="general-description"></a>

‌

The VP430 Software Board Support Package \(BSP\) includes two components. The first component is the software and associated PetaLinux BSP running on the Zynq RFSoC processor. The second component is VP430 BSP running on either a Windows or Linux host machine.‌

The Zynq PetaLinux BSP features all control and interface with the onboard peripherals such as Ethernet ports, AXI interconnects and the ARM processors. It also features communication with the single board computer using RFAPI through TCP/IP or PCIe communication.‌

The VP430 BSP resides on the host computer in either Windows or Linux. An application uses the RFAPI interface on the host computer to communicate with the Zynq RFSoC to access the FPGA and any firmware functionalities.​MISSING IMAGE![Asset no longer exists](https://static.gitbook.com/images/e372a3642662304a70a27e252a9a2ed5.svg)‌

Figure 1 shows the architecture of the entire system. The User Application communicates with the RFAPI on the host computer. The RFAPI on the host computer uses the 4FM library or the TCP/IP link to communicate with another device. The Zynq RFSoC processors run a custom Linux kernel to allow for communication with the onboard Ethernet ports using TCP/IP. RFAPI commands are sent to a user-level application running on the Zynq RFSoC processors under PetaLinux.‌

## Quick Start: VP430 <a id="quick-start-vp430"></a>

‌

Please refer to the SG021 document \(VP430 Getting Started Guide\) for all relevant information on how to get started with the VP430.‌

## Host side Board Support Package <a id="host-side-board-support-package"></a>

‌

The host side BSP is composed of an API, RFAPI, as well as reference applications. vp430\_refapp demonstrates how to control and use a VP430 board. vp430\_qspi\_programming allows users an easy way to reprogram the QuadSPI non-volatile memory present on the VP430‌

### RFAPI <a id="rfapi"></a>

‌

RFAPI is delivered as an opensource library. Precompiled binaries are also provided for user’s convenience. This API is able to communicate with a VP430 board via either TCP/IP or PCIe. Please refer to UM090 \(RFAPI user manual\) for more information.‌

### VP430\_REFAPP <a id="vp-430-_refapp"></a>

‌

This vp430\_refapp application configures all the IP Cores in the design so the VP430 can play waveforms on the D/A converters and acquire waveforms on from the A/D converters. The application was developed for a loop back scenario. Every D/A output is connected to the A/D input using loopback cables.‌

The application will upload D/A waveform to the D/A converters waveform memory from ASCII files, acquire waveforms from the A/D converters and finally save the waveforms as ASCII files on the hard disk.‌

When using the PCIe interface, this application will also perform tests on the DDR memory attached to the PL portion of the RFSOC device.‌

#### Running VP430\_REFAPP <a id="running-vp-430-_refapp"></a>

‌

Usage:‌

`vp430_refapp {interface type} {IP address/Device Type} {DDC Frequency (MHz)} {DUC Frequency (MHz)} {Clock Mode}`‌

`{interface type}` can be either 0 \(PCIe\) or 1 \(TCP/IP\)‌

`{IP address/Device Type}` should be a device type such as VP430 or an IP address such as 192.168.1.10‌

 `{DDC Frequency (MHz)}` is the DDC frequency \(MHz\) to apply to all ADC channels. Valid Range is -Fs/2: Fs/2‌

`{DUC Frequency (MHz)}` is the DUC frequency \(MHz\) to apply to all DAC channels. Valid Range is -Fs/2: Fs/2‌

 `{Clock Mode}` can be either 0 \(internal clock, internal reference\), 1 \(internal clock, external reference or 2 \(external clock\)‌

Examples:‌

 `vp430_refapp 0 VP430 -1000 1000 0`‌

 `vp430_refapp 1 192.168.1.22 -1000 1000 0`‌

### VP430\_QSPI\_PROGRAMMING <a id="vp-430-_qspi_programming"></a>

‌

This application can program both kernel \(usually named image.ub\) images and boot \(usually named BOOT.BIN\) images to the QSPI flash memory present on the VP430. More details about that in the QSPI programming chapter.‌

### Xilinx RFDC API <a id="xilinx-rfdc-api"></a>

‌

Xilinx has made available an API, the RFDC API, to configure and fine tune the A/D and D/A converters. Abaco made available RFAPI functions to control the RFDC API through PCIe or TCP/IP. These functions are not documented in the UM090 RFAPI User Manual because they are architecture specific This chapter documents the functions available on the VP430 BSP. Note that an additional header file needs to be included in your C/C++ code. This header file contains declaration for those functions.‌

`#include <RFAPI_rfdc.h>`‌

#### RFAPI\_RFDC\_Set\_Nyquist <a id="rfapi_rfdc_set_nyquist"></a>

‌

`RFAPI_STATUS RFAPI_RFDC_Set_Nyquist(RFAPI_HANDLE *devhandle, uint32_t mask_adc_tiles, uint32_t nyquist_zone);`‌

**Description**‌

Configure Odd/Even zone operation on a given set of A/D tiles.‌

**Parameters**‌

`devhandle` a handle to a VP430 device instance previously opened from `RFAPI_DeviceInit()`.‌

`mask_adc_tiles` is a mask on which A/D tiles to be configured. As an example 0xF will configure all the A/D tiles available.‌

`Nyquist_zone` is the zone to be configured for all the `mask_adc_tiles` passed as argument. This argument can be either `RFDC_ODD_NYQUIST_ZONE` or `RFDC_EVEN_NYQUIST_ZONE`‌

#### RFAPI\_RFDC\_MTS\_Synchronize <a id="rfapi_rfdc_mts_synchronize"></a>

‌

**Description**‌

Synchronize A/D and D/A tiles.‌

**Parameters**‌

devhandle _a handle to a VP430 device instance previously opened from RFAPI\_DeviceInit\(\)._‌

#### RFAPI\_RFDC\_Set\_Coarse\_Mixer <a id="rfapi_rfdc_set_coarse_mixer"></a>

‌

`RFAPI_STATUS RFAPI_RFDC_Set_Coarse_Mixer(RFAPI_HANDLE *devhandle, uint32_t mask_adc_tiles, uint32_t mask_dac_tiles, uint32_t mixer_mode);`‌

**Description**‌

Configure A/D and D/A tiles coarse mixer operations.‌

**Parameters**‌

`devhandle` a handle to a VP430 device instance previously opened from RFAPI\_DeviceInit\(\).‌

`mask_adc_tiles` is a mask on which A/D tiles to be configured. As an example 0xF will configure all the A/D tiles available.‌

`mask_dac_tiles` is a mask on which A/D tiles to be configured. As an example 0x3 will configure all the D/A tiles available.‌

`mixer_mode` is the coarse mixer mode to be configured. `RFDC_COARSE_MIXER_BYPASS` no coarse mixer, `RFDC_COARSE_MIXER_FS_DIV_2` is sampling frequency divided by 2, `RFDC_COARSE_MIXER_FS_DIV_4` is sampling frequency divided by 4 and `RFDC_COARSE_MIXER_MIN_FS_DIV_4` is sampling frequency divided my minus 4.‌

#### RFAPI\_RFDC\_Set\_Fine\_Mixer <a id="rfapi_rfdc_set_fine_mixer"></a>

‌

`RFAPI_STATUS RFAPI_RFDC_Set_Fine_Mixer(RFAPI_HANDLE *devhandle, uint32_t mask_adc_tiles, uint32_t mask_dac_tiles, float ddc_nco_freq_mhz, float duc_nco_freq_mhz);`‌

**Description**‌

Configure A/D and D/A tiles fine mixer operations.‌

**Parameters**‌

`devhandle` a handle to a VP430 device instance previously opened from RFAPI\_DeviceInit\(\).‌

`mask_adc_tiles` is a mask on which A/D tiles to be configured. As an example 0xF will configure all the A/D tiles available.‌

`mask_dac_tiles` is a mask on which A/D tiles to be configured. As an example 0x3 will configure all the D/A tiles available.‌

`ddc_nco_freq_mhz` is the frequency in MHZ of the DDC.‌

`duc_nco_freq_mhz` is the frequency in MHZ of the DUC.‌

#### RFAPI\_RFDC\_Set\_Decimation\_Factor <a id="rfapi_rfdc_set_decimation_factor"></a>

‌

`RFAPI_STATUS RFAPI_RFDC_Set_Decimation_Factor(RFAPI_HANDLE *devhandle, uint32_t mask_adc_tiles, uint32_t decimation_factor);`‌

**Description**‌

Configure the decimation factor for A/D tiles.‌

**Parameters**‌

`devhandle` a handle to a VP430 device instance previously opened from RFAPI\_DeviceInit\(\).‌

`mask_adc_tiles` is a mask on which A/D tiles to be configured. As an example 0xF will configure all the A/D tiles available.‌

`Decimation_factor` can be `RFDC_DECIMATION_OFF` for no decimation, `RFDC_DECIMATION_1x` for decimation 1x, `RFDC_DECIMATION_2X` for decimation 2x, `RFDC_DECIMATION_4X` for decimation 4x and `RFDC_DECIMATION_8X` for decimation 8x.‌

## Zynq RFSoC PetaLinux Board Support Package <a id="zynq-rfsoc-petalinux-board-support-package"></a>

‌

### BSP Package and Directory Structure <a id="bsp-package-and-directory-structure"></a>

‌

The VP430\_2018.1\_&lt;revision&gt;.bsp board support package is based on PetaLinux 2018.1.‌

The required software to build and use PetaLinux:‌

* ​[Petalinux SDK 2018.1](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools.html)​
* ​[Xilinx Software Development Kit XSDK 2018.1](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools.html)​

‌

The first step is to import the _VP430\_2018.1\_&lt;revision&gt;.bsp_ package file into a working copy of the PetaLinux build system. Import the _**VP430\_2018.1\_&lt;revision&gt;.bsp**_ file using the command:‌

`petalinux-create -t project -s <path-to-bsp-file> -n <proj_name>`‌

This will generate a directory named _**`<proj_name>`**_ in the current working directory with the directory structure listed in Figure 2.​​‌

Figure 2: Petalinux project directory structure‌

The &lt;proj\_name&gt;/components/ext\_sources folder contains the custom PetaLinux 2018.1 kernel source base and the u-boot source base.‌

The &lt;proj\_name&gt;/components/plnx\_workspace folder contains the default device tree configuration and the first stage bootloader source base.‌

The &lt;proj\_root&gt;/project-spec/meta-user contains the user specific additions to the Yocto bitbake build system.‌

This folder contains the apps, libraries and drivers needed to run the Abaco software interface framework on the Zynq RFSoC.‌

### Linux <a id="linux"></a>

‌

The customized kernel included in the BSP is a modified version of the standard PetaLinux 2018.1 kernel. These modifications are necessary to support the custom hardware configuration present on the VP430.‌

### U-Boot <a id="u-boot"></a>

‌

In addition to onboard non-volatile memory, U-Boot allows the user to boot a kernel image from a TFTP server. First, stop the boot process when the count down in U-Boot appears on the terminal, then run:‌

`setenv serverip <tftp-server-ip-address>`‌

`setenv ipaddr <ip-address>`‌

`saveenv`‌

Use the _`run netboot`_ command in the U-Boot prompt to boot from the TFTP server on your host machine.‌

U-Boot environment variables are stored in the environment partition in QSPI flash. You may overwrite the settings in order to have the configuration loaded from flash every time u-boot starts. The _**saveenv**_ command writes the new values to the non-volatile memory.‌

For example, to start _netboot_ as well as have the default fallback _qspi boot_ if netboot fails enter the following command in the U-Boot prompt:‌

`setenv default_bootcmd ‘run netboot || run qspi_boot’`‌

`saveenv`‌

### First Stage Boot Loader <a id="first-stage-boot-loader"></a>

‌

The FSBL is the first initialization code for the Zynq MPSoC PS system. The source files are generated based on pre-configuration settings imported from the Vivado HDF file and a base source library provided by the PetaLinux SDK.‌

### Modified GEM Kernel module <a id="modified-gem-kernel-module"></a>

‌

The Xilinx Gigabit Ethernet driver module was modified to support in-band status signaling via SGMII. An additional Device Tree entry was created to support enabling this feature:‌

* **managed = “in-band-status”;**
* This entry is only valid when the PHY mode is SGMII. This configures the GEM MAC driver to use in-band SGMII status signaling instead of an MDIO bus connection.

‌

### AXI DMA Kernel module <a id="axi-dma-kernel-module"></a>

‌

The AXI DMA module accesses the DMA control registers on Zynq RFSoC PL firmware to send and receive data to the RFSoC PL. The DMA module is polling driven with sleeps in between the accesses.‌

### RTM31x Kernel module <a id="rtm-31-x-kernel-module"></a>

‌

A new kernel module \(driver\) was added to configure the optional RTM31x rear transition module. The required device tree entries for this module and the RTM31x I2C are:

```text
&i2c1 {  status = "okay";  clock-frequency = <100000>;  i2c-mux@70 {    compatible = "nxp,pca9548";    reg = <0x70>;    #address-cells = <1>;    #size-cells = <0>;    i2c@0 {      #address-cells = <1>;      #size-cells = <0>;      reg = <0x0>;      abaco_rtm31x: abaco_rtm31x@10 {        compatible = "abaco,rtm31x";        reg = <0x10>;        #address-cells = <1>;        #size-cells = <0>;        vp430-mode;      };    };  };};
```

‌

* The `clock-frequency = <100000>;`is important as the RTM31x does not work correctly when the I2C clock is the default 400KHz.
* The `vp430-mode` line tells the kernel module to switch the RTM31x to "VP430 mode”. Currently this just changes the PHY for eth0 to enable auto negotiation to work with the VP430. If this line is not there, the driver will leave the RTM31x in a mode that is compatible with the VP880.
* The mux portion is important in that it sets up the appropriate drivers to automatically control the I2C MUX to connect to the correct downstream I2C bus. Eight \(8\) new I2C buses \(I2C2 through I2C9\) are created to represent the I2C bus segments from the PCA9548 I2C switch. I2C2 represents the bus segment connected to “Port 1” shown in the RTM31x User Manual \(UM081 and UM082\). I2C3 corresponds to Port 2, etc. Note that I2C6 through I2C9 are currently not used by the RTM31x.
* A “SYSFS” file node is created by the device driver when loaded named /sys/bus/i2c/devices/&lt;RTM I2C device name&gt;/vp430\_mode
  * Write a “1” to this file to enable VP430 mode
  * Write a “0” to return the RTM31x to default \(VP880\) mode.
  * Reading from this will return “1” or “0” based upon current mode

‌

###  libzynqMP4dsp <a id="libzynqmp-4-dsp"></a>

‌

This is the library directory for the API interfaces between the host application on the host machine \(Single Board Computer or other setups\) and the Zynq RFSoC host software that translates RFAPI commands to access the AXI memory mapped addresses on the Zynq RFSoC.‌

####  `libzynqMP4dsp.c` <a id="libzynqmp-4-dsp-c"></a>

‌

This file contains the various API function calls used by the user software on the Zynq RFSoC to communicate between the Host Computer commands and the internal address spaces required. An interface to the PCIe root-complex is available as well as communication to the SPI and I2C peripherals connected to the Zynq PS.‌

**Function:** **`open_peripheral`**‌

This function opens one peripheral on the ZynqMP and provides the virtual address mapping.‌

Function prototype:‌

`int open_peripheral(pdevice_info pdev_info, const char *dev_path, const char *devsize_path)`‌

Function arguments:‌

`pdevice_info pdev_info`: pointer to device\_info structure‌

`const char *dev_path`: kernel device path string input‌

`const char *devsize_path`: kernel device size path input string‌

Returns code `0` on success.‌

**Function** **`open_peripherals`**‌

This function opens all peripherals on the ZynqMP. This includes DMA access for ZynqMP PL and the other UltraScale FPGAs.‌

Function prototype:‌

`int open_peripherals(pdevices pdev)`‌

Function arguments:‌

`pdevices pdev`: pointer to structure of type devices‌

Returns code `0` on success.‌

 **Function: lib\_i2c\_read**‌

This function reads data from I2C peripherals.‌

Function prototype:‌

`int lib_i2c_read(pdevices pdev, uint32_t i2c_device_index, uint8_t i2c_address, int reg_address, uint8_t *pdata, uint32_t read_len)`‌

Function arguments:‌

`pdevices pdev`: pointer to structure of type devices‌

`uint32_t i2c_device_index`: I2C peripheral index on the Zynq MPSoC, 0 for the i2c-0 device and 1 for i2c-1 device‌

`uint8_t i2c_address`: I2C address of the slave device on the I2C bus‌

`int reg_address`: register address in the I2C slave device, value -1 to ignore‌

`uint8_t *pdata`: reference to memory location to hold the read results‌

`uint32_t read_len`: number of bytes to read from the slave device‌

Returns code `0` on success.‌

**Function:** **`lib_i2c_write`**‌

This function writes data to the I2C peripheral on the ZynqMP.‌

Function prototype:‌

`int lib_i2c_write(pdevices pdev, uint32_t i2c_device_index, uint8_t i2c_address, int reg_address, uint8_t *pdata, uint32_t data_len)`‌

Function arguments:‌

`pdevices pdev`: pointer to structure of type devices‌

`uint32_t i2c_device_index`: I2C peripheral index on the Zynq MPSoC, 0 for the i2c-0 device and 1 for i2c-1 device‌

`uint8_t i2c_address`: I2C address of the slave device on the I2C bus‌

`int reg_address`: not available‌

`uint8_t *pdata`: reference to memory location to hold the data‌

`uint32_t read_len`: number of bytes to write to the slave device‌

Returns code `0` on success.‌

**Function:** **`devmem_write`**‌

This function can be used to access the full memory mapped AXI range on the ZynqMP.‌

Function prototype:‌

`int devmem_write(pdevices pdev, uint64_t address, alignment_t alignment, uint32_t value)`‌

Function arguments:‌

pdevices pdev: pointer to structure of type devices‌

_`uint64_t address`_: address to write to‌

`alignment_t alignment`: 32-bit, 16-bit or 8-bit aligned writes‌

`uint32_t value`: value to write‌

Returns code `0` on success.‌

**Function:** **`devmem_read`**‌

This function can be used to access the full memory mapped AXI range on the ZynqMP.‌

Function prototype:‌

`int devmem_read(pdevices pdev, uint64_t address, alignment_t alignment, uint32_t *value)`‌

Function arguments:‌

`pdevices pdev`: pointer to structure of type devices‌

`uint64_t address`: address to read from‌

`alignment_t alignment`: 32-bit, 16-bit or 8-bit aligned reads‌

`uint32_t *value`: pointer to memory location to hold the result‌

Returns code `0` on success.‌

**Function:** **`wait_bram_interrupt`**‌

This function processes interrupts coming from the _interrupt gpio_ peripheral connected to the PS.‌

Function prototype:‌

`int wait_bram_interrupt(pdevices pdev, uint32_t *pin_source)`‌

Function arguments:‌

`pdevices pdev`: pointer to structure of type devices‌

`uint32_t *pin_source`: pointer to memory location to hold the gpio data register value‌

Returns `0` on success.‌

**Function:** **`send_data`**‌

This function starts a DMA transfer on the ZynqMP DMA engine to transmit data to the PL.‌

Function prototype:‌

`int send_data(pdevices pdev, void *ptr, uint32_t size)`‌

Function arguments:‌

`pdevices pdev`: pointer to structure of type devices‌

`void *ptr`: pointer to memory location holding the data‌

`uint32_t size`: number of bytes to send‌

Returns code `0` on success.‌

**Function:** **`receive_data`**‌

This function starts a DMA transfer on the ZynqMP DMA engine to receive data from the PL.‌

Function prototype:‌

`int receive_data(pdevices pdev, void *ptr, uint32_t size)`‌

Function arguments:‌

`pdevices pdev`: pointer to structure of type devices‌

`void *ptr`: pointer to memory location holding the data‌

`uint32_t size`: number of bytes to receive‌

Returns code `0` on success.‌

### User applications <a id="user-applications"></a>

‌

#### `bootscript` <a id="bootscript"></a>

‌

The bootscript application is a startup script that could be used to auto-start your custom application during kernel boot.‌

####  `hostif` <a id="hostif"></a>

‌

The hostif application is the main application that will be running as a background process when the kernel boots.‌

The application will service client tcp/ip connections from UnitAPI, PCIe end-point and RFSoC interrupt handling and DMA data transfers to the PL.‌

#### `libzynqmpsoc4dsp` <a id="libzynqmpsoc-4-dsp"></a>

‌

This application project will be widely used by the hostif application to interact with the AXI memory mapped Zynq PL, I2C peripherals and SPI peripherals on the ZynqMP.‌

#### `librfapi` <a id="librfapi"></a>

‌

This is a native port of the SBC RFAPI library. The only difference is that this API only support TCP/IP as communication layer. The communication with the hostif application is done over the local TCP/IP loopback which is 127.0.0.1.‌

#### `vp430_refapp` <a id="vp-430-_refapp-1"></a>

‌

This application project is the native version of the SBC application covered by chapter VP430\_REFAPP. The D/A buffers required for the application to run are located under /etc. The application will return with an error if these buffers are not found. It is expected from the user to run this application from the /etc folder.‌

## Building and compiling PetaLinux BSP <a id="building-and-compiling-petalinux-bsp"></a>

‌

### QSPI flash partition configuration <a id="qspi-flash-partition-configuration"></a>

‌

This section provides details on how to configure flash partitions with PetaLinux menuconfig.‌

1. Change into the root directory of your PetaLinux project.

‌

`$ cd <plnx-proj-root>`‌

1. Launch the top level system configuration menu.

‌

`$ petalinux-config`‌

1. Select **`Subsytem AUTO Hardware Settings ---> Flash Settings --->`**
2. Set the size of each partition.

‌

Example:​MISSING IMAGE![Asset no longer exists](https://static.gitbook.com/images/e372a3642662304a70a27e252a9a2ed5.svg)‌

BOOT.BIN is stored in the ‘boot’ partition. The U-Boot environment is stored in the ‘bootenv’ partition. And the FIT image \(Linux kernel, device tree blob, and root file system\) image.ub is stored in the ‘kernel’ partition. Choose partition sizes that are large enough to store their respective files. Make sure that the total combined size of the partitions does not exceed the total size of the flash part \(1Gbit\).

Writing an image \(BOOT.BIN, image.ub\) that is larger than the partition size may corrupt the flash. Instead of corrupting the flash, revert the change that increased the size of the image, increase the partition sizes, boot PetaLinux so that it recognizes the larger partition sizes, and then generate a larger image and attempt to program it via the vp430\_qspi\_programming tool.‌

### Boot image storage configuration <a id="boot-image-storage-configuration"></a>

‌

This section provides details on how to configure boot image storage such as QSPI flash or SD/MMC.‌

1. Change into the root directory of your PetaLinux project.

‌

`$ cd <plnx-proj-root>`‌

1. Launch the top level system configuration menu.

‌

`$ petalinux-config`‌

1. Select **`Subsytem AUTO Hardware Settings ---> Advanced Bootable Images Storage Settings ---> boot image settings ---> Image Storage Media`**
2. Select boot device. To set QSPI flash as the boot device select **`primary flash`**. To set SD/MMC card as the boot device select **`primary sd`**.
3. Select **`Subsytem AUTO Hardware Settings ---> Advanced Bootable Images Storage Settings ---> kernel image settings ---> Image Storage Media`**
4. Select storage device. To set QSPI flash as the boot device select**`primary flash`**. To set SD/MMC card as the boot device select **`primary sd`**.

‌

### Boot image generation <a id="boot-image-generation"></a>

‌

This section provides details on how to build boot images.‌

1. Change into the root directory of your PetaLinux project.

‌

`$ cd <plnx-proj-root>`‌

1. Build PetaLinux

‌

`$ petalinux-build`‌

1. Package the images

‌

`$ petalinux-package --boot --force --fsbl images/linux/zynqmp_fsbl.elf --fpga --u-boot –kernel`‌

1. BOOT.BIN, image.ub, and other files can be found in images/linux/ directory.

‌

For programming QSPI flash via JTAG, additional steps are required to generate qspi\_image.bin with bootgen tool.‌

1. If it does not exist, create a bootgen.bif file with the following contents.

‌

the\_ROM\_image:

```text
{ [fsbl_config] a53_x64 [bootloader] ./images/linux/zynqmp_fsbl.elf [pmufw_image] ./images/linux/pmufw.elf [destination_device=pl] ./images/linux/system.bit [destination_cpu=a53-0, exception_level=el-3, trustzone] ./images/linux/bl31.elf [destination_cpu=a53-0, exception_level=el-2] ./images/linux/u-boot.elf [destination_cpu=a53-0, offset=0x2040000, exception_level=el-2] ./images/linux/image.ub}
```

‌

The offset used for image.ub is calculated from the QSPI partitions. boot \(size 0x2000000\) and bootenv \(size 0x4000\) partitions come before the kernel partition, which places the kernel partition at 0x2000000 + 0x4000 = 0x2040000 offset.‌

1. Generate the new QSPI image.

‌

`$ bootgen -image bootgen.bif -o qspi_image.bin -arch zynqmp -log -debug -w on`‌

## QSPI Programming <a id="qspi-programming"></a>

‌

The VP430 has a non-volatile memory device containing both boot and kernel image. This chapter covers the various options available to the users.‌

The PetaLinux environment is creating two images:‌

* BOOT.BIN is the boot image file. It contains the first stage boot loader, the programmable logic bitstream as well as UBOOT boot loader.
* image.ub is the kernel image. The kernel image is loaded and executed by UBOOT during boot.

‌

### Host computer connected to VP430 <a id="host-computer-connected-to-vp430"></a>

‌

The vp430\_qspi\_programming application can be run on the host computer to program images over PCIe or TCP/IP. The programming of such an image can take up to **10 minutes** due to the image size.‌

`vp430_qspi_programming` is a command line application with four arguments:‌

* `{Interface Type}` can be either 0 for PCIe or 1 for TCP/IP
* `{IP Address / Device type}` can either be an IP address when using the application on TCP/IP or a device type when using the application on PCIe.
* `{QSPI Destination}` is the QSPI partition where to write the image file. This argument can be 0 to program the boot image or 1 to program the kernel image
* `{Path to image}` is the path on the file system where the image file is located.

‌

`vp430_qspi_programming` has the following syntax:‌

`vp430_qspi_programming {Interface Type} {IP Address / Device type} {QSPI Destination} {Path to image}`‌

Example 1, programming image.ub over PCIe on the kernel partition:‌

`vp430_qspi_programming 0 VP430 1 C:\image.ub`‌

Example 2, programming BOOT.BIN over TCP/IP on the boot partition:‌

`vp430_qspi_programming 1 192.168.1.22 0 C:\BOOT.BIN`‌

### PetaLinux <a id="petalinux"></a>

‌

The Xilinx PetaLinux has a shell application able to program the QSPI. The application is named **`flashcp`** and has three arguments:‌

* `{Options}` is -v for verbose
* `{Path to image}` is the location where the image file is located on the file system.
* `{QSPI Destination}` is file system node of the partition to program.

‌

Example 1 programming the boot image:‌

`flashcp -v ./BOOT.BIN /dev/mtd0`‌

Example 2 programming the kernel image:‌

`flashcp -v ./image.ub /dev/mtd2`‌

### JTAG <a id="jtag"></a>

‌

This section provides details on how to use Vivado to program QSPI image over JTAG. Vivado 2018.1 was used for the following example.‌

1. Follow steps in Section 8.3 to generate images/linux/zynqmp\_fsbl.elf and qspi\_image.bin.
2. Launch Vivado and use Hardware Manager to connect to the hardware target.

​MISSING IMAGE![Asset no longer exists](https://static.gitbook.com/images/e372a3642662304a70a27e252a9a2ed5.svg)‌

1. Right-click xczu27dr\_0 and select ‘Add Configuration Memory Device…’.

​MISSING IMAGE![Asset no longer exists](https://static.gitbook.com/images/e372a3642662304a70a27e252a9a2ed5.svg)‌

1. Search for ‘mt25ql01g-qspi-x4-single’, select it, and then hit ‘OK’.
2. Use qspi\_image.bin for the Configuration file, and zynqmp\_fsbl.elf for the Zynq FSBL.
3. Power down the system to ensure boot mode switches are set to ‘QSPI32’ according to UM080 2.7 First Stage Boot Loader NOR Flash.

‌

## Known Issues / limitations <a id="known-issues-limitations"></a>

‌

* **eth0** is not accessible by U-Boot. Any network connections used by U-Boot must be performed via eth1.
* **eth1** is not able to signal the status of the Ethernet \(RJ45\) link to Linux or the user. The link will always show as up even with no Ethernet cable connected.
* During boot, a kernel “warning” \(similar to a kernel panic but without a kernel halt\) will be seen on the console port. This can be safely ignored.


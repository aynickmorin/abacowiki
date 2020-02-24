# Getting Started Guide

## About This Document

### Safety

This module presents no hazard to the user. Users should be cautions that the PCB and front panel can have sharp edges.

### EMC

This module is designed to operate within an enclosed host system built to provide EMC shielding. Operation within the EU EMC guidelines is not guaranteed unless it is installed within an adequate host system.

### Requirements and Handling Instructions

* The VP430 must be installed in a 3U VPX slot.
  * Convection cooled VP430 boards require a minimum of 1.0 inch pitch.
  * Conduction cooled VP430 boards require a minimum of 1.0 inch pitch.
* Prevent electrostatic discharge by observing Electro Static Damage \(ESD\) precautions when handling the card.

### Technical Support Contact Information

You can find technical assistance contact details on the website [support page](http://support.4dsp.com).

Abaco will log your query in the Technical Support database and allocate it a unique Case number for use in any future correspondence.

Alternatively, you may also contact Abaco’s Technical Support via [support@4dsp.com](mailto:support@4dsp.com).

### Returns

If you need to return a product, there is a Return Materials Authorization \(RMA\) form available via the website [support page](http://support.4dsp.com).

Do not return products without first contacting the Abaco Repairs facility.

## 1. Introduction

This document describes how to get started with the VP430. For more information on any of the hardware, software, or firmware, consult the relevant documentation listed under the Related Documentation section of this document.

## 2. Hardware Components

{% hint style="warning" %}
Please refer to the user manual shipped with the hardware you purchased and strictly follow the handling instructions. Faulty handling may seriously damage the card and void the warranty. The most important part of these handling instructions concerns the allowed operating temperatures. The devices should not be used at a temperature exceeding Abaco System’s specification.
{% endhint %}

### 2.1 VP430

The VP430 is a 3U VPX FPGA carrier card featuring a Zynq Ultrascale+ Radio Frequency System on Chip. For more information, consult the VP430 User Manual.

## 3. Getting Started

### 3.1 Host Computer

Go through the following steps to prepare your host computer.

* Install the VP430 BSP Installer by running `setup.exe` from the installation.
* Download and install the USB UART driver. At the time of writing this document [CP210x USB to UART Bridge VCP Drivers v6.7.3](https://www.silabs.com) were used.
* Check Windows Device Manager to understand which virtual COM port the Zynq PS connects to, which is the COM port associated with Silicon Labs USB to UART bridge, in this example COM4.

* Download and install PuTTY, the UART terminal. At the time of writing this document [PuTTY 0.67](http://www.putty.org/) was used. 
* Open the Network and Sharing Center on Windows. Click “Change adapter settings”. Open Properties on Local Area Connection. Click “Internet Protocol Version 4 \(TCP/IPv4\)” and open Properties. TCP/IPv4 must set as seen in the following picture

{% hint style="info" %}
Install the VP430 BSP installer using the .4lf license file provided to you by Abaco Systems. The VP430 installer installs components to the directory `C:\Program Files (x86)\Abaco`. Since several applications in the firmware design workflow do not work with spaces in the path, or with long path names, it is recommended to use the Windows `subst` command to substitute the path `C:\Program Files (x86)\Abaco\` for a virtual drive. In a Windows command prompt run the following:

`subst: A: “C:\Program Files (x86)\Abaco\”`to substitute `C:\Program Files (x86)\Abaco\` for the virtual drive A.
{% endhint %}

#### 3.1.1 Firmware

The VP430 BSP installer installs the firmware reference design to the following directory:

`A:\Common\Firmware\Extracted\`

At a minimum, the following reference designs should have been installed:

`A:\Common\Firmware\Extracted\755_vp430_axi`

The number preceding the name of the reference design \(ex, 730, 722, 750\) is a unique identifier for a firmware reference design. Pre-compiled bitfiles and boot images for the licensed reference designs can be found in the ‘recovery’ folder:

`A:\Common\Firmware\Recovery\755_vp430`

More information about the firmware reference designs can be found in section 4.1.

#### 3.1.2 Software

The VP430 Board Support Package \(BSP\) installer extracts the reference application C++ source that is licensed with the user’s .4lf license file to the following directories:

1. `A:\VP430 Standard Development Kit\Sources\libRFAPI (RFAPI OSL API source code)`
2. `A:\VP430 Standard Development Kit\Plug-Ins\VP430\Reference Application`

Pre-compiled binaries should be installed here:

1. `A:\VP430 Standard Development Kit\Bins\vp430_refapp.exe`
2. `A:\VP430 Standard Development Kit\Bins\libRFAPI.dll`

More information about the reference design software and running the reference design can be found in Sections 4.2 and 5.2.

{% hint style="info" %}
If any of the aforementioned reference application directories or binaries do not exist, the design is either unlicensed or there was an issue during installation. To troubleshoot the former, contact Abaco Technical Support or create a service request on [support.4dsp.com](file:///O:\Gettings%20Started\support.4dsp.com) and attach your .4lf license file.
{% endhint %}

### 3.2 VP430 Hardware

Make sure that the VP430 is configured to boot from QSPI flash. The boot mode switches should all be configured to 0. This configures the boot to QSPI with 32-bit addressing, which is the default mode. The VP430 will not boot from the default image if the switches are not properly configured.

Please refer to Table 2-4 \(RFSoC Boot Mode Selection\) in the VP430 User Manual for more information about the boot modes \[1\].

## 4. Reference Designs

This chapter describes the firmware and software for the VP430 Ultrascale+ RFSoC.

### 4.1 Firmware

#### 4.1.1 755\_vp430\_axi

This is the reference design for the Zynq Ultrascale+ RFSoC on the VP430. The RFSoC runs a TCP/IP server called _hostif_ that receives commands and data from a 1000BASE-X TCP/IP connection on the VPX backplane. Additionally, PCIe can be used to communicate with the PCIe endpoint implemented on the Ultrascale+ RFSoC device.

{% hint style="info" %}
If any of the aforementioned reference design directories do not exist, the design is either unlicensed or there was an issue during installation. To troubleshoot the former, contact Abaco Technical Support or create a service request on [support.4dsp.com](file:///O:\Gettings%20Started\support.4dsp.com) and attach your .4lf license file.
{% endhint %}

#### 4.1.2 Firmware Design Documentation

An overview of the reference design can be found in the ‘constellation document’ \[3\].

### 4.2 Software

#### 4.2.1 Overview

A Windows or Linux host computer communicates with the VP430 through one of two interfaces:

1. PCIe Gen3 x8 on the UltraScale+ RFSoC
2. TCP/IP on the UltraScale+ RFSoC

An API called ‘RFAPI’ abstracts the connection interface \(PCIe or TCP/IP, in this case\) and is used to communicate with the VP430. More information about RFAPI can be found in \[1\].

The Zynq US+ MPSoC runs a custom version of the PetaLinux kernel that provides a TCP/IP interface to a Windows or Linux host. The VP430 BSP installs the PetaLinux BSP to `A:\VP430 Standard Development Kit\PetaLinux\BSP`_._ The PetaLinux BSP was built in Vivado 2018.1. More information about the VP430 PetaLinux board support package can be found in the PetaLinux chapter in this document.

#### 4.2.2 VP430 Reference Application

As discussed in section 3.1.2, the VP430 BSP installs the VP430 Reference Application to `A:\VP430 Standard Development Kit\Bins`. The VP430 Reference Application provides a reference for performing the following functions:

1. Configure generation settings and download waveforms to VP430 onboard memory.
2. Configure acquisition settings .
3. Start DAC generation and ADC acquisition on a software trigger.
4. Save waveforms on the hard disk.
5. Display some board diagnostics.
6. Run a PL memory test when controlled using PCIe.

The application requires eight DAC waveform files to run successfully. The reference application expects the file to be named DAC0.txt, DAC1.txt, DAC2.txt, DAC3.txt, DAC4.txt, DAC5.txt, DAC6.txt and DAC7.txt. The files are installed in two locations.

* `A:\VP430 Standard Development Kit\Bins`
* `A:\VP430 Standard Development Kit\Sources\Reference Application`

## 5. Running the Reference Design

### 5.1 Verifying PetaLinux Boots

The first thing to do is to verify that PetaLinux boots. Start PuTTY on the host computer and use the COM port associated CP210x USB to UART Bridge \(see [Section 3.1]() for details\) with the following parameters: 115200 bauds, 8 bit per character, 1 stop bit, no parity and no flow control. The login credentials are username: root, password: root. The TCP/IP server _hostif_ will run right after the kernel is finished booting so there is no need to use the PuTTY shell for anything other than displaying debug messages from the system.

If PetaLinux has not booted, confirm that the VP430 boot mode selection is set correctly. By default, the VP430 should be set to boot from QPSI \(VP430 SW1 set to 0x00\). For more information, consult Table 2-4 and Table 6-3 of the VP430 User Manual. If the Zynq is stuck in U-BOOT running the command _run qspi\_flash_ will complete the boot process by loading the kernel image from QSPI flash.

### 5.2 Running the VP430 Reference Application

#### 5.2.1 Precompiled Executable

The VP430 Reference Application executable, located at `A:\VP430 Standard Development Kit\Bins\vp430_refapp.exe`, can be run from the Windows command line without any arguments. The application will then print usage instructions for the user. The following figure displays the command line output after running with no arguments:

#### 5.2.2 Reference Source

The source for the reference application, located at `A:\VP430 Standard Development Kit\Sources\Reference Application\`_,_ is supported in Visual Studio 2012. After compiling the project, the generated executable can be executed as described in section 5.2.1.

The source for the RFAPI, located `A:\VP430 Standard Development Kit\Sources\libRFAPI\`_,_ is supported in Visual Studio 2012

## 6. PetaLinux Development Flow

### 6.1 Workstation Requirements

We strongly recommend against using a virtual machine for installation since the extra compile time can really add up, especially when a complete rebuild is required. To compare performance, we performed complete rebuilds as follows:

| **Virtual Machine** | **PC** | **Type** | **Compile Time** |
| :--- | :--- | :--- | :--- |
| No | 6th Gen i7 CPU, 32GB RAM, 500 GB Sata HD | Full Rebuild | 7 minutes |
| Yes | 8th Gen i7 CPU, 32GB RAM \(16 GB for VM\), 250 GB SSD HD | Full Rebuild | 33 minutes |

### 6.2 Xilinx Reference Guides

ug1144-PetaLinux-tools-reference-guide.pdf, version 2018.1

ug1157-PetaLinux-tools-command-line-guide.pdf, version 2018.1

### 6.3 Ubuntu Installation

Install Ubuntu 16.04.1 and choose to install updates automatically \(will actually make it Ubuntu 16.04.3\)

### 6.4 Package Installation

To install all needed packages, type the following commands:

`sudo apt-get --assume-yes install python tofrodos gawk xvfb git libncurses5-dev tftpd zlib1g-dev libssl-dev flex bison chrpath socat autoconf`

`sudo apt-get --assume-yes install libtool texinfo gcc-multilib libsdl1.2-dev screen pax lib32z1 lib32ncurses5 lib32stdc++6 zlib1g:i386`

`sudo dpkg-reconfigure dash`

When prompted

* enter the root password for sudo.

Once the packages are installed, you will be prompted to use Dash as the system shell.

* Choose: NO

This is because PetaLinux requires Bash, not Dash

### 6.5 PetaLinux Installation

Create a directory for the PetaLinux installation in a suitable location that does not require root privileges. Note that the PetaLinux Tools Reference Guide \(UG144\) suggests you install in the /opt/ directory, but this directory requires root privileges, making things difficult in the future. Instead, install in a user directory, such as:

`mkdir /home/<user>/PetaLinux_2018_1/`

Change to the new directory:

`cd /home/<user>/PetaLinux_2018_1/`

Download the PetaLinux installer from the Xilinx site, for PetaLinux version 2018.1 and copy it to the installation directory created above. The PetaLinux installer filename is `PetaLinux-v2018.1-final-installer.run`

Set permissions so that the installer is executable:

`chmod 0777 PetaLinux-v2018.1-final-installer.run`

Install PetaLinux:

`./PetaLinux-v2018.1-final-installer.run`

When prompted for the license agreements, press—Enter--to read the license. Next press _q_ to quit reading, and then _y_ followed by _Enter_ to accept. You will need to repeat this sequence 2 more times since 3 different licenses exist.

When prompted to install in the existing working directory:

* Choose: y

Once installation is complete, the installer file can be deleted:

`rm PetaLinux-v2018.1-final-installer.run`

### 6.6 Xilinx SDK Installation

To install the Xilinx SDK, copy the file `Xilinx_SDK_2018.1_0405_1_Lin64.bin` to a directory in a suitable location that does not require root privileges, such as:

`/home/<user>/`

Note that a subdirectory will automatically be created here, named `SDK/2018.1/`, once the SDK has been installed.

Change to the directory:

`cd /home/<user>/`

Download the Xilinx SDK installer from the Xilinx site, for SDK version 2018.1, and copy it to the installation directory selected above. The SDK installer filename is `Xilinx_SDK_2018.1_0405_1_Lin64.bin`

Set permissions so that the installer is executable:

`chmod 0777 Xilinx_SDK_2018.1_0405_1_Lin64.bin`

Install the Xilinx SDK:

`./Xilinx_SDK_2018.1_0405_1_Lin64.bin`

When prompted, decline upgrading the SDK by select _Continue_.

* The build system requires SDK version 2018.1. You will need to sign on with your Xilinx account credentials to continue the installation.
* Click: all 3 checkboxes to agree to the licenses
* Select the _XSDK_ Edition for installation
* Click: Next

At the _Select Destination Directory_ screen, click the browse button and navigate to and select the installation directory you created previously, such as

* `/home/<user>/SDK/2018.1/`
* Click: _Next_
* Click: _Install_

Once installation is complete, the installer file can be deleted:

`rm Xilinx_SDK_2018.1_0405_1_Lin64.bin`

### 6.7 Environment Setup

Both PetaLinux and the Xilinx SDK must have their environmental variables setup before they can be used. This is done by sourcing the settings scripts for each package, from the command line. For example:

`source /home/<user>/PetaLinux_2018_1/settings.sh`

`source /home/<user>/SDK/2018.1/settings64.sh`

The scripts must be sourced one time, each time you first open a command shell. As an alternative, these commands can be added to the user's `.bashrc` file so that they are executed automatically each time a command shell is opened. The `.bashrc` file is located in the user's home directory. Note that it may be necessary to select _View-&gt;Show Hidden Files_ if using the file explorer or `ls -al` if using the command line, to see the file. To add the commands to the `.bashrc`file, you must use the vi editor with root privileges:

`sudo gedit /home/<user>/.bashrc`

Once saved, you will need to close the command shell and then reopen a new one.

Once the setting scripts have been executed, Linux will be able to locate the PetaLinux tools. One of the first things you may want to do is disable the diagnostic tool that communicates with Xilinx. This is optional:

`PetaLinux-util --webtalk off`

### 6.8 BSP Installation

To install the Abaco VP430 BSP, copy the file `vp430_PetaLinux_2018_1.bsp` to a directory in a suitable location that does not require root privileges, such as:

`/home/<user>/`

Note that a subdirectory will automatically be created here, named`xilinx-zcu102-2018.1`, once the BSP has been installed.

Change to the directory:

`cd /home/<user>/`

Set permissions so that the installer is executable:

`chmod 0777 vp430_PetaLinux_2018_1.bsp`

Install the BSP:

`PetaLinux-create -t project -s vp430_PetaLinux_2018_1.bsp`

This will create the folder `xilinx-zcu102-2018.1` __containing several subdirectories

Once installation is complete, the installer file can be deleted:

`rm vp430_PetaLinux_2018_1.bsp`

### 6.9 Initial Build

Change to the `xilinx-zcu102-2018.1` project directory. It's from this directory that all build steps should occur. For the first build, ensure everything has been cleaned, run the initial configuration, and then build the project:

Clean with:

`PetaLinux-build -x cleanall`

Configure with this. First option is for default BSP build, second option is for a recompiled HDF. Select _Exit_ when prompted:

`PetaLinux-config`

-OR-

`PetaLinux-config --get-hw-description=<path to .sdk directory with HDF file>`

Build the project:

`PetaLinux-build`

Note that the initial build may take considerable time, but subsequent builds should be shorter. You can ignore the warnings encountered. This will generate the `image.ub` file.

To generate the boot file _BOOT.BIN_ type:

`PetaLinux-package --prebuilt --force -a images/linux/pmufw.elf:images -a images/linux/image.ub:images --fpga images/linux/system.bit`

`PetaLinux-package --force --boot --fsbl pre-built/linux/images/zynqmp_fsbl.elf --fpga pre-built/linux/implementation/system.bit --pmufw pre-built/linux/images/pmufw.elf --u-boot --output pre-built/linux/images/BOOT.BIN`

The output images will show up in the `images/linux/` project folder as:

* `BOOT.BIN`
* `image.ub`

You can copy these files to an SD to run them.

### 6.10 Subsequent Builds

The PetaLinux documentation contains various guidelines on build steps to perform at different times. As with many software packages, avoiding rebuilding the entire project every time can save a lot of time. For most situations, the following build approach works for us:

* run cleanall on the specific component you want rebuilt, for example.

  `PetaLinux-build -c rootfs -x cleanall`

* rebuild the overall project:

  `PetaLinux-build`

* create BOOT.BIN

  `PetaLinux-package --prebuilt --force -a images/linux/pmufw.elf:images -a images/linux/image.ub:images --fpga images/linux/system.bit`

  `PetaLinux-package --force --boot --fsbl pre-built/linux/images/zynqmp_fsbl.elf --fpga pre-built/linux/implementation/system.bit --pmufw pre-built/linux/images/pmufw.elf --u-boot --output pre-built/linux/images/BOOT.BIN`

Note that some situations may need a configuration step, prior to the build step. In such situations, configuring the overall project usually works well, unless you need to enable/disable specific items in specific packages:

`PetaLinux-config`

The PetaLinux documentation contains the names of the various components that can be operated upon individually. The components are:

* `-c bootloader`
* `-c kernel`
* `-c u-boot`
* `-c rootfs`
* `-c pmufw`
* `-c arm-trusted-firmware`
* `-c device-tree`

Additionally, many user software applications can also be operated upon individually, since this is built in to their makefiles upon installation. For example, to clean and rebuild only the Abaco `hostif` application:

* `PetaLinux-build -c hostif -x cleanall`
* `PetaLinux-build`
* `PetaLinux-package --force --boot --fsbl pre-built/linux/images/zynqmp_fsbl.elf --fpga pre-built/linux/implementation/system.bit --pmufw pre-built/linux/images/pmufw.elf --u-boot --output pre-built/linux/images/BOOT.BIN`

### 6.11 Distributing Software and Version Control

An easy way to share the latest build with other users, and as a simple means of version control, is to repackage the project into a single BSP file, by typing:

`PetaLinux-package --bsp --clean -o <bsp path/bsp name> -p .`

Notice the '.' at the end, specifying the current working directory \(project directory\). This will create a new BSP in the specified path, with the specified name. The BSP contains all source from the project, including the applications.fdsahfdka fdsa'




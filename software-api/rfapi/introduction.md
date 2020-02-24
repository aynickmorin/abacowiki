# Introduction

The API focuses on a generic set of commands that Abaco 4DSP will support for programming, initializing, and changing RF/FMC components. This can be internal SOC-based RF programming, such as the RFSOC from Xilinx, or it can be an FMC based card from Abaco 4DSP. The API is designed so that the user does not need to know much of the underlying layer of additional device specific API functions.

The API is an OSL, open-source library provided as source code as well as precompiled for a Windows environment. The library itself can be compiled either using GCC on Linux or Visual Studio on Windows. The API is licensed “AS-IS”, meaning the user is free to use and modify the library for their own project and no warranty comes with the library. Bug reports are welcome and appreciated and will help improve the library over time.

RFSOC API functions interface with internal FMC reference libraries created by Abaco/4DSP. Based on the firmware, the carrier card, and specific FMC card, the RFAPI manages these function calls internally on a per FMC/constellation ID/carrier card basis.

The API supports both TCP/IP and PCIe. The abstraction between interfaces is done by the API itself so the user can simply use the same programming interface for both TCP/IP and PCIe.

### Use case 1 \(Standalone running on PetaLinux\)

The RF API communicates with a host interface application running in the background on the PetaLinux operating system. The host interface application is a TCP/IP server and the RF API is a TCP/IP client. The communication is done via a local IP loopback. The operating system local loopback is a high-speed IP link between the computer and itself. A local IP loopback does not impact throughput or performances between RF API and the TCP/IP client because it only goes down to the IP stack.

### Use case 2 \(Controlled by a host computer using PCI Express\)

The RF API communicates directly with a PCIe endpoint implemented in the programmable logic, either a separate FPGA or the PL portion of a Zynq/RFSoc device.

### Use case 3 \(Controlled by a host computer using Ethernet\)\)

The RF API communicates with a host interface application running in the background on the PetaLinux operating system. The host interface application is a TCP/IP server and the RF API is a TCP/IP client on the host computer.

### Triggering

There are three trigger levels supported by RFAPI

* The main trigger which can be either software or hardware triggered. In the case of hardware triggering, the trigger can be configured for edge or level triggering. The trigger source is the external trigger connector TRIG. This trigger is configured using[ `RFAPI_ConfigureTrigger()` ]()and a software trigger is sent using [`RFAPI_SendSoftwareTrigger()`]().
* The signal acquisition trigger which can be either software or hardware triggered. When signal acquisition and signal generation are in hardware trigger mode, the main trigger is the trigger source. This trigger is configured using [`RFAPI_SA_ConfigureTrigger()`]().
* The signal generation trigger which can be either software or hardware triggered. In hardware trigger mode, the main trigger is the trigger source. This trigger is configured using [`RFAPI_SG_ConfigureTrigger()`]().

There is no provision for sending the software trigger directly to the signal acquisition and signal generation blocks, but this could be achieved by using low-level function calls if required. This is because software triggers to the signal acquisition and signal generation subsystems are sent by RFAPI at the right moment when calling [`RFAPI_SG_Initiate()`]() and [`RFAPI_SG_SendWaveform()`]() as well as [`RFAPI_SA_Initiate()` ]()and [`RFAPI_SA_ReceiveWaveform()`](). This allows the user to not have to handle software triggering and only send waveforms or acquire waveforms.

Configuring triggering for signal acquisition and signal generation to use hardware triggering disables the software triggers sent by RFAPI. Both signal acquisition and signal generation are then expecting to be triggered by the main trigger which can be software or hardware triggered. This mode of operation has the advantage of providing synchronous acquisition on all the channels.

The following pages describe the three most common triggering use cases.

#### Use case 1 \(Automatic asynchronous triggering\)

The user configures both signal acquisition and signal generation to be software triggered. In this case RFAPI will automatically send one software trigger per channel while calling [`RFAPI_SG_Initiate()`](https://abaco.gitbook.io/wiki/user-manuals/rfapi-programmers-guide#rfapi_sg_initiate) and [`RFAPI_SG_SendWaveform()`](https://abaco.gitbook.io/wiki/user-manuals/rfapi-programmers-guide#rfapi_sg_sendwaveform) as well as [`RFAPI_SA_Initiate()`](https://abaco.gitbook.io/wiki/user-manuals/rfapi-programmers-guide#rfapi_sa_initiate) and [`RFAPI_SA_ReceiveWaveform()`](https://abaco.gitbook.io/wiki/user-manuals/rfapi-programmers-guide#rfapi_sa_receivewaveform). Note that placing SG and SA triggering in hardware triggering mode disable this feature. This is the easiest use of RFAPI because the user does not need to keep triggering in mind.

**Sequence:**

1. The user calls [`RFAPI_SA_ConfigureTrigger()`](https://abaco.gitbook.io/wiki/user-manuals/rfapi-programmers-guide#rfapi_sa_configuretrigger) with a trigger mode set to `RFAPI_TRIGGER_SW`
2. The user calls [`RFAPI_SG_ConfigureTrigger()`](https://abaco.gitbook.io/wiki/user-manuals/rfapi-programmers-guide#rfapi_sg_configuretrigger) with a trigger mode set to `RFAPI_TRIGGER_SW`

#### Use case 2 \(software synchronous triggering\)

The user configures both signal acquisition and signal generation to be hardware triggered, main trigger to be software triggered. The user can then send software triggers to the main trigger. The advantage of this method is that all the channels will be triggered at the same time providing synchronous acquisition and generation on all the channels.

**Sequence:**

1. The user calls [`RFAPI_SA_ConfigureTrigger()`]() with `RFAPI_TRIGGER_HW` as trigger mode.
2. The user calls [`RFAPI_SG_ConfigureTrigger()`]() with `RFAPI_TRIGGER_HW` as trigger mode.
3. The user calls [`RFAPI_ConfigureTrigger()`]() with `RFAPI_TRIGGER_SW` as trigger mode.
4. The user can then call [`RFAPI_SendSoftwareTrigger()`]()when required.

#### Use case 3 \(external synchronous hardware triggering\)

The user configures the main trigger, signal acquisition trigger, and signal generation trigger to be hardware triggered. The source of the hardware trigger will be the external trigger connector \(`TRIG`\). The advantage of this method is that all the channels will be triggered at the same time providing synchronous acquisition on all the channels.

The user can specify which edge or level to trigger on:

* Rising edge triggering \(`RFAPI_TRIGGER_HW_RE`\)
* Falling edge triggering \(`RFAPI_TRIGGER_HW_FE`\)
* Level triggering \(`RFAPI_TRIGGER_HW_LVL`\).

**Sequence:**

1. The user calls [`RFAPI_SA_ConfigureTrigger()`]() with `RFAPI_TRIGGER_HW` as trigger mode.
2. The user calls [`RFAPI_SG_ConfigureTrigger()`]() with `RFAPI_TRIGGER_HW` as trigger mode.
3. The user calls [`RFAPI_ConfigureTrigger()`]() with `RFAPI_TRIGGER_HW` as trigger mode and `RFAPI_TRIGGER_HW_RE`, `RFAPI_TRIGGER_HW_FE` or `RFAPI_TRIGGER_HW_LVL` as trigger type.

## 


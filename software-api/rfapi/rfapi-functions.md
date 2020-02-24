# RFAPI Functions

### `RFAPI_SystemInit`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_SystemInit(uint32_t api_version)
```

**Description:**

Global RFAPI initialization. Call this function before all other RFAPI functions.

**Parameters:**

`api_version -` API mode version. 

**Returns:**

`RFAPI_ERR_OK`

`RFAPI_ERR_VERSION`

`RFAPI_ERR_GENERIC`

### `RFAPI_DeviceInit`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_DeviceInit(RFAPI_HANDLE *handle, RFAPI_DEVICE_PARAMS *device_params)
```

**Description:**

Opens the device specified, initializes the device and associated FMC card. Returns card specific information to the user. 

**Parameters:**

`handle` – Pointer to a previously allocated `RFAPI_HANDLE`.

`device_params` – Pointer to a previously allocated [`RFAPI_DEVICE_PARAMS`]() structure. The user should populate the structure before passing it to this function. An example of that would be the device type such as `VP430`. The rest of the structure will be populated by the function itself. As per the current implementation, only the device type field needs to be populated.

**Returns:**

`RFAPI_ERR_OK` – The handle can be used for further communication with the board, the board is ready and initialized.

`RFAPI_ERR_NULL` – Unexpected NULL argument passed as argument.

`RFAPI_ERR_INVALID_HANDLE` – Invalid handle passed as argument.

`RFAPI_ERR_VERSION` – The version of the `RFAPI_DEVICE_PARAMS` is not the right version.

`RFAPI_ERR_NO_SYS_INIT` – User should call `RFAPI_SystemInit()` before calling this function.

`RFAPI_ERR_GENERIC`

### `RFAPI_GetVersion`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_GetVersion(uint32_t *version_min, uint32_t *version_max);
```

**Description:**

Return the various API versions mode supported by the library, back/forward compatibility

**Parameters:**

`version_min` – Pointer to a variable receiving the minimum API version mode supported by the library.

`version_max` – Pointer to a variable receiving the maximum API version mode supported by the library.

**Returns:**

`RFAPI_ERR_OK`

`RFAPI_ERR_NULL` – Unexpected NULL argument passed as argument.

`RFAPI_ERR_NO_SYS_INIT` – User should call `RFAPI_SystemInit()` before calling this function.

**Notes:**

Not yet implemented. This function will be implemented in future RFAPI versions.

### `RFAPI_GetLastErrors`

```cpp
void RFAPI_CALL RFAPI_GetLastErrors(RFAPI_HANDLE *handle,
 RFAPI_MODULES_ERRORS_PARAMS *perrors);
```

**Description:**

This function populates an error structure received as argument. This could be an operating system socket error, a TCP/IP or PCIe communication error or an abstraction layer error.

The low-level error codes are described in the [Error Codes Definitions \(Low Level\)]() table.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

`perrors` – Pointer to a previously allocated [`RFAPI_MODULES_ERRORS_PARAMS`]() error structure.

**Returns:**

None.

### `RFAPI_GetNumberConverters`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_GetNumberConverters(RFAPI_HANDLE *handle,
int32_t *nbr_adc_converters, int32_t *nbr_dac_converters) ;
```

**Description:**

Retrieve the number of ADC and DAC converters present on the device.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

`nbr_adc_converters` – Pointer to a variable receiving the number of ADC converters on the device.

`nbr_dac_converters` – Pointer to a variable receiving the number of DAC converters on the device.

**Returns:**

`RFAPI_ERR_NO_SYS_INIT` – User should call [`RFAPI_SystemInit()`]() before calling this function.

`RFAPI_ERR_INVALID_HANDLE` – Invalid handle passed as argument.

`RFAPI_ERR_NULL` – Unexpected NULL argument passed as argument.

`RFAPI_ERR_OK`

### `RFAPI_SetConvertersFrequencies`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_SetConvertersFrequencies(RFAPI_HANDLE
*handle, float *freq_array_adc, float *freq_array_dac);
```

**Description:**

Configure sampling frequencies of A/D and D/A converters on the device. The user creates an array for A/D converters as well as an array for D/A converters and passes them to this function. Note that the arrays should be as large as the value returned by [`RFAPI_GetNumberConverters()`]().

In some cases, the converters won’t be able to be configured to the requested frequencies and this function will return with an error.

The sampling frequencies that are available for the hardware supported by this API are summarized in the chapter [Supported Converters Sampling Frequencies](). It is recommended to only pass supported frequencies to the function.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

`freq_array_adc` – Pointer to the sampling frequencies array for ADC converters. Note that a NULL argument can be passed if the targeted device only has DAC converters, this argument is optional.

`freq_array_dac` – Pointer to the sampling frequencies array for DAC converters. Note that a NULL argument can be passed if the targeted device only has ADC converters, this argument is optional.

**Returns:**

`RFAPI_ERR_NO_SYS_INIT` – User should call [`RFAPI_SystemInit()`]() before calling this function.

`RFAPI_ERR_INVALID_HANDLE` – Invalid handle passed as argument.

`RFAPI_ERR_NULL` – Unexpected NULL argument passed as argument.

`RFAPI_ERR_ADC_DAC_CFG` – Could not configure at least one of the converters.

`RFAPI_ERR_OK`

**Notes:**

* This function is will only return when the clocking is configured or with a `RFAPI_ERR_ADC_DAC_CFG` error if the clock could not be configured as per the user inputs. Runtime duration of this function is approximately 100ms

### `RFAPI_GetConvertersFrequencies`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_GetConvertersFrequencies(RFAPI_HANDLE
 *handle, float *freq_array_adc, float *freq_array_dac);
```

**Description:**

Returns the sampling frequencies at which the converters are configured. Note that the arrays should be as large as the value returned by[`RFAPI_GetNumberConverters()`]().

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

`freq_array_adc` – Pointer to the frequencies array for ADC converters. Note that a NULL argument can be passed if the targeted device only has DAC converters, this argument is optional.

`freq_array_dac` – Pointer to the frequencies array for DAC converters. Note that a NULL argument can be passed if the targeted device only has ADC converters, this argument is optional.

**Returns:**

`RFAPI_ERR_NO_SYS_INIT` – User should call `RFAPI_SystemInit()` before calling this function.

`RFAPI_ERR_INVALID_HANDLE` – Invalid handle passed as argument.

`RFAPI_ERR_NULL` – Unexpected NULL argument passed as argument.

`RFAPI_ERR_OK`

**Notes:**

Not yet implemented. This function will be implemented in future RFAPI versions.

### `RFAPI_GetSamplesBuffer`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_GetSamplesBuffer(size_t sample_count,
 samples_buf_ptr_t *pdata)
```

**Description:**

Allocates a buffer in host memory for transferring I/Q samples to and from device memory. An I/Q sample is 4 bytes.

**Parameters:**

`sample_count` – Number of I/Q samples. Must be a multiple of 32.

`pdata` – A samples\_buf\_t pointer. This pointer can then be passed to functions such as [RFAPI\_LL\_SendData\(\)](), [RFAPI\_LL\_ReceiveData\(\)](), [RFAPI\_SG\_SendWaveform\(\)]() and [RFAPI\_SA\_ReceiveWaveform\(\)]().

**Returns:**

`RFAPI_ERR_NO_MEMORY` – Could not allocate the buffer, no more memory available.

`RFAPI_ERR_OK`

### `RFAPI_FreeSamplesBuffer`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_FreeSamplesBuffer(samples_buf_ptr_t pdata)
```

**Description:**

Free a buffer previously obtained by calling [RFAPI\_GetSamplesBuffer\(\).]()

**Parameters:**

_`pdata`_ – A samples\_buf\_t pointer.

**Returns:**

`RFAPI_ERR_OK`

### `RFAPI_GetBuffer`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_GetBuffer(size_t size, void **pdata)
```

**Description:**

Allocates a buffer in host memory for transferring data to and from device memory.

**Parameters:**

`size` – Number of bytes. Must be a multiple of 128.

`pdata` – Address of pointer which points to allocated memory after function call returns.

**Returns:**

`RFAPI_ERR_NO_MEMORY` – Could not allocate the buffer, no more memory available.

`RFAPI_ERR_OK`

### `RFAPI_FreeBuffer`

```cpp
RFAPI_STATUS RFAPI_CALL RFAPI_FreeBuffer(void *pdata)
```

**Description:**

Free a buffer previously obtained by calling [RFAPI\_GetBuffer\(\).]()

**Parameters:**

`pdata` – Pointer to memory needing to be freed.

**Returns:**

`RFAPI_ERR_OK`

### RFAPI\_GetDeviceDiagnostics

RFAPI\_STATUS RFAPI\_CALL RFAPI\_GetDeviceDiagnostics\(RFAPI\_HANDLE \*handle,

diagnostics\_ptr\_t \*diags, uint32\_t \*diags\_type\);

**Description:**

Retrieve board diagnostics. This function will provide the caller with a a generic pointer to a device specific diagnostic structure as well as a diagnostic type. The caller can then cast the generic pointer to a given device specific type.

All device specific diagnostic structure elements are described in the [device specific diagnostics chapter]().

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_diags_ – Pointer to a generic diagnostics structure. The API will write address to the structure at the pointer received as argument.

_diags\_type_ – Pointer to a variable about to receive the type of diagnostics returned by the function. [Diagnostics types]() enumerates all the currently implemented types.

**Returns:**

RFAPI\_ERR\_VERSION – The version of the [RFAPI\_DIAGNOSTICS\_PARAMS]() is not the right version.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call [RFAPI\_SystemInit\(\)]() before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_OK

**Example:**

if\(RFAPI\_GetDeviceDiagnostics\(&hdev, &diags, &diags\_type\) != RFAPI\_ERR\_OK\) {

 return -1;

}

if\(diags\_type==RFAPI\_DIAGS\_TYPE\_VP430\_V1\) {

 printf\("\n------------------------------------------------------------------------\n"\);

 printf\("-- Displaying diagnostics \n"\);

 printf\("------------------------------------------------------------------------\n"\);

printf\("Die temperature : %3.2f\[C\]\n", \(\(VP430\_DIAGS\_V1 \*\)diags\)-&gt;onchip\_temp\);

}

### RFAPI\_GetOnBoardFrequency

RFAPI\_STATUS RFAPI\_CALL RFAPI\_GetOnBoardFrequency\(RFAPI\_HANDLE \*handle,

int8\_t index, float \*frequency\);

**Description:**

Read a signal frequency from the on-board frequency counter.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_index_ – Index of an on-board frequency to obtain. The indexes are device-specific and are described in [Device specific frequency measurement]() chapter.

_frequency_ – Pointer to a variable to receive the frequency of a given on-board frequency index in MHz.

**Returns:**

RFAPI\_ERR\_VERSION – The version of the [RFAPI\_DIAGNOSTICS\_PARAMS]() is not the right version.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call [RFAPI\_SystemInit\(\)]() before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_OK

### RFAPI\_DisplayOnBoardFrequency

RFAPI\_STATUS RFAPI\_CALL RFAPI\_DisplayOnBoardFrequency\(RFAPI\_HANDLE \*handle,

int8\_t index\);

**Description:**

Display the frequency in MHz to the console. The frequency displayed is retrieved from a given on board frequency index.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_index_ – Index of an on-board frequency to obtain. The indexes are device specific and are described in [Device specific frequency measurement]() chapter.

**Returns:**

RFAPI\_ERR\_VERSION – The version of the [RFAPI\_DIAGNOSTICS\_PARAMS]() is not the right version.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call [RFAPI\_SystemInit\(\)]() before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_OK

### RFAPI\_ConfigureTrigger

RFAPI\_STATUS RFAPI\_CALL RFAPI\_ConfigureTrigger\(RFAPI\_HANDLE \*handle,

int32\_t trigger\_mode, int32\_t trigger\_type\);

**Description:**

Configure triggering in the main control IP block in the programmable section of the reference firmware. This trigger is not directly related to SG \(signal generation\) or SA \(signal acquisition\) but is to be considered as the main trigger.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_trigger\_mode_ – Trigger mode, either RFAPI\_TRIGGER\_SW for software triggering or RFAPI\_TRIGGER\_HW for hardware triggering.

_trigger\_type_ – This argument is discarded for software triggering. In the case of hardware triggering the caller can pass RFAPI\_TRIGGER\_HW\_RE \(rising edge\), RFAPI\_TRIGGER\_HW\_FE \(falling edge\) or RFAPI\_TRIGGER\_HW\_LVL \(level\).

**Returns:**

RFAPI\_ERR\_VERSION – The version of the [RFAPI\_DIAGNOSTICS\_PARAMS]() is not the right version.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call [RFAPI\_SystemInit\(\)]() before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_OK

### RFAPI\_SendSoftwareTrigger

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SendSoftwareTrigger\(RFAPI\_HANDLE \*handle\);

**Description:**

Send a software trigger to the main trigger block. This trigger is not directly related to SG \(signal generation\) or SA \(signal acquisition\) but is to be considered as the main trigger.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_trigger\_mode_ – Trigger mode, either RFAPI\_TRIGGER\_SW for software triggering or RFAPI\_TRIGGER\_HW for hardware triggering.

_trigger\_type_ – This argument is discarded for software triggering. In the case of hardware triggering the caller can pass RFAPI\_TRIGGER\_HW\_RE \(rising edge\), RFAPI\_TRIGGER\_HW\_FE \(falling edge\) or RFAPI\_TRIGGER\_HW\_LVL \(level\).

**Returns:**

RFAPI\_ERR\_VERSION – The version of the [RFAPI\_DIAGNOSTICS\_PARAMS]() is not the right version.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call [RFAPI\_SystemInit\(\)]() before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_OK

### RFAPI\_SendShellCommand

RFAPI\_STATUS RFAPI\_CALL RFAPI\_ConfigureTrigger\(RFAPI\_HANDLE \*handle,

uint8\_t \*shell\_cmd, uint8\_t \*results, int32\_t timeout\_ms\);

**Description:**

Send a shell command to the Petalinux operating system.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_shell\_cmd_ – Pointer to the shell command string to be sent over to the Petalinux operating system. The string should be NULL terminated.

_results_ – Pointer to a previously allocated buffer about to receive the answer from the Petalinux operating system.

_timeout\_ms_ – Maximum time in milliseconds the function should wait before returning in the event no response is received from the Petalinux operating system.

**Returns:**

RFAPI\_ERR\_VERSION – The version of the [RFAPI\_DIAGNOSTICS\_PARAMS]() is not the right version.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call [RFAPI\_SystemInit\(\)]() before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

_RFAPI\_ERR\_SHELL\_CMD\_FAILED_ – Failed to send the shell command

RFAPI\_ERR\_OK

### RFAPI\_UploadQSPIImage

RFAPI\_STATUS RFAPI\_CALL RFAPI\_UploadQSPIImage\(RFAPI\_HANDLE \*handle, int8\_t destination, const char \*filename\);

**Description:**

Configure one of the partitions in the QSPI device on the VP430. Both a kernel or a boot image can be programmed. There is no restriction on the image file name. The default names in PetaLinux are BOOT.BIN for the boot image and image.ub for the kernel image.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_destination_ – The destination argument tells the API where to upload the image file, in other words on which QSPI partition; VP430\_UPLOAD\_IMAGE\_DEST\_BOOT for the boot partition or VP430\_UPLOAD\_IMAGE\_DEST\_KERNEL for the kernel partition.

_filename_ – Path to the image file in the operating system.

**Returns:**

RFAPI\_ERR\_NO\_SYS\_INIT – User should call [RFAPI\_SystemInit\(\)]() before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_GENERIC– Failed to upload the file.

RFAPI\_ERR\_OK

### RFAPI\_DeviceClose

RFAPI\_STATUS RFAPI\_CALL RFAPI\_DeviceClose\(RFAPI\_HANDLE \*handle\)

**Description:**

Frees all allocated resources. Closes the device.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

**Returns:**

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_OK

### RFAPI\_Sleep

void RFAPI\_CALL RFAPI\_Sleep\(uint32\_t time\_ms\)

**Description:**

Blocking function returning after a given number of milliseconds.

**Parameters:**

_time\_ms_ – Number of milliseconds the function will block before returning.

**Returns:**

None.

### RFAPI\_SystemClose

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SystemClose\(void\)

**Description:**

Frees the allocated resources.

**Parameters:**

None.

**Returns:**

RFAPI\_ERR\_NO\_SYS\_INIT – User must call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_OK

### RFAPI\_LL\_ReadRegister

RFAPI\_STATUS RFAPI\_CALL RFAPI\_LL\_ReadRegister\(RFAPI\_HANDLE \*handle,

 uint32\_t address, uint32\_t \*pvalue\)

**Description:**

Read a register from the hardware

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_address_ – Absolute address \(32-bit address\) where the register is located.

_pvalue_ – Pointer to a 32-bit variable receiving the value contained by the register.

**Returns:**

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_TIMEOUT – Could not complete the operation within the timeout period.

RFAPI\_ERR\_OK

### RFAPI\_LL\_WriteRegister

RFAPI\_STATUS RFAPI\_CALL RFAPI\_LL\_WriteRegister\(RFAPI\_HANDLE \*handle,

 uint32\_t address, uint32\_t value\)

**Description:**

Write a register in the FPGA firmware.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_address_ – Absolute address \(32bit address\) where the register is located.

_value_ – Value about to be written to the register.

**Returns:**

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_TIMEOUT – Could not complete the operation within the timeout period.

RFAPI\_ERR\_OK

### RFAPI\_LL\_SendData

RFAPI\_STATUS RFAPI\_CALL RFAPI\_LL\_SendData\(RFAPI\_HANDLE \*handle, size\_t

 size, samples\_buf\_ptr\_t \*pdata\)

**Description:**

Send data to the hardware.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_size_ – Number of bytes to be transferred. Size needs to be a multiple of 128 bytes.

_pdata_ – Pointer to a buffer allocated using [RFAPI\_GetSamplesBuffer\(\)]().

**Returns:**

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_TIMEOUT – Could not complete the operation within the timeout period.

RFAPI\_ERR\_OK

### RFAPI\_LL\_ReceiveData

RFAPI\_STATUS RFAPI\_CALL RFAPI\_LL\_ReceiveData\(RFAPI\_HANDLE \*handle,

size\_t size, samples\_buf\_ptr\_t \*pdata\)

**Description:**

Receive data from the hardware.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_size_ – Number of bytes to receive. Size should be a multiple of 128 bytes.

_pdata_ – Pointer to a buffer allocated using [RFAPI\_GetSamplesBuffer\(\)]().

**Returns:**

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_TIMEOUT – Could not complete the operation within the timeout period.

RFAPI\_ERR\_OK

### RFAPI\_LL\_Read\_I2C

RFAPI\_STATUS RFAPI\_CALL RFAPI\_LL\_Read\_I2C\(RFAPI\_HANDLE \*handle,

uint32\_t address, uint32\_t offset, uint32\_t len, uint8\_t \*results\)

**Description:**

Read I2C data from an I2C slave device connected to the processing system, PS, portion of the system on chip.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_address_ – Seven-bit slave address of the I2C device to obtain data from.

_offset_ – Register address or start address to read from on the I2C device_._

_len_ – Byte length of the data to be received from the I2C device.

_results_ – Pointer to a previously allocated buffer. The buffer should be as large as len argument passed to the function.

**Returns:**

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_TIMEOUT – Could not complete the operation within the timeout period.

RFAPI\_ERR\_OK

**Notes:**

Only supported when controlling the device from the TCP/IP interface. Support of this function on the PCIe interface will be implemented in future RFAPI versions.

### RFAPI\_LL\_Write\_I2C

RFAPI\_STATUS RFAPI\_CALL RFAPI\_LL\_Write\_I2C\(RFAPI\_HANDLE \*handle,

uint32\_t address, uint32\_t offset, uint32\_t len, uint8\_t \*data\)

**Description:**

Write I2C data to an I2C slave device connected to the processing system, PS, portion of the system on chip.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_address_ – Seven-bit slave address of the I2C device to send data to.

_offset_ – Register address or start address to write on the I2C device_._

_len_ – Byte length of the data to be sent to the I2C device.

_results_ – Pointer to a buffer containing the data to be sent to the I2C device. The buffer should be as large as len argument passed to the function.

**Returns:**

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_TIMEOUT – Could not complete the operation within the timeout period.

RFAPI\_ERR\_OK

**Notes:**

Only supported when controlling the device from the TCP/IP interface. Support of this function on the PCIe interface will be implemented in future RFAPI versions.

### RFAPI\_SG\_ConfigureGeneration

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SG\_ConfigureGeneration\(RFAPI\_HANDLE \*handle, int32\_t\* channels, int32\_t num\_channels, int32\_t repeat\_count, int32\_t burst\_size\)

**Description:**

Specifies which channels to configure for waveform generation.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_channel_ – Pointer to an array of channels on which to generate data. Examples {0}, {0,2,4,6}, {0,1,2,3,4,5,6,7}.

_num\_channels_ – Length of the channels array, less than RFAPI\_MAX\_CHANNELS.

_repeat\_count_ – Specifies how many times to repeat the waveform during generation. A value of 0 will result in the waveform being looped indefinitely.

_burst\_size_ – Number of I/Q samples to play back. Must be a multiple of 32.

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_VERSION – The version of the RFAP\_DEVICE\_PARAMS is not the right version.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_GENERIC

### RFAPI\_SG\_ConfigureDUC

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SG\_ConfigureDUC\(RFAPI\_HANDLE \*handle, int32\_t mixer\_mode, int32\_t \*channels, int32\_t num\_channels, int32\_t coarse\_mode, float fine\_nco\_frequency, int32\_t interpolation\_factor\)

**Description:**

Configures the digital upconverters \(DUCs\) for the specified channels. The mixers can be configured in coarse or fine mode. In the coarse mixer mode, the user can choose the NCO frequency to be 0MHz, Fs/2, Fs/4 or -Fs/4. In the fine mixer mode, the user can choose an arbitrary frequency ranging between -Fs/2 and Fs/2. All channels listed in the _channels_ array will be updated synchronously. The function also configures the interpolation factor but the firmware currently only supports 8x interpolation.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_mixer\_mode_ – Configure DUC operation to either coarse mixer mode \(RFAPI\_DXC\_COARSE\_MIXER\_MODE\) or fine mixer mode \(RFAPI\_DXC\_FINE\_MIXER\_MODE\)

_channels_ – Array of length _num\_channels_ specifying the channels to configure.

_num\_channels –_ Number of channels to configure synchronously.

_coarse\_mode_ – Configure the coarse mixer mode to 0MHz \(RFAPI\_DXC\_COARSE\_MIXER\_BYPASS\), Fs/2 \(RFAPI\_DXC\_COARSE\_MIXER\_FS\_DIV\_2\), Fs/4 \(RFAPI\_DXC\_COARSE\_MIXER\_FS\_DIV\_4\) or -Fs/4 \(RFAPI\_DXC\_COARSE\_MIXER\_MIN\_FS\_DIV\_4\). This argument is discarded if ‘mixer\_mode’ is configured to RFAPI\_DXC\_FINE\_MIXER\_MODE.

_fine\_nco\_frequency_ – Frequency of the DUC numerically controlled oscillator \(NCO\) in MHz. This argument is discarded if ‘mixer\_mode’ is RFAPI\_DXC\_COARSE\_MIXER\_MODE .

_interpolation\_factor_ – Interpolation factor of the DUC. 8x interpolation \(RFAPI\_DXC\_DECIMATION\_INTERPOLATION\_8X\)

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_VERSION – The version of the RFAP\_DEVICE\_PARAMS is not the right version.

RFAPI\_ERR\_NO\_DUC\_HW – No upconverter available on the hardware.

RFAPI\_ERR\_GENERIC

### RFAPI\_SG\_ConfigureTrigger

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SG\_ConfigureTrigger \(RFAPI\_HANDLE \*handle, int32\_t trigger\_mode\)

**Description:**

Specifies how the start of waveform generation will be triggered.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_trigger\_mode_ – Type of triggering, RFAPI\_TRIGGER\_SW, RFAPI\_TRIGGER\_HW, RFAPI\_TRIGGER\_BOTH.

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_TRIGGER – Unsupported triggering mode received as argument

### RFAPI\_SG\_SendWaveform

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SG\_SendWaveform\(RFAPI\_HANDLE \*handle, int32\_t channel, samples\_buf\_ptr\_t\* waveform\_data, int32\_t waveform\_size\)

**Description:**

Writes waveform data to device memory. Clears device memory before writing the new waveform.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_channel_ – The channel to write waveform data to. The channel must have been included in the channel list specified in [RFAPI\_SG\_ConfigureGeneration\(\)]().

_waveform\_data_ – Pointer to a DAC data buffer. Buffer management is the responsibility of the user. Data must be 4096 bytes aligned and allocated to waveform\_size at a minimum.

_waveform\_size_ – Number of I/Q samples in the _waveform\_data_ buffer to write to the hardware. The buffer can contain more samples, but they will simply be discarded. Must be a multiple of 32.

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

### RFAPI\_SG\_Initiate

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SG\_Initiate\(RFAPI\_HANDLE \*handle\)

**Description:**

Verifies all parameters previously set by the user and commits the settings to hardware. If a hardware trigger was previously configured with RFAPI\_SG\_ConfigureTrigger\(\), arms repeat FIFOs to begin generating on the next trigger. If a software trigger was previously configured with [_RFAPI\_SG\_ConfigureTrigger\(\)_](), then this function issues a software trigger.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

### RFAPI\_SG\_Abort

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SG\_Abort\(RFAPI\_HANDLE \*handle\)

**Description:**

Stops waveform generation out of the VP430 DACs and resets the read pointers of the waveform generation memories.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

### RFAPI\_SA\_ConfigureAcquisition

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SA\_ConfigureAcquisition\(RFAPI\_HANDLE \*handle, int\* channels, int32\_t num\_channels, int32\_t num\_bursts, int32\_t burst\_size\)

**Description:**

Specifies which channels to capture data for, as well as the number of bursts to acquire per channel, and the number of samples per burst. For snapshot-based acquisitions the size of num\_channels \* num\_burst \* burst\_size \* sample\_size should be smaller than the storage size in the FPGA firmware. For streaming applications there is no limit in size.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_channel_ – Pointer to an array of channels on which to acquire data. Examples {0}, {0,2,4,6}, {0,1,2,3,4,5,6,7}.

_num\_channels_ – Length of the channels array, less than MAX\_CHANNELS.

_num\_bursts_ – Number of burst to acquire per channel.

_burst\_size_ – Number of I/Q samples in a burst. Must be a multiple of 32.

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

### RFAPI\_SA\_ConfigureDDC

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SG\_ConfigureDDC\(RFAPI\_HANDLE \*handle, int32\_t mixer\_mode, int32\_t \*channels, int32\_t num\_channels, int32\_t coarse\_mode, float fine\_nco\_frequency, int32\_t decimation\_factor\)

**Description:**

Configures the digital downconverters \(DDCs\) for the specified channels. The mixers can be configured in coarse or fine mode. In the coarse mixer mode, the user can choose the NCO frequency to be 0MHz, Fs/2, Fs/4 or -Fs/4. In the fine mixer mode, the user can choose an arbitrary frequency ranging between -Fs/2 and Fs/2. All channels listed in the _channels_ array will be updated synchronously. The function also configures the decimation factor but the firmware currently only supports 8x decimation.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_mixer\_mode_ – Configure DDC operation to either coarse mixer mode \(RFAPI\_DXC\_COARSE\_MIXER\_MODE\) or fine mixer mode \(RFAPI\_DXC\_FINE\_MIXER\_MODE\)

_channels_ – Array of length _num\_channels_ specifying the channels to configure.

_num\_channels –_ Number of channels to configure synchronously.

_coarse\_mode_ – Configure the coarse mixer mode to 0MHz \(RFAPI\_DXC\_COARSE\_MIXER\_BYPASS\), Fs/2 \(RFAPI\_DXC\_COARSE\_MIXER\_FS\_DIV\_2\), Fs/4 \(RFAPI\_DXC\_COARSE\_MIXER\_FS\_DIV\_4\) or -Fs/4 \(RFAPI\_DXC\_COARSE\_MIXER\_MIN\_FS\_DIV\_4\). This argument is discarded if ‘mixer\_mode’ is configured to RFAPI\_DXC\_FINE\_MIXER\_MODE.

_fine\_nco\_frequency_ – Frequency of the DDC numerically controlled oscillator \(NCO\) in MHz. This argument is discarded if ‘mixer\_mode’ is RFAPI\_DXC\_COARSE\_MIXER\_MODE .

_decimation\_factor_ – Interpolation factor of the DDC. 8x interpolation \(RFAPI\_DXC\_DECIMATION\_INTERPOLATION\_8X\)

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_VERSION – The version of the RFAP\_DEVICE\_PARAMS is not the right version.

RFAPI\_ERR\_NO\_DUC\_HW – No upconverter available on the hardware.

RFAPI\_ERR\_GENERIC

### RFAPI\_SA\_ConfigureTrigger

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SA\_ConfigureTrigger \(RFAPI\_HANDLE \*handle, int32\_t trigger\_mode\)

**Description:**

Specifies how an acquisition will be triggered.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_trigger\_mode_ – Type of triggering, RFAPI\_TRIGGER\_SW, RFAPI\_TRIGGER\_HW, RFAPI\_TRIGGER\_BOTH.

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

RFAPI\_ERR\_TRIGGER – Unsupported triggering mode received as argument.

### RFAPI\_SA\_Initiate

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SA\_Initiate \(RFAPI\_HANDLE \*handle\)

**Description:**

Verifies all parameters previously set by the user and commits the settings to hardware. Resets capture FIFOs. If a hardware trigger was previously configured with RFAPI\_SA\_ConfigureTrigger\(\), this function arms capture FIFOs to begin capturing on the next trigger. If a software trigger was previously configured with [_RFAPI\_SA\_ConfigureTrigger\(\)_](), then this function issues a software trigger.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

### RFAPI\_SA\_ReceiveWaveform

RFAPI\_STATUS RFAPI\_CALL RFAPI\_SA\_ReceiveWaveform\(RFAPI\_HANDLE \*handle,int32\_t channel, int32\_t burst\_number, samples\_t\* waveform\_data, uint32\_t waveform\_size\)

**Description:**

Fetches acquired data from device memory to host memory.

**Parameters:**

`handle` – pointer to a handle populated by calling [`RFAPI_DeviceInit()`]().

_channel_ – The channel from which to fetch data. The channel must have been included in the channel list specified in [RFAPI\_SA\_ConfigureAcquisition\(\)]().

_burst\_number_ – If a multi-record acquisition was configured, this specifies which burst to fetch.

waveform\_data – Pointer to ADC data buffer. Buffer management is the responsibility of the user. Data must be 4096 bytes aligned and allocated to burst\_size at a minimum.

_waveform\_size_ – Number of I/Q samples to fetch from device memory for each channel. Must be a multiple of 32.

**Returns:**

RFAPI\_ERR\_OK

RFAPI\_ERR\_NO\_SYS\_INIT – User should call RFAPI\_SystemInit\(\) before calling this function.

RFAPI\_ERR\_NULL – Unexpected NULL argument passed as argument.

RFAPI\_ERR\_INVALID\_HANDLE – Invalid handle passed as argument.

## 


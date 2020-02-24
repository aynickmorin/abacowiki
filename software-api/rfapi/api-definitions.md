# API Definitions

### Triggering

* `RFAPI_TRIGGER_SW` _–_ Software trigger.
* `RFAPI_TRIGGER_HW` _–_ Hardware trigger such as trigger input.
* `RFAPI_TRIGGER_BOTH` _–_ Both software and hardware trigger.
* `RFAPI_TRIGGER_HW_RE` _–_ Rising edge hardware trigger.
* `RFAPI_TRIGGER_HW_FE` _–_ Falling edge hardware trigger.
* `RFAPI_TRIGGER_HW_LVL` _–_ Level hardware trigger.

### Converters Clocking

* `RFAPI_INTERNAL_CLK_INTERNAL_REF` – Internal clock and internal reference.
* `RFAPI_INTERNAL_CLK_EXTERNAL_REF` – Internal clock and external reference.
* `RFAPI_EXTERNAL_CLK` _–_ External clock.

### Configuration

* `RFAPI_MAX_CHANNELS` _–_ Maximum number of channels supported by the RF API.
* `RFAPI_VERSION_1` _–_ API operation mode VERSION 1.0.
* `RFAPI_ENFORCE_ERROR_ENABLED` _–_ [`RFAPI_SetConvertersFrequencies()`]() will only succeed if all the converters can be configured to the requested frequencies.
* `RFAPI_ENFORCE_ERROR_DISABLED` _–_ [`RFAPI_SetConvertersFrequencies()`]() will configure converters as close as the requested frequency as possible.

### Communication

* `RFAPI_INTERFACE_PCIE` – PCIe is the target interface.
* `RFAPI_INTERFACE_TCPIP` – TCP/IP is the target interface.

### Diagnostics types

* `RFAPI_DIAGS_TYPE_VP430_V1` – The type of diagnostics retrieved by calling [`RFAPI_GetDeviceDiagnostics()`]() is VP430 diagnostics version 1.

### Diagnostics pointers

* `VP430_DIAGS_V1` – Device specific pointer to be used for casting the generic pointer received by [`RFAPI_GetDeviceDiagnostics()`]() if the diagnostic type received was `RFAPI_DIAGS_TYPE_VP430_V1`. Please refer to [`RFAPI_GetDeviceDiagnostics()`]() for an example.

### RF Converters

The following definitions relate to [`RFAPI_SA_ConfigureDDC()`]() and [`RFAPI_SG_ConfigureDUC()`]().

| Macro | Description |
| :--- | :--- |
| `RFAPI_DXC_COARSE_MIXER_MODE` | Coarse mixer mode for the ADC or DAC tiles. |
| `RFAPI_DXC_FINE_MIXER_MODE` | Fine mixer mode for the ADC or DAC tiles. |
| `RFAPI_DXC_COARSE_MIXER_BYPASS` | Coarse mixer is bypassed \(disabled\). |
| `RFAPI_DXC_COARSE_MIXER_FS_DIV_2` | Coarse mixer is configured to Fs/2. |
| `RFAPI_DXC_COARSE_MIXER_FS_DIV_4` | Coarse mixer is configured to Fs/4. |
| `RFAPI_DXC_COARSE_MIXER_MIN_FS_DIV_4` | Coarse mixer is configured to -Fs/4. |
| `RFAPI_DXC_DECIMATION_INTERPOLATION_OFF` | No interpolation or decimation. |
| `RFAPI_DXC_DECIMATION_INTERPOLATION_1X` | 1x interpolation or decimation. |
| `RFAPI_DXC_DECIMATION_INTERPOLATION_2X` | 2x interpolation or decimation. |
| `RFAPI_DXC_DECIMATION_INTERPOLATION_4X` | 4x interpolation or decimation. |
| `RFAPI_DXC_DECIMATION_INTERPOLATION_8X` | 8x interpolation or decimation. |

### Others

| Macro | Description |
| :--- | :--- |
| `RFAPI_MAX_SHELL_RESULT_SZ` | Maximum byte size a shell answer can be when calling [`RFAPI_SendShellCommand()`]()\`\` |
| `VP430_UPLOAD_IMAGE_DEST_BOOT` | The boot image destination when calling [`RFAPI_UploadQSPIImage()`]() |
| `VP430_UPLOAD_IMAGE_DEST_KERNEL` | The kernel image destination when calling [`RFAPI_UploadQSPIImage()`]()\`\` |


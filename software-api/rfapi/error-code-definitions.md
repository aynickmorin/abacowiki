# Error Code Definitions

### High-Level

| **Macro** | **Value** | **Description** |
| :--- | :--- | :--- |
| `RFAPI_ERR_OK` | 0 | No error encountered, success. |
| `RFAPI_ERR_VERSION` | -1 | Version of a data structure is not supported by the current API mode operation. |
| `RFAPI_ERR_GENERIC` | -2 | Low-level error encountered, call [`RFAPI_GetLastErrors()`]() in order to get the actual low-level error codes. |
| `RFAPI_ERR_INVALID_HANDLE` | -3 | Invalid handle received as argument. The handle should be obtained by calling [`RFAPI_DeviceInit()`](). |
| `RFAPI_ERR_TRIGGER` | -4 | Invalid triggering mode received as argument. |
| `RFAPI_ERR_NULL` | -5 | Invalid NULL argument received as argument. |
| `RFAPI_ERR_TIMEOUT` | -6 | Timeout on a low-level function call, error communicating with the hardware. |
| `RFAPI_ERR_NO_SYS_INIT` | -7 | [`RFAPI_SystemInit()`]() should be called before calling any other functions. |
| `RFAPI_ERR_ADC_DAC_CFG` | -8 | \`\`[`RFAPI_SetConvertersFrequencies()`]() could not configure one of the converters to the requested frequency and the en_force\_error_ argument was enabled. [Supported Converters Sampling Frequencies]() describes the supported frequencies. |
| `RFAPI_ERR_NO_MEMORY` | -9 | Could not allocate a memory buffer; no more memory available in the system. |
| `RFAPI_ERR_NO_DUC_HW` | -10 | No digital upconverter available in the hardware. |
| `RFAPI_ERR_NO_DDC_HW` | -11 | No digital downconverter available in the hardware. |
| `RFAPI_ERR_DEVICE_INIT` | -12 | Could not initialize the device. |
| `RFAPI_ERR_DEVICE_NOT_SUPPORTED` | -13 | The requested device is not supported by the API. |
| `RFAPI_ERR_NOT_IMPLEMENTED` | -14 | A function received an argument causing not implemented mode to be used. |
| `RFAPI_ERR_SHELL_CMD_FAILED` | -15 | A remote procedure call to the PetaLinux shell via [`RFAPI_SendShellCommand()`]() has failed. |
| `RFAPI_ERR_POLLING` | -16 | Maximum number of attempts when polling a register exceeded. |
| `RFAPI_ERR_INVAL` | -17 | Invalid argument. |
| `RFAPI_ERR_FILE` | -18 | File I/O related error. |

### Low-Level

| **Definition** | **Value** | **Description** |
| :--- | :--- | :--- |
| TBD | TBD | None defined as per today, elements will be added to this table during implementation |


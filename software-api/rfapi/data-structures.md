# Data Structures

### `RFAPI_DEVICE_PARAMS`

```cpp
typedef struct RFAPI_DEVICE_PARAMS
{
    /* initialization fields by user */
    uint32_t format_version;
    uint32_t interface_type;
    char* device_type; /* “VP430” */
    uint32_t device_index;
    uint32_t fmc_type;
    uint32_t sampling_clock_mode;
    bool ref_out_enable;
    bool sync_out_enable;
    uint32_t timeout_ms;
    void* unit_api_specific_config;
    void* fmc_device_specific_config;
    /* fields returned by function, i.e. device capabilities */
    double max_adc_sample_rate;
    double max_dac_sample_rate;
    uint32_t nbr_adc_channels;
    uint32_t nbr_dac_channels;
    uint32_t device_mode; /* ADC, DAC, ADC/DAC */
    uint32_t DDC_supported;
    uint32_t DUC_supported;
    uint32_t adc_sample_size;
    uint32_t adc_sample_type;
    uint32_t dac_sample_size;
    uint32_t dac_sample_type;
    uint32_t input_storage_byte_size;
    uint32_t output_storage_byte_size;
    void *device_specific_return_params;
} RFAPI_DEVICE_PARAMS;
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Variable</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>format_version</code>
      </td>
      <td style="text-align:left">Version of the data structure</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>interface_type</code>
      </td>
      <td style="text-align:left">
        <p><code>RFAPI_INTERFACE_PCIE</code>
        </p>
        <p><code>RFAPI_INTERFACE_TCPIP</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>device_type</code>
      </td>
      <td style="text-align:left"><code>VP430</code>, <code>192.168.1.10</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>device_index</code>
      </td>
      <td style="text-align:left">For PCIe device index. For TCP, port 1000.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>fmc_type</code>
      </td>
      <td style="text-align:left">
        <p>Expected FMC card(s).</p>
        <p>(This field can specify the constellation, and SOCs such as RFSOC).</p>
        <p>Bits 28-31:</p>
        <p> <code>0</code> &#x2013; FMC card type</p>
        <p> <code>1</code> &#x2013; Constellation</p>
        <p> <code>2</code> &#x2013; SOC
          <br />
        </p>
        <p>Bits 0-27:</p>
        <p>FMC card type:</p>
        <p>ABACO_FMC150 &#x2013; <code>0</code>
        </p>
        <p>ABACO_FMC126 &#x2013; <code>1</code>
        </p>
        <p>Etc.
          <br />
        </p>
        <p>Constellation_ID:</p>
        <p>Specify the actual constellation ID for specialized firmware (PC821 with
          different FMC cards as an example)</p>
        <p>SOC:</p>
        <p>VP430 &#x2013; <code>0</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>sampling_clock_mode</code>
      </td>
      <td style="text-align:left">Internal clock with internal reference (<code>RFAPI_INTERNAL_CLK_INTERNAL_REF</code>),
        internal clock with external reference (<code>RFAPI_INTERNAL_CLK_EXTERNAL_REF</code>)
        and external clock (<code>RFAPI_EXTERNAL_CLK</code>)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>ref_out_enable</code>
      </td>
      <td style="text-align:left">If true, enables the REF_OUT signal on supported devices.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>sync_out_enable</code>
      </td>
      <td style="text-align:left">If true, enables the SYNC_OUT signal on supported devices. Only supported
        for VP460 devices.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>timeout_ms</code>
      </td>
      <td style="text-align:left">Timeout for low-level operation in milliseconds.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>fmc_device_specific_config</code>
      </td>
      <td style="text-align:left">TBD &#x2013; for devices with advanced features (RFSOC, etc)</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>max_adc_sample_rate</code>
      </td>
      <td style="text-align:left">Maximum ADC sample rate</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>max_dac_sample_rate</code>
      </td>
      <td style="text-align:left">Maximum DAC sample rate</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>nbr_adc_channels</code>
      </td>
      <td style="text-align:left">Number of ADC channels available</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>nbr_dac_channels</code>
      </td>
      <td style="text-align:left">Number of DAC channels available</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DDC_supported</code>
      </td>
      <td style="text-align:left"><code>0</code>&#x2013; DDC not supported by hardware
        <br /><code>1</code> &#x2013; DDC supported by hardware</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>DUC_supported</code>
      </td>
      <td style="text-align:left"><code>0</code> &#x2013; DUC not supported by hardware
        <br /><code>1</code> &#x2013; DUC supported by hardware</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>device_mode</code>
      </td>
      <td style="text-align:left"><code>1</code> &#x2013; ADC
        <br /><code>2</code> &#x2013; DAC
        <br /><code>3</code> &#x2013; ADC and DAC
        <br /><code>4</code> &#x2013; &#x201C;Other&#x201D;</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>adc_sample_size</code>
      </td>
      <td style="text-align:left">How many bytes per ADC samples</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>adc_sample_type</code>
      </td>
      <td style="text-align:left"><code>RFAPI_SAMPLES_COMPLEX </code>(I/Q combined) or <code>RFAPI_SAMPLES_REAL </code>for
        ADC</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>dac_sample_size</code>
      </td>
      <td style="text-align:left">How many bytes per DAC samples</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>dac_sample_type</code>
      </td>
      <td style="text-align:left"><code>RFAPI_SAMPLES_COMPLEX </code>(I/Q combined) or <code>RFAPI_SAMPLES_REAL </code>for
        DAC</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>input_storage_byte_size</code>
      </td>
      <td style="text-align:left">The maximum number of bytes of internal storage in the FPGA firmware.
        Setting the acquisition to a value greater than this is not supported.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>output_storage_byte_size</code>
      </td>
      <td style="text-align:left">The maximum number of bytes of internal storage in the FPGA firmware.
        Setting the generation to a value greater than this is not supported.</td>
    </tr>
  </tbody>
</table>### `RFAPI_MODULES_ERRORS_PARAMS`

This structure is passed to [`RFAPI_GetLastErrors()`](). The function will populate various error codes encountered by the API. The error codes encountered are found [here]().

```cpp
typedef struct RFAPI_MODULES_ERRORS_PARAMS
{
	/* initialization fields by user */
	uint32_t last_socket_error;
	uint32_t last_comm_tcpip_error;
	uint32_t last_comm_tcpip_sip_error;   
	uint32_t last_comm_pcie_error;
	uint32_t last_comm_error;
	uint32_t last_devices_error;
} RFAPI_MODULES_ERRORS_PARAMS;
```

| Variable | Description |
| :--- | :--- |
| `last_socket_error` | Socket error, operating system related errors such as why a socket could not be created or why access to a socket is denied. This relates to TCP/IP |
| `last_comm_tcpip_error` | TCP/IP communication error, strictly error between the TCP/IP server \(the device\) and the TCP/IP client \(any SBC or computer controlling the device\). |
| `last_comm_tcp_sip_error` | Protocol error between the FPGA firmware and the host computer. |
| `last_comm_pcie_error` | PCIe communication error, strictly between the PCIe end point \(the device\) and the PCIe root complex \(any SBC or computer controlling the device\). |
| `last_comm_err` | Abstraction \(between PCIe and TCP/IP\) layer error. |
| `last_devices_error` | Abstraction \(between Abaco devices\) layer error. |

## 


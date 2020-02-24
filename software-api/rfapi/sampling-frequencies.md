# Sampling Frequencies

This chapter lists the valid sampling frequencies for the converters on a given product as well as the number of converters available on the hardware

### VP430

The VP430 has 8 A/D converters as well as 8 D/A converters. The frequencies are in an increment of 500MHz and two D/A frequencies are supported for each A/D frequency. Note that all the A/D must be configured to the same frequency and all the D/A converters must be configured to the same frequency also. Note that the frequencies in the table below are in MHz and that ‘Clock mode’ is different than _sampling\_clock\_mode_ in the [RFAPI\_DEVICE\_PARAMS]() data structure.

| **Clock mode** | **ADC/DAC relationship** | **ADC** | **DAC** |
| :--- | :--- | :--- | :--- |
| 1 | 1:1.6 | 4000 | 6400 |
| 2 | 1:1 | 4000 | 4000 |
| 3 | 1:1.6 | 3500 | 5600 |
| 4 | 1:1 | 3500 | 3500 |
| 5 | 1:1.6 | 3000 | 4800 |
| 6 | 1:1 | 3000 | 3000 |
| 7 | 1:1.6 | 2500 | 4000 |
| 8 | 1:1 | 2500 | 2500 |
| 9 | 1:1.6 | 2000 | 3200 |
| 10 | 1:1 | 2000 | 2000 |
| 11 | 1:1.6 | 1500 | 2400 |
| 12 | 1:1 | 1500 | 1500 |
| 13 | 1:1.6 | 1000 | 1600 |

## 


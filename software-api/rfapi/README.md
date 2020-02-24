# RFAPI

The RFAPI documentation describes the Abaco 4DSP API for programming products containing A/D converters and D/A converters. The first release of the API is only targeting the Abaco VP430 and might be extended to include Abaco FMC specific cards in the future.

There are four groups of functions:

* Functions containing `SA`in the name are functions relating to signal acquisition.
* Functions containing `SG` in the name are functions relating to signal generation.
* Functions containing `LL` in the name are low level functions. These functions allow the programmer to directly read and write registers at a given address as well as receiving and sending data. Most users generally won’t need these functions, but expert users might want to use them during their development
* The rest are ‘housekeeping’ functions that are used to provide board diagnostics, initialize the boards, etc.

fd

saf

dsa

fdsafdsa


# Known Issues/ Limitations



* `eth0` is not accessible by U-Boot. Any network connections used U-Boot must be performed via `eth1`. 
* `eth1`is not able to signal the status of the Ethernet \(RJ45\) link to Linux or the user. The link will always show as up even with no Ethernet cable connected.
* During boot, a kernel “warning” \(similar to a kernel panic but without a kernel halt\) will be seen on the console port. This can be safely ignored.




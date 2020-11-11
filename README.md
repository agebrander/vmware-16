# vmware-16
solved problem installing vmware 16 in kali.
When installing vmware 16 inkali there is a erro ' GCC compatible compiler not found' not found.
with thanks to gdelcampo (https://communities.vmware.com/t5/VMware-Workstation-Player/Debian-Bullseye-testing-unable-to-compile-kernel-modules-gcc/m-p/2310048)

'T seems to be more related to the kernel version rather than GCC and to the way vmware-modconfig (/usr/lib/vmware/bin/appLoader) retrieves information about Kernel GCC major version. I suspect the culprit is on extracting gcc version: note that from version gcc-10 we have 2 digit instead of 1 for the version number and in the logs we have  "GCC major version 10 does not match Kernel GCC major version 0." where 0 is the last digit of number 10.

Anyway, as user root you can manually compile the vmware host modules.
This will solve the error.

cd /usr/lib/vmware/modules/source<br>
tar xvf vmnet.tar<br>
cd vmnet-only<br>
make<br>
cd ..<br>
tar xvf vmmon.tar<br>
cd vmmon-only<br>
make<br>
cd ..<br>
cp vmmon.o /lib/modules/`uname -r`/kernel/drivers/misc/vmmon.ko<br>
cp vmnet.o /lib/modules/`uname -r`/kernel/drivers/misc/vmnet.ko<br>
depmod -a<br>
systemctl restart vmware.service<br>

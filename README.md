# vmware-16
solved problem installing vmware 16 in kali.
When installing vmware 16 inkali there is a erro ' GCC compatible compiler not found' not found.
with thanks to gdelcampo (https://communities.vmware.com/t5/VMware-Workstation-Player/Debian-Bullseye-testing-unable-to-compile-kernel-modules-gcc/m-p/2310048)

'T seems to be more related to the kernel version rather than GCC and to the way vmware-modconfig (/usr/lib/vmware/bin/appLoader) retrieves information about Kernel GCC major version. I suspect the culprit is on extracting gcc version: note that from version gcc-10 we have 2 digit instead of 1 for the version number and in the logs we have  "GCC major version 10 does not match Kernel GCC major version 0." where 0 is the last digit of number 10.

Anyway, as user root you can manually compile the vmware host modules.
This will solve the error.

cd /usr/lib/vmware/modules/source
tar xvf vmnet.tar
cd vmnet-only
make
cd ..
tar xvf vmmon.tar
cd vmmon-only
make
cd ..
cp vmmon.o /lib/modules/`uname -r`/kernel/drivers/misc/vmmon.ko
cp vmnet.o /lib/modules/`uname -r`/kernel/drivers/misc/vmnet.ko
depmod -a
systemctl restart vmware.service

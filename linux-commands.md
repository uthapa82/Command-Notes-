#### Timeshift backups 
* Delete the directory (empty)
 
  `$ sudo rm -d <file name> `
 
* Delete non empty directory 

  `$ sudo rim -r <file name>`

#### Ubuntu installation Partition
  * EFI 512 MB
    * Primary 
    * Begining 
    * EFI System 

  <br>

  * System Partition Root 50000 MB
    * Primary 
    * Begining 
    * Ext4 Journaling File System
    * Mount point /

    <br>
  
  * Swap Area (if 1GB RAM double the size else same as RAM)
    * 2000 MB
    * Primary 
    * Begining 
    * Swap Area 

    <br>
  
  * Home (for user files and folders)
    * Remaining
    * Primary 
    * Begining 
    * Ext4 Journaling file System 
    * mount point /home

* [https://docs.nvidia.com/networking/display/MLNXENv543681LTS/Ethernet+Driver+Usage+and+Configuration ] (NVIDIA Config)

* [https://docs.nvidia.com/networking/display/MLNXENv543681LTS/Installing+MLNX_EN]( Nvidia driver)

* [https://network.nvidia.com/products/ethernet-drivers/linux/mlnx_en/#tabs-1](driver download)

```
$ fdisk -l

$ mkdir /media/usbfile

$ mount /dev/sdb<name> /media/usbfile/

$ mount | grep sdb1

$ cp /media/usbfile/<filename> /home/admin1/

$ lsmod | grep loop

$ mkdir -p /mnt/driver (no need)

$ mount -o ro,loop <mlnx-en>.iso /mnt

$ /mnt/install 

$ mount -l or cat /proc/mounts

$ ifconfig -a doesn't show anything then next command

$ /etc/init.d/mlnx-en.d restart

```

#### Timeshift backups 
<<<<<<< HEAD
* Dlete the directory (empty)
=======
* Delete the directory (empty)
>>>>>>> 90ed800d95622b63dca0a9ad6a2980b20a174363
 
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


#### Auto shutdown 
* Shutdown after 20 minutes 

  `$ shutdown +20 `

* listing the port number in user to determine PID 

  `$ sudo netstat -lpn | grep :port_number `

* killing the running process

    `$ kill -9 PID `


#### Toubleshooting Wired Ethernet Connection in Ubuntu 20.04 
* show the network interface information

  `$ ifconfig `

* Show hardware/Ethernet controller information 

  `$ lspci`

* Turn off/on the ethernet interface and restart network manager 

  ```
    $ sudo ifconfig <enp4s0f1> down
    $ sudo ifconfig <enp4s0f1> up
    $ sudo /etc/init.d/network-manager  restart 

  ``` 
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

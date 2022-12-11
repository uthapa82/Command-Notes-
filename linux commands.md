#### Timeshift backups 
* Dlete the directory (empty)
 
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

  

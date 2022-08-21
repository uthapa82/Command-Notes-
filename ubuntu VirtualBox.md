### Ubuntu Virtual Box Guest Edition Procedure 

* Unmount the existing ...guestedition.iso file either through CD icon right click or using VirtualBox main menu setting--> storage --->controller IDE ---> right click remove 

* Then open the ubuntu VM and type following commands 

`$ sudo apt update `

`$ sudo apt install -y build-essential linux-headers-$(uname -r)`

* After executing these above commands , install the guest edition again using Device ---> Insert Guest Additions CD image 

* if the install does not start automatically , go inside the CD icon folder right click and run autorun.sh script 

`..../VBox_GAs_6.1.30$ ./autorun.sh `

* Authenticate by providing password and after completion, press return then restart the VM, Now You should have the full screen VM 
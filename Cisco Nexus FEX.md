### Cisco Nexus Fabric Extender (FEX)

#### Official Documentation 
* Install feature-set fex 

`n9K-1(config)# install feature-set fex`

* Enable feature-set fex

`n9K-1(config)# feature-set fex`

* Specify a port channel to configure 

`n9K-1(config)#  interface port-channel 4`

* Set the port channel to support an external fabric Extender 

`n9K-1(config)#  switchport mode fex-fabric`

* Associates a FEX number to the Fabric Extender unit attached to the interface. 
The range is from 101 to 199

`n9K-1(config)# fex assoxiate 101`

* Displays the association of a fabric Extender to a port channel interface 

`n9K-1(config)# do show interface port-channel 4 fex-intf`

* This example shows how to associate the Fabric Extender to a port channel interface on the parent device:

```
switch# configure terminal
switch(config)# interface ethernet 1/28
switch(config-if)# channel-group 4
switch(config-if)# no shutdown
switch(config-if)# exit
switch(config)# interface ethernet 1/29
switch(config-if)# channel-group 4
switch(config-if)# no shutdown
switch(config-if)# exit
switch(config)# interface ethernet 1/30
switch(config-if)# channel-group 4
switch(config-if)# no shutdown
switch(config-if)# exit
switch(config)# interface ethernet 1/31
switch(config-if)# channel-group 4
switch(config-if)# no shutdown
switch(config-if)# exit
switch(config)# interface port-channel 4
switch(config-if)# switchport
switch(config-if)# switchport mode fex-fabric
switch(config-if)# fex associate 101
```

* This example shows how to display the association of the Fabric Extender and the parent device:
```
switch# show interface port-channel 4 fex-intf
Fabric           FEX
Interface        Interfaces
---------------------------------------------------
Po4              Eth101/1/48   Eth101/1/47   Eth101/1/46   Eth101/1/45
                 Eth101/1/44   Eth101/1/43   Eth101/1/42   Eth101/1/41
                 Eth101/1/40   Eth101/1/39   Eth101/1/38   Eth101/1/37
                 Eth101/1/36   Eth101/1/35   Eth101/1/34   Eth101/1/33
                 
 ```

* Initial Configuration
    * ssh key rsa 2048 
```
    n9K-1(config)# write erase 

    n9K-1(config)# sh run 

    n9K-1# sh version | ex Processor 

    n9K-1# sh module | end sw

    n9K-1(config)#
```
*  Fex configuration 
```
    n9K-1# config

    n9K-1(config)# sh int stat

    n9K-1(config)# feature fex

    n9K-1(config)# int range ..

    n9K-1(config)# description FEX ..

    n9K-1(config)# channel-group 100 ((1-199))

    n9K-1(config)# int po100
```
[cisco Nexus 9K](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus2000/sw/configuration/guide/n9k_rel_703I11/b_Cisco_Nexus_2000_Series_NX-OS_Fabric_Extender_Configuration_Guide_for_Cisco_Nexus_9000_Series_Switches_Release_7x/b_Cisco_Nexus_2000_Series_NX-OS_Fabric_Extender_Configuration_Guide_for_Cisco_Nexus_9000_Series_Switches_Release_7x_chapter_011.html)
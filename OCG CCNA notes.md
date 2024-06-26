### Securing Switch CLI 
 
#### Basic Password Configuration 
```bat
switch(config)# username admin secret "password"
switch # config t
switch(config)# line console 0
switch(config-line)# login local

switch(config)# line vty 0 15 
switch(config-)# login local
```
#### SSH specific Configuration
```bat
switch(config)# hostname sw1
switch(config)#ip domain-name example.com
switch(config)#crypto key generate rsa 
! modulus at least 768 bits to support SSHv2
switch(config)#ip ssh version 2

switch(config)#line vty 0 15
switch(config)#login local
switch(config)#username admin secret cisco

!support for telnet or ssh 
switch(config)#transport input telnet ssh

! display ssh status -> lists status information about ssh server 
SW1# sh ip ssh 

! list information about each ssh cient currently connected into the switch
SW1#show ssh 

! tells IOS to sync the syslog message display with the messages requested using show command 
# logging synchronous 

! disable DNS resolve in switch 
switch(config)#no up domain-lookup 
```

#### EXEC command Reference 
| Command  | Purpose |
| ---------|---------|
| `$ write erase` 
| `$ erase startup-config ` | these enable mode EXEC commands erase the startup-config file | 
| `$ erase nvram:` 
| `$ show mac address-table ` | shows all MAC table entries of all types |
| `$ show mac address-table dynamic ` | shows all dyamically learned MAC table entries |
| `$ show mac address-table dynamic vlan <vlan-id> ` | shows the dynamically learned MAC table entries in that VLAN | 
| `$ show mac address-table dynamic interface <interface-id> ` | shows all dynamically learned MAC table entries associated with that interface | 
| `$ show mac address-table count ` | Shows the number of entries in the MAC table and the total number of remaining empty slots in the MAC table |
| `$ show mac address-table aging-time ` | shows the global and per-VLAN aging timeout for inactive MAC table entries |
| `$ clear mac address-table dynamic` | Empties the MAC table of all dynamic entries |
| `$ show interfaces status` | list one line per interface on the switch, with basic status and operating information for each |
| `$ clear mac address-table dynamic [vlan vlan-number] [interface interface-id][address mac-address]` | clears(removes) dynamic MAC table entries:either all(with no paramters), or a subset based on VLAN ID, interface ID, or a specific MAC address |


#### Basic Switch Management 
| Command  | Purpose |
| ---------|---------|
| `$ show dhcp lease ` | Lists any information the switch acquires as a DHCP client. This includes  IP address, subnet mask, and default gateway information |
| `$ show crypto key mypublickey rsa` | Lists the public and shared key created for use with SSH using the crypto key generate rsa global configuration command. |
| `$ show ip ssh ` | Lists status information for the SSH server, including the SSH version |
| `$ show ssh ` | Lists status information for current SSH connection into and out of the local switch |
| `$ show intefaces vlan number ` | Lists the interface status, the switch's IPv4 address and mask etc |
| `$ show ip default-gateway` | Lists the switch's setting for its IPv4 default gateway |
| `$ terminal history size x` | Changes the length of history buffer for the current user only, only for the current login to the switch |
| `$ show history  `| Lists the commands in the current history buffer | 


#### Configuring and Verifying Switch Interface 
| Command | Mode/Purpose/Description |
|---------|--------------------------|
| `$ interface type port-number ` | Changes context to interface mode. Fast Ethernet or Gigabit ethernet | 
| `$ interface range type port-number - end-port number ` | Changes the context tot interface mode for a range of consecutively numbered interfaces |
| `$ description text  `| Interface mode. Helps by providing information needed to track for the interfaces | 
| `$ show running-config interfacc type number ` | Displays the running-configuration excerpt of the listed interface and its subcommands only |


#### Trunking Operational Mode Based on the configured Administrative Modes 
| Administrative Mode  | Access  | Dynamic Auto | Trunk | Dynamic Desirable |
| ---------|---------|-----------|---------|----------|
|access | Access | Access | Do Not Use | Access |
|dynamic auto | Access | Access | Trunk | Trunk |
|trunk | Do Not Use | Trunk | Trunk | Trunk |
|dynamic desirable | Access | Trunk | Trunk | Trunk| 
*When two switches configure mode of "access" on one end and "trunk" on the other, problem occurs so Avoid the combination*


| Command | Description |
|---------|--------------------------|
| `$ show mac address-table dynamic [interface type number] [vlan vlan-id] ` | Lists the dynamically learned entries in the switch's address(forwarding) table, with subsets by interface and/or VLAN |
| `$ show mac address-table static [interface type number]` | Lists the static MAC addresses and MAC addresses Learned or defined with port security|


#### Virtual LANs 
| Command | Description |
|---------|--------------------------|
| `$ vlan vlan-id` | Global config command that both creates the VLAN and puts the CLI into VLAN configuration mode|
| `$ name vlan-name` | VLAN subcommand that names the VLAN |
| `$ shutdown vlan vlan-id` | Global config command that has the same effect as the [no] shutdown VLAN mode subcommands |
| `$ switchport mode [access or dynamic {auto or desirable } or trunk]` | Interface subcommand that configures thetrunking administrative mode on the interface |
| `$ switchport access vlan vlan-id` | Interface subcommand that statically configures the interface into that one VLAN | 
| `$ switchport trunk encapsulation [dot1q or isl or negotiate]` | Interface subcommand that defines which type of trunking to use, assuming that trunking is configured or negotiated |
| `$ switchport trunk native vlan vlan-id` | Interface subcommand that defines the native VLAN for a trunk port |
| `$ switchport nonegotiate` | Interface subcommand that disables the negotiation of VLAN trunking |
| `$ switchport voice vlan vlan-id` | Interface subcommand that defines the voice VLAN on a port meaning that the switch uses 802.1Q tagging for frames in this VLAN |
| `$ switchport trunk allowed vlan {add or all or except or remove} vlan-list` | Interface subcommand that defines the list of allowed VLANs|
| `$ show interfaces interface-id switchport` | Lists information about any interface regarding administrative settings and operational state |
| `$ show interfaces interface-id trunk` | Lists information about all operational trunk but no other interfaces, including the lists of VLANs that can be forwarded over the trunk|  
| `$ show vlan [brief or id vlan -id or name vlan-nameor summary]` | Lists information about the VLANs|


#### Configuration Command Reference 
| Command | Description |
|---------|--------------------------|
|`$ spanning-tree mode {pvst or rapid-pvst or mst}` | Global configuration command to set the STP mode |
|`$ spanning-tree [vlan vlan-number] root primary` | Global configuration command that changes this switch to the root switch. The switch's priority is changed to the lower of either 24,576 or 4096 less than the priority of the current root bridge when the command was issued|
`$ spanning-tree [vlan vlan-number] root secondary ` | Global configuation command that sets this switch's STP base priority to 28,672 |   
`$ spanning-tree vlan vlan-id priority priority` | Global configuation command that changes the bridge priority of this switch for the specified VLAN |
`$ spanning-tree [vlan vlan-number] cost cost ` | Interface subcommand that changes the STP cost to the configured value  |  
`$ spanning-tree [vlan vlan-number] port-priority priority ` | Interface subcommand that changes the STP port priority in that VLAN (0 to 240, in increments of 16) |
`$ channel-group channel-group-number mode {auto or diserable or active or passive or on }` | Interface subcommand that enables EhterChannel on the interface |  


#### EXEC Command Reference 
| Command | Description |
|---------|--------------------------|
|`$ show spanning-tree` | Lists details about the state of STP on the switch including the state of each port |
|`$ show spanning-tree vlan vlan-id` | Lists STP information for the specified VLAN |
|`$ show etherchannel [channel-group-number] {brief or detail or port or port-channel or summary}` | Lists information about the state of EtherChannels on this switch |

#### Rip Configuration 

```
# router rip 
# version 2
# no auto-summary 
# network <ip-address> eg. 10.0.0.0
# network <ip-address> eg. 172.16.0.0
(config-router)# passive-interface <interface-name> eg. g2/0

! command to tell about the default route 
(config-router)# default-information originate 
# show ip protocols 
```

#### Rip Configuration

```
# router eigrp <AS number> eg 1
# no auto-summary
# passive-interface g2/0
# network <ip-address>
# network <ip-address> can specify wildcard mask <0.0.0.15>

# eigrp router-id <id eg 1.1.1.1>

# show ip protocol

# show ip eigrp neighbors 
# show ip route eigrp 
# show ip eigrp topology 
```

#### OSPF Configuration

```
# router ospf process-id
# router ospf 1 <area_id >
# network <ip> <wildcard> area <area number>
# default-information originate 
# passive-interface <interface name>

# show ip ospf database 
# show ip ospf neighbor 
# show ip ospf interface 
# show ip ospf interface <interface-name>
(config-if)# auto-cost reference-bandwidth megabits-per-second
(config-if)# ip ospf cost <cost>
# show ip ospf brief 

! Activate OSPF directly on an interface 
# ip ospf process-id area <area>
# passive-interface default ---> for all interface 
# int g0/0
# ip ospf 1 area 0
```
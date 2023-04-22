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

| `$ show mac address-table static [interface type number]` | Lists the static MAC addresses and MAC addresses Learned or defined with port security  |

#### Virtual LANs 
| Command | Description |
|---------|--------------------------|
| `$ vlan vlan-id` | Global config command that both creates the VLAN and puts the CLI into VLAN configuration mode|
| `$ name vlan-name` | VLAN subcommand that names the VLAN |
| `$ shutdown vlan vlan-id` | Global config command that has the same effect as the [no] shutdown VLAN mode subcommands |
| `$ switchport mode [access | dynamic {auto |desirable }| trunk]` | Interface subcommand that configures thetrunking administrative mode on the interface |
| `$ switchport access vlan vlan-id` | Interface subcommand that statically configures the interface into that one VLAN | 
| `$ switchport trunk encapsulation [dot1q | isl | negotiate]` | Interface subcommand that defines which type of trunking to use, assuming that trunking is configured or negotiated |
| `$ switchport trunk native vlan vlan-id` | Interface subcommand that defines the native VLAN for a trunk port |
| `$ switchport nonegotiate` | Interface subcommand that disables the negotiation of VLAN trunking |
| `$ switchport voice vlan vlan-id` | Interface subcommand that defines the voice VLAN on a port meaning that the switch uses 802.1Q tagging for frames in this VLAN |
     
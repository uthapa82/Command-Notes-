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

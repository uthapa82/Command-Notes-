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

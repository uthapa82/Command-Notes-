#### Troubleshooting Issues 
* SW4’s configuration included commands that removed VLAN 12 from being allowed to pass over both of SW4’s trunks. To allow VLAN 12 to pass over the trunks, just configure the following commands on SW4:
```
SW(config)# interface fastethernet0/1

switchport trunk allowed vlan add 12

interface GigabitEthernet0/1

switchport trunk allowed vlan add 12
```

* SW2 was misconfigured so that the port connecting to S2 was in a different VLAN than all other PCs. To fix this, the port was reconfigured into VLAN 12 with all other PCs.
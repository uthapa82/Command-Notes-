### F5 TMOS


* Displays the configuration of an objects 

    `tmsh list - `

* Displays the information of an objects

    `tmsh show -`

* Show pool

    `tmsh show ltm pool <pool-name>`

* Create a VLAN on an untagged interface 

    `tmsh create /net vlan <vlan-name> interfaces add { <interface> }`

    eg. 
    `tmsh create /net vlan test-vlan interfaces add { 1.1 }`

* save the changes 

    `tmsh save /sys config` 

* verify 

    `tmsh show /net vlan `

* Self floating ip

    `(tmos)# list net self `    

* Create self ip 

    `tmsh create net self <name-of-ip> address <ip-address/mask> vlan <vlan-name>`

    eg. `tmsh create net self customer_vlan_ip address 10.1.20.241/24 vlan internal`

* Modify Self-IP

    `tmsh modify net self <ip-address/mask> vlan <vlan-name> traffic-group <traffic-group-name>`

    eg. `tmsh modify net self 10.1.1.1/16 vlan external traffic-group /common/traffic-group-1`    

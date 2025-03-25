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

* Determine if the VELOS chassis is using AC/DC PSU 

    ```
    syscon-1-active# show components component psu-* state

    components component psu-1
    state serial-no 19331BPJ0020
    state part-no SPAFFIV-07
    state empty false
    components component psu-2
    state serial-no 19332BPJ0086
    state part-no SPAFFIV-07
    state empty false
    components component psu-3
    state serial-no "Not Available"
    state part-no "Not Available"
    state empty true
    components component psu-4
    state serial-no "Not Available"
    state part-no "Not Available"
    state empty true
    components component psu-controller-1
    state firmware-version 1.02.669.0.1
    state software-version 2.00.781.0.1
    state serial-no  sub0759g002X
    state part-no    "SUB-0759-04 REV A"
    state empty      false
    components component psu-controller-2
    state firmware-version 1.02.669.0.1
    state software-version 2.00.781.0.1
    state serial-no  sub0759g002U
    state part-no    "SUB-0759-04 REV A"
    state empty      false
    ```
    - Here part number **SPAFFIV-07** indicates AC &  **SPDFFIV-08** indicates DC.

* List current allowed IPs to ssh 

    ```
    [root@bigip16-1:Active:Standalone] config # tmsh list /sys sshd allow
    sys sshd {
        allow { ALL }
    }
    # no commas, no netmask when adding single host 
    root@(bigip16-1)(cfg-sync Standalone)(Active)(/Common)(tmos)# modify /sys sshd allow add { 192.168. 10.10.10.1 }

    root@(bigip16-1)(cfg-sync Standalone)(Active)(/Common)(tmos)# modify /sys sshd allow add { 192.168.0.0/255.255.0.0 }

    root@(bigip16-1)(cfg-sync Standalone)(Active)(/Common)(tmos)# modify /sys sshd allow add { 192.168.0.0/16 }

    ! delete no longer needed IP addresses
    root@(bigip16-1)(cfg-sync Standalone)(Active)(/Common)(tmos)# modify /sys sshd allow delete { ALL }

    root@(bigip16-1)(cfg-sync Standalone)(Active)(/Common)(tmos)# save /sys config
    ```


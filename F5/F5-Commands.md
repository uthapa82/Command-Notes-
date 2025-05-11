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

* Configure Management IP address

    - Can use `config` command from bash in F5 BIG-IP CLI 

    
    ```bash 
    tmsh 
    create /sys management-ip [ip address/netmask]

    or 

    create /sys management-ip [ip address /prefixlen]
    For example:
    create /sys management-ip 192.168.1.245/255.255.255.0
    or
    create /sys management-ip 192.168.1.245/24

    # configure a default management gateway 
    create /sys management-route default gateway <gateway ip address>
    For example:
    create sys management-route default gateway 192.168.1.254

    save /sys config partitions all 

    # IPv6 address configuration examples 
    create /sys management-ip 2001:db8::2/FFFF:FFFF:FFFF:FFFF:0000:0000:0000:0000
    create /sys management-ip 2001:db8::2/64

    # configure a default ipv6 management gateway
    create /sys management-route default-inet6 gateway <gateway ip address>
    For example:
    create /sys management-route default-inet6 gateway 2001:db8::1
    ```

* Show Management IP address 

    ```bash
    tmsh list /sys management-ip

    sys management-ip 192.168.1.245/24 {
    description configured-statically
    }
    tmsh list /sys management-route

    sys management-route default {
    description configured-statically
    gateway 192.168.1.254
    network default
    }

    # using iControl REST interface 
    # management ip
    curl -sk -u <username>:<password> -H "Content-Type: application/json" -X GET https://<big-ip ip address>/mgmt/tm/sys/management-ip  | jq -M .

    # management route 
    curl -sk -u <username>:<password> -H "Content-Type: application/json" -X GET https://<big-ip ip address>/mgmt/tm/sys/management-route  | jq -M .
    ```

* View the services that are listening 

    `netstat -ltu or ss -ltu `




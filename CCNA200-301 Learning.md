## CCNA Notes- Jeremy IT Lab
#### IPv4 Addressing
* IPv4 address classes 
* finding the 
            - Max number of hosts
            - network address
            - broadcast address
            - first usable address
            - last usable address of a particular network
* configuring IP addresses on Cisco devices 
* Class -A Oxxxxxxx ---> (0-127) => 1-126   ---> Size of network no. bit field ==> 8  rest ==> 24 (host portion)
* Class -B 10xxxxxx --->128 - 191   ---> Size of network no. bit field => 16  rest==> 16 (host portion)
* Class -C 110xxxxx ---> 192 - 223  ---> Size of network no. bit field ==> 24  == 8 (host portion)
* class -D 1100xxxx ---> 224 - 239  
* class -E 1111xxxx ---> 240 - 255

##### Maximum Hosts per Network 
192.168.1.0/24 -----> 192.168.1.255/24

host portion = 8 bits = 2^8 = 256

host portion of all 0's = network address (ID)
host portion of all 1's = broadcast address

Max hosts per network = 2^8 -2 = 254

Layer 1 Status => status 
Layer 2 Status => protocol column 

```
R1(config)#do sh ip int br 
R1# sh interfaces description
R1# int g0/0
R1#(config-if)# description (our note) to SW!
```
#### Switch Interfaces 
* interfaces speed & duplx
* speed and duplex autonegotiation
* interface status 
* interface counters & errors 

*ASR 10000-X Router*
*Catalyst 9200 switch*
<span style="color:red"> *router interfaces are in administratively disable by default--> shutdown command enables whereas in switch interfaces they don't have shutdown command applied Status->up* </span> 

```
SW1(config)#do sh interfaces status
Port Name  Status vlan Duplex Speed Type(SFP or RJ45)`
SW1(config)# int f0/1
SW1(config-if)#speed ?
SW1(config-if)#speed 100
SW1(config-if)#duplex full
*multiple interfaces at same time*
SW1(config)#int range f0/5 - 6, f0/9 - 12
```

#### IPv4 Headers 
**Version => length 4 bits**
* IPv4 ->4 -> 0100
* IPv6 = 6  (0110)

**Internet Header Length (IHL) => 4 bits in length**
* Min value is 5 (20 bytes)
* Maximum value is 15 (15 * 4 bytes = 60 bytes)
* Min IPv4 Header length = 20 bytes 
* Max IPv4 Header length = 60 bytes 

**DSCP Differentiated Services Code Point => 6 bits in Length**
* Used for QoS(Quality of Service)
* Used to prioritize delay-sensitive data(streaming voice, video etc)

**ECN => 2bits in length**
* Explicit Congestion Notification
* Provides end-to-end(betwn two endpoints) notification of network congestion without dropping packets 
* Optional feature that requires both endpoints, as well as the underlying network infrastructure, to support it
    
**Total length=> 16bits**
* Indicates the total length of the packet(L3 header + L4 segment)
* Measured in bytes (not 4 byte increments like IHL)
* Minimum value of 20 (=IPv4 header with no encapsulated data)
* Maximum value of 65,535 (maximum 16-bit value)
    <span style="color:red"> Maximum value of IPv4 packet 65,535</span> 

**Identification 16 bits in length**
* If a packet is fragmented due to being too large, this field is used to identify which packet the fragment belongs to.
* All fragments of the same packet will have their own IPv4 header with the same value in this field 
* Packets are fragmented if larger than the MTU (Maximum Transmission Unit)
* **MTU is 1500 bytes**
* Fragments are reassembled by the receiving host 

**Flags 3 bits**
* Used to control/identify fragments
* Bit 0: Reserved, always set to 0
* Bit 1: Don't Fragment(DF bit), used to indicate a packet that should not be fragmented 
* Bit 2: More Fragments(MF bit), set to 1 if there are more fragments in the packet, set to 0 for the last fragment 
* <span style="color:red"> Unfragmented packets will always have their MF bit set to 0 </span> 

**Fragment Offset 13 bits in length**
 * Used to indicate the position of the fragment within the original, unfragmented IP packet 
* Allows fragmented packets to be reassembled even if the fragments arrive out of order
    
**Time to Live = 8 bits**
* A router will drop a packet with a TTL of 0
* Used to prevent infinite loops 
* Originally designed to indicate the packet's maximum lifetime in seconds 
* In practice, indicates a "hop count" : each time the packet arrives at a router, the router decreases the TTL by 1
* <span style="color:red"> The current recommended default TTL is 64 </span> 

**Protocol 8 bits in length**
* indicated the protocol of the encapsulated L4PDU
* value of 6 :TCP
* value of 17: UDP
* value of 1: ICMP
* value of 89: OSPF (dynamic routing Protocol)
   
**Header checksum => 16 bits**
* A calculated checksum used to check for errors in the IPv4 header 
* When a router receives a packet, it calculates the checksum of the header and compares it to the one in this field of the header 
* if they don't match the router drops the packet 
* Used to check for errors only in the IPv4 Header 
* IP relies on the encapsulated protocol to detect errors in the encapsulated data
* Both TCP and UDP have their own checksum fields to detect errors in the encapsulated data 

**Source IP address**
**Destination IP address**
* 32 bits each
* IPv4 of the sender and receiver 

**Options(if IHL > 5)**
* length 0 - 320 bits 
* rarely used 
* if the IHL field is greater than 5, it means that options are present

`R1# ping 192.168.1.2 size 10000`

`R1# ping 192.168.1.2 df-bit`
**df - don't fragment bit**

#### Static Routing 
**Things covered**
* IP routing process
* The routing table on  a Cisco router 
* Configuring Static routes 

`R1# show ip route `
* connected route = the network the interface is connected to
* Local route = the actual IP address on the interface (with a /32 mask)

**Configuring a Default Route**
* To configure the gateway of the last resort on a Cisco router, you must configure a default route
* A default route is a route that matches all possible destinations 
* It is used only if a more specific route match isn't found in the routing table
* The default route is the least specific route possible:
    *  IP address: 0.0.0.0
    *  Mask : 0.0.0.0
        * <span style="color:red"> To set the default route/gateway of last resort, configure a route to 0.0.0.0/0</span> 
        * 0.0.0.0/0 range includes 0.0.0.0 ~ 255.255.255.255 ( all possible addresses)
        
` ip route destination-address mask next-hop`

**Switches flood frames with unknown destinations(destinations not in the MAC table)**
**Routers drop packets with unknown destinations IP address**

`ip route destination-address mask exit-interface`

`R1(config)# ip route 192.168.4.0 255.255.255.0 g0/0`

* **one way reachability**
* When a router looks up a destination address in it's routing table it looks for the most specific matching route 
* Most specific = longest prefix length (/32 > /24 > /16 > /8 > /0)


#### Life of a Packet 
* Entire process of sending a packet to a remote destination
* Including ARP, encapsulation, de-encapulation etc.

#### Subnetting 
* CIDR (Classless Inter-Domain Routing)
* The process of Subnetting 
* Class A - /8
* Class B - /16
* Class C - /24
* IANA - Internet Assigned Number Authority 
* 203.0.113.0 /25 =>2n -2 =>2^7 - 2 = 126 usable addresses
* 203.0.113.0 /26 =>2n -2 =>2^6 - 2 = 62 usable addresses
* 203.0.113.0 /27 =>2n -2 =>2^5 - 2 = 30 usable addresses
* 203.0.113.0 /28 =>2n -2 =>2^4 - 2 = 14 usable addresses
* 203.0.113.0 /29 =>2n -2 =>2^3 - 2 = 6 usable addresses
* 203.0.113.0 /30 =>2n -2 =>2^2 - 2 = 2 usable addresses
* 203.0.113.0 /31 =>2n -2 =>2^1 - 2 = 0 usable addresses
    * for point to point network we don't need network and broadcast address
    * cisco routers will give error making sure the two devices are p2p

* 203.0.113.0 /32 =>2n -2 =>2^0 - 2 = -1 usable addresses
    * There are some uses like we want to create a static route to one specific host

#### VLSM (Variable Length Subnet Mask )
* Until now we used FLSM (Fixed Length Subnet Masks)
* VLSM is the process of creating subnets of different sizes to make your use of network addresses more efficient

**VLSM steps**
1. Assign the largest subnet at the start of the address space 
2. Assign the second largest subnet after it
3. Repeat the process until all subnets have been assigned 


#### VLAN 
* Virtual Local Area Networks 
* What is a LAN ? 
    * Group of devices in a single location 
    * A LAN is a single broadcast domain including all devices in that broadcast domain 

* Broadcast domains 
    * The group of devices which will receive a broadcast frame (destination MAC FFFF.FFFF.FFFF) sent by any one of the members.

* What is a VLAN ?
    * Performance: :Lots of unneccessary broadcast traffic can reduce network performance 
    * Even within the same office you meant to limit who has access to what 
    * You can apply security policies on a router/firewall
    * Because this is one LAN, PCs can reach each other directly, without traffic  passing through the router 
    * So, even if you configure security policies they won't have any effect 

    * VLANs are configured on switches on a per-interface basis
    * logically separate end hosts at Layer 2.
    * Switches do not forward traffic directly between different hosts in different VLANs


* Purpose of VLANs
    * Even if we use the separate the three departments into three subnets (Layer 3), they are still in the same broadcast domain (Layer 2)

    * By creating a separate VLAN a switch will not forward traffic between VLANs, including broadcast/unknown unicast traffic 

    * The switch does not perform inter- VLAN routing. It must send the traffic through the router 
    
* How to configure VLANs on Cisco Switches 
    ### `SW1# show vlan brief `
    * VLANs 1, 1002, 1003, 1004 and 1005 exist by default and cannot be deleted 
    * `sw1(config)# interface range g1/0 - 3 `
     
     connected to end hosts will be access mode by default 
     * `sw1(config)# switchport mode access`
     
    assigns the vlan to the ports
    * `sw1(config)# switchport access vlan 10`
    * An access port is a switchport which belongs to a single VLAN and usually connects to end hosts like PCs.

    * Switchports which carry multiple VLANs are called 'trunk ports'.

    * change the default names of the VLAN 
    * ` sw1(config)# vlan 10`
      
      ` sw1(config)# name Engineering`
    
#### VLAN part -2
* What is a trunk port ?
    * In a small network with few VLANs, it is possible to use a separate interface for each VLAN when connecting switches to switches, and switches to routers. 


* What is the purpose of trunk ports ?
    * However when the number of VLANs increases, this is not viable. It will result in wasted interfaces, adn often routers won't have enough interfaces for each VLAN
    
    * You can use trunk ports to carry traffic from multiple VLANs over a single interface 

    * switches will "Tag all frames that they send over a trunk link, this allows the receiving switch to know which VLAN the frame belongs to 
        * Trunk ports = 'tagged' ports 
        * Access ports = 'untagged ports'


* 802.1Q Encapsulation
    * There are two main trunking protocols: 
        * ISL (Inter -Switch Link)
        * IEEE 802.1Q
    
    * ISL is an  old Cisco proprietary protocol created before the industry standard IEEE 802.1Q

    * IEEE 802.1Q is an industry standard protocol created by the IEEE( Institute of Electrical and Electronics Engineers)

    * We will probably NEVER use ISL in the real world. Even modern cisco equipment doesn't support it. (CCNA 802.1Q)

    * Preamble  || SFD   | Destination | Src | 802.1Q | Type 
    * dot1Q is inserted between the source and destination Type/Length fields of the ehternet frame 
    * The tag is 4 bytes (32 bits) in length

    * The tag consists of two main fields :
        * Tag Protocol Identifier (TPID)
        * Tag Control Information (TCI)

    * The TCI consists of three sub-fields

    * 801.Q format 
        * TPID( 16 bits )   || TCI 16 bits 
        *                  PCP (3 bits) | DEI (1 bit) | VID (12 bits)
        
        * #### TPID (Tag Protocol Identifier)
            * 16 bits (2 bytes) in length
            * Always set to a value of 0x8100. This indicates that the frame is 802.1Q tagged 
        * PCP (Priority Code Point) 
            * 3 bits in length
            * Used for class of Service(CoS), which prioritizes important traffic in congested networks 

        * DEI ( Drop Eligible Indicator)
            * 1 bit in length
            * Used to indicate frames that can be dropped if the network is congested 
        
        * VID (VLAN ID)
            * 12 bits in length
            * identifies the VLAN the frame belongs to 
            * 12 bits in length = 4096 total VLANs(2^12), range of 0-4095
            * VLANs 0 and 4095 are reserved and can't be used
            * Therefore, the actual range of VLANs is 1-4094
            * Cisco proprietary ISL also has a VLAN range of 1-4094
        
        * #####  VLAN ranges 
        * The range of VLANs (1-4094) is divided into two sections:
            * Normal VLANs: 1 - 1005
            * Extended VLANs: 1006 - 4094
        * Some older devices cannot use the extended VLAN range, however it's safe to expect that modern switches will support the extended VLAN range

        * #### Native VLAN : feature of dot1Q
        * Native VLAN  is VLAN 1 by default on all trunk ports, however this can be manually configured on each trunk port.
        * The switch does not add an 802.1Q tag to frames in the native VLAN 
        * When a switch recevies and untagged frame on a trunk port, it assumes the frame belongs to the native VLAN.
        (**It is very important that the native VLAN Matches**)

#### Day 18
* VLAN - Part 3
* `encapsulation dot1Q vlan-id native` on subinterface router 
    * int g0/0.10
    * encapsulation dot1q 10 native 

* configure the IP address for the native VLAN on the router's physical interface (the encapsulation dot1q vlan-id command is not necessary)
    * `no interface g0/0.10`
    
    * `interface g0/0`

    * `ip add 192.168.1.62 255.255.255.192`

#### Layer 3 switch (Multilayer switch)
* capable of both switching and routing 
* it's layer 3 aware 
* Can assign IP addresses to interfaces
* Can create virtual interface for each VLAN and assign IP addresses
* SVI (switch Virtual Interfaces)

* `default interface g0/0 `--->reset default setting 

**Enabling layer 3 routing on the swtich**
* `#ip routing `
* `#int g0/1`
* `#no switchport` --->this configures the interface as a routed port( Layer 3 port, not a Layer 2/switchport)

**#show interfaces status**

#### condition for SVI to be UP UP
* VLAN must exist on the switch 
* must have at least one access port in VLAN in an up/up state , AND/OR one trunk port that allows the VLANs that is in an up/up state 
* VLAN must not be shutdown (shutdown command)
* SVI must not be shutdown 

#### Quiz 
1. b and c 
2. a and d (no interfaces in VLAN 225 are up/up)
3. a(no switchport)

### Lab 
* in a router removing ROAS 
* `no interface g0/0/0.10`

* `no interface g0/0/0.20`

* `no interface g0/0/0.30`


### DTP/VTP 
* DTP - Dynamic Trunking Protocol 
    * Cisco proprietary protocol that allows CISCO switched to dynamically determine their interface status(access/trunk) w/o manual configuration
    * DTP is enabled by default on all Cisco switch interfaces 
    * For security purposes manual configuration is recommended. DTP should be disabled on all switchports
    * A switchport in dynamic desirable mode will actively try to form a trunk with other cisco switches.
    
* VTP - VLAN trunking protocol 
* `#vtp domain cisco`

* `#show vtp status`

* `#vtp mode client`

* `#vtp mode transparent`

* `#show vtp status`

* `#vtp domain cisco`

* `#vtp version 2` #changing the version of the VTP 

#### Quiz 
1. (b) Interfaces on old switches default to switchport mode dynamic desirable 

2. `#vtp mode transparent` forward VLAN database information but won't sync its VLAN database 

3. (a)change the VTP domain to an unused domain name , (c) change the switch to VTP transparent mode 


### Spanning Tree Protocol (STP)
* Redundancy in Networks 
* STP 
    * Layer 2 Protocol 
    * Broadcast Storms 
        * The ethernet header doesn't have a TTL field. These broadcast frames will loop around the network indefinitely. If enough of these looped broadcasts accumulate in the network, the network will be too congested for legitimate traffic to use the network. This is called a broadcast storm.

    * MAC Address Flapping 
        * Each time frame arrives on a switchport, Switch uses the source MAC address field to 'learn' the MAC address and update its MAC address table. when frames with the same source MAC address repeatedly arrive on different interfaces, the switch is continuously updation the interface in its MAC address table. This is known as MAC address Flapping 

* IEEE 802.1D 
* prevents Layer 2 loops --> blocking state 
* BPDU (STP messages) --> Bridge Protocol Data Units 
* forwarding and blocking state 
* Hello BPDU message out of all interfaces, default timer is 2 seconds 

* STP BPDU, the Bridge ID field is used to elect root bridge for the network 
* The switch with lowest Bridge ID becomes the root bridge
* ports of ROOT bridge are in a forwarding state and other must have path to reach the root bridge

*               Bridge ID 
* Bridge Priority(16 bits) ||       MAC address (48 bits)

* **Default Bridge Priority is 32,768 on all switches**
* Bridge Priority extended to two parts 
*           Bridge Priority (4bits) || Extended System Id (VLAN ID = 12bits )

* Extended System ID was included because cisco Switches use a version of STP called PVST (per-VLAN spanning Tree) runs separate instance in each VLAN 

* Each port will select it's one port as root port, root ports are in forwarding state 

*           Speed                   |       STP cost
            10 Mbps                            100
            100 Mbps                |           19
            1 Gbps                  |           4
            10 Gbps                 |           2

* STP port ID = port priority (default 128) + port number 

* **Spanning Tree Selection Process**
1. One switch will eleect the root bridge , all ports are designated ports (forwarding state ). Root Bridge --> Lowest Bridge ID ( Priority --> MAC address )

2. Each remaining switch selects one int to be its root 
Root Port Selection:
    1. Lowest root Cost 
    2. Lowest Neighbor Bridge ID 
    3. Lowest Neighbor port ID 

* Every collision domain has a single STP designated port 
    * Designated Port Selection :
    1. Interface on switch with lowest root cost 
    2. Interface on switch with lowest bridge ID 

#### CDP show commands summary 
* shows basic information about CDP (timers, version)

`R1# show cdp `

* Displays how many CDP messages have been sent and received 

`R1# show cdp traffic`

* Displays which interfaces CDP is enabled on 

`R1# show cdp interface`

* lists CDP neighbors and some basic information about each neighbor 

`R1# show cdp neighbors`

* lists each CDP neighbor with more detailed information

`R1# show cdp neighbors detail`

* Displays the same info as above, but for the specified neighbor only 

`R1# show cdp entry name (example R2)`

#### CDP configuration commands 
* CDP is globally enabled by default 

```
R1(config) # no cdp run --> cdp enable/disable globally

R1(config) #[no] cdp enable --> enable\ disable CDP on specific interfaces 

R1(config) #cdp timer seconds --> configure the CDP timer 

R1(config) #cdp holdtime seconds ---> configure the CDP holdtime

R1(config) #[no] cdp advertise-v2 ---> enable\disable CDPv2
```

#### LLLDP configuration commands 
* LLDP is globally disabled by default 

```
R1(config) # [no] lldp run 
! lldp enable/disable globally

R1(config) # lldp transmit
! enable lldp on specific interfaces (tx)

R1(config) # lldp receive
! enable lldp on specific interfaces (rx)

R1(config) #lldp timer seconds
! configure the LLDDP timer 

R1(config) #lldp holdtime seconds
! configure the lldp holdtime

R1(config) #lldp reinit seconds
! configure the lldp reinit timer
```
#### Link Layer Discovery Protocol 
* displays how many lldp messages have been sent and received 

`R1# show lldp traffic `

* Displays which interfaces LLDP tx/rx is enabled on 

`R1# show lldp interface`

* Shows basic information about lldp (timers, version)

`R1# show lldp `

* lists LLDP neighbors and some basic information about each neighbor 

`R1# show lldp neighbors`

* lists each LLDP neighbor with more detailed information

`R1# show lldp neighbors detail`

* displays the same info as above, but for the specified neighbor only

`R1# show lldp entry SW1`



### NTP Network Time Protocol 
* clock -- software 
* calendar -- hardware time 

* clock time is correct 
```
R1# show clock 
R1# show calendar 
R1# clock update-calendar 
R1# show clock
R1# show calendar 
```
* Calendar time is correct ==>
```
R1# show clock 
R1# show calendar 
R1# clock read-calendar 
R1# show clock
R1# show calendar 
```

* Configure Time Zone 

```
R1(config)# do show clock 
R1(config)# clock timezone ?
R1(config)# clock timezone JST ?
R1(config)# clock timezone JST 9 ?
R1(config)# clock timezone JST 9
R1(config)# do show clock
R1(config)# do clock set 15:15:00 Dec 27 2020
R1(config)# do show clock
```
* Daylight saving time 

```
R1(config)# clock summer-time ?
R1(config)# clock summer-time EDT ?
R1(config)# clock summer-time EDT recurring ?
R1(config)# clock summer-time EDT recurring 2 ?
R1(config)# clock summer-time EDT recurring 2 sunday ?
R1(config)# clock summer-time EDT recurring 2 Sunday March ?
R1(config)# clock summer-time EDT recurring 2 Sunday March 02:00(start)  1 Sunday November 02:00 (end)
```

#### Network time Protocol 
* Manual config on devices is not scalable 
* manual configured clocks will drift, resulting in inaccurate time 
* allows automatic synch of time over a network 
* ntp clients request the time from NTP servers 
* a device can be a NTP server and an NTP client at the same time 
* accuracy of ~1 milisecond in same LAN , o r ~50 miliseconds if connecting to NTM server over WAN/the internet 
* The distance of an NTP server from the original referecnce clock is called stratum 
* NTP uses UDP port 123 to communicate 

#### Reference Clock 
* very accurate time device like an atomic clock or a GPS clock
* reference clocks are stratum 0 within the NTP hierarchy 
* NTP servers directly connected to reference clocks are stratum 1
* Devices can also 'peer' with devices at the same stratum to provide more accurate time 
    * This is called symmetric active mode, cisco devices operate in three NTP modes :
        1. Server mode 
        2. Client mode 
        3. Symmetric active mode 
    
* An NTP client can sync to multiple servers 
* Get their time directly from reference clocks ==> primary servers 
* get their time from other NTP servers are called secondary servers (server/client at same time)

#### NTP configuration 

```
R1(config)# ntp server 216.239.35.0 prefer ! preferred NTP server 

R1(config)# ntp server 216.239.35.4

R1(config)# ntp server 216.239.35.8

R1(config)# ntp server 216.239.35.12

R1# show ntp associations 
address  ||  ref clock  |  st     |  when   |  poll | reach

* sys.peer, # selected, + candidate , -outlyer, x falseticker, ~configured 
```
* show ntp status command 
    * synchronized 
    * stratum 2
    * address of reference clock

* do show clock detail 
    * NTP uses only UTC time zone 

* configures the router to update the hardware clock (calendar) with the time learned via NTP 

`R1(config)# ntp update-calendar`

* **The hardware clock tracks the date and time on the device even if it restarts, power is lost etc. When the system is restarted, the hardware clock is used to initialize the software clock**

```
R1(config)# interface loopback0

R1(config-if)# ip address 10.1.1.1 255.255.255.255

R1(config)#  exit

R1(config)# ntp source loopback0
```
* ntm servers with lower stratum levels are preferred 

#### configuring NTP server mode 

```
R1(config) #ntp master ?

R1(config)# ntp master 

R1(config)# do show ntp associations 
```

* The default stratum of the ntp master command is 8

#### Configuring NTP symmetric active mode 

*  ntp peer 10.0.23.0(peer's address )

#### NTP authentication 

* allows ntp clients to ensure they only sync to the intended servers 

* To configure NTP authentication:
    * **ntp authenticate --> enable NTP authentication**
    * **ntp authenticate-key key-number  md5 key ---> create the NTP authentication key(s)**
    * **ntp trusted-key key-number**
    * **ntp server ip-address key key-number ---> specify which key to use for the server**
        * Not needed on the server 

* Sample example:

```
R1(config)# ntp authenticate

R1(config)# ntp authentication-key 1 md5 jeremysitlab

R1(config)# ntp trusted-key 1
```
* NTP configs 

![NTP](images/NTPconfigs.png)



#### Quiz 
1. command to match software clock to hardware clock 
* Ans=> (c) clock read-calendar , clock update-calendar (adjust hardware clock to software)

2. command to configure timezone 
* Ans => (d) R1(config)#clock timezone name offset 

3. show ntp associations 
* Ans => R1(config)# ntp master 9 ===> 127.127.1.1 --> loopback that's why 

4. command to operate router in NTP client mode 
* Ans ==> (c) (config)# ntp server 216.239.35.0

5. must be enabled on NTP client to enable NTP authentication 
* Ans => (c) ntp authenticate, (d) ntp auth-key key-num md5 key, (f) ntp trusted-key key-number
(g) ntp server ip-ad key key-numb

### DNS- Domain Name System 
* The purpose of DNS / Basic functions of DNS 
    * resolve hum-readable names to IP 
    * The DNS server(s) our device uses can be manually configured or learned via DHCP 
    * `> ipconfig /all`
    * cisco Router can act as DNS client and server
    * DNS 'A' record = Used to map names to IPv4 addresses 
    * DNS 'AAAA' record = Used to map names to IPv6 addresses 
    * Standard DNS queries/responses typically use UDP .
    * TCP is used for DNS messages greater than 512 bytes in either case, port 53 is used 
    * DNS cache --> devices will save the DNS server's responses to a local DNS cache.
    * windows dns cache `user> ipconfig /displaydns`
    * windows clear dns cache `user> ipconfig /flushdns`
    * host file `Windows > System32 > drivers > etc `

**configure DNS in cisco IOS** 
* ```
    R1(config)# ip dns server ----> configure R1 to act as a DNS server 
    R1(config)# ip host R1 192.168.0.1 ------> configure a list of hostname/IP address mappings
    R1(config)# ip host PC1 192.168.0.101
    R1(config)# ip host PC2 192.168.0.102
    R1(config)# ip host PC3  192.168.0.103

    R1(config)# ip name-server 8.8.8.8
    ! configure a DNS server that R1 will query if the requested record isn't in its host table 

    R1(config)# ip domain lookup
    !enable R1 to perform DNS queries( enabled by default old version command is ip domain-lookup)
    ```
* `R1# show hosts` --> view configured hosts and host learned via cached DNS 
* Configure cisco router as DNS client 
```
R1(config)# do ping youtube.com
--> unable to resolve
R1(config)# ip name-server 8.8.8.8
R1(config)# ip domain lookup
R1(config)# do ping youtube.com
--> shows result, working
R1(config)# ip domain name jeremysitlab.com ---> optional command 
```
* Command Review 
![dns](images/dns.png)

**Quiz**
1. windows command that displays PC's dns server 
    * ipconfig /all
    * nslookup

2. statement about DNS 
    * 'A' records map hostnames to IPv4 addresses 
    * A cisco router can be configured as a DNS server and DNS client at the same time 

3. No DNS configuration are needed on Router when it uses external server , it will simply forward the packet as normal

4. command to show cached name/IP address mappings
    * R1# show hosts 

5. Protocol that hosts can use to automatically learn the address of their DNS server ==> **DHCP**


### DHCP - Dynamic Host Configuration Protocol 

* The purpose of DHCP 
    * allows host to automatically/dynamically learn various aspects of their network configuration such as IP address, subnet mask, default gateway, DNS server, etc without manual/static configuration 
    * It is essenttial part of modern networks 
        * for example when we connect a phone/laptop to WiFi, we don't ask the network admin which IP address, subnet mask, default gateway, etc the phone/laptop should use ? 
    * Typically used for 'client devices' such as workstations(PCS), phones etc 
    * Devices such as routers, servers etc are usually manually configured 
    * In small home networks, the router typically acts as the DHCP server for hosts in the LAN 
    * In larger networks,the DHCP server is usually a Windows/Linux server 

* Basic Functions of DHCP
    * DHCP server 'lease' IP address to clients 
    * `user> ipconfig /release ` --> clear the learned addresses  DHCP release 
    * DHCP server --> UDP 67, DHCP client UDP 68 
    * `user> ipconfig /renew `
    * **Four messages**

        1. DHCP Discover 
            * Broadcast message from client, asking if there are any DHCP servers in the network 

        2. DHCP Offer: Sent from DHCP server to the client, offering an address for client to  use,  as well as other information like default gateway, DNS server etc 
            * Unicast frame , unicast at layer 3
            * src : UDP 67 Dst: UDP 68 ---> reversed from DHCP Discover 
            * DHCP offer message can be either broadcast or unicast 

        3. DHCP Request: DHCP client to server , telling the server that it wants to use the IP address it was offered 
            * broadcast message 
            * src : UDP 68, dst: UDP 67

        4. DHCP ACK: From server to client, 
            * unicast messages 
            * Bootp flag unicast, DHCP ack message can be broadcast or unicast 
![DHCP](images/dhcp.png)
* Configuring DHCP in Cisco IOS - router can be DHCP client/server/DHCP relay agent

**DHCP relay**
* large enterprises often choose to use a centralized DHCP server 
* If the server is centralized, it won't receive the DHCP clients broadcast DHCP messages (broadcast messages don't leave the local subnet)
* To fix this, we can configure a router to act as a DHCP relay agent 
* The router will forward the clients broadcast DHCP messages to the remote DHCP server as unicast messages 

**DHCP server config in IOS**
```
R1(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
! specify a range of addresses that won't be given to DHCP clients 

R1(config)#  ip dhcp pool LAB_POOL 
! create a DHHCP pool --> subnet of addresses that can be assigned 
! to DHCP clients as well as other info such as DFG and DNS server 

R1(dhcp-config)# network 192.168.1.0 /24
! specify the subnet of addresses to be assigned to clients (except the excluded addresses)

R1(dhcp-config)# dns-server 8.8.8.8
! specify the DNs server that DHCP clients should use 

R1(dhcp-config)# domain-name google.com
! specift the domain name of the network 
! PC1 = pc1.google.com

R1(dhcp-config)# default-router 192.168.1.1
! specify the default gateway 

R1(dhcp-config)# lease 0 5 30 
! specify the lease time 
! lease days hours minutes OR lease infinite (not recommended)
```

### `R1# show ip dhcp binding`


**DHCP Relay Agent configuration in IOS**
```
R1(config)# interface g0/1
! configure the interface connected to the subnet of the client devices 

R1(config)# ip helper-address 192.168.10.10
! configure the IP address of the DHCP server as the 'helper' address 
! (make sure the address is configured OSPF or static )

R1(config-if)# do show ip interface g0/1
``` 

**DHCP client config in IOS** 
```
R1(config)# interface g0/1
R1(config-if)# ip address dhcp 
! use this command mode to tell the router to use DHCP to learn its IP address 
```

**command Summary**
![DHCP commands](images/dhcp1.png)

**Quiz**
1. correct order of messages when a DHCP client gets an IP address from a server 
    * (b) DORA:- Discover, Offer, Request, ACK

2. windows command which cause a PC to discover a DHCP Discover message 
    * (d) > ipconfig /renew

3. wireshark messages 
    * (d) 255.255.255.255 
    * Bootp flags: 0x8000 , Broadcast flag (Broadcast)

4. which of the following DHCP messages can be sent using unicast ?
    * DHCP Offer, DHCP Release(unicast) & DHCP Ack

5. situation when we would configure a router as a DHCP relay agent 
    * (a) when the router is not a DHCP server, there are DHCP clients in the router's connected LAN, and there is no other DHCP server in the connected LAN 

### SNMP (Simple Network Management Protocol)
* Overview
    * industry standard framework  (1988)
* **SNMPv1** 
    * RFC 1065 - Structure and identification of management information for TCP/IP based internets 
    * RFC 1006 - Management information base for network management of TCP/IP based internets 
    * RFC 1067 - A simple network management protocol 
    <br>
    * Used to monitor the status of devices, make configuration changes, etc 
    
    * There are two main types of devices in SNMP 
        1. **Managed Devices**
        - Devices being managed using SNMP, like routers and switches 

        2. **Network Management Station (NMS)**
        - The device/devices managing the managed devices above 
        - This is the SNMP server 

**SNMP operations**
* There are three main operations used in SNMP
   1. Managed devices can notify the NMS of events 
   2. NMS can ask the managed devices for information about their current status
   3. NMS can tell the managed devices to change aspects of thier configuration

**SNMP Components**
![SNMP comp](images/snmp_components.png)

* Green section SNMP software on NMS(PC or server)
    * SNMP Application
        - provides an interface for the network admin to interact with 
            * Displays alerts, statistics, charts  etc (eg. Solarwinds)
            <br>
    * SNMP Manager 
        - The software on the NMS that interacts with the managed devices 
            * It receives notifications, send requests for information, sends configuration changes etc
            <br>

* Blue area:
    * SNMP Agent
        - The SNMP software running on the managed devices that interacts with the SNMP Manager on the NMS 
            * It sends notification to/reveices messages from the NMS 
            <br>
    * Management Information Base (MIB)
        - The structure that contains the variables that are managed by SNMP 
            * Each variable is identified with an **<i>Object ID(OID)</i>**
            * Example variables: Interface status, traffic throughput, CPU usage, temperature, etc 
            <br>
            * SNMP OIDs are organized in a hierarchical structure 

            .1 .  3  . 6  . 1  . 2  . 1  . 1  . 5

            1 -> iso

            3 ->identified organiation

            6 --> dod   

            1 --> internet

            2 --> mgmt

            1 --> mib2

            1 --> system

            5 --> sysName  

* SNMP versions
    * Three main versions:
        1. SNMPv1
            * The original version of SNMP
            <br>

        2. SNMPv2c
            * Allows the NMS to retrieve large amounts of information in a single request, so it is more efficient  
            * c refers to the community strings used as passwords in SNMPv1, removed from SNMPv2 and then added back for SNMPv2c 
            <br>

        3. SNMPv3 
            * A much more secure version of SNMP that supports strong encryption and authentication whenever possible, this version should be used 
            <br>

* SNMP messages 
![SNMP message](images/snmp_message.png)

    * Get 
        * A request sent from the manager to the agent to retrieve the value of a variable (OID) or multiple variables. The agent will send a Response message with the current value of each variable
    * GetNext 
        * A request sent from the manager to the agent to discover the available variable in the MIB
    * GetBulk
        * A more efficient version of the GetNext message( introduced in SNMPv2)
    <br>

    * set
        * A request from the manger to the agent to change the value of one or more variables,
        * The agent will send a response message with the new values 
    <br>

    * Trap 
        * Notification from the agent to the manager, the manager doesnot send a response message to acknowledge that it received the Trap, so these messages are 'unreliable'

    * Inform
        * A notification message that is acknowledged with  Response message
        * Originally used for communications between managers, but later updates allow agents to send inform messages to managers, too.
    <br>

<b>
    ```
    SNMP Agent = UDP 161

    SNMP Manager = UDP 162
    ```
</b>

**SNMPv2c configuration (basic)**

```
R1(config)# snmo-server contact sample@gmail.com
R1(config)# snmp-server location Sample House 
! optional information

R1(config)# snmp-server community username ro 
! configure the SNMP community strings(password)
! ro ---> read only -->  no Set message 

Default  ro= public rw=private 

R1(config)# snmp-server community username2 rw
! rw ----> read/write = can use Set message 

R1(config)# snmp-server host 192.168.1.1 version 2c username
! Specify the NMS, version and community 

R1(config)# snmp-server enable traps snmp linkdown linkup
R1(config)# snmp-server enable traps config
! configure the Trap types to send to the NMS 

```
* SNMPv1 & SNMPv2: there is no encryption
    * The community and message contents are sent in plain-text. 
    * This is not secure as the packets can easily be captured and read 

**Quiz**

1. used by NMS to read the information from the managed devices 

Ans => Get, GetNext, GetBulk
<br>

2. SNMP message sent to UDP port 162

Ans=> Inform , trap (Managed devices )
      Set, Get (UDP port 161)
<br>
3. mass retrieval of information, introduces in SNMPv2

Ans=> GetBulk
<br>
4. Software that runs on SNMP NMS 

Ans=> Manager 
<br>
5. SNMP messages sent without expecting a Response 

Ans=> Trap
<br>





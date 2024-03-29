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

`SNMP Agent = UDP 161`

`SNMP Manager = UDP 162`
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
* Ans => Get, GetNext, GetBulk
2. SNMP message sent to UDP port 162
* Ans=> Inform , trap (Managed devices )
      Set, Get (UDP port 161)
3. mass retrieval of information, introduces in SNMPv2
* Ans=> GetBulk
4. Software that runs on SNMP NMS 
* Ans=> Manager 
5. SNMP messages sent without expecting a Response
* Ans=> Trap


### The Syslog Protocol -RFC 5424 
**Syslog overview** 
* Protocol for message logging
* changes and interface status 
* can be displayed in CLI, saved in the RAM or sent to an external Syslog server 
* Syslog and SNMP are both used for monitoring and troubleshooting of devices

**Syslog message format ** 
* seq:time stamp: %facility-severity-MNEMONIC:description
![syslog_message_format](images/syslog-message.png)

**Syslog facilities and severity levels**

![syslog_severity levels](images/syslog-sev.png)

**Syslog logging locations**
* Console line: Syslog message will be displayed in the CLI when connnected to the device via the console port. By default, all messages (level 0 - level 7 are displayed)
<br>
* VTY lines: displayed in CLI when connected to the device via Telnet/SSH, disabled by default 
<br>
* Buffer: messages saved to RAM, all messaged (level 0 - level 7) are displayed
    * `show logging`
<br>
* External server: configure the device to send Syslog message to an external server 
    * syslog servers will listen for messages on UDP port 514.

**Syslog configuration**
![syslog-conf](images/syslog-conf.png)

* Even if logging monitor level is enabled, by default syslog messages will not be displayed when connected via telnet or ssh 

* For the messages to be displayed, you must use the following command:

    `R1# terminal monitor`

* This command must be used every time we connect to the devices via telnet or SSH 

**Service timestamps / service sequenve-numbers**
<b>

```
R1(config)# service timestamps log datatime

R1(config)# service sequence-number 

```
</b>
<br>

* Syslog Command Summary 
![syslog-command](images/syslog-command.png)

**Syslog vs SNMP** 
* syslog:
    * message logging
    * events occur within the system are categorized based on facility/severity and logged
    * used for system mgmt, analysis and troubleshooting
    * messages are sent from the devices to the server
    * the server can't actively pull information from the devices (like SNMP Get) or modify variables (SNMP set)

* SNMP
    * used to retrieve and organize info about the SNMP managed devices
    * Ip add, current int status, temperature, CPU usage etc
    * SNMP servers can use Get to query the clients and Set to modify varibales on the clients

**Quiz**
1. severity level ..%SYS-5-CONFIG_I:..
    * 5 (Notification)

2. severity level ...%LINK-3-UPDOWN:...
    * 3 (Error)

3. Syslog messages sent to by default 
    * console line
    * Buffer 

4. logging buffered 6
    * Severity 0 to 6

5. syslog message field that might not be displayed 
    * seq
    * time stamp


### SSH- Secure Shell 
**ConsolePort Security**
* By default  No password needed 
![console-port](images/console-port.png)

![login-local](images/login-local.png)

**Layer 2 switch management IP**
* Layer 2 switch don't perform packet routing and don't build a routing table, they are not IP routing aware
* But we can assign an IP address to an SVI to allow remote connections to the CLI of the switch(using telnet or ssh)

```
SW1(config)# interface vlan1
! configure the IP address on the SVI in the same way on multilayer switch

SW1(config)# ip address 192.168.1.253 255.255.255.0
SW1(config)# no shut 
! enable the interface if necessary 

SW1(config)# ip default-gateway 192.168.1.254

```

**Telnet**
* less secure (Teletype Network) developed in 1969
* should use SSH instead of Telnet 
* no encryption, can use wireshark to capture the traffic in plain text 

![telnet](images/telnet.png)

**SSH**
* Secure Shell , 1995 
* SSHv2, 2006
* If devices supports both version 1 and version 2 it is said version 1.99
* More secure and provides data encryption and authentication 
* check ssh support (vios_12-ADVENTERPRISEK9-M)
* Cisco supports NPE(No Payload Encryption) IOS images to countries that have restrictions on encryption technologies 

* NPE IOS images doesnot support cryptographic featurs such as SSH 

**`SW1# show version `**

**`SW1# show ip ssh`**

* RSA at least 768 bits 
* must generate RSA public and private key pair 
* Keys are used for data encryption/decryption, authentication etc 

```
SW1(config)# ip domain name domain.com
! The FQDN of the device is used to name the RSA keys
! FQDM = Fully Qualified Domain Name(host name + domain name )

SW1(config)# crypto key generate rsa 
! size :2048 bits 

or 

SW1(config)# crypto key generate rsa modulus length
! length must be 768bits or greater for SSHv2
```
* restrict SSH to version 2 only 
`SW1(config)# ip ssh version 2`

**SSH configuration**
1. Configure hostname 

`Router(config)# hostname R2`

2. Configure DNS domain name 

`R2(config)# ip domain name company.com`

3. Generate RSA key pair

`R2(config)# crypto key generate rsa`

4. Configure enable PW, username/PW

5. Enable SSHv2 only 
6. configure VTY lines 

**`ssh -l username ip-address OR ssh username@ip-address`**

**SSH Command Summary**

![ssh-summary](images/ssh-sum.png)

**Quiz**
1. crypto key generate rsa command rejected 
    * hostname hasn't been configured
    * a DNS domain name hasn't been configured 

2. both telnet and ssh to be used 
    * transport input telnet ssh
    * transport input all 

3. allow only 192.168.1.1 to connect to R1 via SSH 
    * access-list 199 permit tcp host 192.168.1.1 any eq 22 
    * line vty 0 15 
    * access-class 199 in

4. True SSH statements 
    * (F) a key length of at least 768 bits is required for SSHv2
    *  (b) K9 IOS images support SSH

5. A netowrk admin  using PC1 is remotely configuring SW1 by connecting to the CLI of SW1 via SSH. What is the role of SW1 in this situation ?
    * SSH server 

### FTP and TFTP 
**The purpose of FTP/TFTP**
* File Transfer Protocol and Trivial File Transfer Protocol are industry standard protocols
* Uses client-server model 
    * clients can user FTP or TFTP to copy files from a server
    * cliens can user FTP or TFTP to copy file to a server 
* most common use is while upgrading the OS of a network device 
* can use it to download the newer version of IOS from a server and then reboot the device with the new IOS image 


**FTP/TFTP functions and differences**
* TFTP first standardized in 1981
* It is simple and has only the basic features compared to FTP 
    * only allows a client to copy a file to or from a server 
* was released after FTP, but is not a replacement for FTP
* It is another tool to use when lightweight simplicity is more important than functionality 
* No authentication, so servers will respond to all TFTP request 
* No encryption, so all data is sent in plain text 
* Best used in a controlled environment to transfer small files quickly 
* TFTP servers listen on **UDP port 69**
* TFTP has similar built in features within the protocol itself 

**TFTP Reliability**
* send ACK messages 
* timers are used,  so that it can resend data after certain time 
* TFTP uses 'lock-step' communication, the client and server alternately send a message and then wait for a reply.(+retransmissions are sent as needed)

**TFTP connections**
* 3 phases 
    1. connection : 
        * client sends a request to the server and the server responds back, initializing the connection
    2. Data transfer: 
        * the client and server exchange TFTP messages
        * One sends data and the other sends acknowledgement 
    3. Connection termination
        * After the last data message has been sent a final acknowledgement is sent to terminate the connection

**FTP**
* first standardized in 1971
* FTP uses TCP port 20 and 21 
* Usernames and passwords are used for authentication, however no encryption
* for greater security, FTPS (FTP over SSL/TLS) can be used --upgrade to FTP 
* SSH file Transfer protocol (SFTP) can also be used for greater security 
* FTP is more complex the TFTP and allows not only file transfers, but clients can also navigate file directories, add and remove directories, lst files, etc 
* The clients sends FTP commands to the server to perform these functions 

* FTP uses two types of connection that's why it uses two different ports 
    * FTP control TCP 21 -> to send FTP commands 
    * FTP data TCP 20 --> for data or files transfer 
    * the default method of establishing FTP data connection is active mode, in which the server initiates the TCP connection
    * in FTP passive mode the client initiates the data connection, this is often necessary when the client is behind a firewall, which could block the incoming connection from the server 

    * **Firewall usually don't permit outside devices to initiate connection. In this case, FTP passive mode is used and the client(behind the firewall) initiates the TCP connection**

![TFTP FTP Differe](images/FTP-TFTP%20com.png)

**IOS file systems**
* File system is a way of controlling how data is stored and retrieved 
* Command to view file systems of a cisco IOS devices 

    **`Router# show files systems`**
    * disk :- storage devices such as flash
    * opaque :- used for internal functions
    * nvram :- internal NVRAM. The startup-config file is stored here
    * network :- Represents external file systems, for example external FTP/TFTP servers 

**Using FTP/TFTP in IOS**
* command to show the IOS version

    **`Router1# show version `**

* command to show the content of the flash

    **`R1# show flash `**

sample output: c2900-universalk9-mz.SPA.151-4.M4.bin

* Copying files 
**`R1# copy tftp: flash:`**
    * copy src dst 
    * enter the TFTP server IP 
    * enter the filename on the server 
    * enter the name you want to save it as on flash (*hit enter for the default*)

* Upgrading Cisco IOS 

<b>

```
R1(config)# boot system flash:c2900-universalk9-mz.SPA.151-4.M4.bin

R1(config)# exit
R1(config)# write
R1(config)# reload

R1# show version

R1# delete flash:c2900-universalk9-mz.SPA.151-4.M4.bin

R1# show flash
```
</b>

* *boot system filepath* 

**Copying Files(FTP)**
<b>

```
R1(config)# ip ftp username cisco
R1(config)# ip ftp password ccna
R1# copy ftp:flash
```

</b>

* command summary 

![command-summary](images/ftp-summar.png)

**Quiz**
1. True statements about FTP 
    * FTP data - TCP 20
    * FTP control -TCP 21

2. command to transfer file from external TFTP server to local device's flash storage
    * copy tftp: flash:

3. R1 is behind the firewall and wants to connect to an external FTP server 
    * FTP passive mode for the data connection should be used

4. Used to store the startup-config of a device running Cisco IOS 
    * NVRAM

5. Following function not possible when using TFTP 
    * Create a new directory on a server
    * List the contents of a server 


### Network Address Translation (NAT)
**Private IPv4 addresses**
* RFC 1918 
* Three main short term solutions 
    * CIDR 
    * Private IPv4 addresses 
    * NAT 
* RFC 1918 specifies the following IPv4 address ranges as private :

<br>

    * 10.0.0.0/8 (10.0.0.0 to  10.255.255.255) ------> Class A

    * 172.16.0.0/12 (172.16.0.0 to 172.31.255.255)------> Class B

    * 192.168.0.0/16 (192.168.0.0 to 192.168.255.255)--------> Class C 

</br>

* they don't have to be globally unique 

**Intro to NAT**
* used to modify src and/or dst IP addresses of packets 
* the common reason is to allow hosts with private IP addresses to communicate with other hosts over the internet
* **source NAT**

* statically configuring one-to-one mappings of private IP addresses to public IP addresses 
* An inside local IP address is mapped to an inside global IP address
    * inside local : 
        * IP address of the inside host, from the perspective of the local network
        * The ip address actually configured on the inside host, usually a private address 

    <br>

    * inside global
        * The ip address of the inside host, from the perspective of outside hosts
        * the ip address of the inside host after NAT, usually a public address 

    <br>

    * outside local
        * The ip address of the outside host, from the perspective of the local network

    <br>

    * Outside Global
        * The ip address of the outside host, from the perspective of the outside Network

    <br>


* Static NAT allows devices with private IP addresses to communicate over the internet 
* But as it requires a one-to-one IP address mapping, it doesn't help preserve IP addresses 

**Static NAT configuration**

![NAT](images/Nat-1.png)

<b>

```

R1(config)# int g0/1
R1(config-if)# ip nat inside 
! define inside interfaces connected to the internal network

R1(config-if)# int g0/0
R1(config-if)# ip nat outside 
R1(config-if)# exit 
! define the outside interfaces connected to the external network 


R1(config)# ip nat inside source static 192.168.0.167 100.0.0.1
R1(config)# ip nat inside source static 192.168.0.168 100.0.0.2
R1(config)# exit
! configure 1-1 IP address mappings 
! ip nat inside source static inside-local-ip inside-global-ip

R1# show ip nat translations
```
</b>

* Clear ip nat translation

    **`R1# show ip nat translations`**

    **`R1# clear ip nat translation *`**

    **`R1# show ip nat statistics`**

**Quiz**
1. command to static source NAT mapping of 192.168.10.10 to 203.0.113.10
    * ip nat inside source static 192.168.10.10 203.0.113.10

2. ip nat inside source static 10.0.0.1 20.0.0.1 and ip nat inside ...10.0.0.2 20.0.0.1
    * only 10.0.0.1 will be translated to 20.0.0.1

3. R1# show ip nat stat
Result => Total active translations: 7 (3 static, 4 dynamic; 0 extended)
How many active translations will be there if we issue clear ip nat trans*
    * 3

4.  ipv4 private address 
    * 10.254.255.0
    * 172.20.2.3
    * 10.11.12.13

5. Packet flows and IP
    * outside Global :8.8.8.8
    * outside local : 8.8.8.8
    * inside local: 172.20.0.101
    * inside global: 200.0.0.1

### NAT - Dynamic NAT Part 2
**Dynamic NAT**
* Static NAT -- statically configuring one-to-one mapping of private IP addresses to public addresses 

* Dynamic NAT -- the router dynamically maps inside local addresses to inside global addresses as needed 
* ACL is used to identify which traffic should be translated 
    * src ip is permitted, then src ip is translated 
    * if src ip is denies, the src ip is not translated 
* NAT pool is usedto define the available inside global addresses
* Mapping is still one-to-one (one inside local IP per global IP)
* if there are no enough global IP (all are currently being used), it is called NAT pool exhaustion
    * dynamic NAt entries will time out automatically if not used or we can clear them manually 
* Dynamic NAT default timeout is 24hrs 
* Configuration 
![NAT](images/NAT.png)

**Dynamic PAT (Port Address Translation)**
*  aka NAT overload, translates both the IP and the port number( if neccessary)
*  unique port for each communication flow, a single public address can be used by different internatl hosts 
    * port number are 16 bits => 65,000 available port numbers
* The router will keep track of which inside local address is using which inside global address and port 
* As many inside hosts can share a single public IP, PAT is very useful for preserving public IP addresses, and it is used in networks all over the world 

* **PAT is most widely used**

* Configuration 
![NAT](images/PAT.png)

<br>

**Command Summary**

![NAT summary](images/NAT-sum.png)

**Quiz**
1. Best NAT types which help in preserving public IPv4
    * NAT overload

2. Translate inside local addresses from 172.16.1.0/24 to addreses from the subnet 203.0.113.0/25
    ```
    access-list 1 permit 172.16.1.0 0.0.0.255
    ip nat pool POOL1 203.0.113.0 203.0.113.127 netmask 255.255.255.128

    ip nat inside source list 1 pool POOL1

    interface g0/0
    ip nat inside

    interface g0/1
    ip nat outside
    ```
3. what happens if all 10 addresses are being used by inside hosts 
    * It discards the packet 

4. 10.0.1.0/27 to use the IP address of the router's G0/1 interface 
    ```
    access-list 1 permit 10.0.1.0 0.0.0.31
    ip nat inside source list 1 interface gigabitethernet0/1 overload

    interface g0/0
    ip nat inside

    interface g0/1
    ip nat outside 

    ```
5. access-list 1 deny 192.168.1.0 0.0.0.255, what happens to 192.168.1.0 in NAT
    * The packets they send will noe be translated by R1

    
### Quality of Service (QoS)
**IP Phones/Voice VLANs**
* Traditional phone PSTN:- Public Switched Telephone Network 
* Also called POTS (Plain Old Telephone Service)
* IP phone uses VoIP (Voice Over IP) technologies to enable calls over an IP network, such as the Internet 
* IP phone have an internal 3-port switch
    * 1 port is the 'uplink' to external switch
    * 1 port is the 'downlink'  to the PC
    * 1 port connects internally to the phone itself 
*  PC and the IP phone to share a single switch port
* Traffic from the PC passes through the IP phone to the switch 
* Recommended to separate 'voice' traffic (from the IP phone) and data traffic (From the PC ) by placing them in separate VLANs
    * voice VLAN
    * traffic from the PC will be intagged, but traffic from the phone will be tagged with a VLAN ID 

    ```
    SW1(config)# interfave gigabitethernet0/0
    SW1(config)# switchport mode access
    SW1(config)# switchport access vlan 10

    SW1(config)# switchport voice vlan 11
    ! PC1 will send traffic untagged, as normal SW1 will use CDP to tell PH1 to tag
    PH1's traffic in VLAN 11

    SW1# show interfaces g0/0 switchport 
    ! even though interface sends/receives traffic from two VLANs it is 
    not considered a trunk port. It is considered an access port 

    ```

**Power Over Ethernet (PoE)**
* Power Sourcing Equipment (PSE) to provide power to Powered Devices (PD) over an Ethernet
* typically PSE is a switch and the PDs are the IP phones, IP cameras, wireless access points etc 
* The PSE receives AC power from the outlet, converts it to DC power, and supplies that DC power to the PDs
* PoE has a process to determine if a connected device needs power and how much power it needs 
    * when a device is connected to a PoE enabled port the PSE switch sends low power signals, monitors the response, and determines how much power the PD needs 
    * if the device needs power the PSE supplies the power to allow the PD to boot
    * The PSE continues to monitor the PD and supply the required amount of power(but not too much)
* Power Policing can be configures to prevent a PD from taking too much power 
    * `power inline police`  configures power policing with the default settings: disable the port and send a Syslog message if a PD draws too much power
        * equivalent to `power inline police action err-disable` 
        * the interface will be put in an 'error-disabled' state and can be re-enabled with **shutdown** followed by **no shutdown**

    <br>

    * `power inline police action log` does not shut down the interface if the PD draws too much power. IT will restart the interface and send a Syslog message

    **Preventing PD's from drawing too much power**

**Intro to Quality of Service(QoS)**
* Voice traffic and data traffic used to use entirely separate networks 
    * voice traffic used the PSTN 
    * Data traffic used the IP network (enterprise WAN, internet, etc)
* QoS wasn't necessary as the different kinds of traffic didn't complete for bandwidth
* modern network shares the same IP network 
* **QoS is a set of tools used by network devices to apply different treatment to different packets**
*  manage following characteristics of network traffic :
    1. Bandwidth 
        * Kbps, Mbps, Gbps etc 
        * reserve a certain amount of a link's bandwidth for specific kinds of traffic, example 20% voice traffic, 30 % for specific kind of data traffic, leaving 50 % for other traffic
    2. Delay 
        * one-way delay (src- dst)
        * two-way delay (src-dst and return )
    3. Jitter 
        * variation in one-way delay between packets sent by the same applicaiton
        * IP phones have a 'jitter buffer' to provide a fixed delay to audio packets.
    
    4. Loss 
        * the % of packets sent that do not reach their destination
        * faulty cables, queues get full and the device starts discarding packets

* 1-way delay : 150 ms or less
* Jitter: 30 ms or less
* Loss: 1% or less

**QoS Queuing**
* queued messages will be forwarded in FIFO manner 
* Queue full new packets dropped => tail drop 
* TCP global Synchronization:
    * TCP sliding window 
    * increase/decrease the rate at which they send traffic as needed 
    * when a packet is dropped it will be re-transmitted
    * when a drop occurs, the sender will reduce the rate it sends traffic 
    * It will then gradually increase the rate again 

    <br>

    Network Congestion ---> Tail Drop -------> Global TCP window size decrease ----> Network underutilized -------> Global TCP window size ---> Network congestion again

* Random Early Detection (RED) ---> solution to prevent tail drop 
* when the amount of traffic in the queue reached a certain threshold, the device will start randomly dropping packets from select TCP flows 
* improved version , Weighted Random Early Detection (WRED) allows us to control which packetd are dropped depending on the traffic class 

**Quiz**
1. -Voice traffic tagged in VLAN 99, data traffic untagged 
2. The interface will be err-disabled and a Syslog message will be generated --> `power inline police`
3.  Delay 150 ms or less, Jitter 30 ms or less, Loss: 1% or less 
4. TCP global Sync
5. FIFO is the default manner of forwarding queued packets 

### Quality of Service (Part 2)
**Classification/Marking**
* organizes network traffic (packets) into traffic classes (categories)
* we have to identify which types of traffic to give priority to
* Many methods of classifying traffic
    * like ACL 
    * NBAR: Network Based Application Recognition : performs a deep packet inspection, looking beyond Layer 3 and Layer 4 information up to Layer 7 to identify the specific kind of traffic 
    * In Layer 2 and Layer 3 headers there are specific fields used for this purpose 
* PCP (Priority Code Point) field of the 802.1Q tag (in the Ethernet header) can be used to identify high/low priority traffic 
    * Only when there is a dot1q tag 
* DSCP (Differentiated Services Code Point) field of the IP header can also be used to identify high/low priority traffic 
* PCP i also known as CoS (Class Of Service). It use is defined by IEEE 802.1p
* 3 bits = 8 possible values (2^3 = 8)

<b>

```
 PCP Value             ||      Traffic Types     `
    0                           Best effort (default)
    1                           Background
    2                           Excellent effort
    3                           Critical Applications
    4                           Video
    5                           Voice 
```

</b>

* IP phones mark call signaling traffic (used to establish calls) as PCP3
    * They mark the actual voice traffic as PCP5

* As PCP is found in the dot1q header, it can only be used over the following connections:
    1. trunk links
    2. access links with a voice VLAN

* RFC 2474 defines the DSCP field 
* We should be aware of the following standard markings:
    * Default Forwarding (DF) - best effor traffic 
        * It is used for best-effort traffic 
        * The DSCP marking for DF is 0
        * 000000

    * Expedited Forwarding (EF) - low loss/latency/jitter traffic(usually voice)
        * Used for traffic that requires low loss/latency/jitter 
        * The DSCP marking for EF is 46
        * Binary 101110
    
    * Assured Forwarding(AF) - A set of 12 standard values
        * Defines four traffic classes, all packets in a class have the same priority 
        * Within each class, there are three levels of drop precedence 
            * Higher drop precedence = more likely to drop the packet during congestion
            * 0/1 0/1 0/1(class) 0/1 (drop precedence) 0/1 (always 0)  ==> AFXY 
            * X -----> Decimal number of class , Y is decimal number of Drop precedence
            * Binary 001010
            * 32 | 16 | 8 | 4 | 2  | 0--> DSCP (8X + 2Y)
            * 4  | 2  | 1 | 2 | 1  | 0 ---> AF 

    * Class Selector(CS) - A set of 8 standard values, provides backward compatibility with IPP 
        * 3 bits that were added for DSCP values are set to 0, and the original IPP bits are used to make 8 values 
        * 32    |    16    |  8   |   4     |    2
        * 0/1   |   0/1    |  0/1 |   0     |    0    | 0
        * 0     |     1    |   2  |   3     |    4    | 5  |   6   |   7 ---> IPP
        * CS0   |    CS1   |  CS2 |  CS3    |   CS4   | CS5 |  CS6 | CS7 ====> CS
        * 0     |    8     |   16 |  24     |   32    |  40 |  48  |  56 -----> DSCP (decimal)

    * **RFC 4954** was developed with the help of Cisco to bring all of these values together and standardize their use 
    * The RFC offers many specific recommendations, but here are a few key ones :
        1. Voice traffic: EF
        2. Interactive Video: AF4x
        3. Streaming video: AF3x
        4. High Priority data: AF2x
        5. Best effort: DF 
        
    * `R1(config)# class-map TEST`

**Trust Boundaries**
* defines where devices trust/don't trust the QoS markings of received messages 
* If markings are trusted, the device will forward the message without changing the markings
* If markings aren't trusted, the device will change the markings according to the configured policy 
* if an IP phone is connected to the switch port, it is recommended to move the trust boundary to the IP phones
* This is done via configuration on the switch port connected to the IP phone
* If a user marks their PC's traffic with a high priority, the marking will be changed (not trusted)

**Queuing/Congestion Management**
* The device is only able to forward one frame out of an interface at once, so a scheduler is used to decide which queue traffic is forwarded from next 
    * Prioritization allows the scheduler to give certain queues more priority than others 

    ![QoS](images/QoS.png)

* A common scheduling method is weighted round-robin
    * round-robin == packets are takedn from each queue in order, cyclically 
    * weighted == more data is taken from high priority queues each time the scheduler reached that queue 

* CBWFQ(Class-Based Weighted Fair Queuing) is a popular method of scheduling, using a weighted round-robin scheduler while guaranteeing each queue a certain percentage of the interface's bandwidth during congestion 

* Round-Robin scheduling is not ideal for voice/video traffic 

* LLQ (Low Latency Queuing) designates one (or more) queues as strict priority queues

* This is very effective for reducing the delay and jitter of voice/video traffic 

* Downside of starving other queues if there is always traffic in the designated strict priority queue
    - Policing can control the amount of traffic allowed in the strict priority queue so that it can't take all of the link's bandwidth 


**Shaping/Policing**
* traffic shaping and policing are both used to control the rate of traffic 
* Shaping buffers traffic in a queue if the traffic rate goes over the configured rate 
* Policing drops traffic if the traffic rate goes over the configured rate 
    * Burst traffic over the configured rate is allowed for a short period of time 
    * the amount of burst traffic allowed is configurable 


**Quiz**
1. CoS markings consistent with standard practice 
    * Best effort = CoS 0
    * Voice = CoS 5
    * Video = CoS 4

2. bit pattern in DSCP field of a packet marked as EF 
    * 101 110

3. AF markings that provides the best service 
    * AF 41 

    ![AF marking](images/af.png)

4. General Best Practice regarding QoS 
    * Trust markings from IP phones, Don't trust markings from PCs

5. Creates a strict priority queue for data that requires low delay/jitter/loss
    * LLQ (Low Latency Queuing)

### Security Fundamentals
**Key Security Concepts**
* CIA Triad:- 
    * Confidentiality : 
        * only authorized users should be able to access data
        * Some information/data is public and can be accessed by anyone, some is secret and should only be accessed by specific people
    * Integrity
        * Data should not be tampered with (modified) by unauthorized users
        * Data should be correct and authentic 
    * Availability
        * The network/systems should be operational and accessible to authorized users
    
<br>

* A vulnerability is any potential weakness that can compromise the CIA of a system/info
    * A potential weakness isn't a problem on its own

* An exploit is something that can potentially be used to exploit the vulnerability
    * Something that can potentially be used as an exploit isn't a problem on it's own 

* A threat is the potential of a vulnerability to be exploited 
    * A hacker exploiting a vulnerability in your system is a threat

* A mitigation technique is something that can protect against threates 
    * client devices, switches, routers, firewall, etc 

<br>
  
**Common attacks**
* DoS (denial-of-service) attacks
    * threaten the availability of a system 
    * TCP SYN flood 
        * SYN | SYN-ACK | ACK
        * The attacker sends countless TCP SYN messages to the target 
        * The target sends a SYN-ACK message in response to each SYN it receives
        * The attacker never replies with the final ACK of the TCP three-way handshake
        * The incomplete connections fill up the target's TCP connection table 
        * The target is no longer able to make legitimate TCP connections
    * Distributed Denial-Of-Service attack, the attacker infects many target computers with malware and uses them all to initiate a denial-of-service attack (**botnet**)
    
    <br>

* spoofing attacks ---> Availability 
    * use fake source address (IP or MAC address)
    * Numerous attacks involve spoofing
    * DHCP exhaustion attack
    * Attacker uses spoofed MAC addresses to flood DHCP Discover message 
    * The target server's DHCP pool becomes full, resulting in a denial-of-service to other devices

    <br>

* Reflection/amplification attacks 
    * The attacker sends traffic to a reflector and spoofs the source address of its packets using the target's IP address
    * The reflector (i.e a DNS server) sends the reply to the taget's IP address
    *  A reflection attack becomes an **amplification** attack when the amount of traffic sent by the attacker is small, but it triggers a large amount of traffic to be sent from the reflector to the target 
    
    <br>

* Man-in-the-middle attacks (Confidentiality and Integrity)
    * attacker places himself between the src and dst to eavesdrop on communication or to modify traffic before it reaches the destination
    * Common example is ARP spoofing, also known as ARP poisoning
    * A host sends an ARP request, asking for the MAC address of another device
    * The target of the request sends an ARP reply informing the requestor of its MAC address 

    <br>

* Reconaissance attacks
    * Used to gather information about a target which can be used for a future attack 
    * This is often publicly available information
    * i.e nslookup to learn the IP address of a site 
    * `$ nslookup visa.com`
    * or WHOIS query to learn email addresses, phone numbers, physical addresses, etc 
    
* Malware 
    * malicious software , harmful program that can infect a computer 
    * viruse infect software  (a 'Host program'), spread as the software is shared by the user 
    * worms do not require a host program, they are standalone malware and they are able to spread on their own, without user interaction, can congest the network but the 'paylod' of a worm
    * Trojan Horses, harmful software that is disguised as legitimate software , they spread through user interaction such as opening email attachments or downloading a file from the internet 


* Social Engineering attacks
    * psychological manipulation to make the target reveal confidential information or perform some action
    * Phisiing
        * fraudulent emails that appear to come from a legitimate bussiness 
        * contain links to a fraudulent website that seems legitimate 
        * **spear phising** is more targeted form of phising, i.e aimed at employee of a certain company 
        * **whaling** is phishing targeted at high profile individuals, i.e a company president 
    * Vishing (voice phishing) is phishing performed over the phone 
    * Smishing (SMS phishing)
    * Watering hole attacks compromise sites that target the victim frequently visits. If a malicious link is placed on a website the target trusts, they might not hesitate to click it 
    * Tailgating attacks involve entering restricte, secured areas by simply walking in behind an authorized person as they enter 

* Password-related attacks
    * uername/password 
    * Attackers can learn a user's password via multiple methods
        * Guessing
        * Dictionary attack 
        * Brute Force attack: possible combination of letters, numbers and special characters to find the target's password 

        <br>

        ![common attack](/images/common_attack.png)

**Passwords/Multi-Factor Authentication(MFA)**
* Providing more than just a username/password to prove your identity

* Somthing you know 
    * a username/password combination, a PIN etc
 * Something you have 
    * pressing notifcation on phone or a badge that is scanned 
* Somthing  you're 
    * biometrics such as a face scan, palm scan, fingerprint scan, retina scan etc 
* **digital certificates** are another form of authentication used to prove the identity of the holder of the certificate
* they are used for websites to verify that the website being accessed is legitimate 
* certificate signing Request to CA (certificate Authority) which will generate and sign the certificate 

**Authentication, Authorization, Accounting(AAA)**
* Framework for controlling and monitoring users of a computer system (i.e a network)
* Authentication 
    * process of verifying a user's identity 
    * logging in (ideally using MFA) ==> authentication
* Authorization 
    * process of granting the user the appropriate access and permissions.
    * granting the user access to some files/services, restricting access to other files/services = authorization 
* Accounting 
    * process of recording the user's activities on the system 
    * logging when a user makes a changes to a file => accounting 
* Enterprises typically use a AAA server to provide AAA services 
    * ISE(Identity Services Engine) is Cisco's AAA server 

* AAA servers usually support the following two AAA protocols
    1. RADIUS: an open standard protocol. Uses UDP ports 1812 and 1813
    2. TACAS+ : A cisco propriety protocol. Uses TCP port 49

**Security Program Elements**
* set of security policies and procedures 
* User awarness programs are designed to make employees aware of potential security threats and risks 
    * false phising emails 
* User training programs
    * more formal than user awareness programs 
    * corporate security policies 
    * how to create strong passwords, and how to avoid potential threats 
* Physical access control
    * protects equipment and data from potential attackers by only allowing authorized users into protected areas such as network closets or data center floors 
    * **multifactor loks** can protects access to restricted areas 
        * i.e door that requires users to swipe a badge and scan their fingerprint to enter 
        * permissions of the badge can be easily changed, for eg. permissions can be removed when an employee leaves the company 

**Quiz**
1. CIA triad ensures the systems are running and accessible by user 
    * Availability 

2. Real possibility that a potential weakness is taken advantage of to attack a system 
    * Threat 

3. Door locks that require a badge to be scanned and pass code to be entered, this is example of :
    * Physical Access Control 
    * MFA 

4. Not an example of MFA 
    * Doing a retina scan and then doing a fingerprint scan

5. Accounting in AAA model 
    * Logging the date and time a user logged in to the system 

#### Kali Linux Demo 
* attacker use all of the dhcp pool addresses and the other users cannot get ip address assigned 

```
R1# sh run | sec dhcp 

R1# sh ip dhcp pool

R1# sh ip dhcp binding 

R1# clear ip dhcp binding *

```
#### Port Security 
**Intro to port security**
* Security feature of Cisco Switches 
* allows us to control which source MAC address(es) are allowed to enter the switchport 
* If an unauthorized source MAC address enters the port, an action will be taken
    * default action is to place the interface in an 'err-disabled' state 
* when we enable port security on an interface with the default settings, one MAC address is allowed
    * we can configure the allowed MAC address manually 
    * if we don't configure it manually the switch will allow the first source MAC address that enters the interface 
    * We can change the maximum number of MAC addresses allowed 

**Why we use port security**
* Allows network admin to control which devices are allowed to access the network 
* However, MAC address spoofing is a simple task
    * it is easy to configure a device to send frames with a different source MAC address 
* Rather than manually specifying the MAC addresses allowed on each port, port security's ability to limit the number of MAC addresses allowed on an interface is more useful 
* DHCP starvation attack 
    * the attacker spoofed thousands of fake MAC addresses 
    * the DHCP server assigned IP addresses to these fake MAC addresses, exhausting the DHCP pool
    * switch's MAC address table can also become full due to such an attack 
* So, limiting the number of MAC addresses on an interface can protect against those attacks 

**Port security configuration**
* enabling port security 

```
SW1(config)# interface g0/1

SW1(config-if)# switchport port-security 
! cmd rejected: GigabitEthernet0/0 is a dynamic port 

SW1(config-if)# do sh int g0/1 switchport 
!Name: Gi0/1
Switchport: Enabled
Administrative Mode: dynamic auto
Operational Mode: static access 

* port security only on access or trunk not in dynamic auto or desirable 

SW1(config-if)# switchport mode access

SW1(config-if)# do show int g0/1 switchport
!Name: Gi0/1
Switchport: Enabled
Administrative Mode: static access
Operational Mode: static access 

SW1(config-if)# switchport port-security 

SW1# show port-security interface g0/1
```

* re-enabling an interface(manually)
    1. disconnect the unauthorized device 
    2. shutdown and then no shutdown the interface

    ```
    SW1(config)# interface g0/1
    SW1(config-if)# shutdown
    SW1(config-if)# no shutdown     
    ```
* re-enabling an interface (ErrDisable Recovery)

`SW1# show errdisable recovery`
* every 5 minutes(by default), all err-disabled interfaces will be re-enabled if err-disable recovery has been enabled for the cause of the interface's disablement 

`SW1(config)# errdisable recovery cause psecure-violation`

`SW1(config)# errdisable recovery interval 180`

`SW1(config)# show errdisable recovery`

* ErrDisable Recovery is useless if you don't remove the device that caused the interface to enter the err-disabled state 

**Violation Modes**
* 3 dofferent violation modes that determine what the switch will do if an unauthorized frame enters an interface configured with port security 
    1. **shutdown**
        * effectively shuts down the port by placing it in an err-disabled state 
        * Generates a Syslog and/or SNMP message when the interface is disabled 
        * the violation counter is set to 1 when the interface is disabled 

    2. **Restrict**
        * the switch discards traffic from unauthorized MAC addresses
        * the interface is NOT disabled 
        * Generates a Syslog and/or SNMP message each time an unauthorized MAC is detected 
        * the violation counter is incremented by 1 for each unauthorized frame 

    ```
    SW1(config-if)# switchport port-security

    SW1(config-if)# switchport port-security mac-address 000a.000a.000a

    SW1(config-if)# switchport port-security violation restrict 

    ```


    ![restrict mode](images/restrict_mode.png)
    

    3. **Protect**
        * the switch discards traffic from unauthorized MAC addresses 
        * The interface is NOT disabled 
        * doesn't generate syslog and SNMP message 
        * doesn't increment the violation counter
    
    ![protect mode](images/protect.png)

*secure MAC address aging**
* by default secure MAC address will not 'age out' (Aging Time : 0 mins)
    * can be configured with switchport port-security aging time minutes 

* the default aging type is Absolute 
    * **Absolute** means : after the secure MAC address is learned, the aging timer starts and the MAC is removed after the timer expires, even if the switch continues receiving frames from that source MAC address 
    * **Inactivity**: After the secure MAC address is learned , the aging timer starts but is reset every time a frame from that source MAC address is received on the interface 
    * aging is configured with `switchport port-security aging type {absolute | inactivity}`

* secure static MAC aging (addresses configured with switchport port-security mac-address x.x.x) is disabled by default 
    * Can be enabled with switchport port-security aging static 

![secure mac](images/secure_mac.png)

**sticky Secure MAC addresses*
* can be enabled with following command

`SW1(config-if)# switchport port-security mac-address sticky`

* when enabled, dynamically-learned secure MAC addresses will be added to the running config like this 

`SW1(config-if)# switchport port-security mac-address sticky mac-address`

* sticky secure MAC addresses will never age out
    * We need to save the running-config to the startup-config to make them truly permanent(or else they will not be kept if the switch restarts)

* after issusing the command sw port-sec mac-add sticky all current dynamically-learned secure MAC addresses will be converted to sticky secure MAC addresses and vice versa 

![sticky secure](images/sticky_secure.png)

* all command review 

![command review](images/command_port_sec.png)

**Quiz**
1. sticky mac addresses: 3
2. which of the following violation occurs in restrict mode: 
    * the violation counter is inceremented (e)
    * unauthorized traffic is discarded (b)

3. Violation mode is: Protect, so unauthorized traffic will be dropped 

4. Will re-enable an interface that was disabled by port security 
    * shutdown and then no shutdown
    * errdisable recovery cause psecure-violation in global config mode 

5. switchport port-security command command : need Administrative mode: Static access 


#### DHCP Snooping 
**what is DHCP snooping**
* filter DHCP messages received on untrusted ports 
* only filters DHCP messages, Non-DHCP messages aren't affected 
* all ports are untrusted by default 
    * usually, uplink ports are configured as trusted ports, and downlink ports remain untrusted 
    * CHADDR ===> Client Hardware Address 
        * indicates the MAC address of the client 

* **DHCP poisoning (Man-in-the-middle)** 
* similar to ARP poisoning, DHCP poisoning can be used to perform a Man-in-the-Middle attack 
* A spurious DHCP sercer replies to clients 'DHCP Discover messages and assigns them IP addresses, but makes the client use the spurious server's IP as the default gateway.
* this will cause the client to send traffic to the attacker instead of the legitimate default gateway 
* The attacker can then examine/modify the traffic before forwarding it to the legitimate default gateway 

* when dhcp snooping filters messages, it differentiates between DHCP server message and DHCP client 
* **DHCP servers**--> OFFFER, ACK, NAK == opposite of ACK, used to decline a client's Request 
* **DHCP client** ---> DISCOVER, REQUEST, RELEASE ==> used to tell the server that the client no longer needs its IP address 
* DHCP client DECLINE ==> Used to decline the IP address offered by a DHCP server 

**How does it work?**
* if DHCP message is received on a trusted port, forward it as normal without inspection
* if received on an untrusted port, inspect it and act as follows:
    * if it is a DHCP server message, discard it 
    * if it is a DHCP client message, perform following checks:
        *Discover/Request message---> check if the frame's source MAC address and the DHCP message's CHADDR fields match. Match => forward, mismatch = discard 
        * Release/Decline message: check if the packet's source IP address and the receiving interface match the entry in the DHCP snooping binding table. Match = forward, mismatch = discard 
        * when a client successfully leases an IP address from a server, create a new entry in the DHCP snooping Binding Table 
        
        ```
        SW2(config)# ip dhcp snooping 
        SW2(config)# ip dhcp snooping vlan 1
        SW2(config)# no ip dhcp snooping information option
        SW2(config)# interface g0/0
        SW2(config)# ip dhcp snooping trust
        * same configuration for sw1

        SW2# show ip dhcp snooping binding

        ```
**DHCP snooping rate-limiting**
* limit the rate which DHCP messages are allowed to enter an interface
* if therate of DHCP messages crosses the configured limit, the interface is err-disabled
* like with port security the interface can be manually re-enabled or automatically re-enabled with errdisable recovery 

![rate limit 1](images/rate-limit_1.png)

![rate limit 2](images/rate-limit_2.png)

* DHCP option 82 (information option)
    * also known as DHCP relay agent information option
    * provides additional information about which DHCP relay agent received the client's message on which interface in which VLAN etc 
    * DHCP relay agent can add Option 82 to messages they forward to the remote DHCP server 
    * With DHCP snooping enabled by default Cisco switched will add Option 82 to DHCP messages they receive from client, even if the switch isn't acting as a DHCP relay agent 

**DHCP snooping coniguration**

![command review](images/comand%20_review.png)

**Quiz**
1. Discarded if received on a DHCP snooping untrusted interface
    * OFFER, ACK and NAK 

2. Not stored in the DHCP snooping binding database 
    * Default Gateway 

3. functions of DHCP snooping 
    * limiting the rate of DHCP messages 
    * filtering DHCP messages on untrusted ports 

4. when DHCP snooping inspects a DHCP Discover message that arrives on an untrusted interface, what does it check?
    * Source MAC address and CHADDR, client hardware Mac address

5. DHCP rate-limiting is configured on SW1's G0/1 interface. What happens if the DHCP messages are received on G0/1 at a rate faster than the configured limit ?
    * the interface will be disabled 

#### Dynamic ARP inspection 
**What is dynamic ARP inspection ?**
* feature of switch that is used to filter ARP messages received on untrusted ports 
* DAI only filters ARP messages. Non-ARP messages aren't affected 
* All ports are untrusted by default 
    * typically all ports connected to other network devices(switched, routers) should be configured as trusted, while interfaces connected to end hosts should remain untrusted 

**How does it work?**
**What attacks does it prevent?**
* ARP poisoning (Man-in-the-middle)
    * attacker manipulating targets' ARP tables so traffic is sent to the attacker 
    * attacker can send gratuitous ARP messages using another device's IP address 
* DAI inspects the sender MAC and sender IP fields of ARP messages received on untrusted ports and checks that there is a matching entry in the DHCP snooping binding table 
    * if there is a matching entry, the ARP message is forwarded normally 
    * if there isn't a matching entry, the ARP message is discarded 

    `SW1# show ip dhcp snooping binding `

    * DAI doesn't inspect messages received on trusted ports, they are forwarded normally 
    
    * ARP ACLs can be manually configured to map IP addresses/MAC addresses for DAI to check 
        * useful for hosts that don't use DHCP 
    * DAI can be configured to perform more in depth checks also
    * DAi supports rate-limiting to prevent attackers from overwhelming the switch with ARP messages

**DAI configuration**

```
SW2(config)# ip arp inspection vlan 1
SW2(config)# interface range g0/0 - 1
SW2(config-if-range)# ip arp inspection trust

SW1(config)# ip arp inspection vlan 1
SW1(config)# interface g0/0
SW1(config-if)# ip arp inspection trust 

```
* unlike DHCP snooping which requires two commands to enable it like ip dhcp snooping and ip dhcp snooping vlan vlan-number, DAI only requires one: ip arp inspection valn vlan-number 

**`SW1# show ip arp inspection interfaces`**

* DAI rate limiting is enabled on untrusted ports by default with a rate of 15 packets per second 

* DAI has a feature called burst interval which allows us to configure rate limiting using x packets per y seconds 

**DAI rate limiting**

![DAI rate limiting](images/dai%20rate%20limiting.png)

**DAI optional checks**

```
SW1(config)# ip arp inspection validate ?
    dst-mac validate destination MAC address
    ip      validate IP addresses
    src-mac validate source MAC address

SW1(config)# ip arp inspection validate ip src-mac dst-mac 
! must enter all of the validation we want in a single command 

```
**ARP ACL**
* `SW2(config)# arp access-list ARP-ACL-1`

* `SW2(config-arp-nacl)# permit ip host 192.168.1.100 mac host oc29.2f1e.7700`

* `SW2(config)# ip arp inspection filter ARP-ACL-1 vlan 1`

![command summary](images/arp-command-summary.png)

**Quiz**
1. when we enter ip arp inspection vlan 1 command on sw1
    * all interfaces in vlan 1 are untrusted 
    
2. when ip inspection validate ip / src-mac /dst-mac are entered in different line as a command 
    * DAI validation is only enabled for dst-mac (the last command takes effect)

3. DAI rate limiting ( also allows to configure burst limit)
    * it is enabled on untrusted ports by default 
    * it is enabled at a rate of 15 packets per second by default 

4. DAI inspects sender IP and MAC it checks against 
    * DHCP snooping binding table 
    * ARP ACLS 

5. comand that limit ARP messages to max avg of 15 per second 
    * ip arp inspection limit rate 15
    * ip arp inspection limit rate 45 burst interval 3

#### LAN Architecture 
**2-Tier and 3-Tier Architecture**
* basic network design/architecture 
* There are standard 'best practices' for network design
* **star** topology 
* **full mesh**
* **partial mesh** some devices are connected to each other but not all 
* **Access Layer & Distribution Layer**
* Also called a collapsed core design because it omits a layer that is found in the Three Tier design: the CORE layer
* Access Layer 
    * end hosts connect to (PCs, printer, cameras etc)
    * lots of ports for end hosts to connect to 
    * QoS marking is typically done here
    * Security services like port security, DAI etc are typically performed here 
    * switchports might be POE-enables for wireless APs, IP phones etc 

* Distribution Layer (*Core-Distribution Layer*)
    * aggregates connection from the Access Layer switched 
    * typically is the border between Layer 2 and Layer 3
    * connects to services such as internet, WAN etc
* in lare LAN networks with many Distribution Layer switches , the number of connection required betwn Distribution Layer Switches grows rapidly 

* To help scale Large LAN networks we can add a core layer 
* Cisco recommends adding a Core layer if there are more than three Distribution Layers in a Single location 

* Core Layer 
    * connects Distribution Layers together in large LAN networks 
    * The focus is speed ('fast transport')
    * CPU-intensive operations such as security, QoS marking/classification, etc should be avoided at this layer 
    * connections are all Layer 3. No spanning-tree
    * should maintain connectivity throughout the LAN even if devices fails 
    
![summary 3 tier](images/3%20tier.png)

**Spine Leaf Architecture (Data Center)**
* DC are the dedicated spaces/buildings used to store computer systems such as servers and network devices
* Traditional Data center designs used a three-tier architecture(Access-Distribution-Core)
* This architecture worked well when most traffic in the data center was North-South

![North-south](images/North-south.png)

* With the precedence of virtual servers, applications are often deployed in a distributed manner(accross multiple physical servers), which increases the amount of East-West traffic in the data center 

* *the traditional three tier architecture led to bottlenecks in bandwidth as well as variablility in the server-to-server latency depending on the path the traffic takes*

* to solve this, spine leaf architecture(also called clos architecture) has become prominent in data centers 

* Basic rules about spine -leaf architecture 
    1. Every leaf switch is connected to every spine switch 
    2. Every spine switch is connected to every leaf switch 
    3. Leaf switch is not connected to other leaf switches 
    4. Spine switches do not connect to other spine switches
    5. End hosts(servers etc) only connect to leaf switches 

* The path taken by traffic is randomly chosen to balance the traffic load among the spine switches 
* Each server is separated by the same nunber of 'hops'(except those connected to the same leaf) providing consistent latency for East-west Traffic 


**SOHO (Small Office/Home Office)**
*  Small Office/Home Office 
* don't have complex needs, all functionality provided by a single device 
* Once device can serve as 'Router' 'Switch' 'firewall', 'Wireless Access Point', 'Modem'

**Quiz**
1. boundary betn layer 2 and layer 3 in a traditional 2-tier or 3-tier network
    * Distribution 

2. Not find in the core layer of a traditional 3-tier LAN
    * STP

3. PoE enabled switchports in a traditional 3-tier LAN
    * Access Layer 

4. In Spine leaf architecture which of the following should not be connected to a leaf switch 
    * A Leaf Switch 

5. Wireless router functions 
    * routing, switching, wireless access, security 

#### WAN Architectures 
**Intro to WANs**
* etends over a large geographical area 
* geographically separate LANs
* Typically used to refer to an enterprise's private connections that connect their offices, data centers, and other sites together 
* VPNs Virtual Private Networks can be used to create private WAN connections 

![wan](images/WAN.png)

**Leased lines**
* dedicated physical link, typically connecting two sites 
* Leased lines use serial connections (PPP or HDLC encapsulation)
* T-carrier and E-carrier systems , standards that provide different speeds and different standards 
* higher cost, higher installation lead time, and slower speeds of leased lines, Ethernet WAN technologies are becoming more popular 

**MPLS VPNs**
* Multi Protocol Label Switching 
* service providers MPLS networks are shared infrastructure because many customer enterprises connect to and share the same infrastructure to make WAN connections
* label switching in the name of MPLS allows VPNs to be created over the MPLS infrastructure through the use of labels 
* Some important terms: 
    * CE router = Customer Edge Router
    * PE router = Provider Edge Router 
    * P router = Provider Core Router 
* When the PE routers receive frames from the CE routers, they add a label to the frame
* These labels are used to make forwarding decisions within the service provider network, not the destination IP.
* These labels are used to make forwarding decisions within the service provider network, not the destination IP 
* The CE routers do not use MPLS, it is only used by the PE/P routers 
* When using a Layer 3 MPLS VPN, the CE and PE routers peer using OSPF, for example to share routing information

![MPLS](images/MPLS.png)

**Internet Connectivity**
* there are countless ways for an enterprise to connect to the intenet
* for example, private WAN Technologies such as leased lines and MPLS VPNS can be used to connect to a service provider's internet infrastructure 
* CATV and DSL 
* A DSL modem (modulator-demodulator) is required to convert data into a format suitable to be sent over the phone lines 

**Redundant Internet Connections**
* 1 connections to 1 ISP ==> Single Homed 
* 2 connections to 1 ISP ==> Dual Homed
* 1 connections to each of 2 ISPs ==> Multi Homed 
* 2 connections to each of 2 ISPs ==> Dual Multihomed

**Internet VPNs**
* When using the internet as a WAN to connect sites together, there is no built-in security by default 
* to provide secure communications over the internet, VPNs are used 
* 2 kinds of Internet VPNs:
    1. **site-to-site VPNs using IPsec**
    * between two devices and is used to connect two sites together over the internet 
    * 'tunnel' is created between the two devices by encapsulating the original IP packet with a VPN header and a new IP header 

    ![vpn](images/vpn.png)

    * the sending device combines the original packet and session key(encryption key) and runs them through an encryption formula 
    * the sending device encapsulated the encrypted packet with a VPN header and a new IP header 
    
    * a tunnel is formed only between two tunnel endpoints (for example, the two routers connected to the internet)
    
    * **Limitations of standard IPsec**
        1. IPsec doesn't support broadcast and multicast traffic, only unicast. This means that routing protocols such as OSPF can't be used over the tunnels, because they rely on multicast traffic 
            * can be solved with **'GRE over IPsec'**
            * Generic Routing Encapsulation creates tunnels like IPsec, however it does not encrypt the original packet, so it is not secure 
            * it has advantage of being able to encapsulate a wide variety of Layer 3 protocols as well as broadcast and multicast messages 
            * To get the flexibility of GRE with security of IPsec, 'GRE over IPsec' can be used 

            * **`IP packet + GRE Header + IP Header--> encrypted -----> IPsec VPN header + IP Header`**

        2. configuring a full mesh of tunnels between many sites is a labor-intensive task
            * can be solved with **cisco's DMVPN**
            * Dynamic Multipoint VPN is a cisco developed solution that allows routers to dynamically create a full mesh of IPsec tunnesl without having to manually configure every single tunnel 
            
    <br>

    2. **Remote-access VPNs using TLS** 
    * Site-to-Site is used to make a point-to-point connection between two sites over the internet, the remote-access VPNs are used to allow end devices (PC, mobile phones) to access the company's internal resources securely over the internet 
    * Remote-access VPNs typically use TLS (Transport Layer Security)
        * TLS is also what provides security for HTTPS 
        * TLS was formerly known as SSL(Secure Sockets Layer) and developed by Netscape, but it was renamed to TLS when it was standardized by the IETF 
    * VPN client software (like Cisco AnyConnect) is installed on end devices like company laptop
    * these end devices then form secure tunnels to one of the company's routers/firewalls acting as a TLS server 
    * This allows the end users to securely access resources on the company's internal network without being directly connected to the company network 

    ![remote vpn](images/remote_vpn.png)

* **comparision between site-to-site and remote-access VPN

|Site-to-Site    |                        Remote-Access     | 
|----------|:-------------:|
| Typically uses IPsec   |   typically uses TLS |
| provide service to many devices within the sites they are connecting  |   provide service to the one end device the VPN client software is installed on    |
| Used to permanently connect two sites over the internet | used to proide on-demand access for end devices that want to securely access company resources while connected to a network which is not secure | 

**Quiz**
1. leased line standard that provide 1.55 Mbps  of bandwidth 
    * T1
2. In MPLS which of the following routers doesn't run MPLS 
    * CE 
3. allows CE routers to directly form OSPF peerings with each other 
    * Layer 2 MPLS VPN 
4. already-installed phone lines 
    * DSL 
5. which of the following protocols can be used in combination with IPsec to provide more flexibility by allowing multicast traffic to be forwarded in the tunnel
    * GRE 

#### Virtualization and Cloud 
**Intro to Virtualization**
* Virtual Servers
    * Cisco hardware servers such as UCS (Unified Computing System)
    * largest vendors of hardware servers are Dell EMC, HPE, and IBM
    * Hardware Components(CPU, RAM, Storage, NIC) -------> OS -------> Apps(web server, Email server etc )
    * virtualization allows us to break the one-to-one relationshop of hardware to OS, allowing multiple OS's to run on a single physical server 
    
    ![virtualiztion](images/virtualization.png)

    * hypervisor used to manage and allocate the hardware resources (CPU, RAM etc) to each VM
    * Another name for a hypervisor is VMM(Virtual Machine Monitor)
    * Type1 (directly runs on top of the hardware)
        * VMware ESXi, Microsoft Hyper-V etc 
        * also called bare-metal or native hypervisor as they run directly on the hardware(metal)
    * Type 2, or hosted hypervisor 
        * OS running directly on the hardware is called HOST OS, and the OS running in a VM is called a Guest OS 
    * **Why  Virtualization**
        * Partitioning : Run multiple OS on one physical machine 
            - Divide system resources between virtual machines 
        * Isolation:
            - Provide fault and security isolation at the hardware level
            - Preseve performance with advanced resource controls 
        * Encapsulation:- save the entire state of a virtual machine to files
            - Move and copy virtual machines as easily as moving and copying files 
        * Hardware Independence: Provision or migrate any virtual machine to any physical server 

        ![vms](images/vms.png)

**Intro to Cloud Computing**
* Essential Characteristics 
    * Traditional IT infrastructure deployments 
    1. On-premises
    2. Colocation : data centers that rent out space for customers to put their infrastructure (servers, network devices)
        * The data center provides the space electricity and cooling 
        * The servers, network devices etc are still the responsibility of the end customer, although they are not located on the customer's premises 

* The American NIST defined cloud computing in SP (Special Publication) 800-145
* https://csrc.nist.gov/publications/detail/sp/800-145/final

* Outlined SP 800-145:
    1. five essential characteristics
    * on-demand self-service : A customer can unilaterally provision computing capabilities, such as server time and network storage as needed automatically without requiring human interaction with each service provider 

    * Broad network access:
        * capabilities are available over the network and accesses through standard mechanisms that promote use by heterogeneous thin or thick client platforms(eg: mobile phones, tablets, laptops and workstations)
    * Resource pooling
    * Rapid elasticity 
    * Measured service 

    2. Three service models
        * Software as a Service (SaaS)
        * Platform as a Service (PaaS)
        * Infrastructure as a Service (IaaS)

    3. Four deployment models
        * Private Cloud
        * Community Cloud 
        * Public Cloud 
        * Hybrid Cloud 

**Benefits of Cloud Computing**
* cost 
    * CapEx (Capital Expenses) of buying hardware and software, setting up data centers etc. are reduced or eliminated 
* Global scale 
    * cloud services can scale globally at a rapid pace. services can be set up and offered to customers from a geographic location close to them 
* Speed/Agility 
    * Services are provided on demand, and vast amounts of resources can be provisioned within minutes 
* Productivity 
    * Cloud services remove the need for many time-consuming tasks such as procuring physical servers, racking them, cabling, installing and updating operating systems etc 
* Realiability 
    * Backups in the cloud are very easy to perform. Data can be mirrored at multiple sites in different geographic locations to support disaster recovery  
    
**Connecting to Public Clouds**
* Using Private WAN Service Provide like MPLS 
* Internet 
* IPsec VPN Tunnel 

![cloud](images/cloud.png)

**Quiz**
1. The hypervisor is used to manage and allocate hardware resources to VMs 

2. Native Hypervisor 
    * Type 1

3. Not a characteristic of cloud computing
    * infinite resource pool

4. Cloud service types that allows customer to use applications running on the provider's cloud infrastructure 
    * SaaS 
![cloud infras](images/cloud%20infras.png)

5. cloud deployment type which may exist off premises
    * Public, Private, Community 


### Wireless Fundamentals
* Wirelss LANs using Wi-Fi
* IEEE 802.11
* CSMA/CA : Carrier Sense Multiple Access with collision Avoidance is used to facilitate half-duplex communications 
* Assembles frame ----> check if channel is free ---> Not free, wait for random period of time -----> listen again if it's free the transmit frame. 

* RTS and CTS (Request to Send and Clear to send)
* Regulated by various international and national bodies 
* Coverage area must be considered 
    * signal range 
    * signal absorption, reflection, refraction, diffraction and scattering 
    * absorption: passes through material and converted into heat, weakening the original signal
    * Reflection :when a signal bounces off a material, for example metal 
    * Refraction: happens when a wave is bent when entering a medium where the signal travels at a different speed 
        * glass, water 
    * Defraction : happens when a wave encounters an obstacle and travels around it
        * blind spots 
    * Scattering : happens when a material causes a signal to scatter in all directions
        * Dust, smog, uneven surfaces, etc can cause scattering
* Other devices using the same channels can cause interference 

<br>

**Radio Frequency(RF)**
* to send wireless signals, the sender applies an alternating current to an antenna 
    * this creates electromagnetic fields which propagate out as waves 
* Electromagnetic waves can be measured in multiple ways for example amplitude and frequency 
* Amplitude is the maximum strength of the electric and magnetic fields 
* Frequency measures the number of up/dowm cycles per a given unit of time 
* most common measurement of frequency is hertz, (Hz, kHz, MHz, GHz, THz)
* The amount of time of one cycle => Period 
* if frequency is 4Hz the period is 0.25 seconds 
* visible frequency is 400 THz to 790 THz
* RF is from 30Hz to 300Hz and is used for many purposes 
* WiFi  uses two main bands (frequency ranges)
    * 2.4 GHz band
        * actual range is 2.400GHz to 2.4835GHz

    * 5 GHzband
        * the actual range is from 5.150 GHz to 5.825 GHz
        * Divided into four smaller bands: 5.150GHz to 5.250GHz, 5.250 GHz to 5.350 GHz
**Wi-Fi Standards**
* WiFi 6 (802.11ax) has expanded the spectrum range to include a band in the 6GHz range 
* Each band is divided up into multiple channels 
    * devices are configured to transmit and receive traffic on one (or more devices ) of these channels 
    * The 2.4 GHz band is divided into several channels, each with a 22 MHz range 
* In small wireless LAN with only a single AP we can use any channel but in largers WLANs with multiple APs, it's important that adjacent APs don't use overlapping channels, this helps avoid interference 
* In 2.4GHz band it is recommended to use channel 1, 6 and 11
* 5 GHz band conists of non overlapping channels 
* Uing 1, 6 and 11 we can place APs in a honeycomb pattern to provide complete coverage of an area without interference between channels 

![honey comb](images/honey-comb.png)

**Service Sts**
* 802.11 defines different kinds of service sets which are groups of wireless network devices 

* There are three main types:
    * Independent 
    * Infrastructure 
    * Mesh

* All devices in a service set share the same SSID(service set identifier)
* The SSID is a human-readable name which identifies the service set 
* The SSID does not have to be unique 
* **IBSS (Independent Basic Service Set)**
* Wireless network in which two or more wireless devices connect directly without using an AP(Access Point)
* Also called an **ad hoc** network 
* Can be used for file transfer (i.e AirDrop)
* Not scalable beyond a few devices 
* **BSS Basic Service Set**
* kind of infrastructure Service set in which clients connect to each other via an AP(Access Point), but not directly to each other 
* A BSSID is used to uniquely identify the AP
    * Other APs can use the same SSID but not the same BSSID
    * The BSSID is the MAC address of the AP's radio 
* wireless device that have associated with the BSS are called 'clients' or 'stations'

* The area around an AP where its signal is usable is called a BSA(Basic Service Area)
* Traffic must flow through AP 
* **Extended Service Set**
    * Used to create larger Wireless LANs beyond the range of a single AP 
    * APs with their own BSSs are connected by a wired network
        * Each BSS uses the same SSID
        * Each BSS has a  unique BSSID
        * Each BSS uses a different channel to avoid interference 
* clients can pass between APs without having to reconnect, providing a seamless Wi-Fi experience when moving between APs
    * This is called roaming 

![extended](images/extended.png)

* **MBSS Mesh Basic Service Set**
    * Can be used in situations where it's difficult to run an Ethernet connection to every AP 
    * Mesh APs use two radios: one to provide a BSS to wireless clients and one to form a 'backhaul network' which is used to bridge traffic from AP to AP 
    * At least one AP is connected to wired network, and it is called the RAP (Root Access Point)

* in 802.11 the upstream wired network is called the DS (Distribution System)
* **A workgroup bridge (WGB)** operated as a wireless client of another AP and can be used to connect wired devices to the wireless network

** **An Outdoor Bridge** can be used to connect networks over long distances without a physical cable connecting them

![wireless summary](images/wireless-summary.png)

**Quiz**
1. 2.4 GHz band which channels should be used:
    * (b) 1,6,11

2. If the enterprise network is mostly wired what is the purpose of an AP in the network 
    * To connect wireless devices to the wired network

3. Commonly used bands by WLANS:
    * 2.4 GHz and 5 GHz 

4. statements about ESSs :
    * Each BSS uses a unique BSSID
    * Roaming can provide seamless connectivity when moving between APs 

5. AP that provides multiple BSSs :
    * Each BSS doesn't share the same BSSID

### Wireless Architectures 
**802.11 messages/frame format**
* Frame Control : 2 bytes (16bits)
    * Proivides information such as the message type and subtype 
* Duration/Id: 2 bytes 
    * Depending on the message type , this field can indicate:
        * the time(in microseconds) the channel will be dedicated for transmission of the frame 
        * and the identifier for the association (connection)
* Addresses: Up to four addreses can be present in 802.11 frame
    * Destination Address (DA): Final recipient of the frame 
    * Source address (SA): original sender of the frame
    * Receiver Address (RA): Immediate recipient of the frame 
    * Transmitter Address (TA): Immediate Sender of the frame 
* sequence control : 2 bytes 
    * Used to reassemble fragments and elimiate duplicate frames 
* QoS Control: 2 bytes 
    *  Used in QoS to prioritize certain traffic 
* HT (High Throughput) Control: 4 bytes 
    * Added in 802.11n to enable High Throughput operations 
    * 802.11n is also known as 'High Throughput (HT)' Wifi
    * 802.11ac also known as 'Very High Thoughput' (VHT) WiFi

* FCS (Frame Check Sequence): 4 bytes 
    * Same as in an Ethernet frame, used to check for errors

**802.11 Association Process**
* AP bridge traffic between wireless stations and other devices 
* for a station to send traffic through the AP, it must be associated with the AP 
* There are three 802.11 connection states 
    * Not authenticated, not associated 
    * Authenticated, not associated 
    * Authenticated and associated 
* The station must be authenticated and associated with the AP to send traffic through it 
    * There are 2 ways a station can scan for a BSS
        * Active Scanning: The station sends probe requests and listens for a probe response from an AP 
        * Passive Scanning: The station listens for beacon messages from an AP 
        Beacon messages are sent periodically by APs to advertise the BSS 
* There are three 802.11 message types :
    * Management: used to manage the BSS 
        * Beacon 
        * Probe request and probe response 
        * Authentication
        * Association request, association response 
    * Control: used to control access to the medium (RF). assists with delivery of management and data frames 
        * RTS (Request to Send)
        * CTS (Clear to Send)
        * ACK
    * Data: Used to send actual data packets 

![802.11](images/802-11.png)

* Three main wireless AP deployment methods 
* **Autonomous APs**
    * Self contained systems that don't rely on a WLC 
    * Are configured individually by console (CLI), telnet/SSH, or HTTP/HTTPS web connection (GUI)
    * An IP address for remote management should be configured 
    * The RF parameters must be manually configured (transmit power, channel etc)
    * Security policies are handled individually by each AP 
    * QoS rules etc are configured individually on each AP 
    * There is no central monitoring or management of APs
    * Autonomous APs connect to the wired network with a trunk link
    * Each VLAN has to stretch accross the entire network. This is considered bad practice:
        * Large broadcast domains 
        * Spanning tree will disable links 
        * Adding/deleting VLANs is very labor-intensive 

    * Autonomous APs can be used in small networks, but they are not viable in medium to large networks 

* **Lightweight APs**
    * handle 'real-time' opetrations like transmitting/receiving RF traffic, encyption/decryption of traffic, sending out beacons/probes etc 
    * Other functions are carried out by a WLC for example RF management, security/QoS management, client autentication, client association/roaming management etc 
    * **split-MAC architecture**
    * WLC and the lightweight APs authenticate each other using digital certificates installed on each device (X.509 standard certificates)
    * CAPWAP (Control And Provisioning Of Wireless Access Points) to communicate 
    * Based on an older protocol called LWAPP (Lightweight Access Point Protocol)
    * Datagram Transport Layer Security (DTLS)
    
* **Cloud-based APs**
    * in between autonomous AP and split-MAC architecture 
        * Autonomous APs that are centrally managed in the cloud 
    * Cisco Meraki is a popular cloud-based Wi-Fi solution
    * The Meraki dashboard can be used to configure APs, monitor the network, generate performance reports, etc 

**Wireless LAN Controller (WLC) deployments**
* In a split-MAC architecture, there are four main WLC deployment models 
    * Unified 
    * Cloud-based 
    * Embedded 
    * Mobility Express

**Quiz**
1. 802.11 Probe Request is:
    * Management 

2. AP that are centrally managed 
    * Lightweight 
    * Cloud based 

3. AP types that uses the CAPWAP protocol
    * Lightweight 

4. lightweight AP mode that offer a BSS for clients 
    * Local and FlexConnect 

5. WLC deployments supports the greatest number of APs 
    * Unified 


### Wireless Security
**intro to wireless network security**
* A MIC (Message encryption methods)
* MIC helps to determine the integrity of the message 

**Authentcation**
* Open Authentication
    * included in 802.11 
    * client sends an authentication request, and the AP accepts it
    * Not  a secure authentication method 

* WEP (Wired Equivalent Privacy)
    * included in 802.11
    * use to provide both authentication and encryption of wireless traffic 
    * WEP uses the **RC4 algorithm**
    * shared-key protocol, requiring sender and receiver to have the same key 
    * can be 40bits and 104bits in length
    * combined with a 24-bit 'IV' (Initialization Vetor)
    * not secure and can be easily cracked 

* EAP (Extensible Authentication Protocol)
    * authentication framework 
    * defines a standard set of authentication functions that are used by various EAP methods 
    * 802.1X port based network access control 
        * Supplicant : device that wants to connect 
        * Authenticator: The device that provides access to the network
        * Authentication server (AS): the device that receives client credentials and permits/denies access 
            * usually a RADIUS server 

    * LEAP (Lightweight EAP)
        * Cisco's improvement over WEP 
        * Clients must provide a username and password to authenticate 
        * mutual authentication is provided by both the client and server sending a challenge phrase to each other 
        * Dynamic WEP keys are used, meaning that WEP keys are changed frequently 
        * It's vulnerable and should not be used anymore 

    * EAP-FAST (EAP Flexible Authentication via Secure Tunneling)
        * Also developed by Cisco 
        * Consists of three phases:
            1. A PAC (Protected Access Credential) is generated and passed from the server to the client
            2. A secure TLS tunnel is established between the client and authentication server 
            3. Inside of the secure (encrypted) TLS tunnel, the client and server communicate further to authenticate/authorize the client 

    * PEAP (Protected EAP)
        * inolving establishing a secure TLS tunnel between the client and server 
        * instead of a PAC, the server has a digital certificate 
        * The client uses this digitial certificate to authenticate the server 
        * The certificate is also used to establish a TLS tunnel 
        * Because only the server provides a certificate for authentication, the client must still be authenticated within the secure tunnel, for example by using MS-CHAP (Microsoft Challenge-Handshake Authentication Protocol)

    * EAP-TLS(EAP Transport Layer Security)
        * EAP-TLS requires a certificate on the AS and on every single client 
        * Is the most secure wireless authentication method, but it is more difficult to implement than PEAP because every client device needs a certificate 
        * Because the client and server authenticate each other with digital certificates, there is no need to authenticate the client within the TLS tunnel 
        * The TLS tunnel is still used to exchange encryption key information (encryption methods will be discussed next)


**Encryption/Integrity methods**
* TKIP (Temporary Key Integrity Protocol)
    * WEP was found to be vulnerable, but wireless hardware at the time was built to use WEP 
    * A temporary solution was needed until a new standard was created and new hardware was built 
    * TKIP adds various security features: 
        * A MIC is added to protect the integrity of message 
        * A key mixing algorithm is used to create a unique WEP key for every frame 
        * The initialization vector is doubled in length from 24 bits to 48 bits making brute-force attacks much more difficult
        * The MIC includes the sender MAC address to identify the frame's sender 
        * A timestmap is added to the MIC to prevent replay attacks . Replay attacks involve re-sending a frame that has already been transmitted 
        * A TKIP sequence number is used to keep track of frames sent from each source MAC  address. This also protects against replay attacks

* CCMP (Counter/CBC-MAC Protocol)
    * More secure than TKIP
    * Used in WPA2
    * To use CCMP, it must be supported by the device's hardware. Old hardware built only to use WEP/TKIP cannot use CCMP 
    * CCMP consists of two different algorithms to provide encryption and MIC 
        1. **AES (Advanced Encryption Standard) counter mode encryption** 
            * Most secure encryption protocol currently available. It is widely used all over the world 
            * Multiple modes of operation for AES. CCMP uses 'counter mode'
        2. CBC-MAC (Cipher Block Chaining Message Authentication Code) is used as a MIC to ensure the integrity of message 
        
* GCMP (Galois/Counter Mode Protocol) 
    * GCMP is more secure and efficient than CCMP
    * Its increased effciency allows higher data throughput than CCMP
    * It is used in WPA3
    * GCMP consists of two algorithms:
        1. AES Counter mode encryption
        2. GMAC (Galois Message Authentication Code) is used as a MIC to ensure the integrity of messages 
        
    ![images](images/authentication-methods.png)


**WiFi protected Access (WPA)**
* Three WPA certifications for wireless devices:
    * WPA 
    * WPA2
    * WPA3
* To be WPA-certified, equipment must be tested in authorized testing lab
* All of the above support two authentication modes :
    * Personal mode: A pre-shared key (PSK) is used for authentication. when you connect to a home Wi-Fi network, enter the password and are authenticated, that is personal mode.
    This is common in small networks
        * The PSK itself is not sent over the air. A four-way handshake is used for authentication and the PSK is used to generate encryption keys 

    * Enterprise mode: 802.1X is used with an authentication server (RADIUS server)
    * No specific EAP method is specified, so all are supported (PEAP, EAP-TLS etc)
* WPA was developed after WEP was proven to be vulnerable and includes the following protocols 
    * TKIP (based on WEP) provides encryption/MIC
    * 802.1X authentication (Enterprise mode) or PSK (Personal mode)
* WPA2 was released in 2004 and includes the following protocols 
    * CCMP provides encryption/MIC
    * 802.1X authentication (Enterprise Mode) or PSK(Personal mode)

* WPA3 was released in 2018 and includes the following protocols:
    * GCMP provides encryption/MIC
    * 802.1X authentication(Enterprise mode) or PSK(Personal mode)
    * Several additional security features for example:
        * PMF(Protected Management Frames), protecting 802.11 management frames from eavesdropping/forging
        * SAE (Simultaneous Authentication of Equals) protects four way handshake when using personal mode authentication 
        * Forward Secrecy prevents data from being decrypted after it has been transmitted over the air. So, an attacker can't capture wireless frames and then try to decrypt them later 
    
**Quiz**
1. What does GMAC provide to secure wireless connection ?
    * MIC (Message Integrity Check)

2. Part of 802.1X authentication architecture :
    * Supplicant
    * Authenticator
    * Authenticator Server 

3. Most secure encryption/integrity method
    * GCMP

4. AES method that requires a certificate on both the supplicant and the AS :
    * EAP-TLS

5. WPA3 security features that protects the four-way handshake when using personal mode authentication:
    * SAE 

### Wireless Configuration
* WLCs only support static LAG (Link Aggregration Group), no PAgP or LACP 

* WLC Initial Setup 

![WLC](images/wlc.png)

* Network Name (SSID): Internal
* Configure DHCP Bridging Mode : No
* Allow Static IP addresses : yes 
* configure a RADIUS server now ?: no 
* Enter country code list : US 

* WLC ports are the physical ports that cables connect to 
* WLC interfaces are the logical interfaces within the WLC (i.e SVIs on a switch)
* WLC have a few different kinds of ports 
    * **Service port**: A dedicated management port.
        * used for out-of-band management. Must connect to a switch access port because it only supports one VLAN
        * This port can be used to connect to the device while it is booting, perform system recovery etc 

    * **Distribution System Port**: These are the standard network ports that connect to the distribution system (wired network) and are used for data traffic.
        * These portts usually connect to switch trunk ports, and if multiple distribution ports are used they can form a LAG 
    
    * Console port 
    * Redundancy port : This port is used to connect to another WLC to form a high availability (HA) pair 

* WLCs have a few different kinds of interfaces:
    * Management interface 
        * Used for management traffic such as Telnet, SSH, HTTP, HTTPS, RADIUS, authentication, NTP, Syslog etc 
        * CAPWAP tunnels are also formed to/from the WLC's management interface 
    
    * Redundancy Management Interface 
        * When two WLCs are connected by their redundancy ports, one WLC is active and other is standby. This interface can be used to connect to and manage the standby WLC 

    * Virtual interface :
        * This interface is used when communicating with wireless clients to relay DHCP requests , perform client web authentication etc 
    
    * Service port ineterface :
        * If the service port is used, this interface is bound to it and used for out-of-band management 
    
    * Dynamic interface: These are the interface used to map a WLAN to VLAN
        * For example, traffic from the internal WLAN will be sent to the wired network from the WLC's internal dynamic interface 
    
* Web Authentication :
    * After the wireless clients gets an IP address and tries to access a web page, they will have to enter a username and password to authenticate 

* Web Passthrough:
    * Similar to the above, but no username or password are required. A warning or statement is displayed and the client simply has to agree to gain access to the internet 

* The conditional and Splash page web redirect options are similar, but additionally require 802.1X layer 2 authentication

**Quiz**
1. WLC port that can be used to form an HA Pair with anothre WLC 
    * Redundancy port 

2. WLC interace type that maps a WLAN to a VLAN 
    * Dynamic interface 

3. type of Layer 3 authentication 
    * Web Authentication 

4. WLC QoS setting should be used for video traffic 
    * Gold 

5. WLC port type that can form a LAG to pass standard traffic
    * Distribution System Port 


### Network Automation
* Time consuming and very inefficiet in large scale networks 
* Difficult to ensure that all devices adhere to the organization's standard configurations 
* Networks become much more scalable. New deployments, network wide changes and troubleshooting can be implemented in a fraction of the time 
* Network wide policy compliance can be assured 
* The improved efficiency of network operations reduces the opex (operating expenses)

**Software Defined Network**
* Various functions of network devices can be logically divided up into planes :
    * **Data plane**
        * tasks involved in forwarding user data/traffic from one interface to another are part of the data plane
        * encapsulation, deencapsulations, 802.1q VLAN tags of switches 
        * NAT (changing the src/dst addresses before forwarding)
        * Deciding to forward or discard messages due to ACLs, port security etc 
        * The data plane is also called the 'forwarding plane'

    * **Control plane**
        * How a devices's data plane make its forwarding decisions ?
            * rouing table, MAC address table, ARP table, STP etc 
        * functions that build these tables (and other functions that influence the data plane) are part of the control plane 
        * The control plane controls what the data plane does for example by building the router's routing table 
        * The control plane performs overhead work
        
    * **Management plane**
        * Performs overhead work
        * Doesn't directly affect the forwarding of messages in the data plane 
        * The management plane consists of protocols that are used to manage devices 
            * SSH/Telnet used to connet to the CLI of a device to configure/manage it 
            * Syslog, used to keep logs of events that occur on the device 
            * SNMP, used to monitor the operations of the device 
            * NTP, used to maintain accurate time on the device 

    **The operations of the Mgmt and control plane are usually manged by CPU, however this is not desirable for data plane operations because CPU processing is slow, instead a specialized hardware ASIC (Application-Specific Integrated Circuit) is used. ASICs are chips built for specific purposes.**
    * MAC also called CAM table  address table stored in kind of memory called TCAM (Ternary Content-Addressable Memory) 
    * In short when a device receives control/mgmt traffic it will be processed in the CPU 
    * When a device receives data traffic which should pass through the device it is processed by the ASIC for maximum speed 

### Software-Defined Networking 
* Approach to networking that centralizes the control plane into an application called a controller 
* Also called Software-Defined Architecture (SDA) or Controller-Based Networking 
* An SDN controller centralizes control plane functions like calculating routes 
* Controller can interact programmatically with the network devices using APIs(Application Programming Interface)
* The communication between the devices and controller can be done via **Southbound Interface(SBI)**
    * SBI used for communication between the controller and the network devices it controls 
        * The devices in the network
        * The topology ( how devices are connected together)
        * The available interfaces on each device 
        * Their configurations 

    * It typically consists of a communication protocol and API (Application Programming Interface)
    * APIs facilitate data exchanges between programs 
        * Data is exchanged between the controller and the network devices
        * An API on the network devices allows the controller to access information on the devices, control their data plane tables etc 

    * Examples of SBIs:
        * OpenFlow
        * Cisco OpFlex 
        * Cisco onePK (Open Network Environment Platform Kit)
        * NETCONF
    
**Nortbound Interface(NBI)**
* Allows us to interact with the controller, access the data it gathers about the network, program it, and make changes in the network via the SBI

* A REST API is used on the controller as an interface for apps to interact with it 
    * REST => Representational State Transfer 
* Data is sent in structured (serialized) format such as JSON or XML
    * makes it much easier for programs to use the data 

![NBI](images/nbi.png)

#### Atuomation in traditional networks vs SDN
* The robust and centralized data collected by SDN controllers greatly facilitates these functions
    * The controller collects information about all devices in the network
    * Northbound APIs allow apps to access information in a format that is easy for programs to understand (i.e JSON, XML)
    * The centralized data facilitates network-wide analytics 
* Automation without the requirement of third-party scripts and apps 

**Quiz**
1. Network Automation 
    * Reduced human error 
    * Reduced OpEx

2. SBIs
    * OpenFlow
    * OpFlex

3. functions centralized in SDN
    * Calculating routes

4. Purpose of SBI in SDN architecture 
    * To facilitate data exchange between the controller and network devices

5. NTP functions fit in 
    * Management plane 
    

### JSON, XML and YAML 
**Data Serialization**
* The process of converting data into a standardized format/structure that can be stored (in a file) or transmitted (over a network) and reconstructed later(i.e by a different application)
    * allows the data to be communicated between applications in a way both applications understand 
* Allows us to represent variables with text 

**JSON**
* Javascript Objext Notation is an open standard file format and data interchange format that uses human-readable text to store and transmit data objects 
* It is standardized in RFC 8259 (https://datatracker.ietf.org/doc/html/rfc8259)
* Whitespace is insignificant
* Four primitive data types :- string, number, boolean, null
* Structured data types :- object (key-->string,:  value) and array 

**XML**
* Extensible Markup Language 
* used to format text(font, size, color, headings, etc)
* XML is generally less human-readable than JSON
* Whtiespace is insignificant
* Often used by REST APIs

* `<key>value</key>`

* `show ip interface brief | format`

**YAML**
* Yet Another Markup Language ----> YAML Aint Markup Language 
* Used by the network automation tool Ansible 
* Very human-readable 
* Whitespace is significant
    * indentation is very important 
* YAML files start with ---
* - is used to indicate a list 
* keys and values are represented as key:value 

![yaml](images/yaml.png)

### REST APIs (CRUD, HTTP verbs and data encoding)
* commonly used for Northbound interface in SDN Architecture 
* Application Programming Interface
    * software interface that allows two applications to communicate with each other 
* APIs are essential not just for network automation, but for all kinds of applications 
* In SDN architecture, APIs are used to communicate between apps and the SDN controller (via the NBI), and between the SDN controller and the network devices (via the SBI)
* NETCONF and RESTCONF are popular southbound APIs
* Uniform Resource Identifier 

![http](images/http.png)

* scheme----> authority ----> path 
* example of http request message 
* IP header ------> TCP header ----> verb ----> URI ----> Additional headers ----> Data 
* When a REST client makes an API call (request) to a REST server, it will send an HTTP request 

**HTTP Response**
* The server's response will include a status code indicating if the request succedded or failed, as well as other details 
* The first digit indicates the class of the response 
    * 1xx informational --> the request was received, continuing process
        * 102 Processing

    * 2xx successful ---> the request was successfully received, understood, and accepted 
        * 200 OK
        * 201 Created 

    * 3xx redirection --> further action needs to be taken in order to complete the request 
        * 301 Moved Permanently 

    * 4xx client error --> the request contains bad syntax or cannot be fulfilled 
        * 403 Unauthorized means the client must authenticate to get a response
        * 404 Not Found means the requested resources was not found 

    * 5xx Server Error 
        * 500 Internal Server Error means the server encountered somthing unexpected that it doesn't know how to handle 

**Characteristics of REST**
* Representational State Transfer
* REST APIs are also known as REST-based APIs or RESTful APIS
* Six constraints of RESTful architecture are:
    * Uniform interface 
    * Client-server
    * stateless
        * means that each API exchange is a separate event, independent of all past exchanges between the client and server 
        * the server does not store information about previous requests from the client to determine how it should respond to new requests 
        * If authentication is required, this means that the client must authenticate with the server for each request it makes 
        * TCP is stateful protocol 
        * UDP is stateless protocol --> data is simply sent from host A to host B 

    * Cacheable or non-cacheable
        * REST APIs must support caching of data 
        * storing data for future use 
        * improves performance for the client and reduces the load on the server 

    * Layered System 
    * Code-on-demand(optional)
    * https://developer.cisco.com/docs/dna-center/#!getting-started 
    

### SDN Networking 
* Traditional control planes use a distributed architecture 
* The controller can interact programmatically with the network devices using APIs
* The SBI is used for communications between the controller and the network devices it controls 
* The NBI is what allows us to interact with the controller with our scripts and applications 

![SDN arch](images/sdn-arch.png)

* Cisco SD-Access is cisco's SDN solution for automating campus LANs
    - ACI(Application Centric Infrastructure) is their SDN solution for automating data center networks
    - SD-WAN is their SDN solution for automating WANs 

* Cisco DNA(Digital Network Architecture) Center is the controller at the center of SD-Access

Script App   Direct via DNAC GUI then -------> REST API [DNA Center] -----> Fabric Switches 

* **Fabric** 
    * Underlay is the underlying physical network of devices and connections(including wired and wireless) which provide IP connectivity (i.e. using IS-IS)
        * Multilayer switches and their connections 
    
    * The overlay is the virtual network built on top of the physical underlay network.
        * SD-Access uses VXLAN(Virtual Extensible LAN) to build tunnels 
    
    * The fabric is the combination of the overlay and underlay, the physical and virtual network as a whole 

**Underlay**
* Purpose is to support the VXLAN tunnels of the overlay 
* Three different roles for switches in SD-Access 
    * Edge nodes : Connect to end hosts 

    * Border nodes : Connect to devices outside of the SD-Access domain i.e. WAN routers

    * Control nodes : Use LISP (Locator ID Separation Protocol) to perform various control plane functions 

* Can add SD-Access on top of an existing network (*brownfield deployment*) if our network hardware and software supports it 

* A new deployment(*greenfield deployment*) will be configured by DNA Center to use the optimal SD-Access underlay:
    * All switches are Layer 3 and use IS-IS as their routing protocol 
    * All links between switches are routed ports. This means STP is not needed
    * Edge nodes (access switches) act as the default gateway of end hosts(routed access layer)

**SD-Access Overlay**
* LISP provides the control plane of SD-Acess
    * A list of mappings of EIDs(endpoint identifiers) to RLOCs(routing locators) is kept
    * EIDs identify end hosts connected to edge switches and RLOCs identify the edge switch which can be used to reach the end host
    
* Cisco TrustSec(CTS) provides policy control (QoS, security policy, etc)
* VXLAN provides the data plane of SD-Access

**Cisco DNA Center**
* Has two main roles:
    * The SDN controller in SD-Access
    * A network manager in a traditional network(non-SD-Access)

* DNA center is an application installed on Cisco UCS server hardware 
* It has a REST API which can be used to interact with DNA center 
* The SBI supports protocols such as NETCONF and RESTCONF(as well as traditional protocols like Telnet, SSH, SNMP)
* Enables Intent-Based Networking (IBN)
    * Allows the engineer to communicate their intent for network behavior to DNA center and then DNA center will take care of the details of the actual configurations and policies on devices 
     
| Traditional Network Management   |    DNA Center-based network Management  |
| -------------------------------- |    -------------------------------------|
| Configured one-by-one via SSH or console connection | devices are centrally managed and monitored from the DNA center GUI or other applications using REST API |
| Manually configured via console connection before being deployed |  |
| configurations and policies are managed per-device. (distributed) | The administrator communicated their intended network behavior to DNA center, which changes those intentions into configurations on the managed network devices |
|    |  Configurations and policies are centrally managed |
| New network deployments can take a long time due to the manual labor required | Software versions are also centrally manged. DNA center can monitor cloud servers from new versions and then update the managed devices |
| Errors and failures are more likely due to increased manual effort | New network deployments are much quicker, New devices can automatically receive their configurations from DNA center without manual configuration | 


**Quiz**
1. Network of devices and physical connections 
    * Underlay (underlaying physical network)

2. Layer where Scripts that interact with the controller are located 
    * Application 

3. characteristic of an optimal SD-Access underlay network as configured by DNA-Center 
    * All links between switches are Layer 3

4. Protocol used to create virtual tunnels in the SD-Access overlay 
    * VXLAN 

5. Valid switch roles in Cisco SD-Access 
    * Control, Border and Edge node 


### Ansible, Puppet, Chef 
* Configuration Management Mechanisms
* Configuration drift is when individual changes made over time causes a device's configuration to deviate from the standard/correct configurations as defined by the company 
    * Although each device will have unique parts of its configuration(IP addresses, host name, etc), most of a device's configuration is usually defined in standard templates designed by the network architects/engineers of the company 
* Best to have standard configuration management practices 
    * When a change is made, save the config as a text file and place it in a shared folder 
    * example: hostname_yyyymmdd

**Configuration Provisioning**
* How config chages are applied to devices 
* Traditionally, config provisioning is done by connecting to devices one-by-one via SSH
* Config mgmt tools like Ansible, Puppet and Chef allow us to make changes to devices on a mass scale with a fraction of time/effort 
* Two essential components: templates and variables 

![config](images/config-provisioning.png)

* facilitate the centralized control of large number of network devices 
* Order is Ansible, Puppet and Chef 
* Originally developed to enable server system admins to automate the process of creating, configuring and removing VMs

* These tools can be used to perform tasks like : 
    1. Generate configurations for new devices on a large scale
    2. Perform configurations changes on devices (all devices in our network, or a certain subset of devices)
    3. Check device configurations for compliance with defined standards 
    4. Compare configuration between devices and betweek different versions of configurations on  the same device 

**Ansible**
* Configuration management tool owned by Red Hat 
* Written in Python
* Agentless 
    * doesn't require any special software to run on the managed devices 

* Ansible uses SSH to connect to devices, make configuration changes, extract information, etc 
* Uses push model. The Ansible server(control node) uses SSH to connect to managed devices and push configuration changes to them   
    * Puppet and chef use a pull model

* Playbooks: are the files (blueprints of automation tasks)
    * They outline the logic and actions of the tasks that Ansible should do. Written in YAML 
    * Inventory: list of devices that will be managed by Ansible as well as characteristics of each device such as their device role(access switch, core switch, WAN router, firewall etc). Written in INI, YAML or other formats 
    * Templates: Device config files, but specific values for variables are not provided written in Jinja2 format 
    * Variables: files list variables and their values. These values are substituted into the template to create complete configuration files. Written in YAML 

**Puppet**
* written in Ruby 
* Agent based 
    * Specific software must be installed on the managed devices 
    * Not all cisco devices support a Puppet agent 
* Can be run agentless, in which a proxy agent runs on an external host, and the proxy agent used SSH to connect to the manged devices and communicate with them 
* The puppet server is called " the pupper master"
* Used Pull model (clients 'pull' configurations from the puppet master)
    * clients use TCP 8140 to communicate with the Puppet master 
* Instead of YAML, it uses a proprietary language for files
* *Manifest*: defines the desired config state of a network device
* *Templates*: similar to Ansible templates. Used to generate Manifests 

**Chef**
* written in Ruby
* Agent-based 
    * Specific software must be installed on the managed devices
    * Not all cisco devices support a chef agent 
* Chef use a pull model 
* The server uses TCP 10002 to send configurations to clients 
* Files use a DSL(Domain-Specific Language) based on Ruby 
* Text files used by chef include:
    * *Resources*: Configuration objects managed by chef (ingredients in a recipe)
    * *Recipes*: outline the logic and actions of the tasks performed on the resources (recipes in a cookbook)
    * *Cookbooks*:A set of related recipes grouped together 
    * *Run-list* :An ordered list of recipes that are run to bring a device to the desired configuration state 

![config-mgmt](images/config-mgmt.png)

**Quiz**
1. Config mgmt tools that connect to devices using SSH 
    * Ansible 

2. Tools that use pull model
    * Chef and Puppet

3. Client-server model 
    * chef, ansible and puppet 

4. written in Ruby
    * puppet and chef

5. uses playbooks
    * Ansible

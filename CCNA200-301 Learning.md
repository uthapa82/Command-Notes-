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
    






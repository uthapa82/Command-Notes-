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
    - IPv4 ->4 -> 0100
    - IPv6 = 6  (0110)
**Internet Header Length (IHL) => 4 bits in length**
    - Min value is 5 (20 bytes)
    - Maximum value is 15 (15 * 4 bytes = 60 bytes)
    - Min IPv4 Header length = 20 bytes 
    - Max IPv4 Header length = 60 bytes 

**DSCP Differentiated Services Code Point => 6 bits in Length**
    - Used for QoS(Quality of Service)
    - Used to prioritize delay-sensitive data(streaming voice, video etc)

**ECN => 2bits in length**
    - Explicit Congestion Notification
    - Provides end-to-end(betwn two endpoints) notification of network congestion without dropping packets 
    - Optional feature that requires both endpoints, as well as the underlying network infrastructure, to support it
    
**Total length=> 16bits**
    - Indicates the total length of the packet(L3 header + L4 segment)
    - Measured in bytes (not 4 byte increments like IHL)
    - Minimum value of 20 (=IPv4 header with no encapsulated data)
    - Maximum value of 65,535 (maximum 16-bit value)
    <span style="color:red"> Maximum value of IPv4 packet 65,535</span> 

**Identification 16 bits in length**
    -If a packet is fragmented due to being too large, this field is used to identify which packet the fragment belongs to.
    - All fragments of the same packet will have their own IPv4 header with the same value in this field 
    - Packets are fragmented if larger than the MTU (Maximum Transmission Unit)
    - **MTU is 1500 bytes**
    - Fragments are reassembled by the receiving host 

**Flags 3 bits**
    - Used to control/identify fragments
    - Bit 0: Reserved, always set to 0
    - Bit 1: Don't Fragment(DF bit), used to indicate a packet that should not be fragmented 
    - Bit 2: More Fragments(MF bit), set to 1 if there are more fragments in the packet, set to 0 for the last fragment 
    - <span style="color:red"> Unfragmented packets will always have their MF bit set to 0 </span> 

**Fragment Offset 13 bits in length**
    - Used to indicate the position of the fragment within the original, unfragmented IP packet 
    - Allows fragmented packets to be reassembled even if the fragments arrive out of order
    
**Time to Live = 8 bits**
    - A router will drop a packet with a TTL of 0
    - Used to prevent infinite loops 
    - Originally designed to indicate the packet's maximum lifetime in seconds 
    - In practice, indicates a "hop count" : each time the packet arrives at a router, the router decreases the TTL by 1
    - <span style="color:red"> The current recommended default TTL is 64 </span> 

**Protocol 8 bits in length**
    - indicated the protocol of the encapsulated L4PDU
    - value of 6 :TCP
    - value of 17: UDP
    - value of 1: ICMP
    - value of 89: OSPF (dynamic routing Protocol)
   
**Header checksum => 16 bits**
    - A calculated checksum used to check for errors in the IPv4 header 
    - When a router receives a packet, it calculates the checksum of the header and compares it to the one in this field of the header 
    - if they don't match the router drops the packet 
    - Used to check for errors only in the IPv4 Header 
    - IP relies on the encapsulated protocol to detect errors in the encapsulated data
    - Both TCP and UDP have their own checksum fields to detect errors in the encapsulated data 

**Source IP address**
**Destination IP address**
    - 32 bits each
    - IPv4 of the sender and receiver 

**Options(if IHL > 5)**
    - length 0 - 320 bits 
    - rarely used 
    - if the IHL field is greater than 5, it means that options are present

`R1# ping 192.168.1.2 size 10000`
`R1# ping 192.168.1.2 df-bit`
**df - don't fragment bit**







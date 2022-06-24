## CCNA Notes- Jeremy IT Lab
#### Switch Interfaces 
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

`R1(config)#do sh ip int br `
-

`R1# sh int description`
`R1# int g0/0`
`R1#(config-if)# description (our note) to SW!`
-









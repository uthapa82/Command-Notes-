### F5 Network Firewall Concepts 

* Network Firewall object has all network security configuration for a site and serves as a snigle place to configure all infrastructure security. A fleet object also has a network firewall object that can be used to simplify configuration of network firewall across many sites in a fleet. 

    1. Network Policy Set:
        * Are applied to all physical networks on the sites within fleet.

    2. Service Policy Set 
        * Are applied to all forward proxies and dynamic reverse proxies in all the network connectors in the fleet.

    3. Fasts ACLs
        * Are list of rules that are not session-based and applies to all packets 
        * These rules are also used to mitigate DOS attacks 

* Deploy and Configure Firewall 

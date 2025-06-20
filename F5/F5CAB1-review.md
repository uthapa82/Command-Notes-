- TCP 4353 allows BIG-IP DNS deployments to transfer sync-group data
- UDP 1026 Network Failover 
- Allow All
- Allow None : no connections are allowed on the self ip address, regardless of protocol or service, ICMP traffic is always allowed, for redundant pair, ports listed as exceptions are always allowed from the peer system 
- Allow Custom 
- Virtual Servers in BIG-IP can be thought of as a ears which listens for inbound connection and we can configure our Virutal Servers to load balance or analyze traffic and forward that to pool which consists of varios members.


**Command Review**

```bash
    tmsh list net self-allow 


```
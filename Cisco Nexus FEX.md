### Cisco Nexus Fabric Extender (FEX)

* Fex configuration 
`
    n9K-1# config

    n9K-1(config)# sh int stat

    n9K-1(config)# feature fex

    n9K-1(config)# int range ..

    n9K-1(config)# description FEX ..

    n9K-1(config)# channel-group 100 (or whatever number)
    
    n9K-1(config)# int po100
`
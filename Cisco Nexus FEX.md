### Cisco Nexus Fabric Extender (FEX)

* Initial Configuration
    * ssh key rsa 2048 
```
    n9K-1(config)# write erase 

    n9K-1(config)# sh run 

    n9K-1# sh version | ex Processor 

    n9K-1# sh module | end sw

    n9K-1(config)#
```
*  Fex configuration 
```
    n9K-1# config

    n9K-1(config)# sh int stat

    n9K-1(config)# feature fex

    n9K-1(config)# int range ..

    n9K-1(config)# description FEX ..

    n9K-1(config)# channel-group 100 ((1-199))

    n9K-1(config)# int po100
```
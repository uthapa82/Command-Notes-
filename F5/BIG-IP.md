### F5 Network Firewall Concepts 

* Network Firewall object has all network security configuration for a site and serves as a snigle place to configure all infrastructure security. A fleet object also has a network firewall object that can be used to simplify configuration of network firewall across many sites in a fleet. 

    1. Network Policy Set:
        * Are applied to all physical networks on the sites within fleet.

    2. Service Policy Set 
        * Are applied to all forward proxies and dynamic reverse proxies in all the network connectors in the fleet.

    3. Fasts ACLs
        * Are list of rules that are not session-based and applies to all packets 
        * These rules are also used to mitigate DOS attacks 

* Packet Filtering Firewalls 
    - Basic IP address / TCP/UDP port checking 

* Stateful Firewalls
    - 3 way Handshake 
    - Establish session and allows the return communication
    - Monitors the traffic 

* Application Firewalls
    - OSI 5-7 Application Layer 
    - Rules based on application eg. block facebook games, allow facebook chat

**Securing BIG-IP**
* Securing Management-IP
    - [Overview of securing access to the BIG-IP system](https://my.f5.com/manage/s/article/K13092)
    - [Restrict access to the BIG-IP Management](https://my.f5.com/manage/s/article/K17333)
* [Port Lockdown](https://my.f5.com/manage/s/article/K17333)
* [Firewall Rules  for Self-IPs](https://my.f5.com/manage/s/article/K94615110)
* [Configure DDOS Vectors](https://my.f5.com/manage/s/article/K41305885) 
* [Secure Password Policies](https://my.f5.com/manage/s/article/K15497) 
* [SSHD ACLs](https://my.f5.com/manage/s/article/K5380)
* [HTTPd ACLs]()
* [Ask F5 : Secure your BIG-IP system](https://www.youtube.com/playlist?list=PLDhImkjf0UJzs0EsVLOccp-xBaWYRdTJ2)

*Notes*
* SSHd and HTTPd
    - Daemon processes - background services that run on Unix/Linux systems to handle specific types of network communication. 
    - SSH Daemon : handles incoming SSH connections, allows remote users to log into the system securely, execute command and transfer files (via SFTP or SCP)
    - `systemctl status  sshd`

    - httpd: handles Http/Https web traffic, serves web pages and responds to web browser or API requests. Common implementations: Apache (httpd), Nginx(nginx)


* Configure DDOS Vectors
    - Specific detection and mitigation settings for various DDoS attack types 
    - Protection at multiple layers:
        - Device-level DDoS (protects the system itself)
        - Virtual server-level DDoS (protects specific apps/services)
        - Self IP or route domain-level (used in large environments)
    - Create a DoS Protection Profile 
    - Enables vectors based on risk:
        - SYN flood
        - UDP flood
        - ICMP flood
        - DNS query flood
        - HTTP GET/POST flood
    - Set detection thresholds (pps or bps)
    - Set mitigation thresholds and Rate limits 
    - Optionally enable auto-blacklisting, IP intelligence, or device learning 
    - Add or edit a protected object 
        - Type: Virtual Server 
        - Assign the DoS profile 
        - Define direction (client-side or server-side)
        - Apply thresholds 
    - Configure vectors to protect management services (HTTPd, SSHd)

    *Important Best Practices*
    - Set thresholds based on baseline traffic measurements 
    - Use IP intelligence to block known bad sources 
    - Always protect Self IPs and management interfaces 
    - Enable logging for auditing and tuning

**Identify management connectivity configurations**
* [Identify the configured management-IP address](https://my.f5.com/manage/s/article/K15040)
    - [IPv4 and IPv6 dual-stack support for the management interface](https://my.f5.com/manage/s/article/K12430)
    - [Overview od the management interface (port)](https://my.f5.com/manage/s/article/K7312)
    - [Overview of management interface routing](https://my.f5.com/manage/s/article/K13284)
* [Interpret port lockdown settings to Self-IP](https://my.f5.com/manage/s/article/K000141066)
     
    *Notes*

    The port lockdown setting is to allow connections to “terminate” on the individual Self-IPs. This is only useful for a few scenarios like – connecting to the self IPs as mgmt interfaces (a big no-no), iQuery® traffic, HA / Sync traffic, IPSEC termination endpoints. If you don’t have any of those going on, then you should always set your port lockdown settings to “allow-none”.
    
    - [Setting a self IP to "Allow None" for Port Lockdown will disrupt HA](https://my.f5.com/manage/s/article/K14666670)

* Show remote connectivity to the BIG-IP Management interface
    
    ```bash
    [root@BIG-IP] config # ip addr show mgmt 

    # from tmos shell 
    [root@BIG-IP] config (tmos)# tmsh list sys management-ip

    # check listening services 
    [root@BIG-IP] config # netstat -an | grep -E ':(22|443).*LISTEN'
    sample output:
    tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN     
    tcp6       0      0 :::22                   :::*                    LISTEN     
    tcp6       0      0 :::443                  :::*                    LISTEN 

    # check from a remote host 
    $ ping  <mgmt-ip>
    $ ping6 <ipv6-mgmt-ip>
    > ping -g <ipv6-mgmt-ip> # windows

    # netcat to read/write network connection, 
    # -z flag : zero I/O mode (i.e don't send data just check if the port is open)
    # -v flag : verbose output (shows success/failure messages)
    $ nc -zv <mgmt-ip> 22
    $ nc -zv <mgmt-ip> 443
    
    # powershell on windows 
    Test-NetConnection -HostName <mgmt-ip> -Port 443
    ```

    *Notes*
    
    [The BIG-IP management interface may lose connectivity when using fixed media settings](https://my.f5.com/manage/s/article/K14579)

    tmsh list /net interface mgmt media-active 

    output showing that the interface is set to half duplex:

    net interface mgmt { 
        media-active 100TX-HD
    }

    **workaround**

    - Connect the serial console to the CONSOLE port of the BIG-IP system 
    - Display the SCCP or AOM command menu, type the following key sequence
        - Esc ( 
    - Press 1 to select 'Connect to the Host subsystem console'
    - Log in to the command line of the BIG-IP system 
    - Set the management interface to use auto-negotiation, type the following command:
        - tmsh modify /net interface mgmt media auto 
        - tmsh save /sys config 
        - reboot 
    - when the BIG-IP system is back to an active state, change the media setting from auto to the desired fixed setting 
        - tmsh modify /net interface mgmt media <Desired-Fixed-Media-Setting>
        eg : tmsh modify /net interface mgmt media 100TX-FD
    - tmsh save /sys config 

* Explain management IP connectivity issue
    - check the log for accepted/denied SSH connections 

        `config # tailf /var/log/secure`

    - cleanup 

        ` tmsh modify /sys sshd allow replace-all-with { ALL }`

    *Common reasons for Management IP connectivity issues*
    - Firewall blocking access to mgmt-ip
    - Wrong subnet mask or gateway configuration on the BIG-IP
    - Device is on a different VLAN or broadcast domain.
    - SSH/HTTP services are disabled or access control limits are in place.

    ```bash 
    # ping test 
    ping <mgmt-ip>
    #tcpdump utility on BIG-IP
    tcpdump -ni mgmt port 22 or port 443
    ```

* Identify HTTP/SSH access list to management-IP address
    ```bash
    [root@BIG-IP] config # tmsh list /sys sshd allow 
    [root@BIG-IP] config # tmsh modify /sys sshd allow replace-all-with { 10.1.1.1/32 }
    [root@BIG-IP] config # tmsh save /sys config 

    # HTTPD ACLs
    [root@BIG-IP] config # tmsh list /sys httpd allow 
    [root@BIG-IP] config # tmsh modify /sys httpd allow replace-all-with { 10.1.1.1/32 }
    [root@BIG-IP] config # tmsh save /sys config 

    [root@BIG-IP] config # tailf /var/log/secure 

    # Revert any changes made to the ACLs
    [root@BIG-IP] config # tmsh modify /sys sshd allow replace-all-with { ALL }
    [root@BIG-IP] config # tmsh modify /sys httpd allow replace-all-with { ALL }
    [root@BIG-IP] config # tmsh save /sys config 
    ```


**Explain the processes of licensing, license reactivation, and license modification**
* [Show where to license (activate.F5.com)](https://my.f5.com/manage/s/article/K7752)
    - [Using tmsh to view and manage licenses](https://my.f5.com/manage/s/article/K15055)
* [Identify license issues](https://my.f5.com/manage/s/article/K9245#device)
    - License Expiration or Activation Failure 
        - Expired trial or evaluation license 
        - The system time is incorrect (affects license validation)
        - Connectivity issues preventing contact with F5's license servers
        - Activation with incorrect or invalid license key 

    - Mismatched Hardware/Software License 
        - Attempting to use a license ties to a different hardware platform (eg. moving a license from a physical device to a virtual instance like BIG-IP VE).
        - Licensing a VE instance with a key meant for a different platform size (e.g. 10 Mbps vs 1 Gbps)

    - Overprovisioning or Feature Access Issues
        - The license does not include the desired modules or their usage limits.
        - Some features may require additional licensing (e.g. SSL, TPs, DNS queries etc)
    
    - Clustering (HA) and License Compliance 
        - Licenses between devices in an HA pair are not consistent 
        - F5 requires similar module sets and platform types for a proper HA configuration.

    - License Transfer Issues
        - Attempting to move a license without proper release/deactivation
        - Changing MAC addresse or hardware IDs (UUIDs) in VE environments without updating the license.

    - EULA and Compliance Violations 
        - Using a single license accross multiple instances.
        - Running beyond the licensed bandwidth or TPS
        - Modifying license files or circumventing the license system.
    
    **How to Troubleshoot or Resolve Licensing Issues**
    - Use F5's License Activation Portal
    - Check logs: `/var/log/ltm` and `/var/log/daemon.log` for licensing related errors
    - `tailf or tail -n 10 or grep "10:30" /var/log/ltm | tail -n 10`

* [Identify Service Check Date (upgrade)](https://my.f5.com/manage/s/article/K7727)
    - System upgrade : moving from one major release to another major release, or moving from a minor release to another minor release.
    - System update : moving from one maintenance release or point release to anotehr maintenance or point release within the same major and minor version.
    
    Example:
    moving from 15.1.0 to 16.0.0 is considered an upgrade as the major version number changed, moving from 14.0.0 to 14.1.0 is also an upgrade as the minor version number changed. Moving from 15.0.1 to 15.0.2 (maintenance release) or 15.1.2 to 15.1.2.1(point release) is considered an update.

    - Service check date is located in the BIG-IP license and is the same as the date the license was last activated or the date the service contract for the device expires, whichever date is earlier. 

    - License check date - `/etc/version_date`
    - `tmsh show sys license | grep "Service Check Date"`


* [Show provisioned modules – CLI/config/TMUI](https://my.f5.com/manage/s/article/K36854345)
    - [Provision licensed BIG-IP modules](https://my.f5.com/manage/s/article/K12111)
    
    - `tmsh show sys provision or ltm or ltm afm ` 

    - ` cat /config/bigip_base.conf | grep provision`

    - System --> Resource Provisioning
    - "Nominal" - Normal/default setting -module uses standard resource allocation - Most common in production environments.
    - "dedicated" - Module gets maximum resources, and usually disable others - For modules like vCMP, SSLO, or FPS, where full control is needed


* Report modules which are licensed for – CLI/config/TMUI
    
    ```bash
    # tmsh 
    tmsh show sys license 

    # config 
    cat /config/bigip_base.license 

    # TMUI
    System ---> License 
    ```

**Apply procedural concepts required to manage software images**
* [Given an HA pair, describe the appropriate strategy for deploying a new software image](https://my.f5.com/manage/s/article/K80173021)
    - [F5 Upgrade and Update Guide](https://my.f5.com/manage/s/article/K84205182)
    - [F5 Upgrade and Update Guide-PDF](https://www.f5.com/pdf/deployment-guides/bigip-update-upgrade-guide.pdf)
    - [Update or Upgrade BIG-IP HA systemms using the Configuration Utility](https://my.f5.com/manage/s/article/K15134306)
    - `tmsh load sys config verify` to ensure no config issues prior to upgrade 

- TCP 4353 allows BIG-IP DNS deployments to transfer sync-group data
- UDP 1026 Network Failover 
- Allow All
- Allow None : no connections are allowed on the self ip address, regardless of protocol or service, ICMP traffic is always allowed, for redundant pair, ports listed as exceptions are always allowed from the peer system 
- Allow Custom 
- Virtual Servers in BIG-IP can be thought of as a ears which listens for inbound connection and we can configure our Virutal Servers to load balance or analyze traffic and forward that to pool which consists of varios members.
- DoS vectors 
    - Mitigate (watch, learn, alert, and mitigate)
    - Detect-only (watch, learn, and alert)
    - Learn-Only (collect stats, no mititgation)
    - Disabled (no stat collection, no mitigation)
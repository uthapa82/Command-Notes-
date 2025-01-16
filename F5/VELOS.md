### F5 VELOS Setup

* Console to the System Controller and ensure that we are in Active SC 
    - Option 1 check LED on SC 
    - Option 2 Access the AOM menu:
        - On telnet, hold ESC shift and 9 

        ![AOM image](../images/AOM-Menu.png)

* Change admin password and root password 
* Run VELOS setup wizard 

    `[root@controller-1 ~]# velos-setup-wizard`

* Setup management IP addresses for controller 1 -x.x.x.x
* Setup management IP addresses for controller 2 -x.x.x.x
* Setup Floating Management IP address- x.x.x.x

#### Chassis Partition 
* Virtual System 

![Partition](../images/partition.png)

* System Upgrades : before proceeding we need to make sure that we are running the latest version of the software in order to do that we can navigate to downloads.f5.com and Find latest version of F5OS

* F5 recommends that we upgrade the system controllers first then update the version of the software on the chassis partition 

* Access the velos system using the floation IP address configured above.

* Command to find floating IP via cli 

    `syscon-2-active# show system mgmt-ip`

* After uploading the software image to controllers and partition it may take some time at images are replicated on the standby controller once it's completed on the active system controllers.
    - From UI: Software Management >> Partition Images and Controller Images 

#### Backing up F5OS Configurations 

* To completely backup each tenant's we need to bacup
    - Each TMOS configuration 
    - Each F5OS chassis partition configuration 
    - F5OS system controller cofiguration 

* At chassi level F5OS configuration data includes dns, ntp server setting, Auth server , logging, HA, product licensing etc 
    - System Setting >> configuration Backup >> Name ___ >> create 

* During Rolling upgrade process the standby controller is upgraded first and when completed the standby contoller goes into the active state and the second controller will then be upgraded.

#### TMOS Tenant Deployment 
* System Controller User 
    - Creates and manages chassis partitions 
    - Configure management interfaces 
    - Install system controller level software 
    - Modify syste settings, activate licensing 
    - Set up high availability for the two system controllers
    - Perform user management for the system controllers 

* Chassis Partition User 
    - Create and Manage VLANs
    - Create and manage LAGs
    - Manage interfaces 
    - Manage port groups, as needed 
    - Displays VLAN listeners, if necessary 

* Command to show chassis partition redundancy 

    `Partition-1# show system redundancy `

* Chassis partition administrator is responsible for configuring tenant deployments within chassis partition 

* A tenant is a guest system running software in a chassis partition. we can run several tenants within same chassis partition. Each blade has 128G  (CX410) of memory of which 95G is reserved for tenant. **Max number of lightweight tenants that can be cleared on a single blade is 22**

* A big-ip tenant on the velos is  managed similarly how vCMP guest is managed in VIPRION. Can connect via CLI, RESTAPI or GUI

* A tenant is assigned dedicate VCP and memory resources and is restricted to specific VLANs for network connectivity.



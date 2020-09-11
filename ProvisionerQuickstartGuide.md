# CORTX-Provisioner Quick Start Guide

This guide provides a step-by-step walkthrough for getting you CORTX-Provisioner ready.

- [1.0 Hardware Checklist](#10-Hardware-Checklist)
- [1.1 Prerequisites](#11-Prerequisites)
- [1.2 Known Issues and Limitations](#12-Known-Issues-and-Limitations)
- [1.3 Dual Node Setup on Hardware](#13-Dual-Node-Setup-on-Hardware)
- [1.4 Teardown CORTX](#14-Teardown-CORTX)
- [1.5](#15-Virtual-Machines)
- [1.6 Set up the S3client](#16-Set-up-the-S3client)
- [1.7 Additional Resources](#17-Additional-Resources)

## 1.0 Hardware Checklist

Ensure that you have the correct hardware cofiguration before moving on to the prerequisites section.

<details>
  <summary>Click to expand!</summary>
  <p>
    
* [x] Ensure that you are on a Centos 7.7.1908 Operating System.                                                                                                                 

  :page_with_curl: **Notes:** 
  - Install the vanilla OS for Centos 7.7.1908 release. 
  - Ensure you have root credentials.                                      
                                
* [x] Network:                                                                                                
  
  - Single node CORTX setup: Not Applicable
  - Dual node CORTX setup: Ensure that uniform network interfaces are available on both the nodes. 
  
    **Example:** If there are eth0 and eth1 interfaces available on node1, they should be available and have the same names and properties on node2 (subnet, mtu, etc.)  

* [x] Hardware Configuration and Storage:                                                                             
  
  - You'll need a minimum of 2GB space under /opt/ directory or partition.
  - A minimum of two LUNs should be available from the storage controller or two raw disks locally available on the system (one for metadata and one for data.) 
  
* [x] Miscellaneous:
  
- Ensure that your hardware is connected to cortx-storage.colo.seagate.com.
- You'll need internet connectivity to download and install third-party open source softwares. 
- You'll have to open the following ports:                                                                                                
  - 80 haproxy
  - 443 haproxy
  - 8100 CSM        
  
* [x] Disable SE Linux.  

  Follow these steps to disable SE Linux:  
  
  1. Run `$ vi /etc/selinux/config`
  2. Configure SELINUX=disabled in the /etc/selinux/config file using `$ vi /etc/selinux/config` 
  3. Set SELINUX=disabled 
    
      ```shell
    
          # This file controls the state of SELinux on the system.
          # SELINUX= can take one of these three values:
          #     enforcing - SELinux security policy is enforced.
          #     permissive - SELinux prints warnings instead of enforcing.
          #     disabled - No SELinux policy is loaded.
          SELINUX=disabled
          # SELINUXTYPE= can take one of three two values:
          #     targeted - Targeted processes are protected,
          #     minimum - Modification of targeted policy. Only selected processes are protected.
          #     mls - Multi Level Security protection.
          SELINUXTYPE=targeted
      ```  
   4. Restart your node using: 
    
      `$ shutdown -r now`
    
   5. After rebooting your system, confirm that the getenforce command returns a `Disabled` status:
    
      `$ getenforce`
    
* [x] Provision your Controller: ensure that the storage controller attached to the servers is correctly configured with pools and volumes. 

  The Base Command to provision your controller is:
  
  	`./controller-cli.sh host -h '<controller host>' -u <username> -p '<password>'`
  
  	Where:
	- -h: hostname or IP address of controller
  	- -u: Username of the controller
  	- -p: Password for the Username 

   **Usage:** The code below shows the syntax for using the base command: 
    	
	 ```shell
	
	 ./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin'
	 ```
	
	 or 
	
	 ```shell
	
	 ./controller-cli.sh host -h '192.168.1.1' -u admin -p '!admin'
	 ```

   **Basic Commands**
   
   1. For help, use:
   
      `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' -h | --help`
	
   2. To view the details of the provisioning setup present on your storage enclosure, use:
   	      
      `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' prov -s | --show-prov`
   
   **Controller provisioning**
   
     You can provision the cotroller in two ways:
      
    1. Standard Provisioning: to provision the controller on 'host.seagate.com' with a standard configuration of 2 linear pools with 8 volumes per pool, use:
       
       `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' prov [-a | --all]`

    2. Custom Provisioning:
       
       1. To provision the controller on 'host.seagate.com' with adapt linear pool dg01, disks range in 0.0-41, and default 8 volumes, use:
       
       	  `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' prov -t linear -l adapt -m dg01 -d 0.0-41`
       2. To provision the controller on 'host.seagate.com' with raid5 virtual *pool a*, disks range in 0.42-83, and default 8 volumes, use:
       
           `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' prov -t virtual -l r6 -m a -d 0.42-83`
       3. To provision the controller on 'host.seagate.com' with raid5 virtual *pool b*, disks range in 0.0-9, and 6 volumes, use:
            
	    	`./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' prov -t virtual -l r5 -m b -d 0.0-9 -n 6`
	    
	     	:page_with_curl: **Notes:** 
	        	- -t,-l,-m,-d flags are necessary for custom controller provisioning.
	 		- Supported Custom Parameters:
	   			- Pool-types: linear and virtual
	   			- Levels: r1,r5,r6,r10,r50, and adapt
	   			- Pool-names for virtual pools: a and b 
	   			- Number of volumes: 1,2,3,4,5,6,7, and 8

   4. To eliminate existing standard or custom provisioning on 'host.seagate.com' controller, use:
   
      `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' prov [-c | --cleanup]`
   
   5. To eliminate existing standard or custom provisioning on 'host.seagate.com' controller, and set up standard provisioning on it, use:
   
      `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' prov [-c | --cleanup] [-a | --all]`
      
       :page_with_curl: **Note:** You can use the cleanup flag with custom provisioning.
    
    6. To view the details about available disks on storage enclosure, use:
    
       `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' [-s|--show-disks]`
    7. Print storage enclosure serial number, firmware version, and license details present on controller 'host.seagate.com'
   
       `./controller-cli.sh host -h 'host.seagate.com' -u admin -p '!admin' --show-license` 
    
    </p>
    </details>

## 1.1 Prerequisites

<details>
 <summary>Before you begin</summary>
 <p>
  
  1. Verify and ensure that the IPMI is configured and BMC IPs are assigned on both nodes.
  2. Ensure that you've installed RHEL 7.7 OS and the kernel version is 3.10.0-1062.el7.x86_64. 
  3. Run command `lsb_release -a` to verify that the LSB Module is installed.
  4. Make sure that direct network connection is established between two server nodes for private data network.   
  5. Ensure that SAS connections from controllers to servers are not cross connected.
  6. Verify that the pools and volumes are created on controller and mapped to both the servers. You can do this via controller web-interface from the *Mappings* page.  
  7. For Provisioner to deploy successfully on RHEL servers, you'll need to either enable or disable the subscription manager with standard RHEL and RHEL HA licenses. You'll need to run the CORTX prerequisite script based on the enabled or disabled licenses on your systems. If you are on a CentOS RedHat system, you can directly run the prerequisite script. 
     1. You'll need to check whether licenses are enabled on both the servers. To do that, verifying if the subscription manager is enabled by using:

         ```shell
            $ subscription-manager list | grep Status: | awk '{ print $2 }' 
            && subscription-manager status | grep "Overall Status:" | awk '{ print $3 }'
         ```
          
        **Output**
          
          ```shell
          
            Subscribed  
            Current  
          ```     
          
        - If you get the above output message, then subscription manager is enabled on your system. 
        - If you do not get the above output message, subscription manager is disabled. 
          - Run the following prerequisite script to enable the subscription manager: 
          `$ curl https://raw.githubusercontent.com/Seagate/cortx-prvsnr/Cortx-v1.0.0_Beta/cli/src/cortx-prereqs.sh?token=APVYY2OPAHDGRLHXTBK5KIC7B3DYG -o cortx-prereqs.sh; chmod a+x cortx-prereqs.sh; ./cortx-prereqs.sh --disable-sub-mgr`   
      
     2. Verify if High Availability license is enabled on your system by using:   
      
          `$ subscription-manager repos --list | grep rhel-ha-for-rhel-7-server-rpms`
          
          **Output**
          
          `Repo ID:   rhel-ha-for-rhel-7-server-rpms`
     
        - If the High Availability repository is listed in the output message above, then the High Availability license is also enabled. 
        - If you do not see the High Availability repository listed in the output message, then the High Availability license is not enabled on your system. You can do any one of the following:
          1. Get the High Availability license enabled on both nodes by your Infrastructure team.  
          2. Deploy CORTX with subscription manager disabled on both nodes.   

     3. You can deploy CORTX with or without the subscription manager. Before you deploy CORTX, ensure that: 
        1. You've installed mellanox drivers on both nodes. To install mellanox, run the CORTX prerequisite script:
           
           `curl https://raw.githubusercontent.com/Seagate/cortx-prvsnr/main/cli/src/cortx-prereqs.sh?token=APVYY2KMSGG3FJBCA73EUZC7B3BYG | bash -s`   
         - Once the Mellanox Drivers are installed, your system will reboot. 
         - If Mellanox Drivers are installed on your system already, the system will not reboot. 
        2. Seagate internal repositories are set up manually from `/etc/yum.repos.d/`.
     
        Run the CORTX prerequisite script:
         
         1. To deploy CORTX with the subscription manager enabled, use:   
             1. Navigate to [GitHub](https://github.com/Seagate/cortx-prvsnr).
             2. Select the *DEV* or *BETA* branch. For the latest code, select the *release* branch. 
             3. Click cli and navigate to src.
             4. Click cortx-prereqs.sh and view the RAW file. 
             5. Copy the token link for your Prerequisite script.   
             
                `curl https://raw.githubusercontent.com/Seagate/cortx-prvsnr/main/cli/src/cortx-prereqs.sh?token=APVYY2KMSGG3FJBCA73EUZC7B3BYG | bash -s`   

         2. To deploy CORTX with the subscription manager enabled, use:
        
            `curl https://raw.githubusercontent.com/Seagate/cortx-prvsnr/Cortx-v1.0.0_Beta/cli/src/cortx-prereqs.sh?token=APVYY2OPAHDGRLHXTBK5KIC7B3DYG -o cortx-prereqs.sh; chmod a+x cortx-prereqs.sh; ./cortx-prereqs.sh --disable-sub-mgr`   
      
         :page_with_curl: **Notes:** 
         - You'll need to generate your own tokens for Dev, Release and Beta Builds.
         - To deploy the Beta build, replace `main` with `Cortx-v1.0.0_Beta` in the token url.     

 </p>
 </details>

## 1.2 Known Issues and Limitations

   Before running the commands, please go through the list of [Known Issues for dual node setup](https://github.com/Seagate/cortx-prvsnr/wiki/deploy-eos)

   :warning: **Limitation:** [Single node hardware setup](https://github.com/Seagate/cortx-prvsnr/wiki/Single-node-setup) is currently not supported by CORTX-Provisioner. 
  

## 1.3 Dual Node Setup on Hardware

   1. Ensure that you meet the [Prerequisites](#11-Prerequisites) and your systems are rebooted.  
   2. Make sure that all the volumes or LUNs mapped from the storage enclosure to the servers, are visible on both the servers. 
   3. Run command: 
   
      `$ lsblk -S|grep SEAGATE`

<details>
 <summary>View Output</summary>
 <p>
    
 ```shell
    
    [root@sm10-r20 ~]# lsblk -S|grep SEAGATE
    sda  0:0:0:1    disk SEAGATE  5565             G265 sas
    sdb  0:0:0:2    disk SEAGATE  5565             G265 sas
    sdc  0:0:0:3    disk SEAGATE  5565             G265 sas
    sdd  0:0:0:4    disk SEAGATE  5565             G265 sas
    sde  0:0:0:5    disk SEAGATE  5565             G265 sas
    sdf  0:0:0:6    disk SEAGATE  5565             G265 sas
    sdg  0:0:0:7    disk SEAGATE  5565             G265 sas
    sdh  0:0:0:8    disk SEAGATE  5565             G265 sas
    sdi  0:0:1:1    disk SEAGATE  5565             G265 sas
    sdj  0:0:1:2    disk SEAGATE  5565             G265 sas
    sdk  0:0:1:3    disk SEAGATE  5565             G265 sas
    sdl  0:0:1:4    disk SEAGATE  5565             G265 sas
    sdm  0:0:1:5    disk SEAGATE  5565             G265 sas
    sdn  0:0:1:6    disk SEAGATE  5565             G265 sas
    sdo  0:0:1:7    disk SEAGATE  5565             G265 sas
    sdp  0:0:1:8    disk SEAGATE  5565             G265 sas
    [root@sm10-r20 ~]# 
  ```
    
</p>
</details>

   :page_with_curl: **Notes:** 
    
   - If you do not see the disk devices listed, as shown in the oputput above, please don't proceed. 
   	- Try rebooting the servers and check again. 
   - If you still do not see the disks listed, contact your infrastructure team. 
   
   4. You'll need to deploy CORTX. 

      1. Follow these steps to auto-deploy 2-node Cortx cluster on hardware using a single command.   
   
   <details>
   <summary>Prerequisites</summary>
    <p>	
    
   1. Refer to the [Known Issue 19: LVM issue](https://github.com/Seagate/cortx-prvsnr/wiki/Deploy-Stack#manual-fix-in-case-the-node-has-been-reimaged) before re-imaging the system.
   2. Enable [In-bands setup](https://github.com/Seagate/cortx-prvsnr/wiki/In-band-Setup).  
   3. In order to run end to end auto deployment script successfully the systems need to be in **clean state**   
    * Ensure both the nodes are either:  
      - freshly imaged  
      OR  
      - Cleanly teared down using ./destroy-cortx scirpt with --remove-prvsnr option.  
      - Refer to the steps in Teardown CORTX section [Teardown-Guide](#13-Teardown-CORTX)  
    4. Download the auto-deploy-script on primary node:  
    	- For deploying Release branch build:  
	  `$ curl "https://raw.githubusercontent.com/Seagate/cortx-prvsnr/release/cli/src/auto-deploy?token=APVYY2PZEFTWXPP2OHD74TC7DHDCW" 
	  -o auto-deploy; chmod a+x auto-deploy`  
       - For deploying Cortx-Beta branch build:  
    	 `$ curl "https://raw.githubusercontent.com/Seagate/cortx-prvsnr/Cortx-v1.0.0_Beta/cli/src/auto-deploy-eos?token=APVYY2PY55A5LAXDTDP5NDC7BVTLM" 
	 -o auto-deploy; chmod a+x auto-deploy`  

1.  Gather following details before proceeding further:  
    1.  Secondary node hostname.  
    1.  Secondary node password  
    1.  Cluster IP  (Public data IP)  
    1.  Management VIP (for csm)  
    1.  Network interface name for public data network  
    1.  Network interface name for private data network (direct connect)  
    1.  IP address for n/w interface provided for point 5  
    1.  IP address for interface provided for point 5  
    1.  Controller A IP address  
    1.  Controller B IP address  
    1.  User name of controller  
    1.  Password for controller user  

1.  Run `cortx-prereqs.sh` script:
    ```
    Usage:
         ./cortx-prereqs.sh [-t <target build url>] [--disable-sub-mgr] [-h|--help]

    OPTION:
    --disable-sub-mgr     For RHEL. To install prerequisites by disabling
                          subscription manager (usually, not recommended).
                          If this option is not provided it is expected that
                          either the system is not RHEL or system is already
                          registered with subscription manager.
    -t                    target build url pointed to release bundle base directory,
                          if specified the following directory structure is assumed:
                          <base_url>/
                               rhel7.7 or centos7.7   <- OS ISO is mounted here
                               3rd_party              <- CORTX 3rd party ISO is mounted here
                               cortx_iso              <- CORTX ISO (main) is mounted here


    ```
    `-t` option should be used to provide a path to a hosted ISOs (i.e., available over `http://`) in case internal Seagate's yum repos are not accessible.

    For example, the address of the web server hosting the ISOs is `10.10.10.10` and the `base_url` is `/sgt/cortx`.
    In this case, the `cortx-prereqs.sh` will look like this:

    ```
    cortx-prereqs.sh -t http://10.10.10.10/sgt/cortx
    ```

    **Note:** You don't need to specify the exact path to each directory (rhel7.7, 3rd_party, cortx_iso), only base_url is required. However, on the web-server side, the following should be done:

    * Inside web-server's root, the following directories should be created (assuming `sgt/cortx` in this example):

    ```
    sgt/cortx/rhel7.7
    sgt/cortx/3rd_party
    sgt/cortx/cortx_iso
    ```

    * The respective ISO files should be loopback-mounted to these directories
    * The web-server's configuration should be set to export these directories over `http`

    **Note:** `file://` structure is NOT currently supported.

    Refer to this document - [Cortx-Deployment-with-custom-url-(Hosted-ISO)](https://github.com/Seagate/cortx-prvsnr/wiki/Cortx-Deployment-with-custom-url-(Hosted-ISO)) - to read more about using provisioner with the hosted ISOs (common in non-Seagate environments).

1.  Run `auto-deploy` script:  
    ```
    Usage:  
    auto-deploy { -s <node-2 hostname> -p <node-2 password>  
                      -C <cluster-ip> -V <management VIP>  
                      -t <target build url for CORTX>  
                      [Optional Arguments] }  
    Required Arguments:
        -s <node-2 hostname>  If using 2 node cluster, the fqdn of second node
        -p <node-2 password>  If using 2 node cluster, the root password of second node
        -C <cluster-ip>       Shared static virtual IP for Public Data network
        -V <management VIP>   Shared static virtual IP for Public Management network

    Optional Arguments:  
        -n  <NETWORK IF>      Public n/w interface name (default enp175s0f0) [Check using command `ip a`]  
        -N  <NETWORK IF>      Private n/w interface name (default enp175s0f1).  
                              Network interface for direct connect.  [Check using command `ip a`]  
        --b1  <BMC USER>      BMC User for Node-1. Default ADMIN
        --m1  <BMC PASSWORD>  BMC Password for Node-1. Default
        --b2  <BMC USER>      BMC User for Node-2. Default ADMIN
        --m2  <BMC PASSWORD>  BMC Password for Node-2. Default
        -i  <IP ADDRESS>      IP address for public n/w interface name on node-1.  
                              This IP will be assigned to the n/w interface  
                              provided for -n option.  
                              Omit this option if ip is already set by DHCP  
                              [Don't use this option for LCO Lab HW and VMs]
        -I  <IP ADDRESS>      IP address for public n/w interface name on node-2,  
                              This IP will be assigned to the n/w interface name  
                              provided for -N option.  
                              [Don't use this option for LCO Lab HW and VMs]
                              Omit this option if ip is already set by DHCP.  
        -A  <IP ADDRESS>      IP address of controller A (default 10.0.0.2)
        -B  <IP ADDRESS>      IP address of controller B (default 10.0.0.3)  
        -U  <USER NAME>       User for controller (default 'manage')  
        -P  <PASSWORD>        Password for controller (default 'passwd')  
    ```
    So, -s, -p, -C, -V & -t options are mandetaory. Rest of the options can be skipped depending upon the hardware and default values in cluster.sls and storage_enclosure.sls pillars.  

    **Run auto-deploy.**  
    e.g. for seconday node as **<FQDN_node_2>**, password seagate, cluster ip 172.19.222.27, mgmt vip 10.230.215.45, public network interface name as enp216s0f0, private network interface name enp216sof1, ip address of public network interface (enp216s0f0) is 172.19.22.27, ip address of public network interface (enp216s0f1) as 172.19.22.28, IP addresses of controller A & B as 10.230.10.2, 10.230.10.3 respectively, controller user name as 'manage' and password as '!manage' run following command to install CORTX build no 1846.  

    $ `auto-deploy -s **<FQDN_node_1>** -p 'seagate' -C 172.19.222.27 -V 10.230.215.45 -n enp216s0f0 -N enp216s0f1 -i 172.19.22.27 -I 172.19.22.28 -A 10.230.10.2 -B 10.230.10.3 -U manage -P 'Seagate123$' --b1 ADMIN --m1 ADSEFGRE --b2 ADMIN --m2 HYEQLPIZ -t http://cortx-storage.mero.colo.seagate.com/releases/eos/github/master/rhel-7.7.1908/2628/prod/`    

    **Note:** As BMC credentials for PUNE systems are different, you need use optional parameter for password (--m1,--m2). See auto-deploy-cortx help.

    This will do following  
    - Install cortx-prvsnrr-cli rpms on both nodes  
    - run setup-provisioner  
    - configure pillar values provided as input to the script  
    - run deploy-cortx.  

    Since the script might take some time to finish the logs can be checked at `/var/log/seagate/provisioner/auto-deploy-cortx.log` to monitor the progress separately.  

1.  Check the corosync-pacemaker cluster status using `pcs cluster status` command:  
     ```
      $ pcs cluster status  
      Cluster Status:
        Stack: corosync
        Current DC: **<FQDN_node_1>** (version 1.1.20-5.el7-3c4c782f70) - partition with quorum
        Last updated: Mon Mar 16 15:06:24 2020
        Last change: Mon Mar 16 09:34:37 2020 by root via cibadmin on **<FQDN_node_1>**
        2 nodes configured
        54 resources configured

     PCSD Status:
       **<FQDN_node_1>**: Online
       **<FQDN_node_2>**: Online
     ```
      
1.  Check cortx cluster status in detail with all the services listed using `pcs status`    
    ```
    $ pcs status
    Cluster name: eos_cluster
    Stack: corosync
    Current DC: **<FQDN_node_1>** (version 1.1.20-5.el7-3c4c782f70) - partition with quorum
    Last updated: Mon Mar 16 15:09:59 2020
    Last change: Mon Mar 16 09:34:37 2020 by root via cibadmin on **<FQDN_node_1>**

    2 nodes configured
    54 resources configured

    Online: [ **<FQDN_node_1>** **<FQDN_node_2>** ]

    Full list of resources:

        Clone Set: ClusterIP-clone [ClusterIP] (unique)
            ClusterIP:0        (ocf::heartbeat:IPaddr2):       Started **<FQDN_node_1>**  
            ClusterIP:1        (ocf::heartbeat:IPaddr2):       Started **<FQDN_node_2>**  
        Clone Set: lnet-clone [lnet]
            Started: [ **<FQDN_node_1>** **<FQDN_node_2>** ]
        Resource Group: c1
            ip-c1      (ocf::heartbeat:IPaddr2):       Started **<FQDN_node_1>**
            lnet-c1    (ocf::seagate:lnet):    Started **<FQDN_node_1>**
            consul-c1  (systemd:hare-consul-agent-c1): Started **<FQDN_node_1>**
            hax-c1     (systemd:hare-hax-c1):  Started **<FQDN_node_1>**
            mero-confd-c1      (systemd:m0d@0x7200000000000001:0x9):   Started **<FQDN_node_1>**
            mero-ios-c1        (systemd:m0d@0x7200000000000001:0xc):   Started **<FQDN_node_1>**
        Resource Group: c2
            ip-c2      (ocf::heartbeat:IPaddr2):       Started **<FQDN_node_2>**
            lnet-c2    (ocf::seagate:lnet):    Started **<FQDN_node_2>**
            consul-c2  (systemd:hare-consul-agent-c2): Started **<FQDN_node_2>**
            hax-c2     (systemd:hare-hax-c2):  Started **<FQDN_node_2>**
            mero-confd-c2      (systemd:m0d@0x7200000000000001:0x4f):  Started **<FQDN_node_2>**
            mero-ios-c2        (systemd:m0d@0x7200000000000001:0x52):  Started **<FQDN_node_2>**
        Clone Set: mero-kernel-clone [mero-kernel]
            Started: [ **<FQDN_node_1>** **<FQDN_node_2>** ]
        ldap-c1        (systemd:slapd):        Started **<FQDN_node_1>**
        ldap-c2        (systemd:slapd):        Started **<FQDN_node_2>**
        s3auth-c1      (systemd:s3authserver): Started **<FQDN_node_1>**
        s3auth-c2      (systemd:s3authserver): Started **<FQDN_node_2>**
        els-search-c1  (systemd:elasticsearch):        Started **<FQDN_node_1>**
        els-search-c2  (systemd:elasticsearch):        Started **<FQDN_node_2>**
        statsd-c1      (systemd:statsd):       Started **<FQDN_node_1>**
        statsd-c2      (systemd:statsd):       Started **<FQDN_node_2>**
        haproxy-c1     (systemd:haproxy):      Started **<FQDN_node_1>**
        haproxy-c2     (systemd:haproxy):      Started **<FQDN_node_2>**
        s3backcons-c1  (systemd:s3backgroundconsumer): Started **<FQDN_node_1>**
        s3backprod-c1  (systemd:s3backgroundproducer): Started **<FQDN_node_1>**
        s3backcons-c2  (systemd:s3backgroundconsumer): Started **<FQDN_node_2>**
        s3backprod-c2  (systemd:s3backgroundproducer): Started **<FQDN_node_2>**
        s3server-c1-1  (systemd:s3server@0x7200000000000001:0x22):     Started **<FQDN_node_1>**
        s3server-c1-2  (systemd:s3server@0x7200000000000001:0x25):     Started **<FQDN_node_1>**
        s3server-c1-3  (systemd:s3server@0x7200000000000001:0x28):     Started **<FQDN_node_1>**
        s3server-c1-4  (systemd:s3server@0x7200000000000001:0x2b):     Started **<FQDN_node_1>**
        s3server-c1-5  (systemd:s3server@0x7200000000000001:0x2e):     Started **<FQDN_node_1>**
        s3server-c1-6  (systemd:s3server@0x7200000000000001:0x31):     Started **<FQDN_node_1>**
        s3server-c1-7  (systemd:s3server@0x7200000000000001:0x34):     Started **<FQDN_node_1>**
        s3server-c1-8  (systemd:s3server@0x7200000000000001:0x37):     Started **<FQDN_node_1>**
        s3server-c1-9  (systemd:s3server@0x7200000000000001:0x3a):     Started **<FQDN_node_1>**
        s3server-c1-10 (systemd:s3server@0x7200000000000001:0x3d):     Started **<FQDN_node_1>**
        s3server-c1-11 (systemd:s3server@0x7200000000000001:0x40):     Started **<FQDN_node_1>**
        s3server-c2-1  (systemd:s3server@0x7200000000000001:0x68):     Started **<FQDN_node_2>**
        s3server-c2-2  (systemd:s3server@0x7200000000000001:0x6b):     Started **<FQDN_node_2>**
        s3server-c2-3  (systemd:s3server@0x7200000000000001:0x6e):     Started **<FQDN_node_2>**
        s3server-c2-4  (systemd:s3server@0x7200000000000001:0x71):     Started **<FQDN_node_2>**
        s3server-c2-5  (systemd:s3server@0x7200000000000001:0x74):     Started **<FQDN_node_2>**
        s3server-c2-6  (systemd:s3server@0x7200000000000001:0x77):     Started **<FQDN_node_2>**
        s3server-c2-7  (systemd:s3server@0x7200000000000001:0x7a):     Started **<FQDN_node_2>**
        s3server-c2-8  (systemd:s3server@0x7200000000000001:0x7d):     Started **<FQDN_node_2>**
        s3server-c2-9  (systemd:s3server@0x7200000000000001:0x80):     Started **<FQDN_node_2>**
        s3server-c2-10 (systemd:s3server@0x7200000000000001:0x83):     Started **<FQDN_node_2>**
        s3server-c2-11 (systemd:s3server@0x7200000000000001:0x86):     Started **<FQDN_node_2>**
    Daemon Status:
        corosync: active/enabled
        pacemaker: active/enabled
        pcsd: active/enabled

    ```
    **NOTE:** You may want to check [Known Issue #4](Known-issues-for-deploy-cortx#known-issue-4)  

1.  If above output is seen as part of `pcs` commands mentioned in steps 5 & 6 the Cortx cluster is configured successfully.  

</p>
</details>

   - Deploy Cortx manually, refer to [Cortx setup on VM singlenode](https://github.com/Seagate/cortx-prvsnr/wiki/Cortx-setup-on-VM-singlenode). 

## 1.3 Teardown CORTX  
  
  Follow the [Teardown Guide](https://github.com/Seagate/cortx-prvsnr/wiki/Teardown-Guide) to teardown CORTX components.  

## 1.4 Virtual Machines
  
  - Know how to [Set up Cortx on a Single Node VM](https://github.com/Seagate/cortx-prvsnr/wiki/Cortx-setup-on-VM-singlenode)
  - **TODO** Add link for Dual Node VM. 
  
## 1.5 Set up the S3client   
  
  **TODO** Add link for for s3client setup document.

## 1.6 Additional Resources
  
  **TODO** Add link for CLI

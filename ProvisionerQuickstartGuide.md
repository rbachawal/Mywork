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
   
   4. You'll need to deploy CORTX. There are two ways in which you can do this:  
   		
	  1. [Auto-deploy 2-node CORTX cluster on hardware using a single command](https://github.com/Seagate/cortx-prvsnr/wiki/Deployment-on-HW_Auto-Deploy).   
	  2. [Manually deploy CORTX setup on a VM singlenode](https://github.com/Seagate/cortx-prvsnr/wiki/Cortx-setup-on-VM-singlenode). 
	  

## 1.3 Teardown CORTX  
  
  Follow the [Teardown Guide](https://github.com/Seagate/cortx-prvsnr/wiki/Teardown-Guide) to teardown CORTX components.  

## 1.4 Virtual Machines
  
  - Know how to [Set up Cortx on a Single Node VM](https://github.com/Seagate/cortx-prvsnr/wiki/Cortx-setup-on-VM-singlenode)
  - **TODO** Add link for Dual Node VM. 
  
## 1.5 Set up the S3client   
  
  **TODO** Add link for for s3client setup document.

## 1.6 Additional Resources
  
  **TODO** Add link for CLI

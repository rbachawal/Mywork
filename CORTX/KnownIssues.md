We've listed all the known issues with the CORTX software and the steps to resolve them below. Please refer to our [Support documentation](SUPPORT.md) to report new issues or want us to document steps to resolve an issue. 

# Known Issues

<details>
<summary>Firmware bundle upload and update functionality not working</summary>
<p>

### Symptoms and causes
  
  1. Failed to fetch update status.
  2. Failed to upload firmware bundle file.
  3. Failed to update the firmware.

### Resolution

  1. Failed to fetch update status.
    a. Refresh CSM UI after 30 secs
  2. Failed to upload firmware bundle file
    a. File format is incorrect. Please upload .bin file only
    b. File size is too large. Max file allowed is 2 GB
  3. Failed to update the firmware
    a. Failure reason as per return by provisioner component. Please check the csm_agent and provisioner logs.
    
 </p>
 </details>
 
<details>
<summary>Software package upload and update functionality not working</summary>
<p>

### Symptoms and causes

1. Failed to fetch update status.
2. Failed to upload software bundle file
3. Failed to update the software

### Resolution

1. Failed to fetch update status.
  a. Refresh CSM UI after 30 secs
2. Failed to upload software bundle file
  a. File format is incorrect. Please upload .iso file only
  b. File size is too large. Max file allowed is 2 GB
3. Failed to update the software
  a. Failure reason as per return by provisioner component. Please check the csm_agent and provisioner logs.
  
 </p>
 </details>
  
<details>
<summary>CSM UI is not working</summary>
<p>

### Symptoms and causes

1. URL is incorrect.
2. Csm_web service is not running.
  `pcs resource show | grep csm-web`

### Resolution

1. URL is incorrect. Please enter correct Mgmt_Vip, port and url.
2. Csm_web service is not running.
3. If not in active state please fire the below command:
   `pcs resource enable csm-web`
4. If problem is not solved:
   `pcs resource cleanup csm-web`
5. Please refer to HA chapter for more information.

 </p>
 </details>

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
    a. Refresh CMP UI after 30 secs.
  2. Failed to upload firmware bundle file.
    a. File format is incorrect. Please upload .bin file only.
    b. File size is too large. Max file allowed is 2 GB.
  3. Failed to update the firmware.
    a. Failure reason as per return by provisioner component. Please check the csm_agent and provisioner logs.
    
 </p>
 </details>
 
<details>
<summary>Software package upload and update functionality not working</summary>
<p>

### Symptoms and causes

1. Failed to fetch update status.
2. Failed to upload software bundle file.
3. Failed to update the software.

### Resolution

1. Failed to fetch update status.
  a. Refresh CMP UI after 30 secs.
2. Failed to upload software bundle file.
  a. File format is incorrect. Please upload .iso file only.
  b. File size is too large. Max file allowed is 2 GB.
3. Failed to update the software.
  a. Failure reason as per return by provisioner component. Please check the csm_agent and provisioner logs.
  
 </p>
 </details>
  
<details>
<summary>CMP UI is not working</summary>
<p>

### Symptoms and causes

1. URL is incorrect.
2. csm_web service is not running. Run the command:
  
    `pcs resource show | grep csm-web`

### Resolution

1. URL is incorrect. Please enter correct Mgmt_Vip, port, and url.
2. csm_web service is not running.
3. If not in active state please fire the below command:
   
   `pcs resource enable csm-web`
4. If problem is not solved:
   
   `pcs resource cleanup csm-web`
5. Please refer to HA chapter for more information.

 </p>
 </details>
 
 <details>
 <summary>Capacity Dial showing to Total Space as 0.0 KB</summary>
 <p>
    
### Symptoms and causes

Storage details in capacity widget is incorrect.

### Resolution

Refresh CMP UI after 30 secs.

</p>
</details>

<details>
  <summary>Login failed on CMP UI</summary>
  <p>
    
### Symptoms and causes

1. Admin user not created using pre-boarding.
2. Wrong username or password entered.
3. csm agent not running.

### Resolution

1. Refresh CMP UI after 30 secs.
2. Check whether CMP admin user is created or not. Fire the below command to check if CMP admin user is created or not:

    ```shell

    /opt/seagate/cortx/hare/bin/consul kv get -recurse
    cortx/base/user_collection/obj
    ```

    **Output**

      ```shell
      eos/base/user_collection/obj/admin:{"user_id": "admin",
      "user_type": "csm", "roles": ["admin", "manage"],
      "password_hash":
      "$2b$12$ZAFTHJyeu01wmQBrkk0AsuckVEeLU0z.EriD8IWBmhcqVrrxfK2kq", 
      "updated_time": "2020-05-25T08:38:56.151772+0000",
      "created_time": "2020-05-25T08:38:56.151757+0000"}
      ```
3. If you do not get the output as shown above, please create an admin user from CMP UI pre-boarding:

     https://<Mgmt VIP>:28100/#/preboarding/welcome
  
4. Please check the username or password entered. 
  1. If you have forgotten your password, navigate to Monitor/Manage User **>** Please contact administrator.
  2. If you are the Admin User, delete the admin user from the consul by following the commands below and redo the pre-boarding steps.
  
      `/opt/seagate/cortx/hare/bin/consul kv delete`
   
      `cortx/base/user_collection/obj/admin`
    
  </p>
  </details>
    

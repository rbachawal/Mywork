# Supported S3 IAM REST APIs and CLI Operations

The S3 IAM REST APIs are a collection of resources and methods which are integrated with AWS Identity and Access Management (IAM) service. The IAM service lets an administrator to securely control access to the S3 resources. As an IAM administrator, you can control the authentication and authorizations for an ID to use API Gateway resources. We've listed the S3 IAM REST APIs and CLI operations that are supported by the CORTX-S3 Server Submodule. 

<details>
<summary>CreateAccount</summary>
<p>

The CreateAccount request lets you create an S3 IAM account. 

| Request | Request body attributes  | Request Parameters    |
| :------- | :------------------------ | :--------------------- |
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** CreateAccount </br> **AccountName:** newrandom10 </br> **Email:** newrandom10@xyz.com  | <ul> <li> **AccountName:** The name of the account. This parameter allows </br>(through its regex pattern) a string of characters consisting of upper </br> and lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters:`_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </br> </ul> <ul> <li> **Email:** The email of the account which you want to create. </br> **Type:** String </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes |

### Known Errors

`.InternalError` Account wasn't Created. 
- The request processing has failed because of an unknown error, exception or failure.
- HTTP Status Code: 500

`EntityAlreadyExists` Account wasn't created.
- The request was rejected because it attempted to create an account that already exists.
- HTTP Status Code: 409

</p>
</details>

<details>
<summary>ListAccounts</summary>
<p>
  
 The ListAccounts parameter lists all the S3 IAM accounts.

| Request | Request body attributes  | Request Parameters    |
| :------- | :------------------------ | :--------------------- |
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** ListAccounts | None |
  
### Known Errors

`ServiceFailure` The request processing has failed because of an unknown error, exception or failure.
- HTTP Status Code: 500

</p>
</details>

<details>
<summary>ResetAccountAccessKey</summary>
<p>
  
 The ResetAccountAccessKey parameter lets you reset the access key for your S3 IAM account. 
 
| Request | Request body attributes  | Request Parameters    |
| :------- | :------------------------ | :--------------------- |
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** ResetAccountAccessKey </br> **AccountName:** newrandom6 </br> **Email:** None | <ul> <li> **AccountName:** The name of the account. This parameter allows </br>(through its regex pattern) a string of characters consisting of upper </br> and lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters:`_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </br> </ul> <ul> <li> **Email:** The email of the account which you want to create. </br> **Type:** String </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes |

### Known Errors 

`NoSuchEntity` An error occurred : The request was rejected because it referenced an entity that does not exist.
- HTTP Status Code: 404

`InternalFailure` The request processing has failed because of an unknown error, exception or failure.
- HTTP Status Code: 500

</p>
</details>

<details>
  <summary>DeleteAccount</summary>
  <p>
    
 The DeleteAccount parameter lets you delete your S3 IAM account.
 
| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** DeleteAccount </br> **AccountName:** newrandom6 </br> <p> **Response:** Account Deleted successfully. </p> | **AccountName:** The name of the account. </br> This parameter allows (through its regex pattern) </br> a string of characters consisting of upper and </br> lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters:`_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </br> </ul> |

### Known Errors

`UnauthorizedOperation` An error occurred : You are not authorized to perform this operation. Check your IAM policies, and ensure that you are using the correct access keys.

`InternalFailure` The request processing has failed because of an unknown error, exception or failure.
- HTTP Status Code: 500

</p>
</details>
 



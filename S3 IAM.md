# Supported S3 IAM REST APIs and CLI Operations

The S3 IAM REST APIs are a collection of resources and methods which are integrated with AWS Identity and Access Management (IAM) service. The IAM service lets an administrator to securely control access to the S3 resources. As an IAM administrator, you can control the authentication and authorizations for an ID to use API Gateway resources. We've listed the S3 IAM REST APIs and CLI operations that are supported by the CORTX-S3 Server Submodule. 

<details>
<summary>CreateAccount</summary>
<p>

| Request | Request body attributes  | Request Parameters    |
| :------- | :------------------------ | :--------------------- |
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | Action: CreateAccount </br> AccountName: newrandom10 </br> Email: newrandom10@xyz.com  | <ul> <li> **AccountName:** The name of the account. </br> This parameter allows (through its regex pattern) a string of characters </br> consisting of upper and lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters:`_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </br> </ul> <ul> <li> **Email:** The email of the account which you want to create. </br> **Type:** String </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes |

### Known Errors

`.InternalError` Account wasn't Created. 
- The request processing has failed because of an unknown error, exception or failure.
- HTTP Status Code: 500

`EntityAlreadyExists` Account wasn't created.
- The request was rejected because it attempted to create an account that already exists.
- HTTP Status Code: 409

</p>
</details>



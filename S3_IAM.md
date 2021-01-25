# Supported S3 IAM REST APIs and CLI Operations

The S3 IAM REST APIs are a collection of resources and methods which are integrated with AWS Identity and Access Management (IAM) service. The IAM service lets an administrator to securely control access to the S3 resources. As an IAM administrator, you can control the authentication and authorizations for an ID to use API Gateway resources. We've listed the S3 IAM REST APIs and CLI operations that are supported by the CORTX-S3 Server Submodule. 

<details>
<summary>CreateAccount</summary>
<p>

The CreateAccount request lets you create an S3 IAM account. 

| Request | Request body attributes  | Request Parameters    |
| :------- | :------------------------ | :--------------------- |
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** CreateAccount </br> **AccountName:** newrandom10 </br> **Email:** newrandom10@xyz.com  | <ul> <li> **AccountName:** The name of the account. This parameter allows </br>(through its regex pattern) a string of characters consisting of upper </br> and lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters:`_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </br> </ul> <ul> <li> **Email:** The email of the account which you want to create. </br> **Type:** String </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes |

</p>
</details>

<details>
<summary>ListAccounts</summary>
<p>
  
 The ListAccounts parameter lists all the S3 IAM accounts.

| Request | Request body attributes  | Request Parameters    |
| :------- | :------------------------ | :--------------------- |
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** ListAccounts | None |


</p>
</details>

<details>
<summary>ResetAccountAccessKey</summary>
<p>
  
 The ResetAccountAccessKey parameter lets you reset the access key for your S3 IAM account. 
 
| Request | Request body attributes  | Request Parameters    |
| :------- | :------------------------ | :--------------------- |
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** ResetAccountAccessKey </br> **AccountName:** newrandom6 </br> **Email:** None | <ul> <li> **AccountName:** The name of the account. This parameter allows </br>(through its regex pattern) a string of characters consisting of upper </br> and lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters:`_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </br> </ul> <ul> <li> **Email:** The email of the account which you want to create. </br> **Type:** String </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes |

</p>
</details>

<details>
  <summary>DeleteAccount</summary>
  <p>
    
 The DeleteAccount parameter lets you delete your S3 IAM account.
 
| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** DeleteAccount </br> **AccountName:** newrandom6 </br> **Response:** Account Deleted successfully. </p> | **AccountName:** The name of the account. </br> This parameter allows (through its regex pattern) </br> a string of characters consisting of upper and </br> lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters:`_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </br> </ul> |

</p>
</details>
 
<details>
  <summary>CreateAccountLoginProfile</summary>
  <p>
  
  The CreateAccountLoginProfile parameter creates a password for the specified account.
  
| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Password:** Random12@# </br> **AccountName:** newrandom10 </br> **PasswordResetRequired:** false </br> **Action:** CreateAccountLoginProfile |  **Password:** The new password for the Account. </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 128. </br> **Pattern:** [\u0009\u000A\u000D\u0020-\u00FF]+ </br> **Required:** Yes </br> `password-reset-required` `no-password-reset-required`: </br> Specifies whether you're required to  set a new password during yournext sign-in. </br> If you are passing `--password-reset-required` and `--no-password-reset-required` argument in same command without any sequence, the `PasswordResetRequired` flag will be set as true by default. </br> **Required:** No </br> **AccountName:** The name of the Account to create a password for. The Account must already exist. </br> **Type:** String </br> **Required:** Yes </br> **AccountLoginProfile:** A structure containing the account name and password create date. </br> **AccountName:** A string, that holds the name of the account used to sign in to the S3 Management Console. </br> **CreateDate:** Is a timestamp for the date when the password for the account was created. </br> **PasswordResetRequired:** is a boolean value that specifies whether the account user is required to set a new password on next sign-in. |

**Sample Response:**

b `<?xml version="1.0" encoding="UTF-8" standalone="no"?><CreateLoginProfileResponse
xmlns="https://iam.seagate.com/doc/2010-05-08/"><CreateLoginProfileResult>
<LoginProfile>
<UserName>user01</UserName>
<PasswordResetRequired>true</PasswordResetRequired>
<CreateDate>20201211090307Z</CreateDate>
</LoginProfile>
</CreateLoginProfileResult>
<ResponseMetadata>
<RequestId>88e92110820f423092a2d228316bfb10</RequestId></ResponseMetadata></CreateLoginProfile
Response>`

</p>
</details>

<details>
  <summary>GetAccountLoginProfile</summary>
  <p>
    The GetAccountLoginProfile retrieves the user name and password-creation date for the specified account.

| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** GetAccountLoginProfile </br> **AccountName:** newrandom10 | **AccountName:** The name of the account. This parameter allows </br>(through its regex pattern) a string of characters consisting of upper </br> and lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters:`_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </br> |

**Sample Response:**

b`<?xml version="1.0" encoding="UTF-8" standalone="no"?><GetLoginProfileResponse
xmlns="https://iam.seagate.com/doc/2010-05-
08/"><GetLoginProfileResult><LoginProfile><UserName>s3user1New</UserName><CreateDate>202012110
90305Z</CreateDate><PasswordResetRequired>false</PasswordResetRequired></LoginProfile></GetLog
inProfileResult><ResponseMetadata><RequestId>30e7e40f8bf743b2915fa4ee3fde6f2c</RequestId></Res
ponseMetadata></GetLoginProfileResponse>`

</p>
</details>

<details>
  <summary>UpdateAccountLoginProfile</summary>
  <p>
    The UpdateAccountLoginProfile parameter changes the password for the specified account.
    
| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **AccountName:** <account name> </br> **PasswordResetRequired:** <true or false> </br> **Action:** UpdateAccountLoginProfile </br> **Password:** <password> | <ul> <li> **Password:** The new password for the specified IAM account. </br> **PasswordResetRequired:** Allows this new password to be used only once by requiring the specified IAM account to set a new password on next sign-in.</li> <li> **AccountName:** The name of the account whose password you want to update. This parameter allows (through its regex pattern) a string of characters consisting of upper and lowercase alphanumeric characters with no spaces. </br> You can also include any of the following characters: `_+=,.@-` `--password` </br> **New Password to update:** `--password-reset-required` </br> Optionally you can use `--no-password-reset-required` to force a user to reset the account password on their first login use the `password-reset-flag` or the `no-password-reset-flag`. Passwords are optional, you can specify the accountname (mandatory), and the API will error out saying - `at least specify flag or password` </ul> |

**Sample Response:** 

b `<?xml version="1.0" encoding="UTF-8" standalone="no"?><UpdateLoginProfileResponse
xmlns="https://iam.seagate.com/doc/2010-05-
08/"><ResponseMetadata><RequestId>6fd38b0dab344e21a798e61ec5ae1caf</RequestId></ResponseMetada
ta></UpdateLoginProfileResponse>`

</p>
</details>

<details>
  <summary>CreateUserLoginProfile</summary>
  <p>
    
 The CreateUserLoginProfile parameter creates a password for the specified IAM user.
 
| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **UserName:** newrandom11user </br> **PasswordResetRequired:** false </br> **Action:** CreateLoginProfile </br> **Password:** Random12@# | <ul> <li> **UserName:** The name of the IAM user to create a password for. The user must already exist. This parameter allows (through its regex pattern) a string of characters consisting of upper and lowercase alphanumeric characters with no spaces. You can also include any of the following characters: `_+=,.@-` </br> **Required:** Yes </br> <li> **Password:** The new password for the user. </br> **Type:** String </br> **Length Constraints:** Minimum length of 6. Maximum length of 128. </br> **Pattern:** `[\u0009\u000A\u000D\u0020-\u00FF]+` </br> **Required:** Yes </br> <li> **PasswordResetRequired:** Specifies whether the user is required to set a new password on next sign-in. </br> **Type:** Boolean </br> **Required:** No </li></ul> |

**Sample Response**

- **LoginProfile** (structure): A structure containing the user name and password create date. 
- **UserName (string):** The name of the user, which can be used for signing in to the S3 Management Console. 
- **CreateDate (timestamp):** The date when the password for the user was created. 
- **PasswordResetRequired (boolean):** Specifies whether the  user is required to set a new password on next sign-in. 

</p>
</details>

<details>
  <summary>GetUserLoginProfile</summary>
  <p>
The GetUserLoginProfile parameter retrieves the *user name* and *password-creation date* for the specified IAM user. If the user has not been assigned a password, the operation will return a `404 (NoSuchEntity)` error. 
    
| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **UserName:** newrandom11 </br> **Action:** GetLoginProfile | **UserName:** The name of the user whose login profile you want to retrieve. This parameter allows (through its regex pattern) a string of characters consisting of upper and lowercase alphanumeric characters with no spaces. You can also include any of the following characters:` _+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes </li> </ul> |

**Sample Response** 
 
- **LoginProfile (structure):** A structure containing the user name and password create date.  
- **UserName (string):** The name of the user, which can be used for signing in to the S3 Management Console.  
- **CreateDate (timestamp):** The date when the password for the user was created.  
- **PasswordResetRequired (boolean):** Specifies whether the user is required to set a new password on next sign-in.

</p>
</details>

<details>
  <summary>UpdateUserLoginProfile</summary>

The UpdateUserLoginProfile parameter updates the user login profile for an existing user.

| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **UserName:** newrandom10 </br> **PasswordResetRequired:** false </br> **Action:** UpdateLoginProfile </br> **Password:** Random07@# | <ul><li> **UserName:** The name of the user whose password you want to update. This parameter allows (through its regex pattern) a string of characters consisting of upper and lowercase  alphanumeric characters with no spaces. You can also include any of the following characters: `_+=,.@-` </br> **Required:** Yes</li><li>**Password:** The new password for the specified IAM user. </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 128. </br> **Pattern:** `[\u0009\u000A\u000D\u0020-\u00FF]+` </br> **Required:** Yes </br></li> <li> **PasswordResetRequired:** Specifies whether the user is required to set a new password on next sign-in. Though `password-reset-flag` or `no-password-reset-flag` and password are optional, if you specify the username (mandatory), the API will error out stating: `At least specify flag or password.` </br> **Type:** Boolean </br> **Required:** No </li></ul> |

**Response**

`User login profile updated.`

</p>
</details>

<details>
  <summary>CreateAccessKey</summary>
  <p>
    
The CreateAccessKey creates a new S3 secret access key and corresponding S3 access key ID for the specified user. The  default status for new keys is `Active`. If you do not specify a user name, IAM determines the user name implicitly based on the Access Key ID used while signing the request.

| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** CreateAccessKey </br> **Version:** 2010-05-08 </br> **UserName:** newuserrandom111 | <ul> <li> **UserName:** The name of the user. This parameter allows (through its regex pattern) a string of characters consisting of upper and  lowercase alphanumeric characters with no spaces. You can also include any of the following  characters: `_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 128. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** No </li></ul> |

**Sample Response**

b `<?xml version="1.0" encoding="UTF-8" standalone="no"?><CreateAccessKeyResponse  xmlns="https://iam.seagate.com/doc/2010-05- 
08/"><CreateAccessKeyResult><AccessKey><UserName>changePasswordUserLoginProfileTestUser</UserN ame><AccessKeyId>AKIAgjTAmeFcShmDxtbQqBKQqg</AccessKeyId><Status>Active</Status><SecretAccessK ey>h9G1hwPAKFLcsGAKy7GePiuHCCayka81kh7M45v6</SecretAccessKey></AccessKey></CreateAccessKeyResu lt><ResponseMetadata><RequestId>f03ba9e9876e43cd9ae55e644ba798d3</RequestId></ResponseMetadata ></CreateAccessKeyResponse>`

</p>
</details>

<details> 
  <summary>ListAccessKeys</summary>
  <p>
    
The ListAccessKeys parameter returns information about the Access Key IDs associated with a specified IAM user. If there is none, the operation returns an empty list. 

| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** ListAccessKeys </br> **Version:** 2010-05-08 </br> **UserName:** newuserrandom111 | **UserName:** The name of the user. This parameter allows (through its regex pattern) a string of characters consisting of upper and lowercase alphanumeric characters with no spaces. You can also include any of the following  characters: `_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 128. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** No |

**Sample Response**

b `<?xml version="1.0" encoding="UTF-8" standalone="no"?><ListAccessKeysResponse  xmlns="https://iam.seagate.com/doc/2010-05- 
08/"><ListAccessKeysResult><UserName>root</UserName><AccessKeyMetadata><member><UserName>root< /UserName><AccessKeyId>AKIAGNP1JEOkTHqaeH3jU5bnHQ</AccessKeyId><Status>Active</Status><CreateD
ate>2020-12- 
11T09:02:47.000+0000</CreateDate></member></AccessKeyMetadata><IsTruncated>false</IsTruncated> </ListAccessKeysResult><ResponseMetadata><RequestId>f427b8d0412648eaa72e1431e9a7414c</RequestI d></ResponseMetadata></ListAccessKeysResponse>`

</p>
</details>

<details>
  <summary>UpdateAccessKey</summary>
  <p>
The UpdateAccessKey parameter changes the status of the specified access key from `Active` to `Inactive`, or vice versa. This operation can  be used to disable a user's key during a key rotation workflow. If the UserName is not specified, the user name is determined implicitly based on the S3 Access Key ID used while signing the request.

| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** UpdateAccessKey </br> **Version:** 2010-05-08 </br> **AccessKeyId:** AKIAov_KUl_wRjexnXZe7eUQcA </br> **Status:** Active </br> **UserName:** newuserrandom111 | <ul> <li> **AccessKeyId:** The access key ID of the secret access key you want to update. This parameter allows (through its regex pattern) a string of characters that can consist of any upper or lowercased letter or digit. </br> **Type:** String </br> **Length Constraints:** Minimum length of 16. Maximum length of 128. </br> **Pattern:** `[\w]+` </br> **Required:** Yes </br> <li> **Status:** The status you want to assign to the secret access key. Active means that the key can be used for API  calls to S3, while Inactive means that the key cannot be used. </br> **Type:** String </br> **Valid Values:** `Active` or `Inactive` </br> **Required:** Yes </br> <li> **UserName:** The name of the user whose key you want to update. This parameter allows (through its regex pattern) a string of characters consisting of upper and lowercase  alphanumeric characters with no spaces. You can also include any of the following characters: `_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 128. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** No </li></ul> | 

**Sample Response**

b `<?xml version="1.0" encoding="UTF-8" standalone="no"?><UpdateAccessKeyResponse  xmlns="https://iam.seagate.com/doc/2010-05- 
08/"><ResponseMetadata><RequestId>c2ce14e35ccb4d1bb3ef0d01a10e82ba</RequestId></ResponseMetada ta></UpdateAccessKeyResponse>`

</p>
</details>

<details>
  <summary>DeleteAccesskey</summary>
  
The DeleteAccesskey parameter lets you delete the Access Key Pair associated with an IAM user. If you do not specify a user name, IAM determines the user name implicitly based on the S3 Access Key ID used to sign the request. This operation works for access keys under the S3 account. Consequently, you  can use this operation to manage S3 account root user credentials even if the S3 account has no associated users. 

| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** DeleteAccessKey </br> **Version:** 2010-05-08 </br> **AccessKeyId:** AKIAov_KUl_wRjexnXZe7eUQcA | <ul> <li> **UserName:** The name of the user whose access key pair you want to delete. This parameter allows (through its regex pattern) a string of characters consisting of upper and lowercase  alphanumeric characters with no spaces. You can also include any of the following characters: `_+=,.@-` </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 128. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** No </li> </ul>

**Sample Response** 

b `<?xml version="1.0" encoding="UTF-8" standalone="no"?><DeleteAccessKeyResponse  xmlns="https://iam.seagate.com/doc/2010-05- 
08/"><ResponseMetadata><RequestId>7364ce6973fa462cb81792490649ccde</RequestId></ResponseMetada ta></DeleteAccessKeyResponse>`

</p>
</details>

<details>
  <summary>CreateUser</summary>
  <p>
    
The CreateUser parameter creates a new IAM user.

| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **UserName:** new </br> **Action:** CreateUser | <ul> <li> **Path:** The path for the user name. For more information about paths, see IAM Identifiers in the IAM User Guide. This parameter is optional. If it is not included, it defaults to a slash (/). This parameter allows (through its regex pattern) a string of characters consisting of either a forward slash (/) by itself or a string that must begin and end with forward slashes. In addition, it can contain any ASCII character from the `!(\u0021)` through the DEL character `(\u007F)`, including most punctuation characters, digits, and upper and lowercased letters. </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 512. </br> **Pattern:** `(\u002F)` or `(\u002F[\u0021-\u007F]+\u002F)` </br> **Required:** No </br><li>**UserName:** The name of the user that you want to create. IAM user, group, role, and policy names must be unique within the account. Names are not distinguished by case. For example, you cannot create resources named both `MyResource` and `myresource`. </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 64. </br> **Pattern:** `[\w+=,.@-]+` </br> **Required:** Yes</li></ul> |

**Sample Response**

b`<?xml version="1.0" encoding="UTF-8" standalone="no"?><CreateUserResponse  xmlns="https://iam.seagate.com/doc/2010-05- 
08/"><CreateUserResult><User><Path>/test/</Path><UserName>s3user_1</UserName><UserId>AIDAFFEC1 72CE21849C19</UserId><Arn>arn:aws:iam::918609980441:user/s3user_1</Arn></User></CreateUserResu lt><ResponseMetadata><RequestId>bec144b54c0546cf9367c70213db08e5</Req`

</p>
</details>

<details>
  <summary>ListUsers</summary>

The ListUsers parameter lists the IAM users that with a specified path prefix. If no path prefix is specified, the operation returns all users. If there are none, the operation returns an empty list.

| Request | Request body attributes  | Request Parameters    |  
| :------ | :----------------------- | :-------------------- | 
| POST / HTTP/1.1  </br> Host: <IAM Endpoint>:9443 | **Action:** ListUsers </br> **Version:** 2010-05-08 | <ul><li>**PathPrefix:** The path prefix for filtering the results. For example: `/division_abc/subdivision_xyz/`, which would return all user names with a path that starts with `/division_abc/subdivision_xyz/`. This parameter is optional. If it is not included, it defaults to a slash (/), listing all user names. This parameter allows (through its regex pattern) a string of characters consisting of either a forward slash (/) by or a string that begins and ends with forward slashes. In addition, it can contain any ASCII character from `!(\u0021)` to the DEL character `(\u007F)` including punctuation, characters, digits, and upper and lowercased letters. </br> **Type:** String </br> **Length Constraints:** Minimum length of 1. Maximum length of 512. </br> **Pattern:** `\u002F[\u0021-\u007F]*` </br> **Required:** No </li></ul> |

**Sample Response**

b`<?xml version="1.0" encoding="UTF-8" standalone="no"?><ListUsersResponse
xmlns="https://iam.seagate.com/doc/2010-05-
08/"><ListUsersResult><Users><member><UserId>AIDAF9C78D8CBDC84C849</UserId><Path>/test/success
/</Path><UserName>s3user1New</UserName><Arn>arn:aws:iam::208477331058:user/s3user1New</Arn><Cr
eateDate>2020-12-
11T09:38:34.000+0000</CreateDate></member></Users><IsTruncated>false</IsTruncated></ListUsersR
esult><ResponseMetadata><RequestId>b411b61e14324697bd617d86c4ca5632</RequestId></ResponseMetad
ata></ListUsersResponse>`

































    
    

 
 


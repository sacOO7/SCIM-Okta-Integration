# Okta-Scim-Server
Sample SCIM server written in NodeJS that supports Users and Groups (with group memberships!). This can be used in conjunction with the Okta SCIM application to test SCIM capabilities. Includes action logging.

Note - Follow steps from https://developer.okta.com/docs/guides/build-provisioning-integration/test-scim-api/ 
to run tests in runscope against the server

## Users endpoint

1\. Create User (POST to {SCIM Base Url}/Users)


2\. Get Users (GET to {SCIM Base Url}/Users)


3\. Get User By Id (GET to {SCIM Base Url}/Users/:UserId)


4\. Deactivate User (PATCH to {SCIM Base Url}/Users/:UserId)


5\. Modify/Update User (PUT to {SCIM Base Url}/Users/:UserId)

## Groups endpoint

1\. Create Group (POST to {SCIM Base Url}/Groups)

2\. Get Groups (GET to {SCIM Base Url}/Groups)

3\. Get Group By Id (GET to {SCIM Base Url}/Groups/:GroupId)

4\. Modify Group Name (PATCH to {SCIM Base Url}/Groups/:GroupId)

5\. Update Group (PUT to {SCIM Base Url}/Groups/:GroupId)

# Required Packages
- Install Node Version Manager 
- https://medium.com/@isaacjoe/best-way-to-install-and-use-nvm-on-mac-e3a3f6bc494d
- Go to root dir, type 
```
   nvm use
   npm install
   npm start // to start the server
```

__IMPORTANT: All requests must contain the following two headers:__
```json
Accept: application/scim+json
Content-Type: application/scim+json
```

# Database
- Code uses SQlite as a native db.
- When code runs for the first time, it will create a scim.db file at the root.
- Use https://sqlitebrowser.org/ to view contents of the db

# Starting server
You can use [ngrok](https://ngrok.com/) "ngrok http 8081" to make server available online. use https://xxxxx.ngrok.io in Okta SCIM app or Runscope to test online.

## Using Postman

You can get the collection for the supported actions by clicking [this link](https://www.getpostman.com/collections/0a38ba3aa0383bb9dc4f).

__IMPORTANT: If you change the body type to JSON, Postman will reset the `Content-Type` header to `application/json` and your calls will fail.__

## Requests

### Users

1\. POST {SCIM_Base_Url}/scim/v2/Users

```json
{  
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "userName": "username@example.com",
  "name":
  {  
    "givenName": "<GivenName>",
    "middleName": "undefined",
    "familyName": "<FaimlyName>"
  },
  "emails":
  [{
    "primary": true,
    "value": "username@example.com",
    "type": "work"
  }],
  "displayName": "<display name>",
  "externalId": "<externalId>",
  "groups": [],
  "active": true
}
```

2\. GET {SCIM_Base_Url}/scim/v2/Users?count=2&startIndex=1

3\. GET {SCIM_Base_Url}/scim/v2/Users?count=1&filter=userName eq "username@example.com"&startIndex=1

4\. PUT {SCIM_Base_Url}/scim/v2/Users/<UserID>

```json
{
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:User"],
  "id": "a5222dc0-4dec-11e6-866c-5b600f3e2809",
  "userName": "username@example.com",
  "name":
  {
    "givenName": "<GivenName>",
    "middleName": "undefined",
    "familyName": "<FamilyName>"
  },
  "active": "true",
  "meta":
  {
    "resourceType": "User",
    "location": "<location uri>"
  },
  "emails":
  [{
    "primary": true,
    "type": "work",
    "value": "username@example.com"
  }],
  "displayName": "<display Name>",
  "externalId": "<externalId>",
  "groups": []
}
```
5\. PATCH {SCIM_Base_Url}/scim/v2/Users/<UserID>
```json
{
  "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
  "Operations":
  [{
    "op": "replace",
    "value": { "active":true }
  }]
}
```

### Groups

1\. POST {SCIM_Base_Url}/scim/v2/Groups
```json
{
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
  "displayName": "Test Group 1",
  "members":
  [{
    "value": "<UserID>",
    "$ref": "<UserSCIMLocation>",
    "display": "First Last"
  }]
}
```

2\. GET {SCIM_Base_Url}/scim/v2/Groups?count=2&startIndex=1

3\. GET {SCIM_Base_Url}/scim/v2/Groups?count=1&startIndex=1&filter=displayName eq Test Group 1

4\. PUT {SCIM_Base_Url}/scim/v2/Groups/<GroupID>
```json
{
  "schemas": ["urn:ietf:params:scim:schemas:core:2.0:Group"],
  "id": "<GroupID>",
  "displayName": "<DisplayName>",
  "members":
  [{
    "value": "<UserID>",
    "$ref": "<UserSCIMLocation>",
    "display": "First Last"
  }]
}
```

5\. PATCH {SCIM_Base_Url}/scim/v2/Groups/<GroupID>
```json
{
  "schemas": ["urn:ietf:params:scim:api:messages:2.0:PatchOp"],
  "Operations":
  [{
    "op": "replace",
    "value": { "displayName":"Test" }
  }]
}
```

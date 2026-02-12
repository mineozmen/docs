---
description: >-
  This handler (com.rierino.handler.auth.AuthEventHandler) provides an abstract
  structure for user authentication, as part of Gateway functionality.
---

# Authenticate

AuthEventHandler can not be used directly, instead one of its implementations should configured as the authentication handler. All its implementations share the following configurations and actions:

## Handler Parameters

| Parameter            | Definition                                           | Example       | Default |
| -------------------- | ---------------------------------------------------- | ------------- | ------- |
| attempt.state        | Name of state manager to store login attempt history | auth\_attempt | -       |
| registration.enabled | Whether user registration is enabled or not          | true          | false   |
| initial.disabled     | Whether newly created users should be disabled first | true          | false   |
| apikey.length        | API key length to generate                           | 64            | 32      |
| apikey.secret        | Secret to use for hashing API keys                   | -             | -       |

## Actions

### Register

Registers a user with credentials provided and returns the new user's id, if handler allows user registration:

| Field         | Definition                                         | Example | Default |
| ------------- | -------------------------------------------------- | ------- | ------- |
| inputElement  | Json path for the input in event payload           | auth    | -       |
| outputElement | Json path for the output in response event payload | $.user  | -       |

Registration details (with credentials username/password or client\_id/client\_secret) can be provided in payload input element or the request metadata auth (which is typically used by gateway's token management process).  &#x20;

<details>

<summary>Example</summary>

Input

```json
{
    "auth":{
        "username": "user", 
        "password": "pass"
    }
}
```

Event Metadata

![](<../../../../../../.gitbook/assets/image (79).png>)

</details>

### Login

Logs in a user with credentials (username/password or client\_id/client\_secret) provided and returns tokens (and optionally, the user details):

| Field                     | Definition                                                                                      | Example            | Default                                              |
| ------------------------- | ----------------------------------------------------------------------------------------------- | ------------------ | ---------------------------------------------------- |
| inputElement              | Json path for the input in event payload                                                        | auth               | -                                                    |
| outputElement             | Json path for the output in response event payload                                              | $.token            | -                                                    |
| parameters.resolve        | Whether access token should be resolved to also return user details (such as id and roles)      | true               | false                                                |
| parameters.resolvePattern | Jmespath expression for resolving token contents (access, profile) if resolve parameter is true | {"access": access} | {"user": {"id": access.sub, "roles": access.roles} } |

<details>

<summary>Example</summary>

Input

```json
{
    "auth":{
        "username": "user", 
        "password": "pass"
    }
}
```

Event Metadata

![](<../../../../../../.gitbook/assets/image (80).png>)

</details>

### Validate

Validates and resolves a user with tokens (access\_token & id\_token) provided and returns the user details:

| Field                     | Definition                                                                    | Example            | Default                                              |
| ------------------------- | ----------------------------------------------------------------------------- | ------------------ | ---------------------------------------------------- |
| inputElement              | Json path for the input in event payload                                      | auth               | -                                                    |
| outputElement             | Json path for the output in response event payload                            | $.user             | -                                                    |
| parameters.resolvePattern | Jmespath expression for resolving token contents (access, profile) for output | {"access": access} | {"user": {"id": access.sub, "roles": access.roles} } |

Tokens can be provided in payload input element or the request metadata auth (which is typically used by gateway's token management process). &#x20;

<details>

<summary>Example</summary>

Input

```json
{
    "auth":{
        "access_token": "...", 
        "id_token": "..."
    }
}
```

Event Metadata

![](<../../../../../../.gitbook/assets/image (112).png>)

</details>

### Resolve

Provides same functionality and uses same parameters as Validate.

### Refresh

Refreshes tokens with a provided refresh token and returns new tokens (and optionally, the user details):

| Field                     | Definition                                                                                      | Example            | Default                                              |
| ------------------------- | ----------------------------------------------------------------------------------------------- | ------------------ | ---------------------------------------------------- |
| inputElement              | Json path for the input in event payload                                                        | auth               | -                                                    |
| outputElement             | Json path for the output in response event payload                                              | $.user             | -                                                    |
| parameters.resolve        | Whether access token should be resolved to also return user id and roles                        | true               | false                                                |
| parameters.resolvePattern | Jmespath expression for resolving token contents (access, profile) if resolve parameter is true | {"access": access} | {"user": {"id": access.sub, "roles": access.roles} } |

### Logout

Logs out a user with access token provided:

| Field        | Definition                               | Example | Default |
| ------------ | ---------------------------------------- | ------- | ------- |
| inputElement | Json path for the input in event payload | auth    | -       |

### ResolveKey

Resolves a given API key and returns resolved contents in output:

| Field                     | Definition                                                                              | Example                                        | Default                                                                      |
| ------------------------- | --------------------------------------------------------------------------------------- | ---------------------------------------------- | ---------------------------------------------------------------------------- |
| inputElement              | Json path for the "api\_key" input in event payload (or can be passed in auth metadata) | auth                                           | -                                                                            |
| output                    | Json path for the output in response event payload                                      | $.user                                         | -                                                                            |
| parameters.resolvePattern | Jmespath pattern for converting {user,key} data in output                               | {"user": {"id": key.id, "roles": key.roles } } | {"user": {"id": key.id, "roles": intersect(user.access.roles, key.roles) } } |

### UserRegister

Registers a new user with given profile and credential details (username/password or client\_id/client\_secret) and returns the created user's id as "user\_id".

| Field        | Definition                               | Example | Default |
| ------------ | ---------------------------------------- | ------- | ------- |
| inputElement | Json path for the input in event payload | auth    | -       |

### UserList

Lists current users with profile and access details, using "search" parameter in input element for keyword search:

| Field            | Definition                                | Example         | Default |
| ---------------- | ----------------------------------------- | --------------- | ------- |
| inputElement     | Json path for the input in event payload  | parameters      | -       |
| outputElement    | Json path for the output in event payload | $.users         | -       |
| outputPattern    | Jmespath pattern for list of users        | {userList:list} | -       |
| parameters.skip  | Number of records to skip                 | 10              | -       |
| parameters.limit | Max number of records to return           | 10              | -       |

### **UserGet**

Returns profile and access details of a user with given id.

| Field         | Definition                                | Example  | Default |
| ------------- | ----------------------------------------- | -------- | ------- |
| inputElement  | Json path for the input in event payload  | user     | -       |
| outputElement | Json path for the output in event payload | $.result | -       |
| outputPattern | Jmespath pattern for list of users        | {user:@} | -       |

### **UserDelete**

Deletes user with given id from records.

| Field        | Definition                                       | Example  | Default |
| ------------ | ------------------------------------------------ | -------- | ------- |
| inputElement | Json path for the input in event payload with id | user     | -       |
| idPath       | Json path for the id field in input element      | username | id      |

### UserLogout

Logs out user with given id.

| Field        | Definition                                       | Example  | Default |
| ------------ | ------------------------------------------------ | -------- | ------- |
| inputElement | Json path for the input in event payload with id | user     | -       |
| idPath       | Json path for the id field in input element      | username | id      |

### UserSetProfile

Sets profile details (e.g. name, surname) of a given user id.

| Field        | Definition                                               | Example                            | Default |
| ------------ | -------------------------------------------------------- | ---------------------------------- | ------- |
| inputElement | Json path for the input in event payload                 | user                               | -       |
| inputPattern | Jmespath pattern for converting input into a user record | {id:id, data: {profile: profile\}} | -       |

### UserSetCredential

Sets credential details (e.g. password) of a given user id.

| Field        | Definition                                               | Example                                  | Default |
| ------------ | -------------------------------------------------------- | ---------------------------------------- | ------- |
| inputElement | Json path for the input in event payload                 | user                                     | -       |
| inputPattern | Jmespath pattern for converting input into a user record | {id:id, data: {credential: credential\}} | -       |

### UserSetAccess

Sets access details (e.g. roles) of a given user id.

| Field        | Definition                                               | Example                          | Default |
| ------------ | -------------------------------------------------------- | -------------------------------- | ------- |
| inputElement | Json path for the input in event payload                 | user                             | -       |
| inputPattern | Jmespath pattern for converting input into a user record | {id:id, data: {access: access\}} | -       |

### UserCreateKey

Creates an API key for given user id with given access details.

| Field        | Definition                                               | Example                          | Default |
| ------------ | -------------------------------------------------------- | -------------------------------- | ------- |
| inputElement | Json path for the input in event payload                 | user                             | -       |
| inputPattern | Jmespath pattern for converting input into a user record | {id:id, data: {access: access\}} | -       |

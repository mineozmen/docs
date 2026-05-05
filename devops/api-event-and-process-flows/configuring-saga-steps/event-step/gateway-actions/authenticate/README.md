---
description: >-
  These actions provide ability to authenticate users and manage user access as
  part of gateway functionality.
---

# Authenticate

## Authenticate Actions

### Register

Registers a user with provided credentials and returns the new user's id, if the handler allows user registration.

| Field          | Definition                                         | Example | Default |
| -------------- | -------------------------------------------------- | ------- | ------- |
| Input Element  | Json path for the input in event payload           | auth    | -       |
| Output Element | Json path for the output in response event payload | $.user  | -       |

Registration details can be provided in the payload input element or in request metadata auth.

<details>

<summary>Example</summary>

Input

```json
{
    "auth": {
        "username": "user",
        "password": "pass"
    }
}
```

Event Metadata

![](<../../../../../../.gitbook/assets/image (79).png>)

</details>

### Login

Logs in a user with credentials and returns tokens, and optionally user details.

| Field                     | Definition                                                          | Example            | Default                                              |
| ------------------------- | ------------------------------------------------------------------- | ------------------ | ---------------------------------------------------- |
| Input Element             | Json path for the input in event payload                            | auth               | -                                                    |
| Output Element            | Json path for the output in response event payload                  | $.token            | -                                                    |
| parameters.resolve        | Whether access token should be resolved to also return user details | true               | false                                                |
| parameters.resolvePattern | JMESPath expression for resolving token contents if resolve is true | {"access": access} | {"user": {"id": access.sub, "roles": access.roles} } |

<details>

<summary>Example</summary>

Input

```json
{
    "auth": {
        "username": "user",
        "password": "pass"
    }
}
```

Event Metadata

![](<../../../../../../.gitbook/assets/image (80).png>)

</details>

### Validate

Validates and resolves a user with provided tokens and returns user details.

| Field                     | Definition                                                  | Example            | Default                                              |
| ------------------------- | ----------------------------------------------------------- | ------------------ | ---------------------------------------------------- |
| Input Element             | Json path for the input in event payload                    | auth               | -                                                    |
| Output Element            | Json path for the output in response event payload          | $.user             | -                                                    |
| parameters.resolvePattern | JMESPath expression for resolving token contents for output | {"access": access} | {"user": {"id": access.sub, "roles": access.roles} } |

### Resolve

Provides the same functionality and uses the same parameters as `Validate`.

### Refresh

Refreshes tokens with a provided refresh token and returns new tokens, and optionally user details.

| Field                     | Definition                                                               | Example            | Default                                              |
| ------------------------- | ------------------------------------------------------------------------ | ------------------ | ---------------------------------------------------- |
| Input Element             | Json path for the input in event payload                                 | auth               | -                                                    |
| Output Element            | Json path for the output in response event payload                       | $.user             | -                                                    |
| parameters.resolve        | Whether access token should be resolved to also return user id and roles | true               | false                                                |
| parameters.resolvePattern | JMESPath expression for resolving token contents if resolve is true      | {"access": access} | {"user": {"id": access.sub, "roles": access.roles} } |

### Logout

Logs out a user with the provided access token.

| Field         | Definition                               | Example | Default |
| ------------- | ---------------------------------------- | ------- | ------- |
| Input Element | Json path for the input in event payload | auth    | -       |

### ResolveKey

Resolves a given API key and returns resolved contents in output.

| Field                     | Definition                                                                            | Example                                        | Default                                                                      |
| ------------------------- | ------------------------------------------------------------------------------------- | ---------------------------------------------- | ---------------------------------------------------------------------------- |
| Input Element             | Json path for the `api_key` input in event payload, or can be passed in auth metadata | auth                                           | -                                                                            |
| Output                    | Json path for the output in response event payload                                    | $.user                                         | -                                                                            |
| parameters.resolvePattern | JMESPath pattern for converting `{user,key}` data in output                           | {"user": {"id": key.id, "roles": key.roles } } | {"user": {"id": key.id, "roles": intersect(user.access.roles, key.roles) } } |

### UserRegister

Registers a new user with given profile and credential details and returns the created user's id as `user_id`.

| Field         | Definition                               | Example | Default |
| ------------- | ---------------------------------------- | ------- | ------- |
| Input Element | Json path for the input in event payload | auth    | -       |

### UserList

Lists current users with profile and access details.

| Field            | Definition                                | Example         | Default |
| ---------------- | ----------------------------------------- | --------------- | ------- |
| Input Element    | Json path for the input in event payload  | parameters      | -       |
| Output Element   | Json path for the output in event payload | $.users         | -       |
| Output Pattern   | JMESPath pattern for list of users        | {userList:list} | -       |
| parameters.skip  | Number of records to skip                 | 10              | -       |
| parameters.limit | Max number of records to return           | 10              | -       |

### UserGet

Returns profile and access details of a user with the given id.

| Field          | Definition                                | Example  | Default |
| -------------- | ----------------------------------------- | -------- | ------- |
| Input Element  | Json path for the input in event payload  | user     | -       |
| Output Element | Json path for the output in event payload | $.result | -       |
| Output Pattern | JMESPath pattern for returned user        | {user:@} | -       |

### UserDelete

Deletes the user with the given id from records.

| Field         | Definition                                       | Example  | Default |
| ------------- | ------------------------------------------------ | -------- | ------- |
| Input Element | Json path for the input in event payload with id | user     | -       |
| idPath        | Json path for the id field in input element      | username | id      |

### UserLogout

Logs out the user with the given id.

| Field         | Definition                                       | Example  | Default |
| ------------- | ------------------------------------------------ | -------- | ------- |
| Input Element | Json path for the input in event payload with id | user     | -       |
| idPath        | Json path for the id field in input element      | username | id      |

### UserSetProfile

Sets profile details of a given user id.

| Field         | Definition                                               | Example                            | Default |
| ------------- | -------------------------------------------------------- | ---------------------------------- | ------- |
| Input Element | Json path for the input in event payload                 | user                               | -       |
| Input Pattern | JMESPath pattern for converting input into a user record | {id:id, data: {profile: profile\}} | -       |

### UserSetCredential

Sets credential details of a given user id.

| Field         | Definition                                               | Example                                  | Default |
| ------------- | -------------------------------------------------------- | ---------------------------------------- | ------- |
| Input Element | Json path for the input in event payload                 | user                                     | -       |
| Input Pattern | JMESPath pattern for converting input into a user record | {id:id, data: {credential: credential\}} | -       |

### UserSetAccess

Sets access details of a given user id.

| Field         | Definition                                               | Example                          | Default |
| ------------- | -------------------------------------------------------- | -------------------------------- | ------- |
| Input Element | Json path for the input in event payload                 | user                             | -       |
| Input Pattern | JMESPath pattern for converting input into a user record | {id:id, data: {access: access\}} | -       |

### UserCreateKey

Creates an API key for the given user id with the given access details.

| Field         | Definition                                               | Example                          | Default |
| ------------- | -------------------------------------------------------- | -------------------------------- | ------- |
| Input Element | Json path for the input in event payload                 | user                             | -       |
| Input Pattern | JMESPath pattern for converting input into a user record | {id:id, data: {access: access\}} | -       |

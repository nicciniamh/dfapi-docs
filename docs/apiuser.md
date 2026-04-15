# APIUser

The apiuser module manages users for the DucksFeet API ecosystem. These users are used to access the API with given roles. Please read the section on [Security](/docs/Security.md) for greater details on how security and authorization qre implemented.

## Endpoint Format

With the exception of the list endpoint, each endpoint is specified as
`apiuser/{user}/function/[parameter]`.

* [list](#list) - List users.
* [new](#new) - Create a user.
* [modify](#modify) - Modify a user.
* [delete](#delete) - Delete a user.
* [grant](#grant) - Grant role to user.
* [revoke](#revoke) - Revoke role from user.

### list

* Role required: user_view
* Usage: `apiuser/list`

Shows a list of all users in the users document. Password hashes are not returned.

### new

* Usage: `apiuser/{user}/new`
* Role required: user_modify
* Data required: password

Create a user with the password in data.

### modify

* Usage: `apiuser/{user}/modify`
* Role required: user_modify
* Data required: pasword

Modify a user with the password in data.

### delete

* Usage: `apiuser/{user}/delete`
* Role required: user_modify

Delete a user.

## grant

* Usage: `apiuser/{user}/grant/{role}`
* Role required: user_modify

Grant role to user.

### Revoke

* Usage: `apiuser/{user}/revoke/{role}`
* Role required: user_modify

Revoke a role from a user.

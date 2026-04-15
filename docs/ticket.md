# Ticket

The ticket module is use to obtain and manage authorization tickets for client access. To understand the ticket flow lifecycle it's important to read the sections on [security](/docs/security.md) and the [API architecture](/docs/architecture.md).

The ticket module manages tickets on the part of the client.

## Ticket / Session Data

Every ticket issued may have session data associated with the ticket. The negotiation of this data is managed between client and module. The ticket module does not use this data directly.

## Endpoint Format

* [new](#new) - Obtain a new ticket.
* [validate](#validate) - Validate a ticket.
* [delete](#delete) - Delete a ticket.
* [set_session](#set_session) - Set session data.
* [get_session](#get_session) - Get session data.

### new

* Usage: `ticket/new`
* Roles required: none.
* Data required: `{"user_id": *userid*, "password": *password*}`

Returns new ticket id

### delete

* Usage: `ticket/{ticket}/delete`
* Roles required: none, valid ticket.

### validate

* Usage: `ticket/{ticket}/validate`
* Roles required: none, valid ticket.

### get_session

* Usage: `ticket/{ticket}/get_session`
* Roles required: none, valid ticket.

Return session data.

### set_session

* Usage: `ticket/{ticket}/set_session`
* Roles required: none, valid ticket.
* Data required: data member of payload, any data.

Set and overwrite session data. Session data expires with the ticket.

# Zevere - RESTful Web API

- [Client-Server API](#client-server-api)
  - [Overview](#overview)
  - [Authentication](#authentication)
  - [Errors](#errors)
  - [Special Formats](#special-formats)
    - [DateTime](#datetime)
  - [Endpoints](#endpoints)
    - [`POST /zv/login`](#post-zvlogin)
    - [`POST /zv/logout`](#post-zvlogout)
    - [`POST /zv/users`](#post-zvusers)
    - [`HEAD /zv/users/{user_id}`](#head-zvusersuser_id)
    - [`GET /zv/users/{user_id}`](#get-zvusersuser_id)
    - [`PATCH /zv/users/{user_id}`](#patch-zvusersuser_id)
    - [`DELETE /zv/users/{user_id}`](#delete-zvusersuser_id)
    - [`POST /zv/users/{user_id}/tasks`](#post-zvusersuser_idtasks)
    - [`HEAD /zv/users/{user_id}/tasks/{task_id}`](#head-zvusersuser_idtaskstask_id)
    - [`GET /zv/users/{user_id}/tasks/{task_id}`](#get-zvusersuser_idtaskstask_id)
    - [`PUT /zv/users/{user_id}/tasks/{task_id}`](#put-zvusersuser_idtaskstask_id)
    - [`DELETE /zv/users/{user_id}/tasks/{task_id}`](#delete-zvusersuser_idtaskstask_id)
- [Media Types](#media-types)
  - [Login](#login)
  - [User](#user)
  - [Task](#task)
  - [PartialUpdate](#partialupdate)
  - [Error](#error)
  - [Miscellaneous](#miscelaneous)

## Client-Server API

### Overview

This API is exposed to the clients through a number of endpoints accepting JSON payloads.

### Authentication

Clients can get a Base64 encoded token in login and register process. This token should be provided when necessary as HTTP basic authorization header in the form of `Authorization: Basic {token}`. Requests that require authentication(indicated by _Requires Auth_) and do not have this header, will result in a _401 Unauthorized_ response.

### Errors

In the case of any errors, server's response payload would be of type [`Error`](#error).

Here are the general expected Zevere error codes based on HTTP status code in the response:

- `400`:
  - `zv.error.invalid`: Validation errors. In this case, payload contains the `validation_errors` array.
- `404`:
  - `zv.error.not-found`: Resource does not exist.
- `415`:
  - `zv.error.not-supported`: Media type is not supported for this endpoint and method.
- `500`:
  - `zv.error.fault`: Server failed to process the request.

### Special Formats

#### DateTime

All datetime values in JSON are strings with a format based on [ISO8601](http://en.wikipedia.org/wiki/ISO_8601). See examples [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString).

### Endpoints

Some Endpoints might accept more than one media types in request and return more than one form of representation in response. You can see the full list [here](#media-types).

#### `POST /zv/login`

Retrieve an auth token

- Acceptable Request Content Types:
  - [`application/vnd.zv.login.creation+json`](#applicationvndzvlogincreationjson)
- Expectable Response Media Types:
  - [`application/vnd.zv.login.token+json`](#applicationvndzvlogintokenjson)
- Responses:
  - `200`: Login already exists
  - `201`: New login created

#### `POST /zv/logout`

Revoke auth token. _Requires Auth_.

- Responses:
  - `204`: Token is revoked

#### `POST /zv/users`

Create a new user

- Acceptable Request Content Types:
  - [`application/vnd.zv.user.creation+json`](#applicationvndzvusercreationjson)
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.user.pretty+json`](#applicationvndzvuserprettyjson)
  - [`application/vnd.zv.user.full+json`](#applicationvndzvuserfulljson)
- Responses:
  - `201`: User created
  - `204` (if `application/vnd.zv.empty` media type requested): User created
  - `409`: A user with the same username already exists

#### `HEAD /zv/users/{user_id}`

Check existence of a user by ID

- Path Parameters:
  - `user_id`: id of the user
- Responses:
  - `204`: User exists

#### `GET /zv/users/{user_id}`

Get a user by ID. _Requires Auth_.

- Path Parameters:
  - `user_id`: id of the user
- Expectable Response Media Types:
  - [`application/vnd.zv.user.pretty+json`](#applicationvndzvuserprettyjson)
  - [`application/vnd.zv.user.full+json`](#applicationvndzvuserfulljson)
- Responses:
  - `200`

#### `PATCH /zv/users/{user_id}`

Partially update user information. _Requires Auth_.

- Path Parameters:
  - `user_id`: id of the user
- Acceptable Request Content Types:
  - [`application/vnd.zv.partial-update+json`](#applicationvndzvpartial-updatejson)
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.user.pretty+json`](#applicationvndzvuserprettyjson)
  - [`application/vnd.zv.user.full+json`](#applicationvndzvuserfulljson)
- Responses:
  - `202`: User info updated
  - `204` (if `application/vnd.zv.empty` media type requested): User info updated

#### `DELETE /zv/users/{user_id}`

Remove user. _Requires Auth_.

- Path Parameters:
  - `user_id`: id of the user
- Responses:
  - `204`: User removed

#### `POST /zv/users/{user_id}/tasks`

Create a new task for the user. _Requires Auth_.

- Path Parameters:
  - `user_id`: id of the user
- Acceptable Request Content Types:
  - [`application/vnd.zv.task.creation+json`](#applicationvndzvtaskcreationjson)
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.task.pretty+json`](#applicationvndzvtaskprettyjson)
  - [`application/vnd.zv.task.full+json`](#applicationvndzvtaskfulljson)
- Responses:
  - `201`: Task created
  - `204` (if `application/vnd.zv.empty` media type requested): Task created

#### `HEAD /zv/users/{user_id}/tasks/{task_id}`

Check existence of a task for user by ID. _Requires Auth_.

- Path Parameters:
  - `user_id`: id of the user
  - `task_id`: id of the task
- Responses:
  - `204`: Task exists for that user

#### `GET /zv/users/{user_id}/tasks/{task_id}`

Get a task for user by ID. _Requires Auth_.

- Path Parameters:
  - `user_id`: id of the user
  - `task_id`: id of the task
- Expectable Response Media Types:
  - [`application/vnd.zv.task.pretty+json`](#applicationvndzvtaskprettyjson)
  - [`application/vnd.zv.task.full+json`](#applicationvndzvtaskfulljson)
- Responses:
  - `200`

#### `PUT /zv/users/{user_id}/tasks/{task_id}`

Update or create a task for user with the specified `task_id`. _Requires Auth_.

- Path Parameters:
  - `user_id`: id of the user
  - `task_id`: id of the task
- Acceptable Request Content Types:
  - [`application/vnd.zv.task.full+json`](#applicationvndzvtaskcreationjson)
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.task.pretty+json`](#applicationvndzvtaskprettyjson)
  - [`application/vnd.zv.task.full+json`](#applicationvndzvtaskfulljson)
- Responses:
  - `201`: Task created/updated
  - `204` (if `application/vnd.zv.empty` media type requested): Task created/updated
  - `409`: A task with the same id already exists for this user

#### `DELETE /zv/users/{user_id}/tasks/{task_id}`

Remove a task for user. _Requires Auth_.

- Path Parameters:
  - `user_id`: id of the user
  - `task_id`: id of the task
- Responses:
  - `204`: Task removed

## Media Types

Each type might have different representations. Listed below are types, their various representations, and their custom mime-type names.

### Login

#### `application/vnd.zv.login.creation+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| user_name | string | ✔ | User name |
| passphrase | string | ✔ | passphrase |

#### `application/vnd.zv.login.token+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| token | string | ✔ | Authentication token |

### User

#### `application/vnd.zv.user.full+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | User's ID |
| first_name | string | ✔ | User's first name |
| last_name | string |  | User's last name |
| joined_at | datetime | ✔ | The date account was created in UTC format. Time should be set to the midnight. |

#### `application/vnd.zv.user.pretty+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | User's ID |
| display_name | string | ✔ | User's name that contains first name and optional last name appended with a space character. |
| days_joined | number | ✔ | Number of days this user has joined. |

#### `application/vnd.zv.user.creation+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| name | string | ✔ | The desired user name |
| passphrase | string | ✔ | Passphrase as clear text |
| first_name | string | ✔ | User's first name |
| last_name | string |  | User's last name |

### Task

#### `application/vnd.zv.task.full+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | Task's ID |
| title | string | ✔ | A title/summary for task. Should be 140 characters or less. |
| description | string |  | More description for task |
| created_at | datetime | ✔ | The date and time this task was created in UTC format. |
| due_by | datetime | | The date and time this task would be due. Should always be at least 60 seconds after `created_at`. |

#### `application/vnd.zv.task.pretty+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | Task's ID |
| title | string | ✔ | A title/summary for task. Should be 140 characters or less. |
| description | string |  | More description for task |
| is_due | boolean | ✔ | Whether task's due date and time is passed. |
| due_in | string | | Only if `is_due` is false, contains a user readable representation of the time left such as `2 days 5 minutes`. |

#### `application/vnd.zv.task.creation+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| title | string | ✔ | A title/summary for task. Should be 140 characters or less. |
| description | string |  | More description for task |
| due_by | datetime | | The date and time this task would be due. Should always be at least 60 seconds after the time of making request. |

### PartialUpdate

#### `application/vnd.zv.partial-update+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| patches | [`FieldUpdate`](#fieldupdate)[] | ✔ | An array of field updates |

#### FieldUpdate

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| field | string | ✔ | Name of the field to be updated |
| value | string | ✔ | New value for the field |

### Error

#### `application/vnd.zv.error+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| code | string | ✔ | Machine readable error code |
| message | string | ✔ | User readable error message describing what went wrong |
| validation_errors | [`ValidationError`](#validationerror)[] | | An array of errors ocurred in validation. |

#### ValidationError

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| field | string | ✔ | Name of the field that caused the failure |
| message | string | ✔ | User readable error message describing the failure reason |
| hint | string | | User readable hint message instructing the user how to resolve the issue |

### Miscellaneous

#### `application/vnd.zv.empty`

This media type indicates an empty object and is useful for times that client doesn't need the response body in the case of a successful request. This results in a response status code of 204.

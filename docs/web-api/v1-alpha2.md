# Zevere - Web API Specifications

- [Client-Server API](#client-server-api)
  - [Overview](#overview)
  - [Authentication](#authentication)
  - [Errors](#errors)
  - [Special Formats](#special-formats)
    - [DateTime](#datetime)
  - [Endpoints](#endpoints)
    - [`POST   /zv/login`](#post-zvlogin)
    - [`POST   /zv/logout`](#post-zvlogout)
    - [`POST   /zv/users`](#post-zvusers)
    - [`GET    /zv/users/{user_id}`](#get-zvusersuser_id)
    - [`PATCH  /zv/users/{user_id}`](#patch-zvusersuser_id)
    - [`DELETE /zv/users/{user_id}`](#delete-zvusersuser_id)
    - [`POST   /zv/users/{user_id}/team/{member_id}`](#post-zvusersuser_idteammember_id)
    - [`DELETE   /zv/users/{user_id}/team/{member_id}`](#delete-zvusersuser_idteammember_id)
    - [`GET    /zv/users/{user_id}/lists`](#get-zvusersuser_idlists)
    - [`POST   /zv/users/{user_id}/lists`](#post-zvusersuser_idlists)
    - [`GET    /zv/users/{user_id}/lists/{list_id}`](#get-zvusersuser_idlistslist_id)
    - [`PATCH  /zv/users/{user_id}/lists/{list_id}`](#patch-zvusersuser_idlistslist_id)
    - [`DELETE /zv/users/{user_id}/lists/{list_id}`](#delete-zvusersuser_idlistslist_id)
    - [`GET    /zv/users/{user_id}/lists/{list_id}/tags`](#get-zvusersuser_idlistslist_idtags)
    - [`POST   /zv/users/{user_id}/lists/{list_id}/tags`](#post-zvusersuser_idlistslist_idtags)
    - [`GET    /zv/users/{user_id}/lists/{list_id}/tags/{tag_name}`](#get-zvusersuser_idlistslist_idtagstag_name)
    - [`PUT    /zv/users/{user_id}/lists/{list_id}/tags/{tag_name}`](#put-zvusersuser_idlistslist_idtagstag_name)
    - [`DELETE /zv/users/{user_id}/lists/{list_id}/tags/{tag_name}`](#delete-zvusersuser_idlistslist_idtagstag_name)
    - [`GET    /zv/users/{user_id}/lists/{list_id}/comments`](#get-zvusersuser_idlistslist_idcomments)
    - [`POST   /zv/users/{user_id}/lists/{list_id}/comments`](#post-zvusersuser_idlistslist_idcomments)
    - [`PATCH  /zv/users/{user_id}/lists/{list_id}/comments/{comment_id}`](#patch-zvusersuser_idlistslist_idcommentscomment_id)
    - [`DELETE /zv/users/{user_id}/lists/{list_id}/comments/{comment_id}`](#delete-zvusersuser_idlistslist_idcommentscomment_id)
    - [`GET    /zv/users/{user_id}/lists/{list_id}/tasks`](#get-zvusersuser_idlistslist_idtasks)
    - [`POST   /zv/users/{user_id}/lists/{list_id}/tasks`](#post-zvusersuser_idlistslist_idtasks)
    - [`GET    /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}`](#get-zvusersuser_idlistslist_idtaskstask_id)
    - [`PUT    /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}`](#put-zvusersuser_idlistslist_idtaskstask_id)
    - [`DELETE /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}`](#delete-zvusersuser_idlistslist_idtaskstask_id)
    - [`GET    /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags`](#get-zvusersuser_idlistslist_idtaskstask_idtags)
    - [`POST   /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags`](#post-zvusersuser_idlistslist_idtaskstask_idtags)
    - [`GET    /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags/{tag_name}`](#get-zvusersuser_idlistslist_idtaskstask_idtagstag_name)
    - [`PUT    /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags/{tag_name}`](#put-zvusersuser_idlistslist_idtaskstask_idtagstag_name)
    - [`DELETE /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags/{tag_name}`](#delete-zvusersuser_idlistslist_idtaskstask_idtagstag_name)
    - [`GET    /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments`](#get-zvusersuser_idlistslist_idtaskstask_idcomments)
    - [`POST   /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments`](#post-zvusersuser_idlistslist_idtaskstask_idcomments)
    - [`PATCH  /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments/{comment_id}`](#patch-zvusersuser_idlistslist_idtaskstask_idcommentscomment_id)
    - [`DELETE /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments/{comment_id}`](#delete-zvusersuser_idlistslist_idtaskstask_idcommentscomment_id)
- [Media Types](#media-types)
  - [Login](#login)
  - [User](#user)
  - [Task List](#task-list)
  - [Task](#task)
  - [Comment](#comment)
  - [Error](#error)
  - [Miscellaneous](#miscelaneous)

## Client-Server API

### Overview

This API is exposed to the clients through a number of endpoints accepting JSON payloads.

### Authentication

Clients can get a Base64 encoded token in login and register process. This token should be provided when necessary as HTTP basic authorization header in the form of `Authorization: Basic {token}`. All request require this header unless otherwise noted. Requests that do not have this header, will result in a _401 Unauthorized_ response.

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

Retrieve an auth token.

> This request doesn't require `Authorization` header.

- Acceptable Request Content Types:
  - [`application/vnd.zv.login.creation+json`](#applicationvndzvlogincreationjson)
- Expectable Response Media Types:
  - [`application/vnd.zv.login.token+json`](#applicationvndzvlogintokenjson)
- Responses:
  - `200`: Login already exists
  - `201`: New login created

#### `POST /zv/logout`

Revoke auth token.

- Responses:
  - `204`: Token is revoked

#### `POST /zv/users`

Create a new user.

If creating a _regular_, request doesn't require `Authorization` header.

If creating an organization user, requires `Authorization` header. This means a user should already be logged in so he can create an organization.

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

#### `GET /zv/users/{user_id}`

Get any user by ID.

- Path Parameters:
  - `user_id`: id of the user
- Expectable Response Media Types:
  - [`application/vnd.zv.user.pretty+json`](#applicationvndzvuserprettyjson)
  - [`application/vnd.zv.user.full+json`](#applicationvndzvuserfulljson)
- Responses:
  - `200`

#### `PATCH /zv/users/{user_id}`

Partially update current authorized user information. Send a JSON dictionary of key-value pairs with valid keys: `first_name` and `last_name`.

- Path Parameters:
  - `user_id`: id of the current authorized user
- Acceptable Request Content Types:
  - `application/json`
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.user.pretty+json`](#applicationvndzvuserprettyjson)
  - [`application/vnd.zv.user.full+json`](#applicationvndzvuserfulljson)
- Responses:
  - `202`: User info updated
  - `204` (if `application/vnd.zv.empty` media type requested): User info updated

#### `DELETE /zv/users/{user_id}`

Remove current authorized user.

- Path Parameters:
  - `user_id`: id of the current authorized user
- Responses:
  - `204`: User removed

#### `POST /zv/users/{user_id}/team/{member_id}`

Add a member to an organization's team.

- Path Parameters:
  - `user_id`: id of the organization user
  - `member_id`: id of a new member
- Responses:
  - `201`: New member added

#### `DELETE /zv/users/{user_id}/team/{member_id}`

Remove a member from an organization.

- Path Parameters:
  - `user_id`: id of the organization user
  - `member_id`: id of a new member
- Responses:
  - `204`: Member removed

#### `GET /zv/users/{user_id}/lists`

Get all the task lists for current authorized user by ID. Response is a JSON array.

- Path Parameters:
  - `user_id`: id of the user
- Expectable Response Media Types:
  - [`application/vnd.zv.list.full+json`](#applicationvndzvlistfulljson)
- Responses:
  - `200`

#### `POST /zv/users/{user_id}/lists`

Create a new task list for current authorized user.

- Path Parameters:
  - `user_id`: id of the user
- Acceptable Request Content Types:
  - [`application/vnd.zv.list.creation+json`](#applicationvndzvlistcreationjson)
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.list.full+json`](#applicationvndzvlistfulljson)
- Responses:
  - `201`: List created
  - `204` (if `application/vnd.zv.empty` media type requested): List created

#### `GET /zv/users/{user_id}/lists/{list_id}`

Get a task list by its id.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Expectable Response Media Types:
  - [`application/vnd.zv.list.full+json`](#applicationvndzvlistfulljson)
- Responses:
  - `200`

#### `PATCH /zv/users/{user_id}/lists/{list_id}`

Update a task list with the specified `list_id`. Send a JSON dictionary of key-value pairs with valid keys: `id`, `title`, `description`, `public` and `tags`. Refer to [`application/vnd.zv.list.full+json`](#applicationvndzvlistfulljson) for their descriptions.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Acceptable Request Content Types:
  - `application/json`
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.list.full+json`](#applicationvndzvlistfulljson)
- Responses:
  - `200`: Task list updated
  - `201`: Task list created
  - `204` (if `application/vnd.zv.empty` media type requested): Task list created/updated

#### `DELETE /zv/users/{user_id}/lists/{list_id}`

Remove a task list.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Responses:
  - `204`: Task removed

#### `GET /zv/users/{user_id}/lists/{list_id}/tags`

Get all tags for a task list. Response is a JSON dictionary.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Expectable Response Media Types:
  - `application/json`
- Responses:
  - `200`

#### `POST /zv/users/{user_id}/lists/{list_id}/tags`

Add new tags to task list. Send a JSON dictionary. Tag names are case sensitive. Valid characters are _ASCII alphanumeric characters_, `_`, `.`, and `-`.

> Duplicate tag names are not allowed.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Acceptable Request Content Types:
  - `application/json`
- Expectable Response Media Types:
  - `application/json`
- Responses:
  - `201`: New tags are added to task list

#### `GET /zv/users/{user_id}/lists/{list_id}/tags/{tag_name}`

Get tag value for a task list by `tag_name`.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `tag_name`: name of the tag
- Expectable Response Media Types:
  - `text/plain`
- Responses:
  - `200`

#### `PUT /zv/users/{user_id}/lists/{list_id}/tags/{tag_name}`

Add new or update existing tag of a task list. Tag names are case sensitive. Valid characters are _ASCII alphanumeric characters_, `_`, `.`, and `-`.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `tag_name`: name of the tag
- Acceptable Request Content Types:
  - `text/plain`
- Responses:
  - `200`: Existing tag updated
  - `201`: New tag added

#### `DELETE /zv/users/{user_id}/lists/{list_id}/tags/{tag_name}`

Remove a tag on task list.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `tag_name`: name of the tag
- Responses:
  - `204`

#### `GET /zv/users/{user_id}/lists/{list_id}/comments`

Get all comments on a task list. Response is a JSON array of comment.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Expectable Response Media Types:
  - [`application/vnd.zv.comment.full+json`](#applicationvndzvcommentfulljson)
- Responses:
  - `200`

#### `POST /zv/users/{user_id}/lists/{list_id}/comments`

Post a comment on task list.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Acceptable Request Content Types:
  - `text/plain`
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.comment.full+json`](#applicationvndzvcommentfulljson)
- Responses:
  - `201`: Comment posted
  - `204` (if `application/vnd.zv.empty` media type requested): Comment posted

#### `PATCH /zv/users/{user_id}/lists/{list_id}/comments/{comment_id}`

Edit comment text

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `comment_id`: id of the comment
- Acceptable Request Content Types:
  - `text/plain`
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.comment.full+json`](#applicationvndzvcommentfulljson)
- Responses:
  - `200`: Comment text updated
  - `204` (if `application/vnd.zv.empty` media type requested): Comment text updated

#### `DELETE /zv/users/{user_id}/lists/{list_id}/comments/{comment_id}`

Redact a comment

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `comment_id`: id of the comment
- Responses:
  - `204`

#### `GET /zv/users/{user_id}/lists/{list_id}/tasks`

Get all the tasks in the task list. Response is a JSON array.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Expectable Response Media Types:
  - [`application/vnd.zv.task.pretty+json`](#applicationvndzvtaskprettyjson)
  - [`application/vnd.zv.task.full+json`](#applicationvndzvtaskfulljson)
- Responses:
  - `200`

#### `POST /zv/users/{user_id}/lists/{list_id}/tasks`

Create a new task on the list.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
- Acceptable Request Content Types:
  - [`application/vnd.zv.task.creation+json`](#applicationvndzvtaskcreationjson)
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.task.pretty+json`](#applicationvndzvtaskprettyjson)
  - [`application/vnd.zv.task.full+json`](#applicationvndzvtaskfulljson)
- Responses:
  - `201`: Task created
  - `204` (if `application/vnd.zv.empty` media type requested): Task created

#### `GET /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}`

Get a specific task from task list.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
- Expectable Response Media Types:
  - [`application/vnd.zv.task.full+json`](#applicationvndzvtaskfulljson)
- Responses:
  - `200`

#### `PUT /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}`

Add new or update existing task of a task list. Send a JSON dictionary of key-value pairs with valid keys: `id`, `title`, `description`, `due_by`, and `tags`.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
- Acceptable Request Content Types:
  - `application/json`
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.task.pretty+json`](#applicationvndzvtaskprettyjson)
  - [`application/vnd.zv.task.full+json`](#applicationvndzvtaskfulljson)
- Responses:
  - `200`: Existing task updated
  - `201`: New task created
  - `204` (if `application/vnd.zv.empty` media type requested): Task created/updated

#### `DELETE /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}`

Delete a task from task list.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
- Responses:
  - `204`: Task deleted

#### `GET /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags`

Get all tags for a task. Response is a JSON dictionary.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
- Expectable Response Media Types:
  - `application/json`
- Responses:
  - `200`

#### `POST /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags`

Add new tags to task. Send a JSON dictionary. Tag names are case sensitive. Valid characters are _ASCII alphanumeric characters_, `_`, `.`, and `-`.

> Duplicate tag names are not allowed.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
- Acceptable Request Content Types:
  - `application/json`
- Expectable Response Media Types:
  - `application/json`
- Responses:
  - `201`: New tags are added to task list

#### `GET /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags/{tag_name}`

Get tag value for a task by `tag_name`.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
  - `tag_name`: name of the tag
- Expectable Response Media Types:
  - `text/plain`
- Responses:
  - `200`

#### `PUT /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags/{tag_name}`

Add new or update existing tag of a task. Tag names are case sensitive. Valid characters are _ASCII alphanumeric characters_, `_`, `.`, and `-`.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
  - `tag_name`: name of the tag
- Acceptable Request Content Types:
  - `text/plain`
- Responses:
  - `200`: Existing tag updated
  - `201`: New tag added

#### `DELETE /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags/{tag_name}`

Remove a tag on task.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
  - `tag_name`: name of the tag
- Responses:
  - `204`

#### `GET /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments`

Get all comments on a task. Response is a JSON array of comment.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
- Expectable Response Media Types:
  - [`application/vnd.zv.comment.full+json`](#applicationvndzvcommentfulljson)
- Responses:
  - `200`

#### `POST /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments`

Post a comment on task.

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
- Acceptable Request Content Types:
  - `text/plain`
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.comment.full+json`](#applicationvndzvcommentfulljson)
- Responses:
  - `201`: Comment posted
  - `204` (if `application/vnd.zv.empty` media type requested): Comment posted

#### `PATCH /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments/{comment_id}`

Edit comment text

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
  - `comment_id`: id of the comment
- Acceptable Request Content Types:
  - `text/plain`
- Expectable Response Media Types:
  - [`application/vnd.zv.empty`](#applicationvndzvempty)
  - [`application/vnd.zv.comment.full+json`](#applicationvndzvcommentfulljson)
- Responses:
  - `200`: Comment text updated
  - `204` (if `application/vnd.zv.empty` media type requested): Comment text updated

#### `DELETE /zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments/{comment_id}`

Redact a comment

- Path Parameters:
  - `user_id`: id of the user
  - `list_id`: id of the task list
  - `task_id`: id of the task
  - `comment_id`: id of the comment
- Responses:
  - `204`

## Media Types

Each type might have different representations. Listed below are types, their various representations, and their custom mime-type names.

### Login

#### `application/vnd.zv.login.creation+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| user_name | string | ✔ | User name. User names are case insensitive. Valid characters are _ASCII alphanumeric characters_, `_`, `.`, and `-`. |
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
| joined_at | datetime | ✔ | The date account was created in UTC format. Time should be set to the midnight. |
| last_name | string |  | User's last name |
| team | string[] |  | If organization user, an array of members' user IDs |

#### `application/vnd.zv.user.pretty+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | User's ID |
| display_name | string | ✔ | User's name that contains first name and optional last name appended with a space character. |
| days_joined | number | ✔ | Number of days this user has joined. |
| is_org | boolean | | Indicates whether this is an organization user |

#### `application/vnd.zv.user.creation+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| name | string | ✔ | The desired user name |
| passphrase | string | ✔ | Passphrase as clear text |
| first_name | string | ✔ | User's first name |
| last_name | string |  | User's last name |
| members | string[] |  | If organization user, user ID of team members |

### Task List

#### `application/vnd.zv.list.full+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | Task List's ID |
| title | string | ✔ | A title/summary for list. 140 characters max. |
| created_at | datetime | ✔ | The date and time this task was created in UTC format. |
| description | string |  | More description for list |
| owner | string | | User id of this task list's owner. Present only if current authorized user is not the owner.  |
| public | boolean | | True if list is visible to every user and guest |
| contributors | string[] | | User id of contributors to this task list |
| comments | [`Comment`](#comment)[] | | A JSON array of comments for this task list |
| tags | Object | | A JSON dictionary of tags in the form of key-value pairs(string to string) |

#### `application/vnd.zv.list.creation+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| title | string | ✔ | A title/summary for task list. Should be 140 characters or less. |
| id | string | | Optional id for the task list. If not specified, server will generate one. IDs are case insensitive. Valid characters are _ASCII alphanumeric characters_, `_`, `.`, and `-`. |
| description | string |  | More description for list |
| tags | Object | | A JSON dictionary of key-value pairs(string to string). |

### Task

#### `application/vnd.zv.task.full+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | Task's ID |
| title | string | ✔ | A title/summary for task. Should be 140 characters or less. |
| created_at | datetime | ✔ | The date and time this task was created in UTC format. |
| description | string |  | More description for task |
| due_by | datetime | | The date and time this task would be due. Should always be at least 60 seconds after `created_at`. |
| comments | [`Comment`](#comment)[] | | A JSON array of comments |
| tags | Object | | A JSON dictionary of key-value pairs(string to string). |

#### `application/vnd.zv.task.pretty+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | Task's ID |
| title | string | ✔ | A title/summary for task. Should be 140 characters or less. |
| is_due | boolean | ✔ | Whether task's due date and time is passed. |
| description | string |  | More description for task |
| due_in | string | | Only if `is_due` is false, contains a user readable representation of the time left such as `2 days 5 minutes`. |

#### `application/vnd.zv.task.creation+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| title | string | ✔ | A title/summary for task. Should be 140 characters or less. |
| id | string | | Optional id for the task. If not specified, server will generate one. IDs are case insensitive. Valid characters are _ASCII alphanumeric characters_, `_`, `.`, and `-`. |
| description | string |  | More description for task |
| due_by | datetime | | The date and time this task would be due. Should always be at least 60 seconds after the time of making request. |
| tags | Object | | A JSON dictionary of key-value pairs(string to string). |

#### Comment

#### `application/vnd.zv.comment.full+json`

| Name  | Type | Required | Description |
| -- | :--: | :--: | -- |
| id | string | ✔ | Comment ID |
| by | string | ✔ | User ID of comment poster |
| posted_at | datetime | ✔ | The date and time this comment was posted |
| text | string | ✔ | Text of the comment |
| status | string | ✔ | "posted" by default. "edited" if comment text is modified. "redacted" if comment is deleted. |
| last_modified_at | datetime | | The date and time this comment was modified. Present only when comment is _edited_ or _redacted_. |

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

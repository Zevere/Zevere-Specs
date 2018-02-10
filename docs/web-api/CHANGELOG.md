# Zevere WebAPI Specification Changelog

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- Endpoint `/zv/users/{user_id}/lists`
- Endpoint `/zv/users/{user_id}/lists/{list_id}`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/tags`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/tags/{tag_name}`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/comments`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/comments/{comment_id}`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/tasks`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/tasks/{task_id}`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/tags/{tag_name}`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments`
- Endpoint `/zv/users/{user_id}/lists/{list_id}/tasks/{task_id}/comments/{comment_id}`
- Media type `Task List`
- Media type `Comment`
- Property `Task.tags`
- Property `Task.comments`

### Removed

- Endpoint `HEAD /zv/users/{user_id}`
- Endpoint `/zv/users/{user_id}/tasks`
- Endpoint `/zv/users/{user_id}/tasks/{task_id}`

## [v1-alpha1] - 2018-02-08

### Added

- Authentication section
- `/zv/login` endpoint
- `/zv/logout` endpoint
- Login media types

### Changed

- Require Auth for all necessary endpoints

## [0.0.1] - 2017-10-28

### Added

- Initial draft
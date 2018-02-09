# Zevere WebAPI Specification Changelog

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- `/zv/users/{user_id}/lists` endpoint
- `Task List` media types

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
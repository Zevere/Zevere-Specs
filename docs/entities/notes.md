# Zevere - Entities

## Entities

- Guest: An anonymous user(visitor) that only sees a task/list via a link shared with him.
- User
- Organization: A user can optionally create an organization. Organization has a team which is a group of users in that organization.
- Task List
- Task
- Tag: A set of key-value pairs associated with a list or a task. Tags provide metadata about the resource and can help with providing a better UX to the users.
- Comment: Each task or list could be seen as a thread with comments below it. Comments cannot be nested and are shown sorted by the post date. Comments could be redacted but never deleted. UI shows this as a redacted line of text.

## Entity Relationships

- Each user has zero or many task lists
- Each user has zero or many organizations
- Each organization has one and only one team
- The organization team has at least one member (organization owner by default)
- Each task list has zero or many tasks
- Each task list has zero or many comments
- Each task has zero or many comments

## Tag Conventions

In order to be consistent, it is important that client apps respect the - following tags to present a better view. All tag names starting with `_` are - assumed hidden.
However, this is a convention and clients can opt out of respecting them. - Below are a list of tag names with possible values.

- [Task List] `_kind` : schedule, project
- [Task] `_period`: `{from}-{to}` from & to are time based on UNIX epoch (for - schedule lists)
- [Task] `_kind` : bug, recurrent
- [Task] `_progress` : a number in range: [0, 1]
- [Task] `_stage` : backlog, todo, in_progress, done
- [Task] `_priority`: [ low, medium, high ] or a number (positive infinity means - highest priority)
- [Task] `_every`: `{occurrence}@{time}` (for recurring tasks)
  - `event`: day, week, year
  - `time`: only time in the datetime format ISO8601
- [Task] [Task List] `_icon` : name from font-awesome list

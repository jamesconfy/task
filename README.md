# Task Project

## Instructions on how the task project should look like

## USER

**Personal**

1. Provide email, password, (maybe a username), phone number, first name and last name.
2. JWT (JSON web token) can be used for authentication. Can be stored in a seperate table in the database and returned when a user logs in.

**USER SCHEMA**

```go
type User struct {
    UserId          UUID
    Email           String
    Password        String
    PhoneNumber     String
    AssistantOption Boolean
    AssistantID     String
    FirstName       String
    LastName        String
    DateCreated     Datetime
    DateUpdated     Datetime
}

type JWT struct {
    TokenId     UUID
    Token       String
    Expiry      Datetime
    UserId      UUID
}
```

**USER ROUTES**

1. [/users]() -> POST -> Create users
2. [/users/login]() -> POST -> Login users
3. [/users/:userId]() -> GET -> Get a user by it's id
4. [/users]() -> GET -> Get all users (Admin authorisatin)
5. [/users/:userId]() -> PATCH -> To update user profile (Admin authorisation)
6. [/users/:userId]() -> DELETE -> To delete a user profile (Admin authorisation)
7. [/users/change-password]() -> PATCH -> Change user password (User authorisation)
8. [/users/profile]() -> GET -> Get user profile (User authorisation)
9. [/users/profile]() -> PATCH -> To update user profile (User authorisation)
10. [/users/profile]() -> DELETE -> To delete a user profile (User authorisation)

## TASK

1. User needs to be logged in to access and create a task.
2. User can provide a task name, description, end-date (start-date will be the current time), attachments (if any) and maybe a comment attached to the task.
3. We check if any assistant is tied with the user, if any we are meant to send an email (with any attachment) to him/her.
4. The assistant can reply back to the user on his email and this email (with any attachment) will be displayed on the comment/message section.
5. Sub-tasks can be created also and attached to a task.

**TASK SCHEMA**

```go
type TaskObject struct {
    TaskId      UUID
    Name        String
    Description String
    EndDate     Datetime
    StartDate   Datetime
    Attachment  []Link
    DateCreated Datetime
    DateUpdated Datetime
}

type Task struct {
    TaskObject
    SubTask []TaskObject
}

type Link String (Can be stored on AWS and the link provided here)

type Comment struct {
    CommentId   UUID
    Comment     String
    TaskId      UUID
    DateCreated Datetime
    Attachment  []Link
}
```

**TASK ROUTES (User Authorisation)**

1. [/tasks]() -> POST -> Create task
2. [/tasks/:taskId]() -> GET -> Get a task by it's id
3. [/tasks/:taskId]() -> PATCH -> Update the details of a task
4. [/tasks/:taskId]() -> DELETE -> Delete a task
5. [/tasks/:taskId/comments]() -> POST -> Create a comment on task (Also assistant authorisation)
6. [/tasks/:taskId/comments]() -> GET -> Get all comments associated with a task (Also assistant authorisation)
7. [/tasks/:taskId/comments/:commendId] -> GET -> Get a comment (Also assistant authorisation)

## Admin/Assistant

**Admin**

1. Can create an assistant by providing email, password (optional) and username (also optional). A link to complete registration will be sent to the email of the user. Example of link/route: [/users/complete-registration?user_id=?]() .

**ADMIN SCHEMA**

```go
type Assistant struct {
    Email       String
    Password    String
    PhoneNumber String
    FirstName   String
    LastName    String
    DateCreated Datetime
    DateUpdated Datetime
    Role        String (either of admin or assistant, but admin should be for the main admin)
}
```

**ADMIN ROUTES (Admin Authorisation)**

1. [/assistants]() -> POST -> Create an assistant
2. [/assistants]() -> GET -> Get all assistants
3. [/assistants/:assistantId]() -> GET -> Get an assistant with it's id
4. [/assistants/:assistantId]() -> DELETE -> Delete an assistant
5. [/assistants/:assistantId]() -> PATCH -> Update an assistant

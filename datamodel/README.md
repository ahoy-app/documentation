# Data Model

The text naming style follows the [casex](https://github.com/pedsmoreira/casex) convention.

## Room

Represents an container of ordered messages where certain users has access to. It is administrated by zero, one or more users depending of it's type.

```js
{
  id: uuid || hash(User["id"]+User["id"]),
  type: 'group' || 'duo' || 'system',
  name: 'Na me',
  admin: [User["id"]],
  members: [User["id"]],
}
```

Depending on the type of the room, the restrictions and characteristics are different:

### Group Room

This room holds a group of people. Is managed by a single admin who is responsible to invite the rest of the members.

```js
{
  id: uuid,
  type: 'group',
  name: 'Na me',
  admin: [User["id"]],
  members: [User["id"]],
}
```

- Group room has a limit of **1 admin** and **500 members**

### Duo Room

This room holds a conversation between two individuals. Is managed by them.

```js
{
  id: hash(User["id"]+User["id"]),
  type: 'duo',
  name: 'Na me',
  admin: [User["id"], User["id"]],
  members: [User["id"]],
}
```

- The id will be the a hash of the **ids of the two users sorted alphabetically and concatenated**

- The admins will be the **two users**

### System Room

This special room will be used for broadcasting and other future use cases

```js
{
  id: uuid,
  type: 'system',
  name: 'Na me',
  admin: [],
  members: [],
}
```

- There will be no admin because the **access control to this rooms will be _role-based_**

## User

It contains all the relevant information about a system's registered user

```js
{
  id: uuid,
  name: 'na_me',
  role: 'user' || 'admin',
  crypto: {
    public: 'public_key',
    private: passkey('my_key', 'private_key'),
  }
}
```

- `pk` is stored when

## Message

It represents the message

```js
{
  id: uuid,
  from: User["id"],
  to: Room["id"],
  type: 'text' || 'image' || 'video',
  content: 'text',
}
```

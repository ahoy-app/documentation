# Requirements

## User

### Info

- [x] Chooses a _User_ `nick name`
- [ ] Authenticates vía **Open ID Connect** provided `id`
- [ ] **Chooses a secret `password`** to store it's _private key_

### Messaging

- [x] **Send _Messages_** to a _Room_
- [x] **Receive _Messages_** from a _Room_ they are in
- [ ] Receives the messages that arrived **when disconnected**

### Rooms Admin

#### Duo

- [ ] **Create** a _Duo Room_ **knowing** the _User_ `nick name`
- [ ] **Leave a _Duo Room_** they where invited to or they created

#### Group

- [ ] **Create** an **empty** _Group Room_
- [ ] **Invite members** to the _Group Room_ **knowing** their `nick name`
- [ ] **Leave a _Group Room_** they where invited to
- [ ] **Delete a _Group Room_** they created

## Messages

- [x] Arrive **pushed** from server
- [x] Go from sender to receiver in **less than 60s**
- [x] Displays to _Users_ **its content**
- [x] Displays to _Users_ **the `nick name`** of the _User_ who sent it
- [x] Displays to _Users_ **the _Room_** where it's sent to
- [ ] Content is maximum **`500` chars** long
- [ ] Can have **attached files**
- [ ] **Persists** across sessions
- [ ] Content and attached files are **encrypted _end-to-end_**

## Rooms

- [x] Have a `name`
- [x] Just the joined _Users_ can read the messages
- [ ] Are accessible **vía unique URL**
- [ ] Have a max capacity of `1000` _Users_

## Admin

- [x] Broadcast messages to all _Users_

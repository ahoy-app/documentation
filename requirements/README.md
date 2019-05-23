# Requirements

## User

### Info

- [x] Chooses a _User_ `nick name`
- [x] Authenticates vía **Open ID Connect** provided `id`
- [x] ~~**Chooses a secret `password`** to store it's _private key_~~

### Messaging

- [x] **Send _Messages_** to a _Room_
- [x] **Receive _Messages_** from a _Room_ they are in
- [x] Receives the messages that arrived **when disconnected**

### Rooms Admin

#### Duo

- [x] **Create** a _Duo Room_ **knowing** the _User_ `nick name`
- [x] **Leave a _Duo Room_** they where invited to or they created

#### Group

- [x] **Create** an **empty** _Group Room_
- [x] **Invite members** to the _Group Room_ **knowing** their `nick name`
- [x] **Leave a _Group Room_** they where invited to
- [x] **Delete a _Group Room_** they created

## Messages

- [x] Arrive **pushed** from server
- [x] Go from sender to receiver in **less than 60s**
- [x] Displays to _Users_ **its content**
- [x] Displays to _Users_ **the `nick name`** of the _User_ who sent it
- [x] Displays to _Users_ **the _Room_** where it's sent to
- [x] Content is maximum **`500` chars** long
- [x] Can have **attached files**
- [x] **Persists** across sessions
- [ ] ~~Content and attached files are **encrypted _end-to-end_**~~

## Rooms

- [x] Have a `name`
- [x] Just the joined _Users_ can read the messages
- [x] Are accessible **vía unique URL**
- [x] Have a max capacity of `1000` _Users_

## Admin

- [x] Broadcast messages to all _Users_
- [x] Censorship
- [x] Topics

# Extra requirements

- [x] Multiple concurrent users
- [x] Deployed in >5 nodes entirely on cloud
- [x] Centralized configuration

# Documentation

- [ ] Architectural choices
- [ ] Messaging brokers
- [ ] Use cases
- [ ] Open API
- [ ] Economics plan

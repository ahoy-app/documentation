# Acceptance Tests

## User

### Log In with GitHub

> - **Login** with GitHub with `you account`
>   - Expect the login to be successful

## Rooms

### Basic

> - **Create** a `Duo Room` with the user `mike`
>   - **Fetch** the `Room` `information`
>   - Expect the `Room` to **be created**
>   - Expect the `Room` to **have admins** `you account` and `mike`

> - **Create** a `Group Room` with a the title `San Pepe`
>   - **Fetch** the `Room` `information`
>   - Expect the `Room` to **be created**
>   - Expect the `Room` to **have admin** `you account`
>   - Expect the `Room` to **have** `0` **members**

> - **Invite** other `mike`, `tom`, `hannah` and `lisa` to `San Pepe`
>   - **Fetch** the `Room` `information`
>   - Expect the `Room` to **be created**
>   - Expect the `Room` to **have admin** `you account`
>   - Expect the `Room` to **have members** `mike`, `tom`, `hannah` and `lisa`

> - **Copy** the `URL` of the `Room`
>   - **Open** a new `Browser Tab`
>   - **Paste** the `URL` of the `Room`
>   - Expect the `Browser` to open in the `Room`

### Admin

> - **Login** with `lisa` on a `separate context`
>   - **Kick-out** `lisa` from `San Pepe` with `your account`
>   - Expect `lisa` to **be removed from the room on push**
>   - **Send** a `Text Message` with `your account` to `San Pepe`
>   - Expect `lisa` to **not receive it**

> - **Login** with `lisa` on a `separate context`
>   - **Invite** `lisa` to `San Pepe` with `your account`
>   - Expect `lisa` to **be added** to the room on push
>   - **Send** a `Text Message` with `your account` to `San Pepe`
>   - Expect `lisa` to **receive it**

> - **Login** with `lisa` on a `separate context`
>   - **Try to kick-out** `tom` from `San Pepe` with `lisa`
>   - Expect `lisa` to **be rejected**
>   - Expect `tom` to **be still in the room**

## Messages

### Text

> - **Send** a `Text Message` `500 chars long` to `mike`
>   - Then **login** with `mike`
>   - Expect the `Message` to **arrive even when he was disconnected**
>   - Expect the `Message` to **arrive in less than 60 seconds**

> - **Login** with `tom` on a `separate context`
>   - **Send** a `Text Message` `500 chars long` with `your account` to `San Pepe`
>   - Expect the `Message` to **arrive on push** to `tom`
>   - Expect the `Message` to **arrive in less than 60 seconds**

### Attachment

> - **Send** an `Attachment Message` with a `1MB` `file` to `mike`
>   - Then **login** with `mike`
>   - Expect the `Message` to **arrive** even **when he was disconnected**
>   - Expect the `Message` to **arrive in less than 60 seconds**
>   - **Click** the link in the `Message`
>   - Expect the `Browser` to **download** the `file`

> - **Login** with `tom` on a `separate context`
>   - **Send** an `Attachment Message` with a `1MB` `file` with `your account` to `San Pepe`
>   - Expect the `Message` to **arrive on push** to `tom`
>   - Expect the `Message` to **arrive in less than 60 seconds**
>   - **Click** the link in the `Message`
>   - Expect the `Browser` to **download** the `file`

### Broadcast

> - **Login** with `admin` on a `separate context`
>   - **Send** a `Text Message` with `admin` to `ahoy`

### Censorship

> - **Send** a `Text Message` to `San Pepe` saying `What do you think about blockchain?`
>   - **Add** `blockchain` to `Danger Word` list
>   - **Send** a `Text Message` to `San Pepe` saying `I think that Blockchain is the future`
>   - Expect the `Message` to **arrive censored**
>   - **Check** the `Process Logs`
>   - Expect the `Process Logs` to **report the issue**

### Trending Topics

> - **Access** to `Ahoy Trends`
>   - Expect `top trends` to **appear sorted by popularity**

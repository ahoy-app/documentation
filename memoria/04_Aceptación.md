# Criterios de Aceptación

El objetivo de este proyecto era cumplir todos los requisitos que fueran compatibles propuestos por el Product Owner. Estos requisitos son, la combinación del desglose de las historias de usuario con los requisitos extra sobre la propia plataforma y su documentación.

Para corroborar el grado de cumplimiento de los requisitos, se han definido unas pruebas de aceptación manuales que cubren todos los requisitos de las historias de usuario.

## Pruebas de aceptación

### Usuario

#### Log In con GitHub

##### U-1

> - **Login** with GitHub with `you account`
>   - Expect the login to be successful

### Sala

#### Base

##### S-B-1

> - **Create** a `Duo Room` with the user `mike`
>   - **Fetch** the `Room` `information`
>   - Expect the `Room` to **be created**
>   - Expect the `Room` to **have admins** `you account` and `mike`

##### S-B-2

> - **Create** a `Group Room` with a the title `San Pepe`
>   - **Fetch** the `Room` `information`
>   - Expect the `Room` to **be created**
>   - Expect the `Room` to **have admin** `you account`
>   - Expect the `Room` to **have** `0` **members**

##### S-B-3

> - **Invite** other `mike`, `tom`, `hannah` and `lisa` to `San Pepe`
>   - **Fetch** the `Room` `information`
>   - Expect the `Room` to **be created**
>   - Expect the `Room` to **have admin** `you account`
>   - Expect the `Room` to **have members** `mike`, `tom`, `hannah` and `lisa`

##### S-B-4

> - **Copy** the `URL` of the `Room`
>   - **Open** a new `Browser Tab`
>   - **Paste** the `URL` of the `Room`
>   - Expect the `Browser` to open in the `Room`

#### Administrador de la sala

##### S-A-1

> - **Login** with `lisa` on a `separate context`
>   - **Kick-out** `lisa` from `San Pepe` with `your account`
>   - Expect `lisa` to **be removed from the room on push**
>   - **Send** a `Text Message` with `your account` to `San Pepe`
>   - Expect `lisa` to **not receive it**

##### S-A-2

> - **Login** with `lisa` on a `separate context`
>   - **Invite** `lisa` to `San Pepe` with `your account`
>   - Expect `lisa` to **be added** to the room on push
>   - **Send** a `Text Message` with `your account` to `San Pepe`
>   - Expect `lisa` to **receive it**

##### S-A-3

> - **Login** with `lisa` on a `separate context`
>   - **Try to kick-out** `tom` from `San Pepe` with `lisa`
>   - Expect `lisa` to **be rejected**
>   - Expect `tom` to **be still in the room**

### Mensaje

#### Texto

##### M-T-1

> - **Send** a `Text Message` `500 chars long` to `mike`
>   - Then **login** with `mike`
>   - Expect the `Message` to **arrive even when he was disconnected**
>   - Expect the `Message` to **arrive in less than 60 seconds**

##### M-T-2

> - **Login** with `tom` on a `separate context`
>   - **Send** a `Text Message` `500 chars long` with `your account` to `San Pepe`
>   - Expect the `Message` to **arrive on push** to `tom`
>   - Expect the `Message` to **arrive in less than 60 seconds**

#### Attachment

##### M-A-1

> - **Send** an `Attachment Message` with a `1MB` `file` to `mike`
>   - Then **login** with `mike`
>   - Expect the `Message` to **arrive** even **when he was disconnected**
>   - Expect the `Message` to **arrive in less than 60 seconds**
>   - **Click** the link in the `Message`
>   - Expect the `Browser` to **download** the `file`

##### M-A-2

> - **Login** with `tom` on a `separate context`
>   - **Send** an `Attachment Message` with a `1MB` `file` with `your account` to `San Pepe`
>   - Expect the `Message` to **arrive on push** to `tom`
>   - Expect the `Message` to **arrive in less than 60 seconds**
>   - **Click** the link in the `Message`
>   - Expect the `Browser` to **download** the `file`

#### Broadcast

##### M-B-1

> - **Login** with `admin` on a `separate context`
>   - **Send** a `Text Message` with `admin` to `ahoy`

#### Censura

##### M-C-1

> - **Send** a `Text Message` to `San Pepe` saying `What do you think about blockchain?`
>   - **Add** `blockchain` to `Danger Word` list
>   - **Send** a `Text Message` to `San Pepe` saying `I think that Blockchain is the future`
>   - Expect the `Message` to **arrive censored**
>   - **Check** the `Process Logs`
>   - Expect the `Process Logs` to **report the issue**

#### Trending Topics

##### M-X-1

> - **Access** to `Ahoy Trends`
>   - Expect `top trends` to **appear sorted by popularity**

## Historias de Usuario

### Como usuario Quiero poder enviar mensajes a otros usuarios Para tener conversaciones puntuales

| Requisito                                     | Cubierto por  |
| --------------------------------------------- | ------------- |
| 500 chars                                     | M-T-1         |
| Punto a punto sabiendo el id del otro usuario | S-B-1         |
| Recibir mensajes cuando no estabas conectado  | M-T-1         |
| Recepción on push                             | M-T-2         |
| Compartir mensajes entre al menos 5 usuarios  | S-B-3 y S-A-2 |
| Menos de 60 segundos de latencia              | Transversal   |

### Como usuaria Quiero poder compartir ficheros de mi equipo con otros usuarios Para complementar las conversaciones con información adicional

| Requisito                | Cubierto por |
| ------------------------ | ------------ |
| Ficheros de al menos 1Mb | M-A-1        |

### Como usuario Quiero poder crear salas de chat permanentes Para tener un registro de las conversaciones pasadas

| Requisito                                                      | Cubierto por  |
| -------------------------------------------------------------- | ------------- |
| Sólo el admin puede invitar y echar                            | S-A-1 y S-A-2 |
| Sólo los usuarios invitados pueden leerlos mensajes de la sala | S-B-3         |
| Acceder a salas por su URL única                               | S-B-4         |
| Mensajes persistentes                                          | M-T-1         |

### Como superusuario Quiero poder enviar mensajes a todos los usuarios suscritos Para anuncios de interés y publicidad de nuevas características del sistema

| Requisito                      | Cubierto por |
| ------------------------------ | ------------ |
| Broadcast de mensajes de texto | M-B-1        |

### Como superusuaria Quiero poder censurar algunos mensajes Para cumplir con los requisitos legales exigidos en regímenes represivos con las libertades civiles pero con mercados económicamente rentables

| Requisito                                                                           | Cubierto por |
| ----------------------------------------------------------------------------------- | ------------ |
| Censura de las palabras peligrosas configuradas                                     | M-C-1        |
| Registro de cada mensaje censurado incluyendo contenido, emisor y sala destinataria | M-C-1        |
| Reconfigurar las palabras peligrosas afecta en tiempo real al envío de mensajes     | M-C-1        |

### Como superusuario Quiero saber en tiempo real de qué hablan los usuarios Para poder calcular trending topics etc. y vender esa información al mejor postor

| Requisito                                             | Cubierto por    |
| ----------------------------------------------------- | --------------- |
| Lista de trends generada en tiempo real               | M-X-1           |
| Actualización en tiempo real de la aplicación cliente | No implementada |

## Requisitos extra

| Requisito                                                         | Cubierto por                  |
| ----------------------------------------------------------------- | ----------------------------- |
| Varios usuarios concurrentes.                                     | S-A-2 y M-A-2                 |
| Despliegue en, al menos, 5 máquinas. Las 5 en proveedores cloud.  | Diagrama de despliegue        |
| API REST documentada en OpenAPI.                                  | Enlazado anteriormente        |
| Configuración centralizada del sistema.                           | Dashboard de Heroku           |
| Uso de HTTP2 sobre TLS.                                           | HTTP2 no soportado por Heroku |
| Autentificación OAuth con GitHub.                                 | U-1                           |
| Documentar decisiones arquitecturales: Vista de CyC y Despliegue. | Mostrado anteriormente        |
| Documentar patrones EIP.                                          | Mostrado anteriormente        |
| Diagramas de comportamiento.                                      | Mostrado anteriormente        |
| Costes operativos del sistema en proveedores cloud.               | Mostrado a continuación       |

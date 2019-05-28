# Interfaces del Sistema

Algunas de las interfaces presentadas a continuación ya han sido introducidas en el apartado de Arquitectura, no obstante, se recopilan aquí de nuevo para una mejor comprensión.

## API REST

La API REST está especificada en OpenAPI 3.0 en la siguiente [dirección](https://editor.swagger.io/?url=https://raw.githubusercontent.com/ahoy-app/documentation/master/openapi.yaml).

La única consideración a hacer al respecto una violación del diseño del verbo DELETE, en la cual hay un uso no permitido de un cuerpo de mensaje (body). Esto se debe a que la API fue diseñada antes de aprender el uso de la herramienta Open API. De haberse conocido esta antes y haberse usado para el diseño, el resultado habría sido, sin duda, una API mejor y más coherente con los principios REST.

## Middlewares

Los middlewares, como ya se ha comentado, siguen un patrón impuesto por el framework Express donde toman un objeto request y uno response así como una función para devolver el control al framework.

Un ejemplo de un middleware (simplificado) sería.

```js
function auth(req, res, next) {
  if (checkUser(req.body.user)) {
    req.user = getUser(req.body.user);
    next();
  } else {
    res.status(403);
  }
}
```

Es por esto que no tiene sentido especificar la API de cada uno al compartir todos el método de llamada. En su lugar, cabe destacar las mutaciones que hacen a esos objetos. Los middlewares más interesantes son:

- Morgan: Un logger que registra cada petición HTTP
- CORS: Gestiona las cabeceras de seguridad para Cross Origin access
- BodyParser: Parsea los parámetros de la query y del el body en objetos JSON accesibles desde `req.body`
- censoreMessages + mung: Un middleware especial que actúa sobre la respuesta del Controller, en este caso, para censurar las palabras peligrosas.
- multer: intercepta mensajes de tipo `multipart/form-data` y crea un buffer de lectura para consumir el stream de bytes.
- dispatch: crea una función `dispatch(event)` que: crea un canal AMQP, envía el mensaje/evento `event` y cierra la conexión. Al hacer disponible esta función desde `req.dispatch`, los controller pueden emitir mensajes sin tener que conocer los detalles de la conexión con AMQP.
- verifyHTTPRequest: extrae el token de la URL o de las cabeceras HTTP y verifica su validez. Si es válido, escribe la información del usuario en `req.userId` y `req.admin`.

## Controller API

Las funciones de los controladores tienen un mapeo 1:1 con las funciones de la API REST. Cada endpoint representa una acción o caso de uso y tiene disponible una función en un controlador. De modo similar a cómo funcionan los middlewares en Express, estas funciones han de cumplir también con unas restricciones en cuanto a los parámetros que reciben.

```js
function hello(req, res) {
  res.send(`Hello ${req.body.name}`);
}
```

Lo realmente interesante es conocer los atributos que utilizan y estos son los atributos disponibles en el body/query o en la URI (extraídos mediante patrones del tipo `/rooms/{roomId}`). Por ello, se considera que la especificación de OpenAPI describe las funciones de los Controladores mejor que su propia API y por ello se considera redundante profundizar en ellas más allá de un breve listado.

| Método | Ruta                        | Función                                |
| ------ | --------------------------- | -------------------------------------- |
| POST   | /login:                     | AuthController.login                   |
| GET    | /oauth/redirect:            | AuthController.oauth_redirect          |
| POST   | /rooms:                     | RoomController.newGroupRoom            |
| POST   | /rooms/duo:                 | RoomController.newDuoRoom              |
| GET    | /rooms:                     | RoomController.getAllRooms             |
| GET    | /room/:roomId:              | RoomController.getRoom                 |
| DELETE | /room/:roomId:              | RoomController.deleteRoom              |
| PUT    | /room/:roomId/members:      | RoomController.inviteUser              |
| DELETE | /room/:roomId/members:      | RoomController.kickoutUser             |
| GET    | /room/ahoy/messages:        | MessageController.getBroadcastMessages |
| POST   | /room/ahoy/messages:        | MessageController.broadcastMessage     |
| GET    | /room/:roomId/messages:     | MessageController.getMessages          |
| GET    | /room/:roomId/file/:fileId: | MessageController.getFile              |
| POST   | /room/:roomId/messages:     | MessageController.postMessage          |
| GET    | /user:                      | UserController.getLoggedUser           |
| GET    | /user/:userId:              | UserController.getUser                 |

## Objetos del Modelo

Los objetos del modelo son implementados a través de el decorador de Mongoose.model (librería de mapeo objeto-documental). Por ello, todos los objetos instanciados comparten funciones genéricas como `.save()`. Además, la propia clase del objeto depresentado adquiere funciones de búsqueda típicas de un repositorio como `findById` o un `find()` genérico que acepta una query JSON para MongoDB. La verdadera "lógica" en estos objetos radica en su constructor y sus funciones de validación, que garantizan que el objeto creado o transformado es correcto. Muchas de estas funciones de validación, sin embargo son definidas declarativamente y es Mongoose quien las implementa.

Además, en el caso de las salas de chat, al poder ser de diversos tipos, se ha abstraído su construcción a funciones factoría que replican la estructura de los constructores, que requiriendo menos parámetros (al decidir estas el resto de ellos).

Las clases y los constructores/factorías resultantes son:

### Message

Los objetos de tipo Mensaje representan los mensajes enviados entre los Usuarios del sistema y pertenecen a una Sala concreta.

Función factoría: `createMessage({ from, to, content, type })`

- from: Usuario emisor.
- to: Sala destinataria.
- content: Contenido del mensaje (textual o el id del Attachment).
- type: Tipo del mensaje (text o file).

### Room

Los objetos de tipo Sala representan espacios lógicos a los que pertenecen los Usuarios y donde se envían Mensajes.

Función factoría de Sala Duo: `createDuoRoom({ members = [] })`

- members: Los dos Usuarios que formarán la Sala Duo. No importa el orden.

Función factoría Sala de Grupo: `createGroupRoom({ admin, name })`

- admin: el Usuario que ha creado la Sala
- name: el nombre de la Sala

### User

Los objetos de tipo Usuario representan a los usuarios que utilizan la aplicación de chat y permiten calcular sus permisos de acceso.

Constructor: `constructor({_id, name, role})`

- \_id: el ID único del Usuario. En login con GitHub se usará el correo electrónico primario.
- name: el nombre del usuario. En login con GitHub se usará el nick.
- role: el Usuario rol del usuario (user o admin)

### Attachment

Los objetos de tipo Attachment representan los ficheros adjuntos en mensajes de tipo file.

Constructor: `constructor(metadata)`

- metadata: Contiene información como el nombre del fichero o el ID de la Sala a la que pertenece el Mensaje que lo contiene.

Método: `write(buffer)`

- buffer: El buffer de bytes que va a ser escrito en la base de datos

Método: `read(): buffer`

- buffer: El buffer de bytes del que poder leer de la base de datos

## Eventos

El sistema de eventos está inspirado en los patrones propuestos por la comunidad de Redux. En ésta, los eventos (llamados acciones) son objetos JSON planos con una estructura concreta. Además, una de las buenas prácticas es encapsular su creación en métodos parametrizados para garantizar que estén siempre bien formados.

El resultado de esto es un paquete de funciones que devuelven objetos evento, no los emiten. Para su emisión, se ha de tener acceso a un método `dispatch` previamente configurado (por ejemplo, en un middleware). De este modo, para emitir un evento que informe de que se ha creado un usuario habría que invocar: `req.dispatch(user_created(userId))`.

El listado de funciones creadoras de eventos es:

| Function                     | Event                                                      |
| ---------------------------- | ---------------------------------------------------------- |
| user_authenticated(user)     | `{ key:"auth.user_authenticated", body: user }`            |
| user_created(user)           | `{ key:"auth.user_created", body: user }`                  |
| new_message(message)         | `{ key:"room.\${message.to}.new_message", body: message }` |
| room_deleted(roomId)         | `{ key:"room.\${roomId}.deleted", body: { roomId } }`      |
| room_created(room)           | `{ key:"room.\${room.id}.created", body: room }`           |
| user_invited((userId, room)) | `{ key:"user.\${userId}.invited", body: room }`            |
| user_kicked((userId, room))  | `{ key:"user.\${userId}.kicked", body: room }`             |

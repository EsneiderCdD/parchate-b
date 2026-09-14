# Masterclass 1: mongoose - Conexión y Fundamentos

## mongoose y su relación con MongoDB

MongoDB es una base de datos NoSQL que almacena información en documentos JSON. Para interactuar con ella desde Node.js, necesitamos una herramienta que se encargue de la comunicación entre nuestro código y el servidor de base de datos. Aquí es donde entra mongoose.

mongoose es un **ODM** (Object Data Modeling) para MongoDB y Node.js. Un ODM es una capa de abstracción que nos permite trabajar con la base de datos utilizando objetos y métodos de JavaScript, en lugar de escribir consultas en el lenguaje nativo de MongoDB. Esto no significa que mongoose oculte MongoDB, sino que nos proporciona una API más intuitiva y estructurada para trabajar con él.

La diferencia entre un ODM y un ORM (Object-Relational Mapping) es importante. Un ORM se utiliza para bases de datos relacionales (como MySQL o PostgreSQL) y mapea tablas a clases. Un ODM, como mongoose, trabaja con bases de datos noSQL (como MongoDB) y mapea documentos a objetos. mongoose no intenta ser un ORM para MongoDB porque MongoDB no es una base de datos relacional.

## ¿Por qué mongoose?

Podríamos conectar Node.js directamente con MongoDB utilizando el driver oficial de MongoDB para Node.js. Sin embargo, esto implicaría escribir consultas en el lenguaje nativo de MongoDB, manejar la validación de datos manualmente, y trabajar sin las ventajas que un ODM proporciona.

mongoose nos ofrece varias ventajas significativas. Primero, nos permite definir **schemas** que describen la estructura de los documentos, lo que proporciona una capa de validación automática. Segundo, nos da acceso a **models** que encapsulan la lógica de acceso a datos, haciendo el código más organizado y mantenible. Tercero, proporciona herramientas para manejar relaciones entre documentos, validaciones complejas, y operaciones de consulta de manera más fluida.

En el contexto de nuestro proyecto, mongoose no solo nos permite conectarnos a MongoDB, sino que también será la base sobre la cual construyamos el modelo de datos de la aplicación: eventos, creadores, usuarios, solicitudes.

## La conexión: mongoose.connect

El punto de entrada de mongoose en cualquier proyecto es la función `mongoose.connect`. Esta función establece la conexión con el servidor de MongoDB utilizando una URI (Uniform Resource Identifier) que describe exactamente a qué base de datos conectarse.

La URI de MongoDB tiene un formato específico:

```
mongodb://usuario:password@host:puerto/nombre-base-datos
```

En desarrollo local, cuando no hay autenticación configurada, la URI es más simple:

```
mongodb://localhost:27017/personaje
```

Esta URI indica que nos conectaremos al servidor local (`localhost`) en el puerto `27017`, y utilizaremos la base de datos llamada `personaje`.

En el proyecto mern-b, la URI está almacenada en el archivo `.env` como variable de entorno, y se accede a ella mediante `process.env.MONGO`. Esto es exactamente lo que configuramos en la masterclass anterior con dotenv.

## Proceso de conexión

El proceso de conexión con mongoose sigue un patrón consistente. Primero, importamos mongoose en nuestro archivo principal:

```javascript
const mongoose = require('mongoose');
```

Luego, definimos una función asíncrona que se encargue de establecer la conexión:

```javascript
async function connectDB(url) {
    try {
        await mongoose.connect(url);
        console.log('Conectado a la Base de Datos');
    } catch (e) {
        console.error(e);
        process.exit(1);
    }
}
```

Finalmente, llamamos a esta función pasando la URI de conexión:

```javascript
await connectDB(process.env.MONGO);
```

La función `mongoose.connect` devuelve una Promise, lo que significa que podemos usar `async/await` para manejarla de manera limpia. El bloque `try/catch` nos permite manejar errores de conexión de forma controlada: si la conexión falla, el mensaje de error se muestra en la consola y la aplicación se cierra gracefulmente con `process.exit(1)`.

## Manejo de errores en la conexión

El manejo de errores es fundamental en cualquier aplicación que se conecte a una base de datos. Si el servidor de MongoDB no está disponible, si las credenciales son incorrectas, o si hay problemas de red, la conexión fallará. Sin un manejo apropiado de errores, la aplicación podría comportarse de manera impredecible.

En el ejemplo anterior, el bloque `try/catch` captura cualquier error que ocurra durante la conexión. El `console.error(e)` muestra el error en la consola para debugging, y `process.exit(1)` termina la aplicación con un código de error. En un entorno de producción, podríamos querer implementar una estrategia de reconexión automática o enviar alertas al equipo de desarrollo.

mongoose también proporciona eventos que nos permiten monitorear el estado de la conexión. Podemos escuchar eventos como `connected`, `error`, y `disconnected` para tomar decisiones basadas en el estado de la conexión:

```javascript
mongoose.connection.on('connected', () => {
    console.log('Mongoose conectado a MongoDB');
});

mongoose.connection.on('error', (err) => {
    console.error('Error de conexión de Mongoose:', err);
});

mongoose.connection.on('disconnected', () => {
    console.log('Mongoose desconectado de MongoDB');
});
```

Estos eventos son especialmente útiles en producción, donde necesitamos saber exactamente qué está ocurriendo con la conexión a la base de datos.

## Más allá de la conexión: una vista previa

Aunque en esta etapa solo necesitamos establecer la conexión, es importante entender que mongoose ofrece mucho más. Cuando profundicemos en mongoose, aprenderemos sobre **schemas** y **models**, que son los conceptos fundamentales para trabajar con datos en mongoose.

Un **schema** define la estructura de los documentos: qué campos tiene un documento, qué tipo de datos contienen, y qué restricciones debén cumplir. Un **model** es una interfaz construida sobre el schema que nos permite realizar operaciones de CRUD (Crear, Leer, Actualizar, Eliminar) en la base de datos.

En el proyecto mern-b, ya existe un ejemplo de esto en `model/Persona.js`:

```javascript
const personaSchema = new mongoose.Schema({
    fecha: Date,
    nombre: String,
    imagen: String,
    edad: Number,
    amigos: Array,
    gustos: Object,
});

const Persona = mongoose.model('Persona', personaSchema, 'maria');
```

Aquí vemos cómo se define un schema con diferentes tipos de datos, y cómo se crea un model a partir de ese schema. El tercer argumento `'maria'` especifica el nombre de la colección en MongoDB, que es diferente al nombre del model.

## Conexión local vs. producción

Es importante comprender la diferencia entre la conexión local y la conexión en producción. En desarrollo local, nos conectamos a un servidor MongoDB que se ejecuta en nuestra computadora, sin autenticación, en un puerto conocido. En producción, nos conectaremos a MongoDB Atlas, un servicio de base de datos en la nube que requiere autenticación, tiene restricciones de acceso por IP, y utiliza conexiones seguras.

Esta transición de local a producción es un tema que abordaremos más adelante en el proyecto. Por ahora, es suficiente comprender que la URI de conexión es el puente entre nuestra aplicación y la base de datos, y que esta URI puede cambiar dependiendo del entorno.

## Resumen

mongoose es el ODM que nos permite conectar y trabajar con MongoDB desde Node.js. La conexión se establece mediante `mongoose.connect(url)`, que es una operación asíncrona que debemos manejar con `async/await` y `try/catch`. La URI de conexión se almacena en variables de entorno (gracias a dotenv) y contiene toda la información necesaria para conectarse a la base de datos.

Aunque en esta etapa solo hemos cubierto la conexión, hemos sentado las bases para comprender cómo mongoose funciona. En futuras masterclass, profundizaremos en schemas, models, validaciones, y todas las herramientas que mongoose nos ofrece para construir una base de datos robusta y mantenible.

---

## Siguiente paso

Con la conexión a MongoDB establecida, el siguiente paso es configurar Express, el framework web que nos permitirá crear servidores HTTP, definir rutas, y manejar peticiones. Express será el puente entre los usuarios y nuestra base de datos.

¿Continuamos?

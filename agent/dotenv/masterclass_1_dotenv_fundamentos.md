# Masterclass 1: dotenv - Fundamentos

## El problema de la configuración sensible

Cuando desarrollamos una aplicación que se conecta a una base de datos, necesita información como la dirección del servidor, el nombre de la base de datos y las credenciales de acceso. En un proyecto en desarrollo, esta información suele ser sencilla: un servidor local, un nombre de base de datos conocido, sin contraseñas complejas. Sin embargo, cuando la aplicación pasa a producción, la situación cambia por completo.

En producción, la dirección de la base de datos apunta a un servidor remoto, las credenciales son contraseñas reales que protegen datos importantes, y existe un número limitado de personas que deben tener acceso a esta información. Si esta información quedara expuesta en el código fuente, cualquier persona con acceso al repositorio podría conectarse directamente a la base de datos, modificarla o incluso eliminarla.

Aquí es donde surge un problema fundamental: **¿dónde guardar esta información de manera segura?**

## Las variables de entorno

La solución a este problema son las **variables de entorno**. Una variable de entorno es un par clave-valor que se almacena fuera del código fuente, en el sistema operativo o en un archivo especial. El código puede leer estas variables en tiempo de ejecución, pero nunca quedan expuestas en el repositorio.

En Node.js, las variables de entorno se acceden a través del objeto global `process.env`. Si una variable de entorno se llama `MONGO`, se accede a ella mediante `process.env.MONGO`. Este mecanismo es simple pero poderoso: permite que el mismo código funcione en diferentes entornos (desarrollo, producción, pruebas) sin modificar ni una línea, simplemente cambiando las variables de entorno disponibles.

El concepto no es nuevo. Los sistemas operativos utilizan variables de entorno desde hace décadas para configurar aspectos como la ruta de búsqueda de archivos, el idioma del sistema o la ubicación de directorios temporales. Node.js hereda esta capacidad y la extiende para el desarrollo web.

## El papel de dotenv

Aquí es donde entra **dotenv**. Aunque es posible configurar variables de entorno directamente en el sistema operativo, esto resulta tedioso y propenso a errores en desarrollo. Cada desarrollador del equipo tendría que configurar manualmente las mismas variables en su computadora, y no existiría un registro centralizado de qué variables necesita el proyecto.

dotenv resuelve este problema de manera elegante: carga variables de entorno desde un archivo llamado `.env` que se encuentra en la raíz del proyecto. Cuando se ejecuta `require('dotenv').config()`, dotenv lee este archivo y carga todas las variables que contiene en `process.env`.

El archivo `.env` tiene un formato extremadamente simple: cada línea contiene una variable en el formato `CLAVE=valor`. No hay comillas, no hay comas, no hay sintaxis compleja. Un archivo `.env` típico se ve así:

```
PORT=3000
MONGO=mongodb://localhost:27017/personaje
```

## El archivo .env y la seguridad

El archivo `.env` es tan útil como peligroso si se maneja incorrectamente. La regla más importante es que **nunca debe subirse a un repositorio de control de versiones como GitHub**. Si el archivo `.env` contiene credenciales reales y se sube a un repositorio público, cualquier persona podría acceder a ellas.

Para evitar esta situación, se utiliza el archivo `.gitignore` que ya conocemos de Node.js. Simplemente agregamos una línea:

```
.env
```

Esto le indica a Git que ignore completamente el archivo `.env`. Cuando otro desarrollador clone el repositorio, no recibirá el archivo `.env`, pero sí recibirá un archivo llamado `.env.example` que contiene las mismas variables pero sin valores reales, sirviendo como plantilla.

## Uso práctico en Node.js

El proceso de uso de dotenv en un proyecto de Node.js sigue un patrón consistente. Primero, se instala la dependencia:

```bash
npm install dotenv
```

Luego, en el archivo principal del proyecto (típicamente `app.js` o `index.js`), se carga dotenv **antes** de cualquier uso de `process.env`:

```javascript
require('dotenv').config();
```

A partir de ese momento, todas las variables definidas en `.env` están disponibles en `process.env`. Para acceder a la URI de conexión a MongoDB, simplemente se escribe `process.env.MONGO`.

En el proyecto mern-b, el archivo `.env` contiene:

```
PORT=3000
MONGO=mongodb://localhost:27017/personaje
```

Y en `app.js`, después de cargar dotenv, se utiliza así:

```javascript
await connectDB(process.env.MONGO);
app.listen(process.env.PORT, () => {
    console.log(`Servidor Conectado en puerto ${process.env.PORT}`);
});
```

## Más allá de la conexión: NODE_ENV

Aunque en esta etapa solo necesitamos PORT y MONGO, dotenv permite manejar cualquier variable de entorno. Una de las más importantes en el desarrollo de software es `NODE_ENV`, que indica en qué entorno se está ejecutando la aplicación.

Cuando se ejecuta `NODE_ENV=development`, la aplicación sabe que está en modo de desarrollo y puede comportarse de manera diferente: mostrar mensajes de error detallados, habilitar herramientas de depuración, o conectar a una base de datos de prueba. Cuando se ejecuta `NODE_ENV=production`, la aplicación sabe que está en un entorno real y debe ser más estricta con la seguridad, más eficiente en el rendimiento, y más discreta con la información que muestra.

Esta separación de entornos es un principio fundamental del desarrollo de software profesional. No es lo mismo desarrollar una aplicación en tu computadora personal que ejecutarla en un servidor real con usuarios conectados. Las variables de entorno permiten que el mismo código se adapte a estas diferentes situaciones sin modificaciones.

## Múltiples entornos

En proyectos más avanzados, es común tener diferentes archivos de configuración para diferentes entornos. Podríamos tener un archivo `.env.development` con configuraciones locales y otro archivo `.env.production` con configuraciones del servidor real. Herramientas como `dotenv-expand` o `cross-env` permiten gestionar estas variaciones, aunque por ahora no son necesarias para nuestro proyecto.

Lo importante es comprender el principio: **la configuración vive fuera del código**. Esto permite que el mismo repositorio contenga una aplicación que funcione tanto en desarrollo como en producción, simplemente cambiando las variables de entorno disponibles en cada contexto.

## Resumen

dotenv es una herramienta pequeña pero fundamental en el ecosistema de Node.js. Nos permite separar la configuración del código fuente, proteger información sensible, y gestionar diferentes entornos de ejecución. El proceso es simple: instalar dotenv, crear un archivo `.env` con las variables necesarias, cargar dotenv al inicio del proyecto, y acceder a las variables mediante `process.env`.

La importancia de dotenv va más allá de la conveniencia. Es una práctica de seguridad esencial que todo desarrollador debe adoptar desde el inicio. Un proyecto que almacena credenciales en el código fuente es un proyecto vulnerable, independientemente de cuán bueno sea el resto de su diseño.

---

## Siguiente paso

Con dotenv configurado, el siguiente paso es establecer la conexión con MongoDB utilizando mongoose. mongoose no solo nos permite conectarnos a la base de datos, sino que también proporciona herramientas para modelar, validar y manipular los datos de manera estructurada.

¿Continuamos?

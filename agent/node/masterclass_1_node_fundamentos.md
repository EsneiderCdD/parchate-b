# Masterclass 1: Node.js - El Inicio

## ¿Qué es Node.js?

Para comprender qué es Node.js, primero debemos entender un problema que existía antes de su creación. Durante muchos años, JavaScript fue un lenguaje que exclusivamente en el navegador web. Los desarrolladores utilizaban JavaScript únicamente para crear interactividad en las páginas web, mientras que para el backend, es decir, para construir servidores y aplicaciones del lado del servidor, recurrían a otros lenguajes como PHP, Python, Java o C#. Esto significaba que un desarrollador web necesitaba dominar al menos dos lenguajes diferentes para crear una aplicación completa.

Node.js surgió como solución a esta fragmentación. Se trata de un **entorno de ejecución** para JavaScript en el servidor, no de un lenguaje de programación en sí mismo. La diferencia es importante: JavaScript es el lenguaje que escribimos, mientras que Node.js es el mecanismo que permite ejecutar ese lenguaje fuera del navegador. Gracias a Node.js, ahora es posible utilizar el mismo lenguaje tanto para el frontend, que es lo que el usuario ve en el navegador, como para el backend, que es la lógica del servidor.

El corazón de Node.js es el motor **V8 de Google Chrome**, el mismo motor que hace que JavaScript sea rápido y eficiente en el navegador. Los creadores de Node.js tomaron este motor y lo adaptaron para que pudiera funcionar en cualquier sistema operativo, eliminando la dependencia del navegador. Este resultado es un entorno ligero, rápido y ideal para aplicaciones que manejan muchas conexiones simultáneas, como servidores web y APIs.

La filosofía de Node.js se basa en operaciones de entrada y salida no bloqueantes. Mientras que en otros entornos del servidor una petición que tarda mucho puede bloquear todo el servidor, Node.js está diseñado para manejar muchas peticiones al mismo tiempo sin detener el resto del proceso. Esto lo hace especialmente útil para aplicaciones en tiempo real, como chats, plataformas de streaming o APIs que reciben muchas consultas simultáneas.

---

## Primeros pasos: Verificar la instalación

Antes de comenzar a trabajar con Node.js, es necesario verificar que está correctamente instalado en nuestro sistema. Para ello, abrimos una terminal y ejecutamos el siguiente comando:

```bash
node -v
```

Este comando muestra la versión de Node.js instalada. Si vemos un número, como por ejemplo `v18.17.0` o `v20.5.0`, significa que Node.js está instalado y listo para usar. Si en cambio obtenemos un error, significa que debemos instalarlo desde la página oficial de Node.js.

De manera similar, podemos verificar la versión de npm, que es el gestor de paquetes que viene incluido con Node.js:

```bash
npm -v
```

npm, cuyas siglas significan **Node Package Manager**, es la herramienta que nos permite instalar, actualizar y gestionar bibliotecas de código externas, conocidas como paquetes o dependencias. Casi cualquier proyecto de Node.js utiliza npm para gestionar sus dependencias.

---

## Inicializar un proyecto: npm init

Cuando comenzamos un nuevo proyecto en Node.js, el primer paso es inicializarlo. Esto se hace mediante el comando:

```bash
npm init -y
```

El significado de cada parte es el siguiente: `npm` se refiere al gestor de paquetes, `init` indica que queremos inicializar un nuevo proyecto, y el flag `-y` significa que aceptamos automáticamente todas las preguntas que npm haría sobre el nombre del proyecto, su versión, descripción y otros detalles. Este flag es útil para empezar rápido, aunque siempre podemos modificar los valores después.

Al ejecutar este comando, se genera un archivo llamado `package.json` en la carpeta actual. Este archivo es fundamental porque funciona como el manifiesto del proyecto, conteniendo toda la información esencial que define qué es el proyecto, quién lo creó y qué dependencias necesita para funcionar correctamente.

---

## El archivo package.json

El `package.json` es uno de los archivos más importantes en cualquier proyecto de Node.js. Piensa en él como la tarjeta de presentación de tu proyecto: contiene información básica que cualquier desarrollador necesita para entender y trabajar con el código.

Veamos un ejemplo real del proyecto mern-b que ya conoces:

```json
{
  "name": "mern-b",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "cors": "^2.8.6",
    "dotenv": "^17.4.2",
    "express": "^5.2.1",
    "mongoose": "^9.9.4"
  }
}
```

Los campos más importantes son los siguientes. El campo **name** representa el nombre del proyecto, que en este caso es "mern-b". El campo **version** indica la versión actual del proyecto, siguiendo el estándar de versionado semántico. El campo **main** especifica cuál es el archivo de entrada del proyecto, es decir, por dónde comienza la ejecución; en el caso de mern-b, este archivo es `app.js`.

El campo **scripts** es especialmente relevante porque nos permite definir comandos personalizados que simplifican tareas comunes. Por ejemplo, podríamos definir un script para iniciar el servidor, otro para ejecutar pruebas y otro para ejecutar en modo desarrollo. Cuando ejecutamos `npm run nombre-del-script`, Node.js ejecuta el comando asociado.

Finalmente, el campo **dependencies** lista todas las dependencias que el proyecto necesita para funcionar en producción. Cada paquete incluye su número de versión, lo que garantiza que cualquier persona que clone el proyecto instale exactamente las mismas versiones. En el ejemplo de mern-b, vemos que el proyecto necesita cors, dotenv, express y mongoose.

---

## Instalar dependencias

Las dependencias son paquetes de código escritos por otros desarrolladores que podemos reutilizar en nuestros proyectos. En lugar de escribir todo desde cero, instalamos paquetes que ya resuelven problemas comunes, como crear servidores web, conectar bases de datos o manejar autenticación.

Para instalar una dependencia, utilizamos el comando:

```bash
npm install nombre-del-paquete
```

Al ejecutar este comando, npm realiza tres acciones automáticas. Primero, descarga el paquete desde el registro global de npm. Segundo, lo guarda en la carpeta `node_modules`, que se crea automáticamente en la raíz del proyecto. Tercero, agrega el paquete al campo `dependencies` dentro de `package.json`, registrando así que nuestro proyecto necesita esa dependencia.

Por ejemplo, si ejecutamos `npm install express`, después de la instalación veremos que en `package.json` aparece una nueva entrada:

```json
"dependencies": {
  "express": "^5.2.1"
}
```

Existen dos tipos de dependencias. Las **dependencies** son aquellas que el proyecto necesita para funcionar en producción, es decir, cuando la aplicación está desplegada y accesible para los usuarios. Las **devDependencies**, por otro lado, son herramientas que solo se utilizan durante el desarrollo, como linters, herramientas de testing o compiladores. Para instalar una dependencia de desarrollo, utilizamos el flag `-D`:

```bash
npm install -D nombre-del-paquete
```

---

## La carpeta node_modules

Cuando instalamos dependencias, npm crea una carpeta llamada `node_modules` donde guarda todos los paquetes descargados, junto con sus propias dependencias. Esta carpeta puede crecer rápidamente, llegando a contener cientos o miles de archivos.

Existe una regla fundamental que todo desarrollador debe conocer: **nunca se debe subir la carpeta `node_modules` a un repositorio de control de versiones como GitHub**. La razón es práctica: esta carpeta puede pesar megabytes o incluso gigabytes, y no es necesario subirla porque cualquier persona que clone el proyecto puede regenerarla ejecutando `npm install`. Este comando lee el archivo `package.json` e instala automáticamente todas las dependencias listadas.

Para asegurarse de que `node_modules` no se suba accidentalmente, se utiliza un archivo llamado `.gitignore` que le indica a Git qué archivos o carpetas debe ignorar:

```
node_modules/
```

---

## Ejecutar código con Node.js

Una vez que tenemos Node.js instalado y un proyecto inicializado, podemos empezar a escribir y ejecutar código JavaScript en el servidor. Para ejecutar un archivo JavaScript, utilizamos el comando `node` seguido del nombre del archivo:

```bash
node index.js
```

Creemos un ejemplo práctico. Creamos un archivo llamado `index.js` con el siguiente contenido:

```javascript
console.log("Hola desde Node.js");
```

Al ejecutar `node index.js` en la terminal, veremos la salida:

```
Hola desde Node.js
```

Esto demuestra que JavaScript está ejecutándose fuera del navegador, directamente en nuestra computadora. Aunque es un ejemplo simple, valida que el entorno está correctamente configurado y que podemos empezar a construir la lógica del backend.

---

## Resumen

Hemos cubierto los conceptos fundamentales para comenzar a trabajar con Node.js. Comprendimos que Node.js no es un lenguaje de programación, sino un entorno de ejecución que permite usar JavaScript en el servidor, utilizando el motor V8 de Google Chrome. Aprendimos a inicializar un proyecto con `npm init -y`, que genera el archivo `package.json`, el manifiesto de nuestro proyecto.

Exploramos los campos principales de `package.json`, incluyendo nombre, versión, archivo principal, scripts y dependencias. Vimos cómo instalar dependencias con `npm install` y la diferencia entre dependencias de producción y de desarrollo. Comprendimos por qué la carpeta `node_modules` nunca debe subirse a repositorios y cómo el archivo `.gitignore` nos ayuda a prevenirlo.

Finalmente, aprendimos a ejecutar código JavaScript directamente con Node.js, lo que confirma que el entorno está funcionando correctamente y que podemos empezar a construir la lógica del servidor.

---

## Siguiente paso

Con estas bases de Node.js establecidas, el siguiente paso natural es aprender a configurar variables de entorno con **dotenv**, una herramienta que nos permite guardar información sensible como credenciales y configuraciones fuera del código fuente. Alternativamente, si prefieres profundizar en algún aspecto específico del `package.json` antes de continuar, también podemos hacerlo.

¿Qué prefieres?

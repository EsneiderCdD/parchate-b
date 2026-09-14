# Masterclass 1: dotenv - Fundamentals

## The Problem of Sensitive Configuration

When we develop an application that connects to a database, it needs information such as the server address, the database name, and access credentials. In a development environment, this information is usually simple: a local server, a known database name, no complex passwords. However, when the application moves to production, the situation changes completely.

In production, the database address points to a remote server, credentials are real passwords that protect important data, and there is a limited number of people who should have access to this information. If this information were exposed in the source code, anyone with access to the repository could connect directly to the database, modify it, or even delete it.

This is where a fundamental problem arises: **where to store this information securely?**

## Environment Variables

The solution to this problem is **environment variables**. An environment variable is a key-value pair stored outside the source code, in the operating system or in a special file. The code can read these variables at runtime, but they are never exposed in the repository.

In Node.js, environment variables are accessed through the global object `process.env`. If an environment variable is called `MONGO`, it is accessed via `process.env.MONGO`. This mechanism is simple but powerful: it allows the same code to work in different environments (development, production, testing) without modifying a single line, simply by changing the available environment variables.

The concept is not new. Operating systems have used environment variables for decades to configure aspects such as file search paths, system language, or temporary directory locations. Node.js inherits this capability and extends it for web development.

## The Role of dotenv

This is where **dotenv** comes in. Although it is possible to configure environment variables directly in the operating system, this becomes tedious and error-prone during development. Each developer on the team would have to manually configure the same variables on their computer, and there would be no centralized record of what variables the project needs.

dotenv solves this problem elegantly: it loads environment variables from a file called `.env` located at the project root. When `require('dotenv').config()` is executed, dotenv reads this file and loads all the variables it contains into `process.env`.

The `.env` file has an extremely simple format: each line contains a variable in the format `KEY=value`. There are no quotes, no commas, no complex syntax. A typical `.env` file looks like this:

```
PORT=3000
MONGO=mongodb://localhost:27017/personaje
```

## The .env File and Security

The `.env` file is as useful as it is dangerous if handled incorrectly. The most important rule is that it **should never be uploaded to a version control repository like GitHub**. If the `.env` file contains real credentials and is uploaded to a public repository, anyone could access them.

To prevent this situation, we use the `.gitignore` file that we already know from Node.js. We simply add a line:

```
.env
```

This tells Git to completely ignore the `.env` file. When another developer clones the repository, they will not receive the `.env` file, but they will receive a file called `.env.example` that contains the same variables but without real values, serving as a template.

## Practical Use in Node.js

The process of using dotenv in a Node.js project follows a consistent pattern. First, the dependency is installed:

```bash
npm install dotenv
```

Then, in the project's main file (typically `app.js` or `index.js`), dotenv is loaded **before** any use of `process.env`:

```javascript
require('dotenv').config();
```

From that moment on, all variables defined in `.env` are available in `process.env`. To access the MongoDB connection URI, simply write `process.env.MONGO`.

In the mern-b project, the `.env` file contains:

```
PORT=3000
MONGO=mongodb://localhost:27017/personaje
```

And in `app.js`, after loading dotenv, it is used like this:

```javascript
await connectDB(process.env.MONGO);
app.listen(process.env.PORT, () => {
    console.log(`Server connected on port ${process.env.PORT}`);
});
```

## Beyond Connection: NODE_ENV

Although at this stage we only need PORT and MONGO, dotenv can handle any environment variable. One of the most important in software development is `NODE_ENV`, which indicates what environment the application is running in.

When `NODE_ENV=development` is set, the application knows it is in development mode and can behave differently: show detailed error messages, enable debugging tools, or connect to a test database. When `NODE_ENV=production` is set, the application knows it is in a real environment and must be stricter with security, more efficient in performance, and more discreet with the information it displays.

This separation of environments is a fundamental principle of professional software development. Developing an application on your personal computer is not the same as running it on a real server with connected users. Environment variables allow the same code to adapt to these different situations without modifications.

## Multiple Environments

In more advanced projects, it is common to have different configuration files for different environments. We might have a `.env.development` file with local configurations and another `.env.production` file with real server configurations. Tools like `dotenv-expand` or `cross-env` help manage these variations, although they are not necessary for our project at this stage.

The important thing is to understand the principle: **configuration lives outside the code**. This allows the same repository to contain an application that works in both development and production, simply by changing the available environment variables in each context.

## Summary

dotenv is a small but fundamental tool in the Node.js ecosystem. It allows us to separate configuration from source code, protect sensitive information, and manage different execution environments. The process is simple: install dotenv, create a `.env` file with the necessary variables, load dotenv at the start of the project, and access variables via `process.env`.

The importance of dotenv goes beyond convenience. It is an essential security practice that every developer should adopt from the start. A project that stores credentials in the source code is a vulnerable project, regardless of how good the rest of its design is.

---

## Next Step

With dotenv configured, the next step is to establish the connection with MongoDB using mongoose. mongoose not only allows us to connect to the database, but also provides tools to model, validate, and manipulate data in a structured way.

Shall we continue?

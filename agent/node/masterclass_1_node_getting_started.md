# Masterclass 1: Node.js - Getting Started

## What is Node.js?

To understand what Node.js is, we must first recognize a problem that existed before its creation. For many years, JavaScript was a language that ran exclusively in the web browser. Developers used JavaScript solely to create interactivity in web pages, while for the backend, meaning building servers and server-side applications, they relied on other languages such as PHP, Python, Java, or C#. This meant that a web developer needed to master at least two different languages to build a complete application.

Node.js emerged as a solution to this fragmentation. It is a **runtime environment** for JavaScript on the server, not a programming language itself. The distinction is important: JavaScript is the language we write, while Node.js is the mechanism that allows us to execute that language outside the browser. Thanks to Node.js, it is now possible to use the same language for both the frontend, which is what the user sees in the browser, and the backend, which is the server logic.

The core of Node.js is the **V8 engine from Google Chrome**, the same engine that makes JavaScript fast and efficient in the browser. The creators of Node.js took this engine and adapted it to work on any operating system, eliminating the browser dependency. The result is a lightweight, fast environment ideal for applications that handle many simultaneous connections, such as web servers and APIs.

The philosophy of Node.js is based on non-blocking input and output operations. While in other server environments a slow request can block the entire server, Node.js is designed to handle many requests simultaneously without stopping the rest of the process. This makes it especially useful for real-time applications, such as chats, streaming platforms, or APIs that receive many concurrent queries.

---

## First Steps: Verify the Installation

Before starting to work with Node.js, it is necessary to verify that it is correctly installed on our system. To do this, we open a terminal and run the following command:

```bash
node -v
```

This command displays the installed version of Node.js. If we see a number, such as `v18.17.0` or `v20.5.0`, it means Node.js is installed and ready to use. If we get an error instead, it means we need to install it from the official Node.js website.

Similarly, we can verify the version of npm, the package manager that comes included with Node.js:

```bash
npm -v
```

npm, whose acronym stands for **Node Package Manager**, is the tool that allows us to install, update, and manage external code libraries known as packages or dependencies. Almost any Node.js project uses npm to manage its dependencies.

---

## Initialize a Project: npm init

When we start a new project in Node.js, the first step is to initialize it. This is done using the command:

```bash
npm init -y
```

The meaning of each part is as follows: `npm` refers to the package manager, `init` indicates that we want to initialize a new project, and the `-y` flag means we automatically accept all the questions npm would ask about the project name, version, description, and other details. This flag is useful for starting quickly, although we can always modify the values afterward.

When we run this command, a file called `package.json` is generated in the current folder. This file is fundamental because it functions as the project's manifest, containing all the essential information that defines what the project is, who created it, and what dependencies it needs to function correctly.

---

## The package.json File

The `package.json` is one of the most important files in any Node.js project. Think of it as your project's business card: it contains basic information that any developer needs to understand and work with the code.

Let's look at a real example from the mern-b project that you are already familiar with:

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

The most important fields are the following. The **name** field represents the project name, which in this case is "mern-b". The **version** field indicates the current project version, following semantic versioning standards. The **main** field specifies the project's entry point, meaning where execution begins; in the case of mern-b, this file is `app.js`.

The **scripts** field is especially relevant because it allows us to define custom commands that simplify common tasks. For example, we could define a script to start the server, another to run tests, and another to run in development mode. When we run `npm run script-name`, Node.js executes the associated command.

Finally, the **dependencies** field lists all the dependencies that the project needs to function in production. Each package includes its version number, ensuring that anyone who clones the project installs exactly the same versions. In the mern-b example, we see that the project needs cors, dotenv, express, and mongoose.

---

## Installing Dependencies

Dependencies are packages of code written by other developers that we can reuse in our projects. Instead of writing everything from scratch, we install packages that already solve common problems, such as creating web servers, connecting to databases, or handling authentication.

To install a dependency, we use the command:

```bash
npm install package-name
```

When we run this command, npm performs three automatic actions. First, it downloads the package from the global npm registry. Second, it saves it in the `node_modules` folder, which is automatically created at the project root. Third, it adds the package to the `dependencies` field in `package.json`, thus registering that our project needs that dependency.

For example, if we run `npm install express`, after installation we will see a new entry in `package.json`:

```json
"dependencies": {
  "express": "^5.2.1"
}
```

There are two types of dependencies. **Dependencies** are those that the project needs to function in production, meaning when the application is deployed and accessible to users. **DevDependencies**, on the other hand, are tools used only during development, such as linters, testing tools, or compilers. To install a development dependency, we use the `-D` flag:

```bash
npm install -D package-name
```

---

## The node_modules Folder

When we install dependencies, npm creates a folder called `node_modules` where it stores all downloaded packages along with their own dependencies. This folder can grow rapidly, eventually containing hundreds or thousands of files.

There is a fundamental rule that every developer must know: **you should never upload the `node_modules` folder to a version control repository like GitHub**. The reason is practical: this folder can weigh megabytes or even gigabytes, and it is not necessary to upload it because anyone who clones the project can regenerate it by running `npm install`. This command reads the `package.json` file and automatically installs all listed dependencies.

To ensure that `node_modules` is not accidentally uploaded, a file called `.gitignore` is used, which tells Git what files or folders it should ignore:

```
node_modules/
```

---

## Running Code with Node.js

Once we have Node.js installed and a project initialized, we can start writing and executing JavaScript code on the server. To run a JavaScript file, we use the `node` command followed by the file name:

```bash
node index.js
```

Let's create a practical example. We create a file called `index.js` with the following content:

```javascript
console.log("Hello from Node.js");
```

When we run `node index.js` in the terminal, we will see the output:

```
Hello from Node.js
```

This demonstrates that JavaScript is running outside the browser, directly on our computer. Although it is a simple example, it validates that the environment is correctly configured and that we can start building the backend logic.

---

## Summary

We have covered the fundamental concepts for starting to work with Node.js. We understood that Node.js is not a programming language, but a runtime environment that allows us to use JavaScript on the server, utilizing the V8 engine from Google Chrome. We learned to initialize a project with `npm init -y`, which generates the `package.json` file, our project's manifest.

We explored the main fields of `package.json`, including name, version, main file, scripts, and dependencies. We saw how to install dependencies with `npm install` and the difference between production and development dependencies. We understood why the `node_modules` folder should never be uploaded to repositories and how the `.gitignore` file helps us prevent this.

Finally, we learned to run JavaScript code directly with Node.js, confirming that the environment is functioning correctly and that we can start building the server logic.

---

## Next Step

With these Node.js foundations established, the natural next step is to learn how to configure environment variables with **dotenv**, a tool that allows us to store sensitive information such as credentials and configurations outside the source code. Alternatively, if you prefer to delve deeper into any specific aspect of `package.json` before continuing, we can do that as well.

What do you prefer?

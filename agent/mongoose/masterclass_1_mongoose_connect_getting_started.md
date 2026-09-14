# Masterclass 1: mongoose - Connection and Fundamentals

## mongoose and its Relationship with MongoDB

MongoDB is a NoSQL database that stores information in JSON documents. To interact with it from Node.js, we need a tool that handles the communication between our code and the database server. This is where mongoose comes in.

mongoose is an **ODM** (Object Data Modeling) for MongoDB and Node.js. An ODM is an abstraction layer that allows us to work with the database using JavaScript objects and methods, instead of writing queries in MongoDB's native language. This does not mean that mongoose hides MongoDB, but rather provides us with a more intuitive and structured API to work with it.

The difference between an ODM and an ORM (Object-Relational Mapping) is important. An ORM is used for relational databases (like MySQL or PostgreSQL) and maps tables to classes. An ODM, like mongoose, works with NoSQL databases (like MongoDB) and maps documents to objects. mongoose does not try to be an ORM for MongoDB because MongoDB is not a relational database.

## Why mongoose?

We could connect Node.js directly to MongoDB using the official MongoDB driver for Node.js. However, this would mean writing queries in MongoDB's native language, handling data validation manually, and working without the advantages that an ODM provides.

mongoose offers us several significant advantages. First, it allows us to define **schemas** that describe the structure of documents, providing a layer of automatic validation. Second, it gives us access to **models** that encapsulate data access logic, making the code more organized and maintainable. Third, it provides tools for handling relationships between documents, complex validations, and query operations more fluidly.

In the context of our project, mongoose not only allows us to connect to MongoDB, but will also be the foundation upon which we build the application's data model: events, creators, users, requests.

## The Connection: mongoose.connect

The entry point of mongoose in any project is the `mongoose.connect` function. This function establishes the connection to the MongoDB server using a URI (Uniform Resource Identifier) that describes exactly which database to connect to.

The MongoDB URI has a specific format:

```
mongodb://username:password@host:port/database-name
```

In local development, when no authentication is configured, the URI is simpler:

```
mongodb://localhost:27017/personaje
```

This URI indicates that we will connect to the local server (`localhost`) on port `27017`, and use the database called `personaje`.

In the mern-b project, the URI is stored in the `.env` file as an environment variable, and is accessed via `process.env.MONGO`. This is exactly what we configured in the previous masterclass with dotenv.

## Connection Process

The connection process with mongoose follows a consistent pattern. First, we import mongoose in our main file:

```javascript
const mongoose = require('mongoose');
```

Then, we define an async function responsible for establishing the connection:

```javascript
async function connectDB(url) {
    try {
        await mongoose.connect(url);
        console.log('Connected to Database');
    } catch (e) {
        console.error(e);
        process.exit(1);
    }
}
```

Finally, we call this function passing the connection URI:

```javascript
await connectDB(process.env.MONGO);
```

The `mongoose.connect` function returns a Promise, which means we can use `async/await` to handle it cleanly. The `try/catch` block allows us to handle connection errors in a controlled way: if the connection fails, the error message is displayed in the console and the application closes gracefully with `process.exit(1)`.

## Error Handling in Connection

Error handling is fundamental in any application that connects to a database. If the MongoDB server is not available, if credentials are incorrect, or if there are network issues, the connection will fail. Without proper error handling, the application could behave unpredictably.

In the example above, the `try/catch` block captures any error that occurs during connection. The `console.error(e)` displays the error in the console for debugging, and `process.exit(1)` terminates the application with an error code. In a production environment, we might want to implement an automatic reconnection strategy or send alerts to the development team.

mongoose also provides events that allow us to monitor the connection status. We can listen for events like `connected`, `error`, and `disconnected` to make decisions based on the connection state:

```javascript
mongoose.connection.on('connected', () => {
    console.log('Mongoose connected to MongoDB');
});

mongoose.connection.on('error', (err) => {
    console.error('Mongoose connection error:', err);
});

mongoose.connection.on('disconnected', () => {
    console.log('Mongoose disconnected from MongoDB');
});
```

These events are especially useful in production, where we need to know exactly what is happening with the database connection.

## Beyond Connection: A Preview

Although at this stage we only need to establish the connection, it is important to understand that mongoose offers much more. When we delve deeper into mongoose, we will learn about **schemas** and **models**, which are the fundamental concepts for working with data in mongoose.

A **schema** defines the document structure: what fields a document has, what data types they contain, and what constraints they must meet. A **model** is an interface built on the schema that allows us to perform CRUD operations (Create, Read, Update, Delete) on the database.

In the mern-b project, there is already an example of this in `model/Persona.js`:

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

Here we see how a schema is defined with different data types, and how a model is created from that schema. The third argument `'maria'` specifies the collection name in MongoDB, which is different from the model name.

## Local vs. Production Connection

It is important to understand the difference between the local connection and the production connection. In local development, we connect to a MongoDB server running on our computer, without authentication, on a known port. In production, we will connect to MongoDB Atlas, a cloud database service that requires authentication, has IP access restrictions, and uses secure connections.

This transition from local to production is a topic we will address later in the project. For now, it is sufficient to understand that the connection URI is the bridge between our application and the database, and that this URI can change depending on the environment.

## Summary

mongoose is the ODM that allows us to connect to and work with MongoDB from Node.js. The connection is established via `mongoose.connect(url)`, which is an asynchronous operation that we must handle with `async/await` and `try/catch`. The connection URI is stored in environment variables (thanks to dotenv) and contains all the information necessary to connect to the database.

Although we have only covered the connection at this stage, we have laid the groundwork for understanding how mongoose works. In future masterclasses, we will dive into schemas, models, validations, and all the tools mongoose offers to build a robust and maintainable database.

---

## Next Step

With the MongoDB connection established, the next step is to configure Express, the web framework that will allow us to create HTTP servers, define routes, and handle requests. Express will be the bridge between users and our database.

Shall we continue?

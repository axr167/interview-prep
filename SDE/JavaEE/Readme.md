
## The big picture

Java EE extends Java SE. Used to develop large scale apps. If application demands large scale and having a distributed system, use Java EE. Enterprise applications are applications that are scalable, multi tiered, reliable and secure.

In a **multi tiered application**, the functionality of the application is separated into isolated functional areas called **tiers**. We can have one to many tiers depending on the complexity of our application. Typically multi tiered applications have the following tiers:

- **Client tier**: Client tier consists of client programs. They make requests to the middle tier and are located on a different macine.
- **Middle tier**: Processes client requests and returns response to client. This is where we find Java EE platform. It has its own tiers.
  - **Web tier**: Has components that handle interaction between client and business tier. Its task - dynamically generate content for the client, control the screen of pages, maintain state of data for session
  - **Business tier**: Processes the requests and processes app data storing it into a permanent data store in the data tier. Arranges interaction with external services.
  - **Interoperability tier**: Allows interaction with external services through messaging or web services and exposes a public API that can be used by others.
- **Enterprise Information System (EIS) tier**: Consists of database service, resource planning systems etc. Typically located on a separate machine and are accessed by components on the business tier.

### Why use java EE?

Just as Java SE gives us APIs for collections, Java EE gives us APIs for transactions, messaging or persistence.

Another similarity is when we talk about containers. In java SE when we create an object in memory we rely on the JVM to clean up memory. Here JVM acts like a container that gives low level services to the code. Java EE code is also executed in a container that brings extra services such as transaction, messaging or persistence.

Systems that make EE apps powerful like security and reliability often make these applications complex. Java EE reduces complexity by providing programming model, APIs and runtime env that allows devs to concentrate on business requirements instead of technical concerns.

### Java EE programming model

Java EE is designed to support apps that are inherently complex. Hence it uses a simplified programming model to emphasise comvention over configuration. 

The idea: the container will take default decisions to bring us services. If we dont like the default, we can use metadata like annotations or XML to deviate from convention.

**An Example**: Consider the case where we want to store a book into an RDBMS.

**Case 1:** Java SE

- We have a book class to represent a book.
  - Here book is just a java class such as title, discription, number etc.
  - Additionally it also has constructors, getters and setters.
- We have a different class main
  - Main class has method to persist book to DB and it hass method to find book from DB
  - It has low level connectivity API to connect to DB. We load JDBC driver and return a connection
  - We can now connect to DB. To insert book we must create SQL query to insert to DB and execute it we also need to do a mapping for each attribute in Book to fields of the table.
  - Similarly findbook method takes book ID as param and have it return a resultset.

This has several problems:

- SQL is a different language
- SQL is not easy to refactor. If DB struct changes, we must manually update each statement and we must update book class because object structure is related to DB
- Manual mapping is verbose, hard to read and hard to maintain.

**Case 2:** Java EE

Java EE solves the problems of Java SE by bringing some abstraction.

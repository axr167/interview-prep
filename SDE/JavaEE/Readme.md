
## Table of Contents

1. [Overview](#Overview)
2. [Java EE Basics](#Java-EE-Basics)

## Overview

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

The code is:

    public class Book {
        private long id;
        private string title;
        private string description;

        // Constructor, getters and setters
    }

    public class Main {
    
        // Connection
        static {
            try{
                Class.forName("org.apache.derby.jdbc.ClientDriver");
            } catch (ClassNotFoundException e) {
                e.printStackTrace();        
            }
        }
        private static Connection getConnection() throws SQLException {
            return DriverManager.getConnection("something");
        }

        // persist book
        private static void persistBook(Book book) {
            String query = "INSERT INTO BOOK (ID, TITLE, DESC) VALUES (?, ?, ?)";
            try(PreparedStatement stmt = getConnection().prepareStatement(query)) {
                stmt.setInteger(1, book.getId());
                stmt.setString(2, book.getName());
                stmt.setString(3, book.getDescription());

                stmt.executeUpdate();
            }
        }

        // find book
        private static book findBook(int id) {
            Book book = new Book(id);
            String query =  "SELECT ID, TITLE, DESC FROM BOOK WHERE ID=?";
            try(PreparedStatement stmt = getConnection().prepareStatement(query)) {
                stmt.setInteger(1, id);
                ResultSet rs = stmt.executeQuery();

                while(rs.next()) {
                    book.setTitle(rs.getString("TITLE"));
                    book.setDescription(rs.getString("DESC"));
                }
            }
            return book;
        }

        public static void main(String[] args) {
            // Save book
            persistBook(new Book(1, "Harry Potter", "Fantasy"));
            // Find book
            Book book = findBook(1);
        }
    }

This has several problems:

- SQL is a different language
- SQL is not easy to refactor. If DB struct changes, we must manually update each statement and we must update book class because object structure is related to DB
- Manual mapping is verbose, hard to read and hard to maintain.

**Case 2:** Java EE

Java EE solves the problems of Java SE by bringing some abstraction. It has a std solution that bridges gap between objects and relational database.

- Here we have the book class
  - It is the same except we add metadata - add @Entity on top of book and @Id on top of primary key
  - To save/retreive book define appropriate functions in bookservice class. Thanks to metadata, we can inject entity manager to service.
  - EntityManager is the standard api that maps objects to DB. To save, we just invoke the entity manager persist method. To find, we just use the entity manager find method.

The code is:

    @Entity
    public class Book {
        @Id
        private int id;
        private String title;
        private String description;

        // Constructors, getters and setters
    }
    
    @Transactional
    public class BookService {
        @Inject
        private EntityManager em;

        public void persistBook (Book book) {
            em.persist(book);
        }

        public book findBook(int id) {
            return em.find(Book.class, id);
        }
    }

This is much better. There is no manual mapping, there are no SQL statements, it is non intrusive and ORM mapping is done through metadata (@Entity, @Id etc)

The metadata shows convention over configuration. This lets all attributes be mapped automatically. If table name or column name is different, we can change default configuration as follows:

    @Entity
    @Table(name ="t_book")
    public class Book {
        @Id
        private int id;
        @Column(name="book_title", nullable=false)
        private String title;
        @Column(name="desc")
        private String description;

        // Constructors, getters and setters
    }

Conventions and configurations through metadata is the programming model that Java EE uses for all services.


## Java EE Basics

Java EE was developed by asking the question - how can I quickly and easily develop an enterprise application while being able to read and maintain the code easily.

### Java EE Architecture

The java EE architecture is set around a runtime environment called a container. This container provides services to the components it hosts such as lifecycle management, dependency injection, concurrency etc. The components use well defined contracts of APIs to communicate with the java EE infrastructure and with other components. The components need to be packaged in a standard way before being deployed to the container. After deployment there are several protocols that can be used to access these components.

The various pieces that make up Java EE are described below:

**Container**

- It is a runtime environment whose role is to hide technical complexity, enhance portability and host applications.
- Devs can concentrate on the business logic and leave the container to handle resource management, multithreading etc

**Components**

- The container manages components
- Components are static or dynamic web pages that process requests and construct responses
- Components can also be server side java classes that handle business logic and process data (bookService).

**Services**

- The intent of the container is to provide services to the components such as security, transaction management etc
- Services are configurable (in book example we changed table and col names) and different components can have different services. This is where metadata can be used.

**Metadata**

- Server code is defined by user code and metadata that follow a contract for interacting with the container. Metadata can take the form of Annotations or XML.
- The extra info is given by the component to the container. The container will then configure the service and give it to the component at deployment time and runtime.
- The developer does not need to write the service in the component. The component can just use already existing services provided by the container. This is called **inversion of control**.
  - Inversion of control means the container takes control of business code and provides whatever services we need.

**Application**

- An application is an aggregation of components. 
- If you think of web apps we have web pages, web resources (images, javascript, stylesheets), business components, database access components.

**Packaging**

- For the app to be executed we must assemble all the components together in an archive and deploy it to the container.
- Once deployed the app is ready to run. The packaging has a standard format so it can be executed in any container increasing portability.
- Once we deploy one app to a container we can also deploy other applications. A container can handle several applications and isolate them from each other.
  - Each app has its own resources, components and class loader.
  - Each app is administered separately by the container. This means we can stop/start one application without affecting the others.

**Clients/Protocols**

- To interact with applications, Java EE lets us use protocols like HTTP, HTTPS and RMI
- The clients can be web clients, web apps, SOAP/REST services, browsers and anything that understands the above protocols. 
- Java EE application can also be a client of another java EE application.
  - When we add other java EE containers and let them exchange information, it forms a distributed application.
  - We can distribute components of the same application into separate containers. This allows load balancing etc
  - Distribution is a strength of Java EE as it brings scalability and availability in a transparent way.

### Java EE Services

The services can be grouped by tier. These are:

- Business tier: handles business logic and processes/stores data. Has services for persistence and transaction management.
  - Transactions: Deals with RDBMS, messaging systems etc
  - Persistence: Does the mapping between objects and RDBMS. Lets us query stuff without worrying about the underlying DB
  - Validation: Services to volidate and constraint the model (filed not null, unique key etc)
  - Batch processing services: Allows devs to define long running jobs
- Web tier: Handles interaction between clients and business tier
  - Servelets: Handles HTTP and HTTPs services.
  - Web pages: Allows generation of dynamic web pages.
  - Expression languages: Binding of models to web pages
  - Web Sockets: Bidirectional conversation between client and server
- Interoperability tier: Manages interaction with external services.
  - SOAP/REST services
  - Json processing
  - Messaging services (async communication, publish/subscribe model)
  - Emails etc
- Services common to all tiers:
  - Dependency Injection
  - Interception (changing default behaviour)
  - Security
  - Concurrency




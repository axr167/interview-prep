
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

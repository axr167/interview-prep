
## Overview

Maven is a build tool. Build tools are applications that automatically create executable file from code. It automatically does things like:

- Downloading dependencies
- Compiling source code
- Version control
- Deploying to server

In small projects we do all that manually but in large projects, devs cannot keep track of what needs to be built, what order the dependencies must be added etc. A tool like maven does it for us instead.

One of the main reasons people use Maven is **dependency management**. For example if we are using Hibernate, there is 1 hibernate jar we must import but it has 13-14 other transitive dependencies. With maven, I can just tell it to use hibernate and it will download all transitive dependencies for me. 

### Why use maven?

There are a lot of reasons including:

- **Repratable builds:** It gives us the ability to build the app for any environment. By using maven, we need not change settings for other environments. So you can develop on windows, test on linux and deploy to unix in production.
- **Transitive dependencies:** Downloading/using an item automatically downloads other items it needs.
- Contains everything you need for environment: Does not matter if I am building using ide or command line. Maven builds it.
- **Works with local repo:** Before we downloaded all jars and kept it with project. So If I had 20 projects, we could have same jar downloaded 20 times for each project - each project had its copy of the jar.
  - Maven works with local repo. So we just download jar once and all 20 projects reference jar from local repo.
- Works with any ide: Doesnt matter what ide it is (eclipse, intellij, netbeans) - maven works for it.
- It works well with continuous integration build tools like Jenkins or Bamboo

### Hello World app

Create new project and add pom.xml file. Maven looks for this file in the project pom stands for Project Object Model. The basic pom.xml file looks like this:

    <project>

        <groupId>com.companyname</groupId>
        <artifactId>HelloWorld</artifactId>
        <version>1.0-SNAPSHOT</version>
        <modelVersion>4.0.0</modelVersion>
        <packaging>jar</packaging>

    </project>

This is what the tags mean:

- groupId: It is the same thing your package should be in your java code. Convention is com.company_name.
- artifactId: It is what we want to name our application. In the example, our application is hello world.
- version: Maven is good for versioning. This tag represents the version tag
- modelVersion: This is the xml version that we are using.
- packaging: The packaging type for the project. In this case it is jar.

And we are done. Now we can write the java app. Maven is expecting a standard folder structure this structure is:

    ProjectName -> src -> main -> java -> something.java

Maven knows to start compiling any class in that directory.

    public class HelloWorld {
      
        public static void main(String[] args) {
            System.out.println("Hello World");
        }
        
    }

**Building the app**

Now we must build the app. CD to workspace\project name then run the following commands:

- mvn clean: Downloads whatever plugins are necessary and performs any restructuring that is needed.
- mvn compile: Downloads whatever plugins are necessary and compiles the code

After running compile, now we should have a target directory as follows:

    PojectName -> target -> classes -> HelloWorld.class

Now we can cd into classes and run: java HelloWorld

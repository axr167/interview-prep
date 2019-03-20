
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

- Repratable builds: It gives us the ability to build the app for any environment. By using maven, we need not change settings for other environments. So you can develop on windows, test on linux and deploy to unix in production.
- Transitive dependencies: Downloading/using an item automatically downloads other items it needs.
- Contains everything you need for environment: Does not matter if I am building using ide or command line. Maven builds it.
- Works with local repo: Before we downloaded all jars and kept it with project. So If I had 20 projects, we could have same jar downloaded 20 times for each project - each project had its copy of the jar.
  - Maven works with local repo. So we just download jar once and all 20 projects reference jar from local repo.
- Works with any ide: Doesnt matter what ide it is (eclipse, intellij, netbeans) - maven works for it.
- It works well with continuous integration build tools like Jenkins or Bamboo

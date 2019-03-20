
### Overview

Maven is a build tool. Build tools are applications that automatically create executable file from code. It automatically does things like:

- Downloading dependencies
- Compiling source code
- Version control
- Deploying to server

In small projects we do all that manually but in large projects, devs cannot keep track of what needs to be built, what order the dependencies must be added etc. A tool like maven does it for us instead.

One of the main reasons people use Maven is **dependency management**. For example if we are using Hibernate, there is 1 hibernate jar we must import but it has 13-14 other transitive dependencies. With maven, I can just tell it to use hibernate and it will download all transitive dependencies for me. 

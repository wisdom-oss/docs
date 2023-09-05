# Prepare your development environment
This document outlines the steps required to develop and build a new 
microservice using the template microservice.

## Prerequisites
To be able to develop a new microservice you need the following software on your
development machine:
* [Golang](https://go.dev): This is the programming language in which the microservices are 
    written in the project.
* [Git](https://git-scm.com): A Version control software which is important
* [SSH Client](https://www.openssh.com/): The SSH Client is used to allow access
    to databases that are running on remote hosts, if the WISdoM platform
    already has been [deployed](../deployment.md).

Furthermore, you need a code editor or IDE (e.g. 
[Visual Studio Code](https://code.visualstudio.com/) or 
[GoLand](https://www.jetbrains.com/go/)) that optionally support syntax
highlighting or other convenience features.

### Installing Golang

Download and install Golang from their 
[official website](https://go.dev/doc/install). Using package managers like
APT or winget are supported too. Just make sure to use the currently latest
version available.

Verify the currently installed version using
```sh
go version
```

### Installing Git

Download and install Git from their 
[official website](https://git-scm.com/downloads) and follow their instructions.

🎉 That's it, now you are ready to start developing an microservice on your
machine
# Develop Your Own Microservice
Before creating and developing your own microservice, make sure that you 
installed all prerequisites. If not, the steps are outlined in the
[prepare your development environment](prerequisites.md) guide.

To make the development of a new microservice easier and faster, a template
microservice has been created.
This guide will use the template as a start to explain where to begin the
development of a new service and what is important when using the template.

## Preparing the Template
To start developing a new microservice using the template, you of course need to
download it beforehand.

To download it, open the 
[repository on GitHub](https://github.com/wisdom-oss/microservice-template) and
use the green code button to download the repository as archive. Do not clone
or fork the template repository!

Afterward, extract the files in the repository and move all files to the folder,
your microservice shall be developed in.

After extracting and moving the files, the file structure should look like
this:
```
├── globals
│  ├── connections.go      contains globally available connections
│  ├── variables.go        contains globally available variables
├── resources
│  ├── authConfig.json     contains auth config
│  ├── environment.json    contains the environment setup
│  ├── errors.json         contains http errors
│  ├── queries.sql         contains sql queries for the service
├── routes
│  ├── templates.go        contains three template routes
├── .gitignore
├── init.go                contains code used during startup
├── template-service.go    contains the bootstrapping code for the service
├── go.mod                 contains the dependencies of the service
├── go.sum                 is the lockfile for the dependencies
```

Now open your code editor and open the file `globals/variables.go`.
This file contains a constant named `ServiceName` which sets the service name
for a variety of functions already coded.
Set this constants value to the name your service should have.

After setting the `ServiceName` you now need to rename the file called
`template-service.go` to a name, that does not contain the word template, to
make visible to you, that the template is now set up.

### Updating the Dependencies
Since the template now is completely set up for the start of development, open
a terminal in the folder which contains the microservice. Now use the following
command to update all dependencies that the microservice currently has.
```sh
go get -u
```

🎉 Now all packages used in the microservice are up-to-date and the development
may start, as all preparations are done.

Assembler applications require a variety of components for each application in order to function correctly. Each of these components are created automatically by the *Assembler Cluster* and can be scaled independently (horizontally or vertically) as required.

These components are created and managed automatically by the *Assembler Cluster* and do not require manual deployments or updates.

## Web application / builder 

The web application is responsible for rendering the completed application to end users. In development, it also hosts the application builder for teams to build and manage their applications. 

By default, the Web application is the only component exposed to the internet and all communication between the web application and other components is internal. For more information, see *Network Security.*

The web application is responsible for accepting end user requests and rendering the application. Depending on the workload of the application, it can be advantageous to scale the web application either vertically (increase resource limits) or horizontally (deploy multiple web applications)

## Worker queue

Many tasks completed by the Application are run in the background as they may consume large amounts resources or take a longer time to complete. These typically include:

* Sending notifications and other emails.
* Building documents (Word, Excel etc) to be downloaded by users.
* Running custom scripts
* Importing data in to the application. 

Depending. on the application, worker jobs may require larger amounts of resources, or require a large amount of throughput, the worker queue can be scaled to accomodate both of these requirements.

## Database server

Assembler applications utilise a PostgreSQL database as the primary application data store. 

The database is responsible for storing all local application data, the current 'working directory' of the application and a variety of audit and compliance metrics. Various database configuration items can be managed manually or automatically by Assembler.

Multiple database replicas can be configured to ensure high availability and the resource limits of the database can be scaled independently.

## Script execution server

Assembler has the ability to execute custom scripts written by users building applications. This is done in a secure and performant matter by the Script execution server. For security information about script execution, see *Sandbox execution security* in the security model.

The script execution server has the following responsibilities:

* Run scripts written in the platform for the application.
* Compile the various application assets to produce the application bundle.
* Compile and populate email and document templates

Depending on the scripts being run in the application, the script execution server can be horizontally scaled to increase throughput or vertically scaled if resource consumption is a limiting factor in execution performance.

## Caching server

Various data is cached in the application. Assembler uses Redis as a cache data store. Assembler also utilise the pub/sub functionality in Redis to drive the worker queue.

The caching server contains no application data and is used as a high performance, temporary store for worker queues.

## File server

The following documents are stored in the file server:

* Theme assets such as logos or images.
* Templates for generated documents.
* The resulting generated documents
* Documents uploaded by end users.

The storage capacity of the file server disk can be managed manually or automatically.

Assembler uses [MinIO | High Performance, Kubernetes Native Object Storage](https://min.io/) for file storage.

## Version control server

Assembler applications provide support for full version control, including the ability to branch, merge, commit, resolve conflicts and refresh feature branches.

This functionality is managed by the Version control server. 

Assembler uses git to drive the underlying version control mechanisms.
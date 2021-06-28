# Container security
## Assembler containers

Currently, Assembler produces the following containers which are used in application environments.

| **Name**          | **Purpose**            | **Description**                                                                                                                                                                                                                   |
|-------------------|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| nucleo:<version>  | Web application        | This container manages the Ruby on Rails application responsible for building and rendering the application built on Assembler.                                                                                                   |
| beehive:<version> | Script Sandbox         | This container manages a script sandbox application written in NodeJS/ExpressJS. This application manages the secure execution of application specific, builder provided scripts as well as compiling  application theme bundles. |
| hexagon:<version> | Version control server | This container manages the version control for each application. Written in NodeJS/ExpressJS this service enables branching, committing, merging and conflict resolution of the application resources.                            |

## CIS benchmark for docker

The CIS docker benchmark is the community standard for deploying secure docker containers. All Assembler containers are automatically scanned against the latest CIS Benchmark for docker.

## Vulnerability scanning

All Assembler containers are scanned using a variety of container static analysis tools to detect embedded vulnerabilities in the containers.
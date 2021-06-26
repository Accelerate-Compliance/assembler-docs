# Assembler Platform Architecture
Assembler as a platform is comprised of four major components, each with their own sub-components. 

### 1. Assembler Dashboard

There is a single Assembler Dashboard deployment for each 'Assembler' universe. The Dashboard is responsible for authenticating Assembler builder accounts, maintaining a portfolio of organisations, teams and Assembler applications.

No application data is contained within the Assembler Dashboard except:

* User accounts of Assembler builders (not application end users)
* Application names and hosts.
* A series of application environment-wide configuration items.
* Resource consumption data of applications (CPU, Memory consumption etc).

The assembler dashboard is responsible for the following functionality

* Create and manage organisations.
* Add users to organisations with specific access rights.
* Create new applications in a specific cluster.
* Authenticating Assembler builder accounts.
* Allow users to view the applications owned their organisations.
* Create multiple production environments for applications.
* Publish changes from development / staging to production.
* Monitor application resource consumption and manage costs.

The Assembler Dashboard must be deployed in to a Kubernetes cluster, either as the sole workload or in a dedicated namespace. Kubernetes management is required to manage any required scaling of the Dashboard.

New versions of the Assembler Dashboard are published by the Assembler team on a release schedule and can be upgraded by patching the relevant container definitions in Kubernetes.

### 2. Assembler clusters

An *Assembler cluster* is a Kubernetes cluster which manages the orchestration of Assembler applications in a specific region, cloud hosting provider or private network. 

An Assembler cluster can be thought of in much the same way as a 'Region' in major cloud service providers. A cluster could exist in Australia, Asia, America or in private corporate cloud.

Clusters can either be public or limited to specific *Assembler Dashboard* organisation.

Once the cluster has been deployed and registered within the Assembler Dashboard, new applications can be deployed and managed immediately.

An *Assembler cluster* has the following responsibilities 

* Create the infrastructure for application environments as required by the Assembler Dashboard.
* Update Assembler environments:
  * Application version
  * Resource consumption limits
  * Various environment-wide configuration
* Perform health-check, observability and monitoring over the applications within the cluster.
* Create and manage the infrastructure for authentication servers to be used by applications.
* Deleting infrastructure for application environments no longer required.

### 3. Authentication servers

Applications require an authentication mechanism for end-users. Oftentimes applications may wish to share user groups ("Log in with my 'App' account") so this responsibility cannot be contained within a single application.

*Authentication servers* live within and are managed by an *Assembler Cluster*. 

*Authentication servers* can have multiple *User Lists*. A *User List* is a group of users who use the same account to authenticate with any application that is using that list. An application can use a different or the same lists for Staging/Production environments depending on the requirement. Applications can share a user list or have a one-to-one relationship.

Authentication servers allow for the configuration of *Identity Providers* which can then be utilised by applications. A common scenario is for a Company to create a single user list with a configured corporate SAML IDP to be used for all internal applications.

### 4. Assembler Applications

*Assembler Applications* are comprised of a Development / Staging environment and one or more Production Environments. Applications are built in development branches, merged in to staging and then published to production. 

Currently, all environments for an *Application* must live within the same *Assembler Cluster* however this limitation will be removed in the future.

Each environment resides within a Kubernetes Namespace and is entirely isolated from all other applications within a cluster. See the *Network Security* and *Kubernetes Security Model* sections of the *Security Model* for more information.

Internal application architecture is detailed in the next section.
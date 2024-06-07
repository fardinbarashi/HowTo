# PublicRepos
 
<!-- ABOUT THE PROJECT -->
# About The Project
This Repository's is a howto on install One Identity Manager.


<p align="right">(<a href="#readme-top">back to top</a>)</p>

# Sources
Identity Manager 9.2 - Installation Guide - [Link](https://support.oneidentity.com/technical-documents/identity-manager/9.2/installation-guide)


<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
# License

Distributed under the GPL-3.0 License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>



<!-- CONTACT -->

<!-- SETUP -->
# Setup
![Setup](https://github.com/fardinbarashi/Howto/blob/main/One%20-%20Identity%20Manager/Images/Setup.png)
![VmSetup](https://github.com/fardinbarashi/Howto/blob/main/One%20-%20Identity%20Manager/Images/VMSetup.png)

<!-- ROLES -->
# Roles
<!-- ROLES DATABASE -->
## Database
### About Database
```
The database represents the core of One Identity Manager. It fulfills the main tasks, which are managing data and calculating inheritance. Object properties can be inherited along the hierarchical structures, such as departments, cost centers, location, or business roles. For data management, the database maps managed target systems and ERP structures as well as compliance rules and access permissions.
```
<!-- ROLES JOB SERVER -->
## Job Server
### About Job Server
```
One Identity Manager uses 'processes' for mapping business processes. A process consists of process steps that represent processing tasks and are joined by predecessor/successor relations. This functionality allows flexibility when linking actions and sequences to object events. Processes are modeled using process templates. A process generator (Jobgenerator) is responsible for converting script templates in processes and process steps into a concrete process in the ’Job queue’.
The One Identity Manager Service enables the distribution throughout the network of information that is administrated in the One Identity Manager database. The One Identity Manager Service performs data synchronization between the database and any connected target systems and runs actions at the database and file level. The service must be installed on the One Identity Manager network server to run the processes. A server running the One Identity Manager Service is called the Job server. The Job server must be declared in the One Identity Manager database.
The One Identity Manager Service retrieves process steps from the Job queue. Process steps are run by process components. The One Identity Manager Service also creates an instance of the required process component and transfers the process step parameters. Decision logic monitors the performance of the process steps and determines how processing should continue depending on the results of the run process components. The One Identity Manager Service enables parallel processing of process steps because it can create several instances of process components.
The One Identity Manager Service is the only One Identity Manager component authorized to make changes in the target system.
```
<!-- ROLES APPLICATION SERVER -->
## Application server
### About Application Server
```
Clients connect to an application server storing business logic. The application server provides a connection pool for accessing the database and ensures a secure connection to the database. Clients send their queries to the application server, which processes the objects, for example, by determining values using templates and sending the results back to the clients. The data from the application is sent to the database when an object is saved.
Clients can alternatively work without external application servers by retaining the object layer themselves and accessing the database layer directly. In this case, only the part of the object layer that is required for the acquisition process is mapped in the clients.
```
<!-- ROLES WEB SERVER -->
## Web server
### About Web Server
```
To implement browser-based user interfaces, there is an application running on a web server that is based on a website render engine. Users use a web browser to access the website that has been dynamically set up and customized for them. Data exchange between database and web server can take place either directly or through the application server.
Front-ends
There are different front-ends for different tasks. For example, the front-end used to configure One Identity Manager differs from the one for managing identities. The contents to be displayed and the extent to which it can be altered is determined in conjunction with the access permissions of the respective user through the object layer. Available front-end solutions are both client and browser-based.
```

<!-- INSTALLATION PREREQUISITES -->
# Installation prerequisites

<!-- INSTALLATION PREREQUISITES DATABASE  -->
## Database

<!-- INSTALLATION PREREQUISITES DATABASE SUPPORTED SYSTEMS -->
### Supported database systems
One Identity Manager supports the following database systems:
```
SQL Server
Managed instances in Azure SQL Database
Azure SQL Database
Amazon Relational Database Service (Amazon RDS) 
```

<!-- INSTALLATION PREREQUISITES DATABASE MINIMUM SYSTEM REQUIREMENTS  -->
### Minimum system requirements - database server
```
Processor
8 physical cores with 2.5 GHz+ frequency (non-production)
16 physical cores with 2.5 GHz+ frequency (production)
NOTE: 16 physical cores are recommended on the grounds of performance.

Memory
16 GB+ RAM (non-production)
64 GB+ RAM (production)

Hard drive storage
100 GB

Operating system
Windows operating systems, Note the requirements of Microsoft for the version of SQL Server you are using.

Software
Following versions are supported:
SQL Server 2019 Standard Edition (64-bit) with the current cumulative update
SQL Server 2022 Standard Edition (64-bit) with the current cumulative update

NOTE: 
For performance reasons, the use of SQL Server Enterprise Edition is recommended for live systems.
SQL Server Management Studio (recommended)
```

<!-- INSTALLATION PREREQUISITES DATABASE SERVER SETTINGS -->
### Database server settings
```
   Property                                        Value                                        
1. Language                                        English                                      
2. Server Collation                                SQL_Latin1_General_CP1_CI_AS (recommended)
3. XTP support                                     True 

Comment : 
1. Select English as the default language for database users.
3. Extreme transaction processing supported (Is XTP supported) True One Identity Manager uses In-Memory-OLTP (Online Transactional Processing) for memory-optimized data accesses.
The database server must support extreme transaction processing (XTP). This function is activated by default in a default installation.
The setting is tested by the Configuration Wizard before installing or updating One Identity Manager database. 
If XTP is not activated, the installation or update does not start.

```
<!-- INSTALLATION PREREQUISITES DATABASE SETTINGS -->
### Database settings
```
   Property                                        Value                                        
1. Collation                                       SQL_Latin1_General_CP1_CI_AS

Other settings is checked by the Configuration Wizard before installing or updating the One Identity Manager database and adjusted for the database if necessary.
https://support.oneidentity.com/technical-documents/identity-manager/9.2/installation-guide/3#TOPIC-2083740
```

<!-- INSTALLATION PREREQUISITES DATABASE SETTINGS USER AND PERMISSIONS -->
### Database settings Users and permissions for the One Identity Manager database on an SQL Server
```
   Property                                        Comment                                         
1. Installation user                               The installation user is required for the initial setup of a One Identity Manager database using the Configuration Wizard.
2. Administrative user                             The administrative user is used by components of One Identity Manager that require authorizations at server level and database level, for example, the Configuration Wizard, the DBQueue Processor, or the One Identity Manager Service.
3. Configuration user                              The configuration user can run configuration tasks within the One Identity Manager, for example, creating customer-specific schema extensions or working with the Designer. Configuration users need permissions at the server and database levels.
4. End users                                       End users are only assigned permissions at database level in order, for example, to complete tasks with the Manager or the Web Portal.
```

```                        
1. # Installation users
# Roles
   Property                                        Comment    
1. Member of dbcreator server role                 The server role is only required if the database is created using the Configuration Wizard.
2. Member of the sysadmin server role              This server role is only required if the database is created by the Configuration Wizard and the directories for the file must be selected in the file browser.
                                                   If the files are stored in the default database server directories, permissions are not necessary.

3. Member of securityadmin server role             This server role is required to create SQL logins.

# Permissions
1. view server state permissions with the with grant option option and alter any connection permissions with the with grant option option.
   The server role is only required if the database is created using the Configuration Wizard.

2. alter any server role permissions, The permissions are required to create the server role for the administrative user. alter any server role permissions

# Msdb database:
1. alter any user permissions
The permissions are required to create the necessary database users for the administrative user.
2. alter any role permissions
This permission is required to create the necessary database role for the administrative user.

# master database:
1. alter any user permissions
The permissions are required to create the necessary database users for the administrative user.
2. alter any role permissions
This permission is required to create the necessary database role for the administrative user.
3. Run permissions with the with grant option option for the xp_readerrorlog procedure
The permissions are required to find out information about the database server's system status.

# One Identity Manager database:
1. Member of the db_owner database role
This database role is required for installing the schema with the Configuration Wizard in an existing database or for updating the schema.

```


## Job Server
### About Job Server


## Application server
### About Application Server


## Web server
### About Web Server



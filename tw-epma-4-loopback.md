% Mobile Webservice
% Patrick Sturm
% 30.11.2016

## Information

* Any issues with this presentation? Write a ticket or send me a pull request ;).
* Repo: [https://github.com/siyb/tw-epma-4-loopback](https://github.com/siyb/tw-epma-4-loopback)

# Agenda

## Agenda

* About Loopback
* Getting started
* Setting up a project
  * Documentation
* Configuration
* Custom Code

# About Loopback

## About Loopback - 1 - General

* loopback is an API generator
  * for simple, unauthorized APIs, all you need to create an API is the console
* strongloop, the company behind loopback was bought by IBM
  * used in IBM's "API Connect"
* written in Node.js (almost ECMA 6 compliant)
* extensible via hooks
* wizard based

## About Loopback - 2 - Client SDKs

* Features several SDKs that provide API bindings for clients:
  * iOS
  * Android
  * Angular.js

## About Loopback - 3 - Juggler

* supports multiple, different data source, not only different databases, using the same accessing API
* MySql, PostgreSQL, Oracle, IBM*, MSSql, ...
* ... and even other APIS!

# Getting Started

## Getting Started - 1 - Installation: Node.js

* Install Node.js from: https://nodejs.org/en/download/
  * Use an LTS (Long Time Support) version!
* ... if you are using a prominent linux distribution, https://github.com/nodesource/distributions might be helpful to get the newest packages!

## Getting Started - 2 - Installation: loopback

* run the following command in a terminal

```bash
sudo npm install -g strongloop
```

* npm is the "Node Package Manager"
* this will install loopback globally

# Setting up a Project

## Setting up a Project - 1 - Requirements

* we will be creating a small API in class
  * must be able to store Products and Orders
  * relation between Order and Product
* PostgreSQL as database

## Setting up a Project - 2 - Initialize the project

```bash
~/example $ slc loopback
     _-----_     
    |       |     --------------------------
    |--(o)--|    |  Let's create a LoopBack |
   `---------´   |       application!       |
    ( _´U`_ )     --------------------------
    /___A___\   /
     |  ~  |     
   __'.___.'__   
 ´   `  |° ´ Y ` 

? What's the name of your application? 
  example
? Which version of LoopBack would you like to use? 
  3.x (pre-release)
? What kind of application do you have in mind? 
  empty-server (An empty LoopBack API, without any configured models or datasources)
```

## Setting up a Project - 3 - Create a datasource

```bash
~/example % slc loopback:datasource    
? Enter the data-source name: db
? Select the connector for db: In-memory db 
(supported by StrongLoop)
Connector-specific configuration:
? window.localStorage key to use for persistence 
(browser only): 
? Full path to file for persistence 
(server only): ./memory.db
```

## Setting up a Project - 4 - Explained: Create a datasource

* creates a datasource in server/datasources.json
* can from now on be referenced by using its descriptive name 'db'

## Setting up a Project - 5 - Create a datasource adv.

```bash
~/example $ slc loopback:datasource
? Enter the data-source name: postgres
? Select the connector for postgres: PostgreSQL 
(supported by StrongLoop)
Connector-specific configuration:
? Connection String url to override other settings:
? host: localhost
? port: 5432
? user: myuser
? password: ******
? database: mydb
? Install loopback-connector-postgresql@^2.4 Yes
```

## Setting up a Project - 4 - Creating models

```bash
~/example % slc loopback:model Product                                                                                                        :(
? Enter the model name: Product
? Select the data-source to attach Product to: db (memory)
? Select model\'s base class PersistedModel
? Expose Product via the REST API? Yes
? Custom plural form (used to build REST URL): 
? Common model or server only? common
Let\'s add some Product properties now.

Enter an empty property name when done.
? Property name: name
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]: 

```

## Setting up a Project - 5 - Creating models

```bash
Let\'s add another Product property.
Enter an empty property name when done.
? Property name: price
   invoke   loopback:property
? Property type: number
? Required? Yes
? Default value[leave blank for none]: 
```

## Setting up a Project - 5 - Creating models cont.

```bash
~/example % slc loopback:model Order  
? Enter the model name: Order
? Select the data-source to attach Order to: db (memory)
? Select model\'s base class PersistedModel
? Expose Order via the REST API? Yes
? Custom plural form (used to build REST URL): 
? Common model or server only? common
Let\'s add some Order properties now.

Enter an empty property name when done.
? Property name: name
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]: 
```

## Setting up a Project - 6 - Creating models

* creates model.js and model.json in common/models
* creates model / datasource mapping in server/model-config.json
* common - used for client (Angular.js) and server alike, you can make the models server only as well
* Model.json
  * model definition
* Model.js
  * your custom code

## Setting up a Project - 7 - Setting relations

```bash
~/example % slc loopback:relation
? Select the model to create the relationship from: Order
? Relation type: has and belongs to many
? Choose a model to create a relationship with: Product
? Enter the property name for the relation: products
? Optionally enter a custom foreign key: 
```

## Setting up a Project - 8 - Explained: Setting relations

* creates relation in common/models/order.json

```json
"relations": {
  "product": {
    "type": "belongsTo",
    "model": "Product",
    "foreignKey": ""
  }
}
```

## Setting up a Project - 9 - Simple ACL

```bash
example % slc loopback:acl
? Select the model to apply the ACL entry to: (all existing models)
? Select the ACL scope: All methods and properties
? Select the access type: All (match all types)
? Select the role Any authenticated user
? Select the permission to apply Explicitly grant access
```

## Setting up a Project - 10 - Simple ACL

```bash
example % slc loopback:acl
? Select the model to apply the ACL entry to: (all existing models)
? Select the ACL scope: All methods and properties
? Select the access type: All (match all types)
? Select the role Any unauthenticated user
? Select the permission to apply Explicitly deny access
```

## Setting up a Project - 11 - Explained: Simple ACL

* adds the following ACLs to all 

```json
"acls": [
  {
    "accessType": "*",
    "principalType": "ROLE",
    "principalId": "$authenticated",
    "permission": "ALLOW"
  },
  {
    "accessType": "*",
    "principalType": "ROLE",
    "principalId": "$unauthenticated",
    "permission": "DENY"
  }
],
```

# Configuration

# Custom Code

- supports model inheritance
- different datasources, e.g. for testing
- acls




# Any Questions?


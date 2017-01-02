% Loopback
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
* ACL
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
* generally well documented

## About Loopback - 2 - General cont.

* creates a standardized environment
    * linter rules
    * style guide
    * security checks

## About Loopback - 3 - Client SDKs

* Features several SDKs that provide API bindings for clients:
    * iOS
    * Android
    * Angular.js

## About Loopback - 4 - Juggler

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
* in memory / PostgreSQL as database

## Setting up a Project - 2 - Initialize the project

```
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

```
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

## Setting up a Project - 4 - Explained: Create a datasource cont.

```json
{
  "db": {
    "name": "db",
    "localStorage": "",
    "file": "./db.sqlite",
    "connector": "memory"
  }
}
```

## Setting up a Project - 5 - Create a datasource adv.

```
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

## Setting up a Project - 6 - Creating models

```
~/example % slc loopback:model Product                                                                                                        :(
? Enter the model name: Product
? Select the data-source to attach Product to: db (memory)
? Select model's base class PersistedModel
? Expose Product via the REST API? Yes
? Custom plural form (used to build REST URL): 
? Common model or server only? common
Let's add some Product properties now.

Enter an empty property name when done.
? Property name: name
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]: 

```

## Setting up a Project - 7 - Creating models

```
Let's add another Product property.
Enter an empty property name when done.
? Property name: price
   invoke   loopback:property
? Property type: number
? Required? Yes
? Default value[leave blank for none]: 
```

## Setting up a Project - 8 - Creating models cont.

```
~/example % slc loopback:model Order  
? Enter the model name: Order
? Select the data-source to attach Order to: db (memory)
? Select model's base class PersistedModel
? Expose Order via the REST API? Yes
? Custom plural form (used to build REST URL): 
? Common model or server only? common
Let's add some Order properties now.

Enter an empty property name when done.
? Property name: name
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]: 
```

## Setting up a Project - 9 - Creating models cont.

```
Enter an empty property name when done.
? Property name: done
   invoke   loopback:property
? Property type: boolean
? Required? No
? Default value[leave blank for none]: false

Let's add another Product property.
```

## Setting up a Project - 10 - Explained: Creating models

* creates model.js and model.json in common/models
* creates model / datasource mapping in server/model-config.json
* common - used for client (Angular.js) and server alike, you can make the models server only as well
* Model.json
    * model definition
* Model.js
    * your custom code
* model inheritance is supported!

## Setting up a Project - 11 - Explained: Creating models cont.

```json
{
  "name": "Order",
  "base": "PersistedModel",
  "idInjection": true,
  "options": { "validateUpsert": true },
  "properties": {
    "name": { "type": "string", "required": true},
    "done": {"type": "boolean"}
  },
  "validations": [],
  "relations": {},
  "acls": [],
  "methods": {}
}
```

## Setting up a Project - 12 - Setting relations

```
~/example % slc loopback:relation
? Select the model to create the relationship from: Order
? Relation type: has and belongs to many
? Choose a model to create a relationship with: Product
? Enter the property name for the relation: products
? Optionally enter a custom foreign key: 
```

## Setting up a Project - 13 - Explained: Setting relations

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

## Setting up a Project - 14 - Simple ACL

```
example % slc loopback:acl
? Select the model to apply the ACL entry to: (all existing models)
? Select the ACL scope: All methods and properties
? Select the access type: All (match all types)
? Select the role Any unauthenticated user
? Select the permission to apply Explicitly deny access
```

## Setting up a Project - 15 - Simple ACL

```
example % slc loopback:acl
? Select the model to apply the ACL entry to: (all existing models)
? Select the ACL scope: All methods and properties
? Select the access type: All (match all types)
? Select the role Any authenticated user
? Select the permission to apply Explicitly grant access
```

## Setting up a Project - 15 - Explained: Simple ACL

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
]
```

## Setting up a Project - 16 - Explained: Simple ACL cont.

* disallows access to all routes to users that are not logged in
* allows access to all routes to users that are logged in

## Setting up a Project - 17 - Run & Check out API

```
node .
Web server listening at: http://0.0.0.0:3000
Browse your REST API at http://0.0.0.0:3000/explorer
```

* Visit: http://0.0.0.0:3000/explorer
* Swagger definitions: http://0.0.0.0:3000/explorer/swagger.json
* API: http://0.0.0.0:3000/api/$model

# Configuration

## Configuration - 1 - General: Environment

* When it comes to CI / CD, usually, you want to support different environments
    * test - for running test cases
    * local - local developer machine
    * development - development instance
    * staging - staging instance, preview for stakeholder (e.g. customer)
    * production - production environment, end user environment
* loopback uses NODE_ENV to set these environments

## Configuration - 2 - Loopback Config Files

* **server/component-config.json**
    * used to configure components (such as the explorer)
* **server/datasources.json**
    * contains the configuration of all data sources
* **server/middleware.json**
    * contains the configuration of all middlewares
* **server/model-config.json**
    * contains all models that should be known to the system, controls API exposure
* **server/config.json**
    * general configuration

## Configuration - 3 - Environment specific Configuration

* You can supply different configs for different environments by following the naming convention: $config.$env.json
* e.g
    * middleware.development.conf
    * datasources.test.conf
    * model-config.production.conf
* Use cases:
    * use in memory database for testing (datasource.test.conf or model-config.test.conf)
    * Enable / Disable features (middlewares / components / routes) according to ENV
      * e.g. API explorer should not run in production


# ACL

## ACL - 1 - Static / Dynamic Roles

* loopback supports ACLs (check out the earlier example)
* loopback has the notion of static and dynamic roles:
    * static roles are not bound to other entities or the state of the user
      * e.g. administrator
    * dynamic roles are either bound to other entities or are state specific, you may need to implement custom handlers
      * $owner
      * $authenticated
      * $unauthenticated
      * $everyone
* names of dynamic roles are preceeded by a '$'
* you can create custom static and dynamic roles

## ACL - 2 - Access Rules

* loopback supports the following "permissions" (order == precedence):
    * DENY - disallow access
    * ALLOW - allow access
    * DEFAULT - depends

## ACL - 3 - Access Types

* loopback understands the following "accessType"s:
    * READ
    * WRITE
    * EXECUTE

## ACL - 4 - Access Property

* loopback allows you restrict access to individual properties
    * e.g. find
    * wildcards (*) are supported
* precedence: type (e.g. READ), method (e.g. find), wildcard

## ACL - 5 - Principals

* a principal is the "thing" that enables a user to access a resource
    * we are only dealing with ROLE in this presentation
* every pricipal needs to be accompanied by a principalId, i.e. the name of the ROLE:
    * $owner, $authenticated, etc

## ACL - 6 - Example

```json
{
  "model": "Order",
  "property": "find",
  "accessType": 'EXECUTE',
  "principalType": "ROLE",
  "principalId": "$authenticated",
  "permission": "ALLOW"
}
```

# Custom Code

## Custom Code - 1 - Introduction

* loopback allows customization of logic
* you may add additional logic / routes to models
    * common/models/model.js
* you may add hooks to models
    * commons/models/model.js
* you may create express type middlewares
    * server/middleware/mymiddleware.js
* you may create components
    * server/components/mycomponent.js
* you may create boot scripts that run every time the application starts up
    * server/boot/mybootscript.js
    * useful for roles

## Custom Code - 2 - Logic / Routes

* common/models/order.js

```javascript
module.exports = function (Order) {
  Order.setDone = (id, cb) => {
    return Order
      .findById(id)
      .then(order => {
        if (!order)
          return Promise.reject(new Error('No order'));
        if (order.done)
          return Promise.resolve();
        order.done = true;
        return order.save();
      // pseudocode :)
      }).then((order) => { cb(order) })
      }).catch(cb);
```

## Custom Code - 3 - Logic / Routes cont.

```javascript
  ...
  Order.remoteMethod('markDone', {
    isStatic: true,
    description: 'Mark order done.',
    notes: 'Noop if order is done.',
    http: {
      path: '/markDone/:id',
      verb: 'put',
      status: 200,
      errorStatus: 400
    },
    accepts: {arg: 'id', type: 'number', required: true},
    returns: [
      {type: 'object', root: true}]
    });
  };
}
```

## Custom Code - 4 - Model Hooks

* do not use "model hooks", use "operation hooks" instead
  * access, before save, after save, before delete, after delete, loaded, persisted

## Custom Code - 5 - Example: Model Hooks

* common/models/order.js

```javascript
module.exports = function (Order) {
  MyModel.observe('before save', (ctx, next) => {
    // do something
    
    // do not forget to call next(),
    // will call next handler
    next();
  });
}
```

## Custom Code - 6 - Express Middleware

* See: [Express Middleware](http://expressjs.com/en/guide/using-middleware.html)
* Use case examples:
    * attach current user
    * log requests
    * parse stuff, etc
* server/middleware/mymiddleware.js

```javascript
// this is a function factory
// options comes from config
module.exports = (options) => {
  return (reqest, response, next) => {
    // do something, e.g. get current user
    // and attach her/him to request
    
    // call next!
    next();
  };
};
```

## Custom Code - 76 - Express Middleware cont.

* server/middleware.json

```json
  ...
  "routes:before": {
    "./middleware/mymiddleware": {
      "option1": 1,
      "option2": 2
    },
  },
  ....
```

## Custom Code - 8 - Component

* Use case examples:
    * global logger provider
    * message bus
    * basically: basic service code
* server/components/mycomponent.js

```javascript
// the app is the current app, options are
// provided in config file
module.exports = function (app, options) {
   const Order = app.models.Order;
   
   // do something with an order
}
```

## Custom Code - 9 - Component cont.

* server/component-config.json

```json
  ...
  "./components/mycomponent": {
      "option1": 1,
      "option2": 2
  }
  ...
```

## Custom Code - 10 - Boot Script

* Use case examples:
  * register roles
  * enable server flags (e.g. authentication)
  * basically: all programmatic setup tasks
* server/boot/mybootscript.js

```javascript
module.exports = function (app) {
   const Role = app.models.Role;
   
   // register role here
}
```

# Any Questions?


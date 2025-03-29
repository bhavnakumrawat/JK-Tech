# Steps to Execute

```sh
  # to start bulding the services
  docker-compose up --build 
```

Use this curl to get access to the application as an admin

```c
curl --location 'http://localhost:3000/user/login' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "superadmin@admin.com",
    "password": "admin"
}'
```

## Types of Users
1. Admin

    -Can perform any operation in the application, such as:
    -Registering new users
    -CRUD operations on documents
    -Creating and retrieving details of an ingestion

2. Editor

    -Can perform any operation on a document:
    -CRUD operations on documents
    -Creating and retrieving details of an ingestion

3. Viewer

    -Can perform read-only operations:
    -Viewing ingestion details
    -Reading any document

## API Documentation

The Swagger documentation is accessible at: `http://localhost:3000/api`

## User Authentication

Authentication is implemented using Passport's JWT authentication. An authentication guard is placed on all controller APIs requiring user authentication.

Relevant files:
Please see these locations,

- `gateway/src/global/guards/jwt-auth.guard.ts`

- `gateway/src/user/services/auth/strategies/jwt/jwt.strategy.ts` 

## User Authorization

CASCASL is used as the rule engine module for authorization. More details can be found at CASL Documentation.

ACL rules are built in the CASL module. Key files:
Please find more about this at https://casl.js.org/v6/en/

Bulding ACL rules based on roles is done in the casl module. Please see the the file at `/gateway/src/global/modules/casl/cals-ability.factory.ts`

The above module is used along with a decorator which is used to tag the controller apis with the appropriate permission for invoking.

`/gateway/src/global/decorators/check-permission.decorator.ts`

The guard is then used to identify the invoker's list of permission and whether the invoker can successfully execute the target api.

`gateway/src/global/guards/permission.guard.ts`


## mocked Ingestion Service

The ingestion service is another NestJS microservice running alongside the gateway. It provides APIs for adding and retrieving ingestion details, implemented using @MessagePattern.

## NOTE: 
Once a new ingestion is added, an event is fired to update its status. This event is handled asynchronously to update the status to either success or failed.

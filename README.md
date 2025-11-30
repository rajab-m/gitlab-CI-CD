# GITLAB DATASCIENTEST PROJECT

# Microservices, API Gateway, Authentication with FastAPI

- This repository consists of a set of small microservices following the API gateway approach.
- The planned number of microservices was two, but since the services
   should not create dependencies on each other to prevent SPOF, and also to avoid duplicate code,
   I decided to put an API gateway in front that handles JWT authentication for both services,
   inspired by Netflix/Zuul.
- We have 3 services including a gateway.
- Only the gateway can access the internal microservices via the internal network (users, orders).

## Services

- gateway: Built on top of FastAPI, a simple API gateway whose only duty is to provide clean
   routing while managing authentication and authorization.
- users (a.k.a admin): stores user information in its own fake database (file system).
   Simple CRUD operations can be performed via the service. There is also another
   endpoint for login, but the client is extracted from the real response. Thus, the gateway service
   will handle the login response and generate the JWT token accordingly.
- orders: Users (subscribers - authentication) can create and view (their - authorization) orders.

## Execution
- check `./gateway/.env` => 2 service URLs are set based on the twelve-factor configuration
- `docker-compose --build`
- visit the address => http://localhost:8001/docs

# Request Examples
- There are already 2 users in the users database
- get an API token with the admin user
  ```
  curl --header "Content-Type: application/json" \
       --request POST \
       --data '{"username":"admin","password":"a"}' \
       http://localhost:8001/api/login
  ```
- You will see something similar to below
  ```
  {"access_token":"***","token_type":"bearer"}
  ```
- use this token to make administrative requests
  ```
  curl --header "Content-Type: application/json" \
       --header "Authorization: Bearer ***" \
       --request GET \
       http://localhost:8001/api/users
  ```
- Similar tests can also be done with the default user to create and view orders


##Diagramme
![ScreenShot](https://github.com/DataScientest/gitlab_devops_exams/blob/main/diagram.png)

##Documentation Page
![ScreenShot](https://github.com/DataScientest/gitlab_devops_exams/blob/main/docs.png)

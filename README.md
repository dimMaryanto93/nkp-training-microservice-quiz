## Springboot microservice

This project is to show you how microservice communicate using Rest API using springboot. The architecture look like:

![architecture](docs/architecture.png)

### System requirement to run this project

- Java 17 or later
- Apache maven
- MySQL 8.0 or later, alternative using docker
- PostgreSQL 15 or later, alternative using docker

After your install pre-requirement, then you can download the dependencies using `mvn clean -DskipTests package` command.

### Module customer

Module customer using MySQL Database to store data about customer it self, to run this module you can type `mvn -DskipTests clean -pl customer spring-boot:run` command. After web server started now you can access rest api from [http://localhost:9090]

Before you start, you need modified the environment on `application.yaml` inside folder `customer/src/main/resources` such as datasource connection

For testing the rest api, you can use Postman or the other http client and import this http-request.

```http request
### request to customer
GET http://{{host}}:{{port}}{{context-path}}/api/customer/v1/findById/cust01
Accept: application/json
```

### Module order

Module order using PostgreSQL database to store about transaction order, to run this module you can type `mvn -DskipTests clean -pl orders spring-boot:run` command. After web server started now you can access rest api from [http://localhost:9091]

Before you start, you need modified the environment on `application.yaml` inside folder `ordes/src/main/resources` such as datasource connection, service customer connection 

For testing the rest api, you can use Postman or the other http client and import this http-request.

```http request
### Store request order from customer
POST http://{{host}}:{{port}}{{context-path}}/api/order/v1/checkout
Accept: application/json
Content-Type: application/json
Timeout: 3s

{
  "userId": "cust01",
  "item": "Macbook Pro 13\" (A1723)",
  "qty": "2"
}
```

## Deploy to kubernetes

Please deploy to kubernetes cluster by your namespace

tasklist: 

- compile source-code to image using `packager` service inside docker-compose
- build docker image from docker compose, please edit `<your-name>` in docker-compose.yaml
- publish/push docker image have you build to nexus oss
- create kubernetes manifest template with kustomization, put in `kubernetes` folder
- create kubernetes manifest for deployment, service, configmap/secret and ingress
- create ingress with surefix `<your-name>.microservice.nutanix.local`

information:

```yaml
- PostgreSQL:
    user: postgres
    password: nutanix/4u
    dbname: order_api
    ip: 10.38.103.120
    port: 5432
- MySQL:
    user: root
    password: nutanix/4u
    dbname: customer_api
    ip: 10.38.103.105 
    port: 3306
```

goals:
show me the response with this request

```bash
curl --location '10.38.103.52/api/order/v1/checkout' \
--header 'Content-Type: application/json; charset=UTF-8' \
--header 'Host: dimas.microservice.nutanix.local' \
--data '{
  "userId": "cust01",
  "item": "Macbook Pro 13\" (A1723)",
  "qty": "2"
}'
```

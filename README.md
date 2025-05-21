# Enterprise-Resource-Management

This example demonstrates using Spring Boot, Spring Cloud Netflix Eureka as a discovery server, Zuul as a gateway, OpenFeign for communication and Spring Cloud Config Server.

### Usage

Build the apps with images (we need ji for `config-service` since it contains `curl`):
```shell
$ mvn clean package -Pbuild-image
```

Then run all the containers with `docker-compose`:
```shell
$ docker-compose up
```

#### Run Locally

To run locally. Microservices are exposed on dynamic ports, so you can safely run them all on the same workstation.

You can run Zipkin. However, it is not necessary:
```shell
docker run -d --name zipkin openzipkin/zipkin -p 9411:9411
```

Begin with `config-service`:
```shell
cd config-service
mvn spring-boot:run
```

Then, go to `discovery-service`:
```shell
cd config-service
mvn spring-boot:run
```

Then, go run three microservices: `employee-service`, `department-service`, and `organization-service`, e.g.:
```shell
cd employee-service
mvn spring-boot:run
```

Finally, run `gateway-service`:
```shell
cd gateway-service
mvn spring-boot:run
```

By default, Eureka is running on `8061` port, and gateway is exposed under `8060` port.\
You can access global Swagger UI: http://localhost:8060/swagger-ui.html and switch between services. More details in the articles above.


In the most cases you need to have Maven and JDK8+. In the fourth example with OpenShift you will have to run **Minishift** on your machine. The best way to run the sample applications is with IDEs like IntelliJ IDEA or Eclipse.

## Architecture

Our Enterprise-Resource-Management microservices-based system consists of the following modules:
- **gateway-service** - a module that Spring Cloud Netflix Zuul for running Spring Boot application that acts as a proxy/gateway in our architecture.
- **config-service** - a module that uses Spring Cloud Config Server for running configuration server in the `native` mode. The configuration files are placed on the classpath.
- **discovery-service** - a module that depending on the example it uses Spring Cloud Netflix Eureka as an embedded discovery server.
- **employee-service** - a module containing the first of our sample microservices that allows to perform CRUD operation on in-memory repository of employees
- **department-service** - a module containing the second of our sample microservices that allows to perform CRUD operation on in-memory repository of departments. It communicates with employee-service.
- **organization-service** - a module containing the third of our sample microservices that allows to perform CRUD operation on in-memory repository of organizations. It communicates with both employee-service and organization-service.

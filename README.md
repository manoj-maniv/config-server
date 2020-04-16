# config-server
Application - Spring Cloud Config with Cloud Bus Support using RabbitMQ

### Spring Cloud Bus Refresh


**HTTP Method**: POST


**Resource**: http://localhost:8888/actuator/bus-refresh


**Security Credentials**: `user`/`password`



### RabbitMQ
#### Command to Run RabbitMQ server using Docker


```
docker run -d --hostname my-rabbit --name some-rabbit -p 15672:15672 -p 5672:5672 rabbitmq:3-management
```

RabbitMQ Server Host: http://localhost:15672/


Default credentials to login into RabbitMQ server: Username: `guest` and Password: `guest`

#### Command to Stop Container using Docker

```
docker ps -a
docker container stop <container-id>
```

#### Command to Remove Container using Docker

```
docker ps -a
docker rm <container-id/name>
```

# config-server
Application - Spring Cloud Config with Cloud Bus Support using RabbitMQ

##### build.gradle

```
plugins {
	id 'org.springframework.boot' version '2.3.0.RELEASE'
	id 'io.spring.dependency-management' version '1.0.9.RELEASE'
	id 'java'
}

group = 'com.cluster.corp'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

ext {
	set('springCloudVersion', "Hoxton.SR4")
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-actuator'
  // Spring Cloud Config support
	implementation 'org.springframework.cloud:spring-cloud-config-server'
  // Spring Cloud Bus with RabbitMQ support
	implementation 'org.springframework.cloud:spring-cloud-starter-bus-amqp'
  // Spring security to access HTTP calls
	implementation 'org.springframework.boot:spring-boot-starter-security' 
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

dependencyManagement {
	imports {
		mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
	}
}

test {
	useJUnitPlatform()
}
```

##### application.properties

```
server.port=8888
spring.cloud.config.server.git.uri=https://github.com/manoemails/config-repo.git
spring.cloud.config.server.git.username=<add your github account username here>
spring.cloud.config.server.git.password=<add your github account password here>
spring.cloud.config.server.git.search-paths=*

logging.level.org.springframework.cloud=debug

#Enable Spring Cloud Bus - Refresh
management.endpoints.web.exposure.include=bus-refresh, bus-env

#RabbitMQ Support
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest

#Spring Security - Basic Credentials
spring.security.user.name=user
spring.security.user.password=password
```

##### bootstrap.yml

```
spring:
  application:
    name: config-server
```

##### ConfigServerApplication.java

```
package com.cluster.corp.config.server;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.config.server.EnableConfigServer;

@SpringBootApplication
@EnableConfigServer
public class ConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConfigServerApplication.class, args);
	}
}
```

##### WebSecurityConfiguration.java

```
package com.cluster.corp.config.server.config;

import org.springframework.boot.actuate.autoconfigure.security.servlet.EndpointRequest;
import org.springframework.boot.actuate.health.HealthEndpoint;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;

@Configuration
public class WebSecurityConfiguration extends WebSecurityConfigurerAdapter {

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http.csrf().disable().authorizeRequests().anyRequest().permitAll().and().httpBasic();
	}
}
```

### Spring Cloud Bus Refresh


**HTTP Method**: POST


**Resource**: http://localhost:8888/actuator/bus-refresh


**Security Credentials**: `user`/`password`

**Authentication Type**: `Basic Auth`



### RabbitMQ
#### Command to Run RabbitMQ server using Docker


```
docker run -d --hostname my-rabbit --name rabbit-mq -p 15672:15672 -p 5672:5672 rabbitmq:3-management
```

RabbitMQ Server Host: http://localhost:15672/#/


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

---
title: "Secrets and properties management"
date: 2019-10-17T20:53:32-04:00
draft: false
---

# Secrets and properties management

## Properties files with Spring boot

Specify different profiles (for the different DB for example) like this in the default application.properties file: 
```
spring.profiles.active=local
```
or at runtime like that: 
```bash
java -Dspring.profiles.active=local -jar yourApplication.jar 
```

It will automatically take the corresponding properties file in the classpath application-**local**.propertie

## Secrets with Spring boot

When you have to specify a secret, just define a placeholder instead: 
```
spring.datasource.password=${db-password}
```
and pass the value at runtime with a Java -D variable:
```
-Ddb-password=example
```
Or you can also externalize your config folder or just a properties file:
[Spring Properties File Outside jar](https://www.baeldung.com/spring-properties-file-outside-jar)
and also pass the locations at runtime with an environment variable or just a java -D variable.


## Environment and Java variables

Problem is how to pass the environment variables or the java -D variables when your jar is embeded inside a docker image ? 

### Dockerfile

```dockerfile
#https://docs.docker.com/engine/reference/builder/#using-arg-variables  
FROM openjdk:8-jdk-alpine  
ARG JAR_FILE  
ADD ${JAR_FILE} app.jar  
ENTRYPOINT ["java", "-jar", "-Dspring.profiles.active=${PROFILE}", "/app.jar"]
```

`${PROFILE}` is referencing an environment variable that will be resolved at the runtime only, not during the build time.

`${JAR_FILE}` is referencing an argument passed within the docker build command (launched from **docker-compose** or **maven**). This variable is not used anymore after the build is done.

[more info on the env variables here](https://vsupalov.com/docker-arg-env-variable-guide/#the-dot-env-file-env)

### Docker-compose

```docker-compose
app:  
  image: "dakarinternational/dakar:latest"  
  env_file: .env  
  build:  
    dockerfile: Dockerfile  
    context: .  
    args:  
      JAR_FILE: $JAR_FILE  
  ports:  
   - "8080:8080"  
  container_name: 'app-dakar'  
  depends_on:  
   - "couch"
```

Here is the .env file: 

```docker
PROFILE=prod  
JAR_FILE=target/dakar-0.0.3.jar
```

NB: *Please note that the JAR_FILE could (and should) be put directly in the args section. 
No need for such an environment variable just for the build time, it's here just to demonstrate how we can use env variables within the docker-compose file.*
 

## Maven

Here is the config for the docker plugin: 

```xml
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>dockerfile-maven-plugin</artifactId>
                <version>1.4.10</version>
                <configuration>
                    <!--<serverId>docker-hub</serverId>-->
                    <useMavenSettingsForAuth>true</useMavenSettingsForAuth>
                    <!--<registryUrl>https://index.docker.io/v1/</registryUrl>-->
                    <repository>dakarinternational/dakar</repository>
                    <buildArgs>
                        <JAR_FILE>target/${project.build.finalName}.jar</JAR_FILE>
                    </buildArgs>
                </configuration>
            </plugin>

```

Notice the `<JAR_FILE>` build argument.  

## CircleCI

If you don't want to use Docker-compose environment variables, then you can also pass directly *circleci secret environment variable* like this: 
```
- run:  
    name: java command   
    command: |  
      java -Ddb-password=$DB_PWD -jar blabla.jar 
```
Notice the `$DB_PWD` circleci secret environment variable



## Vault by HashiCorp

Another solution to avoid all these variables transmissions is to use Vault.


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU5MjY2OTQ2Niw0NjQ2Njk4MTcsODczOT
A3MTQ3XX0=
-->
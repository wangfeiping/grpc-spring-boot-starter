# microservice

```
172.17.15.51
172.17.15.52
172.17.15.53
172.17.15.54

2C4G 100GB
```

# 示例代码

https://github.com/yidongnan/grpc-spring-boot-starter

https://github.com/yidongnan/grpc-spring-boot-starter/tree/master/examples

# 172.17.15.54

```
zipkin

$ docker run -d -p 9411:9411 openzipkin/zipkin

http://172.17.15.54:9411/

```

# 172.17.15.53 Eureka 单机

```
gradle

$ wget https://services.gradle.org/distributions/gradle-6.5.1-bin.zip
$ unzip gradle-6.5.1-bin.zip
$ vi /etc/profile
export PATH=$PATH:/apps/gradle-6.5.1/bin
$ gradle -v

cloud-eureka-server

$ git clone https://github.com/wangfeiping/grpc-spring-boot-starter.git
$ cd grpc-spring-boot-starter/
$ git checkout dev

$ ./gradlew :example:cloud-eureka-server:bootRun
or
$ ./gradlew :example:cloud-eureka-server:build
$ java -jar examples/cloud-eureka-server/build/libs/cloud-eureka-server.jar

http://172.17.15.53:8761/

```

# Eureka 集群

### 172.17.15.53

$ vi start-eureka.sh

$ # sh start-eureka.sh

```
#!/bin/bash

# cloud-eureka-server
# http://172.17.15.53:8763/

nohup java -jar /apps/eureka/cloud-eureka-server.jar \
        --spring.profiles.active=eurekaServer53 \
    >> /apps/logs/eureka/eureka.log 2>&1 &

```

### 172.17.15.52

$ vi start-eureka.sh

$ # sh start-eureka.sh

```
#!/bin/bash

# cloud-eureka-server
# http://172.17.15.52:8762/

nohup java -jar /apps/eureka/cloud-eureka-server.jar \
        --spring.profiles.active=eurekaServer52 \
    >> /apps/logs/eureka/eureka.log 2>&1 &

```

# 172.17.15.52

```
cloud-grpc-server

$ git clone https://github.com/wangfeiping/grpc-spring-boot-starter.git
$ cd grpc-spring-boot-starter/
$ git checkout dev

$ vi examples/cloud-grpc-server/src/main/resources/application.yml
spring:
  ...
  zipkin:
    base-url: http://172.17.15.54:9411
...
eureka:
  ...
  client:
    ...
    service-url:
      defaultZone: http://172.17.15.52:8762/eureka/,http://172.17.15.53:8763/eureka/

$ ./gradlew :example:cloud-grpc-server:bootRun
or
$ ./gradlew :example:cloud-grpc-server:build
$ java -jar examples/cloud-grpc-server/build/libs/cloud-grpc-server.jar

http://172.17.15.52:9090/actuator/info

```

# 172.17.15.51

```
cloud-grpc-client

$ git clone https://github.com/wangfeiping/grpc-spring-boot-starter.git
$ cd grpc-spring-boot-starter/
$ git checkout dev

$ vi examples/cloud-grpc-client/src/main/resources/application.yml
spring:
  ...
  zipkin:
    base-url: http://172.17.15.54:9411
...
eureka:
  ...
  client:
    ...
    service-url:
      defaultZone: http://172.17.15.52:8762/eureka/,http://172.17.15.53:8763/eureka/

$ ./gradlew :example:cloud-grpc-client:bootRun
or
$ ./gradlew :example:cloud-grpc-client:build
$ java -jar examples/cloud-grpc-client/build/libs/cloud-grpc-client.jar

http://172.17.15.51:8080/actuator/info

```

curl http://172.17.15.51:8080/

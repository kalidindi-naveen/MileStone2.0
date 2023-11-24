## Multi Stage Build
```
code →→ compile →→ package = bytecode = compiled code = .jar file

To Generate .jar
We need
        JDK
        Maven(with lot of dependencies)

when we include JDK + Maven then Image size will be more
Note: Once .jar file generated we don't need JDK & Maven

To run Java Code
We need
        JRE (lesser size than above)

We get .jar file ?? From Build Stage
```

```
FROM almalinux AS jarGen
RUN yum -y update && yum -y install maven
WORKDIR /opt/shipping
COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve
COPY src /opt/shipping/src/
RUN mvn package

FROM amazoncorretto:8-alpine3.18-jre
EXPOSE 8080
WORKDIR /opt/shipping
ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql
COPY --from=jarGen /opt/shipping/target/shipping-1.0.jar shipping.jar
CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]
```

## Docker Image Best Practicies
```
- use light weight images like alpine,busybox,...
- use Multi Stage builds to remove unnecessary installations
- use non root users
- use volumes for stateful applications
- use env variables insted of hardcoding
- use dedicated custom network
- don't keep secrets in image
- scan images & fix vulnerabilites
- limit resources like CPU,RAM
- Configure Health Check's
```
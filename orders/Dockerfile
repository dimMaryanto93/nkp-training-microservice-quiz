ARG JDK_VERSION=17-alpine
FROM eclipse-temurin:${JDK_VERSION}
LABEL maintainer="Dimas Maryanto <software.dimas_m@icloud.com>"

ARG JAR_FILE="orders-api.jar"
# copy file from local to images then rename to spring-boot.jar
ADD target/$JAR_FILE spring-boot.jar

ENV APPLICATION_PORT=8080
ENV PROFILE=default
ENV DATABASE_USER=mysql
ENV DATABASE_PASSWORD=mysql_password
ENV DATABASE_HOST=localhost
ENV DATABASE_NAME=mysql_db
ENV DATABASE_PORT=3306
ENV FLYWAY_ENABLED=true

# reqired command to run application
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "spring-boot.jar"]

#stage 1
#Start with a base image containing java runtime
FROM openjdk:11-slim as build

WORKDIR application

ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar application.jar extract

FROM openjdk:11-slim
WORKDIR application
# copy each layer
COPY --from=build application/dependencies/ ./
COPY --from=build application/spring-boot-loader/ ./
COPY --from=build application/snapshot-dependencies/ ./
COPY --from=build application/application/ ./

ENTRYPOINT ["java","org.springframework.boot.loader.JarLauncher"]

# docker build . --tag licensing-service
# docker run -it -p 8080:8080 licensing-service:latest
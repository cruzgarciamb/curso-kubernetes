FROM openjdk:17-jdk-alpine AS builder

WORKDIR /app/msvc-gateway

COPY ./pom.xml /app
COPY ./msvc-gateway/.mvn ./.mvn
COPY ./msvc-gateway/mvnw .
COPY ./msvc-gateway/pom.xml .

# descarga dependencias, salta el test y la compilación del codigo
#RUN ./mvnw clean package -Dmaven.test.skip -Dmaven.main.skip -Dspring-boot.repackage.skip && rm -r ./target/
RUN ./mvnw dependency:go-offline

COPY ./msvc-gateway/src ./src

RUN chmod +x ./mvnw
RUN ./mvnw clean package -DskipTests


FROM openjdk:17-jdk-alpine
WORKDIR /app
RUN mkdir ./logs
COPY --from=builder /app/msvc-gateway/target/msvc-gateway-0.0.1-SNAPSHOT.jar .
EXPOSE 8090

CMD [ "java", "-jar", "msvc-gateway-0.0.1-SNAPSHOT.jar"]
FROM openjdk:17-jdk-alpine AS builder

WORKDIR /app/msvc-usuarios

COPY ./pom.xml /app
COPY ./msvc-usuarios/.mvn ./.mvn
COPY ./msvc-usuarios/mvnw .
COPY ./msvc-usuarios/pom.xml .

# descarga dependencias, salta el test y la compilación del codigo
RUN ./mvnw clean package -Dmaven.test.skip -Dmaven.main.skip -Dspring-boot.repackage.skip && rm -r ./target/
#RUN ./mvnw dependency:go-offline

COPY ./msvc-usuarios/src ./src

RUN chmod +x ./mvnw
RUN ./mvnw clean package -DskipTests


FROM openjdk:17-jdk-alpine
WORKDIR /app
COPY --from=builder /app/msvc-usuarios/target/msvc-usuarios-0.0.1-SNAPSHOT.jar .
EXPOSE 8001
ENTRYPOINT [ "java", "-jar", "msvc-usuarios-0.0.1-SNAPSHOT.jar"]
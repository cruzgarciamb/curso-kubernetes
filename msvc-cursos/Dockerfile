FROM openjdk:17-jdk-alpine AS builder

WORKDIR /app/msvc-cursos

COPY ./pom.xml /app
COPY ./msvc-cursos/.mvn ./.mvn
COPY ./msvc-cursos/mvnw .
COPY ./msvc-cursos/pom.xml .

# descarga dependencias, salta el test y la compilación del codigo
#RUN ./mvnw clean package -Dmaven.test.skip -Dmaven.main.skip -Dspring-boot.repackage.skip && rm -r ./target/
RUN ./mvnw dependency:go-offline

COPY ./msvc-cursos/src ./src

RUN chmod +x ./mvnw
RUN ./mvnw clean package -DskipTests


FROM openjdk:17-jdk-alpine
WORKDIR /app
RUN mkdir ./logs
COPY --from=builder /app/msvc-cursos/target/msvc-cursos-0.0.1-SNAPSHOT.jar .
EXPOSE 8002

CMD [ "java", "-jar", "msvc-cursos-0.0.1-SNAPSHOT.jar"]
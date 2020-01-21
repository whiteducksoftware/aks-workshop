# Containerize a Java Spring application

## Sample code

We will use a sample project from Spring. You can clone it here:

```
git clone https://github.com/spring-guides/gs-spring-boot-docker.git
```

## Containerize the app

Now take some time and review the Dockerfile located at `complete\Dockerfile`. Because we would like to build the app using a multi-stage build we need to customize it based on the below example:

```
FROM openjdk:8-jdk-alpine as build
WORKDIR /workspace/app

COPY . ./
RUN ./mvnw install -DskipTests \
  && mkdir -p target/dependency \
  && (cd target/dependency; jar -xf ../*.jar)

FROM openjdk:8-jre-alpine
RUN adduser \
  --disabled-password \
  --home /app \
  --gecos '' app \
  && chown -R app /app
USER app

ARG DEPENDENCY=/workspace/app/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app

EXPOSE 8080
ENTRYPOINT ["java","-cp","app:app/lib/*","hello.Application"]
```

To build the application as well as the container image we will use the Docker CLI and execute it within the `complete` directory:

```
docker build -t myspringapp:latest .
```

## Run the containerized app

We now are ready to run the app locally with Docker:

```
docker run --rm -td -p 8080:8080 myspringapp:latest
```

Now you are ready to test the app with `curl http://localhost:8080`. If everything went well you should now see the output `Hello Docker World`.


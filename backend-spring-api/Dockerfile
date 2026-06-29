FROM gradle:8.10.2-jdk21 AS build

WORKDIR /app

COPY build.gradle .
COPY settings.gradle .
COPY src ./src

RUN gradle clean bootJar --no-daemon

RUN set -eux; \
    ls -la build/libs; \
    JAR_FILE=$(find build/libs -maxdepth 1 -type f -name "*.jar" ! -name "*plain.jar" | head -n 1); \
    test -n "$JAR_FILE"; \
    cp "$JAR_FILE" /app/app.jar; \
    ls -la /app/app.jar

FROM eclipse-temurin:21-jre

WORKDIR /app

COPY --from=build /app/app.jar app.jar

EXPOSE 3004

ENTRYPOINT ["java", "-jar", "app.jar"]

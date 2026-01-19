FROM gradle:8.5.0-jdk21 AS build
WORKDIR /app

COPY gradle gradle
COPY gradlew .
COPY build.gradle* settings.gradle* gradle.properties* ./

RUN chmod +x ./gradlew && ./gradlew dependencies --no-daemon || true

COPY src src
RUN ./gradlew clean bootJar -x test --no-daemon

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /app/build/libs/*.jar app.jar
ENV SPRING_PROFILES_ACTIVE=oracle-dev-group
EXPOSE 8080
ENTRYPOINT ["sh","-c","java -Dspring.profiles.active=${SPRING_PROFILES_ACTIVE} -jar /app/app.jar"]

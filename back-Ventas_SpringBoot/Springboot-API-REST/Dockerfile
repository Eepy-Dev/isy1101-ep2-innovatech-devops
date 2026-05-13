FROM eclipse-temurin:17-jdk-alpine AS builder

WORKDIR /build

COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn
RUN chmod +x mvnw && ./mvnw dependency:go-offline -B

COPY src src
RUN ./mvnw clean package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine AS production

RUN addgroup -S spring && adduser -S spring -G spring
WORKDIR /app

COPY --from=builder /build/target/*.jar app.jar
RUN chown spring:spring app.jar

USER spring
EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]

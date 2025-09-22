ARG MAVEN_VERSION=3.9.6-eclipse-temurin-17
ARG JDK_VERSION=17-jdk-jammy
# -------- Stage 1: Build --------
FROM maven:${MAVEN_VERSION} AS builder
WORKDIR /app
# Copy pom.xml and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Copy source code and build
COPY src ./src
RUN mvn clean package -DskipTests

# -------- Stage 2: Runtime --------
FROM eclipse-temurin:${JDK_VERSION}
WORKDIR /app
# Copy only the built JAR from the builder stage
COPY --from=builder /app/target/history-service-1.0.0.jar app.jar
# Expose application port (from application.properties)
EXPOSE 8081
# Run the Spring Boot app
ENTRYPOINT ["java", "-jar", "app.jar"]

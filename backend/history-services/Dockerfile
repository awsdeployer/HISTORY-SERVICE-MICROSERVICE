# -------- Stage 1: Build --------
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /app
# Copy pom.xml and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Copy source code and build
COPY src ./src
RUN mvn clean package -DskipTests

# -------- Stage 2: Runtime --------
FROM eclipse-temurin:17-jdk-jammy
WORKDIR /app
# Create a non-root user and group
RUN groupadd -r appuser && useradd -r -g appuser appuser
# Copy only the built JAR from the builder stage
COPY --from=builder /app/target/history-service-1.0.0.jar app.jar
# Change ownership to the non-root user
RUN chown appuser:appuser app.jar
# Switch to non-root user
USER appuser
# Expose application port (from application.properties)
EXPOSE 8081
# Run the Spring Boot app
ENTRYPOINT ["java", "-jar", "app.jar"]


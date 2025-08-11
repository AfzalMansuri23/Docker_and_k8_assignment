# ---------- Stage 1: Build Stage ----------
FROM maven:3.9.6-eclipse-temurin-17 AS builder

# Set working directory
WORKDIR /app

# Copy only pom.xml first to cache dependencies
COPY pom.xml ./
RUN mvn dependency:go-offline

# Copy the rest of the source code
COPY src ./src

# Package the application (skip tests for faster build)
RUN mvn clean package -DskipTests


# ---------- Stage 2: Runtime Stage ----------
FROM eclipse-temurin:17-jre-alpine

# Set working directory
WORKDIR /app

# Copy the packaged jar from build stage
COPY --from=builder /app/target/*.jar app.jar

# Add a non-root user for better security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Expose the app's port
EXPOSE 8080

# Command to run the app
ENTRYPOINT ["java", "-jar", "app.jar"]

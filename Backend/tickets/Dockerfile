# Multi-stage build for optimized production image
FROM maven:3.9-openjdk-21-slim AS build

# Set working directory
WORKDIR /app

# Copy Maven wrapper and pom.xml first (for better layer caching)
COPY pom.xml ./
COPY .mvn .mvn/
COPY mvnw ./

# Make mvnw executable
RUN chmod +x mvnw

# Download dependencies (cached layer if pom.xml doesn't change)
RUN ./mvnw dependency:go-offline -B

# Copy source code
COPY src src/

# Build the application
RUN ./mvnw clean package -DskipTests -B

# Production stage - smaller runtime image
FROM openjdk:21-jdk-slim

# Install curl for health checks
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# Create non-root user for security
RUN addgroup --system spring && adduser --system spring --ingroup spring

# Set working directory
WORKDIR /app

# Copy the built JAR from build stage
COPY --from=build /app/target/tickets-*.jar app.jar

# Change ownership to spring user
RUN chown spring:spring app.jar

# Switch to non-root user
USER spring

# Expose port (Render will override this with PORT env var)
EXPOSE 8081

# Health check using your Actuator endpoint
HEALTHCHECK --interval=30s --timeout=10s --start-period=60s --retries=3 \
  CMD curl -f http://localhost:${PORT:-8081}/actuator/health || exit 1

# Set JVM options optimized for containers
ENV JAVA_OPTS="-XX:MaxRAMPercentage=75.0 -XX:+UseContainerSupport -Djava.security.egd=file:/dev/./urandom"

# Run the application
CMD ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]

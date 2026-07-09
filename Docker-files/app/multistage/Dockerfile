# Build stage: use Maven with Eclipse Temurin JDK 21 on Ubuntu Jammy
FROM maven:3.9.9-eclipse-temurin-21-jammy AS BUILD_IMAGE

# Copy the entire project into the build container at /app
COPY . /app

# Set the working directory so all following commands run from /app
WORKDIR /app

# Run Maven install to compile, test, and package the application
RUN mvn install

# Runtime stage: start from an official Tomcat image with JDK 21
FROM tomcat:10-jdk21

# Remove default Tomcat webapps to avoid deploying sample apps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy the built WAR file from the build stage into Tomcat's ROOT deployment path
COPY --from=BUILD_IMAGE app/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

# Declare the port Tomcat listens on at runtime
EXPOSE 8080

# Start Tomcat in the foreground when the container starts
CMD ["catalina.sh", "run"]

# Build Stage
FROM maven:3.6.3-openjdk-11 AS build

WORKDIR /opt/shipping

# Copy only the POM file to resolve dependencies
COPY pom.xml /opt/shipping/

# Resolve Maven dependencies
RUN mvn dependency:resolve

# Copy the rest of the source code
COPY src /opt/shipping/src/

# Build the application
RUN mvn package

# Final Stage
FROM openjdk:8-jdk

EXPOSE 8080

WORKDIR /opt/shipping

ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

# Copy the built JAR from the build stage
COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

CMD ["java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar"]
FROM amazoncorretto:17

ENV JAVA_TOOL_OPTIONS="-XX:MaxRAMPercentage=75.0"
ARG JAR_FILE=build/libs/simple-backend.jar

COPY ${JAR_FILE} application.jar

EXPOSE 8080
EXPOSE 9000

RUN yum install procps -y

CMD ["java", "-jar", "/application.jar"]
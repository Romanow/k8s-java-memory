FROM amazoncorretto:17

ARG JAR_FILE=build/libs/simple-backend.jar
COPY ${JAR_FILE} application.jar

EXPOSE 8080
EXPOSE 9000

RUN yum install procps -y

CMD ["java", "-XX:MaxRAMPercentage=750.0", "-Dcom.sun.management.jmxremote=true", "-Dcom.sun.management.jmxremote.port=9000", "-Dcom.sun.management.jmxremote.rmi.port=9000", "-Djava.rmi.server.hostname=127.0.0.1", "-Dcom.sun.management.jmxremote.ssl=false", "-Dcom.sun.management.jmxremote.authenticate=false", "-Dcom.sun.management.jmxremote.local.only=false", "-jar", "/application.jar"]
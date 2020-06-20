FROM openjdk:8-jdk
ENV  APP="java-app-demo"
COPY $APP/  /$APP/
WORKDIR /$APP
CMD java $JAVA_AGENT_OPTS -jar java-app-demo-0.0.1-SNAPSHOT.jar

FROM openjdk:24-ea-17-jdk-slim
VOLUME /tmp
ADD ./build/libs/*.jar demo-jenkins.jar

ENTRYPOINT ["java", "-jar","/demo-jenkins.jar"]

# bellsoft 의 openjre17을 사용하여 가벼운 이미지 생성
#FROM bellsoft/liberica-openjre-alpine:17

# 빌드 이미지에서 생성된 JAR 파일 복사
#COPY *.jar /app.jar

# 애플리케이션이 실행될 포트 노출
#EXPOSE 8080

# JAR 파일 실행
#ENTRYPOINT ["java", "-jar", "/app.jar"]
# 첫 번째 베이스 이미지 (별칭을 지정하면 이후 다른 베이스 이미지에서 사용 가능)
FROM maven:3.8.4-openjdk-11 AS build

# 이제부터 컨테이너 기본 경로는 /app
WORKDIR /app

# EC2 project -> maven:3.8.4-openjdk-11 image (Host to Image)
# COPY <Host> <Image>
# pom.xml : EC2에 Dokerfile과 같은 곳에 있는 pom.xml
# .       : 이미지의 /app를 의미함 (바로 직전에 기본 경로를 /app으로 설정했기 때문)
COPY pom.xml .

COPY src ./src

# pom.xml과 src를 이용해 빌드 수행
RUN mvn clean package



# 두 번째 베이스 이미지
FROM tomcat:9.0-jdk11

# maven:3.8.4-openjdk-11 image 이미지(별칭 build)에 있는 project-1.0.0.war 파일을 tomcat:9.0-jdk11 이미지에 복사하기
# 다른 이미지의 파일을 복사해 올 땐 --from 옵션을 추가하여 처리
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/project.war

# 포트
EXPOSE 8080

# tomcat 이미지는 이미 CMD ["catalina.sh", "run"] 동작을 하기 때문에 도커 파일에 작성할 필요가 없음
# 첫 번째 베이스 이미지 (별칭을 지정하면 이후 다른 베이스 이미지에서 사용 가능)
FROM maven:3.8.4-openjdk-11 AS mvn-build

# 이제부터 컨테이너 기본 경로는 /app
WORKDIR /app

# EC2 project -> maven image (HOST to Container)
# COPY <HOST> <Container>
# pom.xml : EC2에 Dockerfile과 같은 곳에 있는 pom.xml
# .       : 컨테이너의 /app을 의미함 (바로 직전에 기본 경로를 /app으로 설정했기 때문)
COPY pom.xml .
COPY src ./src

# pom.xml과 src를 이용해 빌드 수행
RUN mvn clean package && echo "app build completed!"

# 두 번째 베이스 이미지
FROM tomcat:9.0-jdk11

# maven 컨테이너에 있는 Project-1.0.0.war 파일을 tomcat 컨테이너에 복사하기
# 다른 컨테이너의 파일 복사는 --from 옵션을 추가하여 처리
COPY --from=mvn-build /app/target/*.war /usr/local/tomcat/webapps/Project.war

# 포트
EXPOSE 8080

## 이 Dockerfile을 빌드하면 내 자바 프로젝트가 Tomcat에서 자동 실행되는 Docker 이미지가 만들어집니다
## 빌드 환경(Maven)과 실행 환경(Tomcat)을 분리해서 이미지 최적화
## 최종 이미지는 Tomcat만 포함 → 가볍고 배포용으로 적합
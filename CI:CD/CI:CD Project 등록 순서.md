# CI/CD Project 등록 순서

> 아래의 예제는 e2e-batch 프로젝트를 예시로 들었습니다.



1. 프로젝트와 gitlab 연동

2. Gitlab -> Jenkins 연동

   - Jenkins 작업 생성 (pipeline)
     - Build Triggers -> Build when a change is pushed to GitLab. GitLab webhook URL: http://...
     - Pipeline script from SCM(Git)

   - gitlab의 web hook(settings -> Integration)
     - URL : http://host:port/project/e2e-batch

3. 프로젝트의 root경로에 아래의 파일을 생성/수정

   - Jenkinsfile

   - pipeline.properties

   - Dockerfile

   - build.gradle : nexus에서 library를 가져오게 설정

     ```groovy
     plugins {
         id 'org.springframework.boot' version '2.1.6.RELEASE'
         id 'java'
     }
     
     apply plugin: 'io.spring.dependency-management'
     
     group = 'com.kt.iot.e2e'
     version = '0.0.1-SNAPSHOT'
     sourceCompatibility = '1.8'
     
     configurations {
         developmentOnly
         runtimeClasspath {
             extendsFrom developmentOnly
         }
         compileOnly {
             extendsFrom annotationProcessor
         }
     }
     
     repositories {
         maven {
             url 'http://host:port/repository/maven-central/'
             credentials {
                 username = "username"
                 password = "password"
             }
         }
     }
     
     dependencies {
     	...
     }
     
     uploadArchives {
         repositories {
             mavenDeployer {
                 repository(url: 'http://host:port/repository/maven-central/') {
                     authentication(userName: 'username', password: 'password')
                 }
             }
         }
     }
     ```

4. kubernetes로 pod를 생성

   - 현재 배포 방법은 기존에 있는 pod에서 image만 교체하여 배포하는 방식 

   - Rolling Deployment

   - k apply -f e2e-api.yaml

     ```yaml
     apiVersion: apps/v1beta1
     kind: Deployment
     metadata:
       name: e2e-api
       labels:
         app: e2e-api
     spec:
       replicas: 3
       minReadySeconds: 5
       strategy:
        type: RollingUpdate
        rollingUpdate:
          maxSurge: 1
          maxUnavailable: 0
       selector:
         matchLabels:
           app: e2e-api
       template:
         metadata:
           labels:
             app: e2e-api
         spec:
           containers:
           - name: e2e-api
             image: reg.dev.com/e2e-api
     //			image: reg.dev.com/e2e-api:latest
             ports:
             - containerPort: 8081
               name: http
           imagePullSecrets:
           - name: regcred
     ---
     apiVersion: v1
     kind: Service
     metadata:
       name: e2e-api
       labels:
         app: e2e-api
     spec:
       selector:
         app: e2e-api
       ports:
       - name: http
         port: 8081
         nodePort: 31001
       type: LoadBalancer
     ```

5. 처음에 reg.dev.com/e2e-batch:latest 라는 이미지가 하나라도 있어야 업데이트 되는구나….


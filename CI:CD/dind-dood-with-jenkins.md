---
title: dind dood with jenkins
typora-copy-images-to: dind dood with jenkins
date: 2019-06-21 16:42:41
tags:
categories: devops
---



1. docker로 실행되는 jenkins안에서 docker build를 하려면 일단 내부에 docker가 필요하다.

2. 그래서, 처음 docker image를 만들때, container에 docker도 함께 설치해야한다. (docker in docker)

   - 이때, os도 함께 설치해주면 dood를 안해도 된다.??
   - dood는 /var/run/docker.sock와 관련이 있다. 이 파일은 unix와 docker daemon과 통신을 도와주는 파일이다.
     근데 container에 os가 없으면 통신할 대상이 없다. 그래서 dood를 이용하여 host os와 docker.sock를 공유하여 사용할수 있다.

   

## docker in docker

> Docker container안에 docker를 설치하여 jenkins로 docker build를 할 수 있게 한다.



jenkins와 docker를 같은 conatiner에 갖도록 Dockerfile을 만들어 주어야 한다.

```dockerfile
FROM jenkins/jenkins:lts
USER root
RUN apt-get update && apt-get install -y tree nano curl sudo
RUN curl https://get.docker.com/builds/Linux/x86_64/docker-latest.tgz | tar xvz -C /tmp/ && mv /tmp/docker/docker /usr/bin/docker
RUN usermod -a -G sudo jenkins
RUN echo "jenkins ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers
USER jenkins
```

docker image를 만들어 준다.

```shell
$ docker build -t jenkins_dind .
```

하지만, 이것만으로는 사용할 수 없다. 위의 image에서는 os가 설치되지 않아 docker daemon이 통신할 대상이 없다. 이 통신할 대상을 밖에서 가지고 와야한다. 그래서 docker out of docker(dood)가 필요하다.



## docker out of docker

> volume으로 sock파일을 연결한다.

```shell
$ docker run -d --name jenkins -p 9001:8080 -p 9002:50000 -v /home/tacsadm/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins_dind
```

- /var/run/docker.sock : docker daemon과 unixsocket과 통신하기 위해 사용
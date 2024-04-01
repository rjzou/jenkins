FROM docker:dind
ENV DOCKER_HOST=unix:///var/run/docker.sock
RUN mkdir -p /etc/docker && echo -e '{"registry-mirrors": ["https://mirror.ccs.tencentyun.com"],"insecure-registries": ["192.168.3.38:80"]}' > /etc/docker/daemon.json


#docker build -t mydocker:dind .
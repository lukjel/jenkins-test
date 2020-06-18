# jenkins-test


Uruchomienie jenkins'a:

volume z certyfikatami
`docker volume create jenkins-docker-certs`

volume z danymi:
 `docker volume create jenkins-data`

uruchomienie dockera z java 11:
`docker run -it --rm -p 8080:8080 -v jenkins-data:/var/jenkins_home -v jenkins-docker-certs:/certs/client -v /var/run/docker.sock:/var/run/docker.sock --name jenkins jenkins/jenkins:jdk11`

zbudowanie własnego jenkinsa:
`docker build -t jenkins-maven .`

alternatywnie z piepeline

`docker network create jenkins`

```
docker container run --name jenkins-docker --rm --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 docker:dind
```

```
docker container run --name jenkins-blueocean --rm --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean
```


`docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword`


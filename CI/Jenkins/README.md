# Dockerfile

1. Open up a command prompt window and similar to the macOS and Linux instructions above do the following
2. Create a bridge network in Docker

```bash
docker network create jenkins
```

3. Run a docker:dind Docker image

```bash
# Replace \ with ^ or delete when running in Windows
docker run --name jenkins-docker --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  docker:dind
```

4. Customise official Jenkins Docker image, by executing below two steps
   - Create Dockerfile with the following content in the root directory
   - Build a new docker image from this Dockerfile and assign the image a meaningful name, e.g. "my-jenkins":

```bash
docker build -t my-jenkins .
```

5. Run your own my-jenkins image as a container in Docker using the following docker run command:

```bash
# Replace \ with ^ or delete when running in Windows
docker run --name my-jenkins-container --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --publish 8080:8080 --publish 50000:50000 my-jenkins
```

## Notes

- We can get the admin password from what command returns.

```bash
docker exec -it my-jenkins /bin/bash
cat /var/jenkins_home/secrets/initialAdminPassword
```

- If you are going to deploy an application within the same container, do not forget to expose the port in the docker run command. e.g.

```Dockerfile
USER root
RUN apt-get update && apt-get install -y curl software-properties-common
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get update && apt-get install -y nodejs
```

```bash
docker run --name my-jenkins-container --detach
  ...
  --publish 80:80 my-jenkins
```

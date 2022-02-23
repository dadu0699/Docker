FROM jenkins/jenkins:lts-jdk11

USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli

USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean:1.25.2 docker-workflow:1.28"

USER root
RUN apt-get update && apt-get install -y curl software-properties-common
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get update && apt-get install -y nodejs


# docker image build -t my-jenkins-docker .

# Replace \ with ^ when running in Windows
# docker run -it --name jenkins-docker \
#   -p 8080:8080 -p 50000:50000 \
#   -e DOCKER_CERT_PATH=/certs/client -e DOCKER_TLS_VERIFY=1 \
#   -v jenkins-data:/var/jenkins_home \ 
#   -v jenkins-docker-certs:/certs/client:ro \
#   --restart unless-stopped my-jenkins-docker

FROM openjdk:11 AS build_image

RUN apt-get update && \
    apt-get install -y wget gnupg && \
    ADD https://packages.apache.org/maven-keys distro-keys && \
    apt-key add distro-keys && \
    echo "deb http://ppa.launchpad.net/linuxuprising/maven/ubuntu focal main" > /etc/apt/sources.list.d/maven.list && \
    apt-get update && \
    apt-get install -y maven git && \
    rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/devopshydclub/vprofile-project.git
RUN cd vprofile-project && git checkout docker && mvn install

FROM tomcat:9-jre11

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=BUILD_IMAGE vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]

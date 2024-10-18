FROM openjdk:11 AS BUILD_IMAGE

# Update and install required packages with retry logic
RUN for i in {1..5}; do \
        apt-get update && \
        apt-get install -y --no-install-recommends maven git && \
        rm -rf /var/lib/apt/lists/* && \
        break || \
        if [ $i -lt 5 ]; then sleep 10; else exit 1; fi; \
    done

# Clone and build the project
RUN git clone https://github.com/devopshydclub/vprofile-project.git && \
    cd vprofile-project && \
    git checkout docker && \
    mvn install

FROM tomcat:9-jre11

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=BUILD_IMAGE vprofile-project/target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080
CMD ["catalina.sh", "run"]
# jogetworkflow/joget-community
FROM jogetworkflow/ubuntu-openjdk-mysql:16.04

# set environment variables
ENV JOGET_VERSION joget-linux-6.0.2
ENV JOGET_VIRTUALHOST false
ENV ASPECTJ_VERSION aspectjweaver-1.6.12
ENV WFLOW_HOME /opt/joget/wflow/
ENV MYSQL_HOST localhost
ENV MYSQL_PORT 3306
ENV MYSQL_DATABASE jwdb
ENV MYSQL_USER root
ENV MYSQL_PASSWORD ""

# set java opts
ENV JAVA_OPTS -Xmx512m -Dwflow.home=${WFLOW_HOME}

# copy installer and scripts
COPY ${JOGET_VERSION}.tar.gz /opt/joget/
COPY run.sh /opt/joget/
RUN cd /opt/joget/ ; \
    tar xvfz ${JOGET_VERSION}.tar.gz ; \
    cd ${JOGET_VERSION}/ ; \
    mv *.* /opt/joget/ ; \
    mv * /opt/joget/ ; \
    rm -rf /opt/joget/${JOGET_VERSION}/ ; \
    rm /opt/joget/${JOGET_VERSION}.tar.gz

# Create an app user so our program doesn't run as root.
RUN \
    groupadd -r app ; \
    useradd -r -g app -d /home/app -c "Docker image user" app ; \
    echo "app:app" | chpasswd ; \
    usermod -aG 0 app ; \
    usermod -aG sudo app ; \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers ; \
    chmod +x /opt/joget/run.sh ; \
    chown -R app:0 /opt/joget ; \
    chmod -R g+rw /opt/joget

# Change to the app user.
USER app

# Expose the ports we're interested in
EXPOSE 8080

# Command to run
ENTRYPOINT ["/opt/joget/run.sh"]
CMD [""]

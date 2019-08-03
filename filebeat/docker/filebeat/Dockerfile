FROM docker.elastic.co/beats/filebeat:7.2.0

# The file to monitor the host is different from the file to monitor docker services.ADD
# So we pass the filename at build time to choose the target (host or services) of the image built.
ARG FILEBEAT_FILE=filebeat.yml
COPY ${FILEBEAT_FILE} /usr/share/filebeat/filebeat.yml

USER root

RUN yum -y install nc

RUN mkdir /var/log/filebeat \
    && chown filebeat /usr/share/filebeat/filebeat.yml \
    && chmod go-w /usr/share/filebeat/filebeat.yml \
    && chown filebeat /var/log/filebeat

USER filebeat
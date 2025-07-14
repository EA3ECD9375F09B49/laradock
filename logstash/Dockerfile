ARG ELK_VERSION
FROM logstash:${ELK_VERSION}

USER root
RUN rm -f /usr/share/logstash/pipeline/logstash.conf
RUN curl -L -o /usr/share/logstash/lib/mysql-connector-java-8.0.30.jar https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.30/mysql-connector-java-8.0.30.jar
ADD ./pipeline/ /usr/share/logstash/pipeline/

RUN logstash-plugin install logstash-integration-jdbc
RUN logstash-plugin install logstash-input-beats
RUN logstash-plugin install --no-verify logstash-output-jdbc


RUN mkdir -p /usr/share/logstash/logs \
    && chown -R logstash:logstash /usr/share/logstash/logs \
    && chmod -R 750 /usr/share/logstash/logs

# 添加自定义配置文件确保使用正确用户
RUN echo "LS_USER=logstash" >> /usr/share/logstash/config/startup.options \
    && echo "LS_GROUP=logstash" >> /usr/share/logstash/config/startup.options

USER logstash

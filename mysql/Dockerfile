ARG MYSQL_VERSION
FROM mysql:${MYSQL_VERSION}

LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"

#####################################
# Set Timezone
#####################################

ARG TZ=UTC
ENV TZ ${TZ}

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && chown -R mysql:root /var/lib/mysql/

COPY my.cnf /etc/mysql/conf.d/my.cnf

#RUN mv /var/lib/mysql /tmp/mysql
#
#RUN mkdir /var/lib/mysql


#RUN chown -R mysql:mysql /var/lib/mysql
RUN chmod -R 777 /var/lib/mysql/
RUN chmod 0444 /etc/mysql/conf.d/my.cnf

EXPOSE 3306

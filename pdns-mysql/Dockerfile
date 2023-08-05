FROM        registry.redhat.io/rhel8/mysql-80:1-263.1689584950
LABEL       description="MySQL Image for PDNS"
MAINTAINER  Brandon B. Jozsa<bjozsa@redhat.com>
ENV         MYSQL_PORT=3306 \
            MYSQL_DATABASE=pdns \
            MYSQL_USER=pdnsadmin \
            MYSQL_PASSWORD=pdnsadmin \
            MYSQL_ROOT_PASSWORD=CHANGEME \
            DB_DIR_IMPORT=/opt/pdns/mysql/init \
            SUMMARY="MySQL 8.0 SQL Database Server for PowerDNS" \
            DESCRIPTION="MySQL is a multi-user, multi-threaded SQL database server. For more \
information about the base image being used for this PDNS project, please see the following URL: \
https://catalog.redhat.com/software/containers/rhel8/mysql-80/5ba0ad4cdd19c70b45cbf48c"
EXPOSE      3306
USER        27
COPY        --chown=27:27 ./pdns-schema.sql /tmp/pdns-schema.sql
ENTRYPOINT  ["container-entrypoint"]
CMD         ["run-mysqld"]

FROM        quay.io/fedora/fedora:38
LABEL       description="PowerDNS Recursor Image"
MAINTAINER  Brandon B. Jozsa<bjozsa@redhat.com>
RUN         yum -yy install \
                pdns-recursor \
                pdns-backend-mysql \
                bind-utils && \
            yum clean all
RUN         mkdir -p /run/pdns-recursor
COPY        ./recursor.conf /etc/pdns-recursor/recursor.conf
EXPOSE      5300 5300/udp
ENTRYPOINT  ["/usr/sbin/pdns_recursor"]

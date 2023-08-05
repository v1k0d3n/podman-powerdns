FROM        quay.io/fedora/fedora:38
LABEL       description="PowerDNS Image"
MAINTAINER  Brandon B. Jozsa<bjozsa@redhat.com>
RUN         yum -yy install \
                pdns \
                pdns-backend-mysql \
                bind-utils && \
            yum clean all
RUN         mkdir -p /run/pdns-recursor
COPY        ./pdns.conf /etc/pdns/pdns.conf
EXPOSE      53 53/udp
ENTRYPOINT  ["/usr/sbin/pdns_server"]

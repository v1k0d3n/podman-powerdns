FROM        registry.redhat.io/ubi8:8.7-1090.1679482075
LABEL       description="PowerDNS Image"
MAINTAINER  Brandon B. Jozsa<bjozsa@redhat.com>
RUN         yum -yy install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm && \
            yum -yy install \
                pdns \
                pdns-backend-mysql \
                bind-utils && \
            yum clean all
EXPOSE      53 53/udp
ENTRYPOINT  ["/usr/sbin/pdns_server"]
# CMD       ["--config"]
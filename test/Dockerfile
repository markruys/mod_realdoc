FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y apache2 php && \
    rm -rf /var/lib/apt/lists/* && \
    apt clean

ADD libapache2-mod-realdoc_1.0-1focal1_amd64.deb /root
RUN dpkg -i /root/libapache2-mod-realdoc_1.0-1focal1_amd64.deb

COPY apache.conf /etc/apache2/mods-enabled/test.conf

WORKDIR /var/www

ADD variant-a.php variant-a/index.php
ADD variant-b.php variant-b/index.php
RUN rm -fr html && ln -s variant-a html

CMD [ "bash" ]

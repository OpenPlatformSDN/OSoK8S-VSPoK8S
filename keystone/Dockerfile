FROM ubuntu-base-os
MAINTAINER BBVA Innovation <eurocloud-oneteam.group@bbva.com>

# File Author / Maintainer
MAINTAINER Eurocloud

ENV OPENSTACK_VERSION=mitaka \
    PBR=9.0.1

# Install requriments and the main packages

RUN    curl -fSL https://github.com/openstack/keystone/archive/${PBR}.zip -o keystone-${PBR}.zip \
    && unzip keystone-${PBR}.zip \
    && cd keystone-${PBR} \
    && pip install -r requirements.txt \
    && PBR_VERSION=${PBR}  pip install . \
    && pip install uwsgi \
    && cp -r etc /etc/keystone \
    && mv /etc/keystone/keystone.conf.sample /etc/keystone/keystone.conf \
    && pip install python-openstackclient \
    && cd - \
    && rm -rf keystone-${PBR}*

ADD     data /

RUN     chown root:root /bootstrap/*.sh && chmod a+x /bootstrap/*.sh

ENTRYPOINT  ["/bootstrap/bootstrap.sh"]
EXPOSE      5000 35357


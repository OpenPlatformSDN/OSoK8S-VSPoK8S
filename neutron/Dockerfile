FROM    ubuntu-base-os
MAINTAINER BBVA Innovation <eurocloud-oneteam.group@bbva.com>


ENV OPENSTACK_VERSION=mitaka \
    PBR=8.1.2

RUN     apt-get update && \
        apt-get install -y iptables dnsmasq dnsmasq-utils arping coreutils && \
        rm -rf /var/lib/apt/lists/*

RUN	curl -fSL https://github.com/openstack/neutron/archive/${PBR}.zip -o neutron-${PBR}.zip \
    && unzip neutron-${PBR}.zip \
    && cd neutron-${PBR} \
    && pip install -r requirements.txt \
    && PBR_VERSION=${PBR}  pip install . \
    && PBR_VERSION=${PBR}  tox -egenconfig \
    && cp -r etc /etc/neutron \
    && mv /etc/neutron/neutron/* /etc/neutron/ \
    && mv /etc/neutron/neutron.conf.sample /etc/neutron/neutron.conf \
    && pip install python-openstackclient \
    && cd - \
    && rm -rf neutron-${PBR}*

ADD     data /

RUN	apt-get -y update \ 
     && dpkg -i  /Nuage-VSP/4.0R4/nuagenetlib_8.0.0-4.0.4-43_all.deb /Nuage-VSP/4.0R4/nuage-openstack-neutron_8.0.0-4.0.4-43_all.deb /Nuage-VSP/4.0R4/nuage-openstack-neutronclient_4.1.0-4.0.4-43_all.deb ; exit 0 

RUN  apt-get -fy install \
     && pip install configobj \
     && chown root:root /bootstrap/*.sh && chmod a+x /bootstrap/*.sh


ENTRYPOINT  ["/bootstrap/bootstrap.sh"]
EXPOSE 9696


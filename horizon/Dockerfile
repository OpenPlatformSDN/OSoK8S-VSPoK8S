FROM    ubuntu-base-os
MAINTAINER BBVA Innovation <eurocloud-oneteam.group@bbva.com>


ENV OPENSTACK_VERSION=mitaka \
    PBR=9.1.0

# Install requriments and the main packages
RUN     apt-get -y update && \
        apt-get install -y apache2 libapache2-mod-wsgi memcached gettext python-pbr python-babel && \
        rm -rf /var/lib/apt/lists/*


ADD     data /

RUN    curl -fSL https://github.com/openstack/horizon/archive/${PBR}.zip -o horizon-${PBR}.zip \
    && unzip horizon-${PBR}.zip \
    && curl -fSL https://bootstrap.pypa.io/get-pip.py -o get-pip.py \
    && python get-pip.py \
    && cd horizon-${PBR} \
    && pip install -r requirements.txt \
    && pip install python-memcached \
    && PBR_VERSION=${PBR}  pip install . \
    && mkdir -p /etc/openstack-dashboard/ \
    && mkdir -p /usr/share/openstack-dashboard \
    && ln -s /etc/apache2/sites-available/openstack-dashboard.conf /etc/apache2/sites-enabled/001-horizon.conf \
    && ln -s /etc/openstack-dashboard/local_settings /usr/local/lib/python2.7/dist-packages/openstack_dashboard/local/local_settings.py  \
    && cp -r /horizon-${PBR}/openstack_dashboard/conf/* /etc/openstack-dashboard/ \
    && cp /horizon-${PBR}/manage.py /usr/share/openstack-dashboard/manage.py \
    && cp -r /usr/local/lib/python2.7/dist-packages/openstack_dashboard/ /usr/share/openstack-dashboard/ \
    && python /usr/share/openstack-dashboard/manage.py collectstatic --noinput --clear \
    && cd /usr/share/openstack-dashboard/openstack_dashboard/ \
    && python /usr/share/openstack-dashboard/manage.py compilemessages \
    && ln -s /etc/openstack-dashboard/ /usr/share/openstack-dashboard/openstack_dashboard/conf \
    && cd /usr/local/lib/python2.7/dist-packages/horizon \
    && python /usr/share/openstack-dashboard/manage.py compilemessages \
    && pip install python-openstackclient \
    && mkdir /etc/apache2/run \
    && chown www-data:www-data /etc/apache2/run \
    && chown -R www-data:www-data /usr/share/openstack-dashboard/* \
    && cd - \
    && rm -rf horizon-${PBR}*

RUN dpkg -i  /Nuage-VSP/4.0R4/nuage-openstack-horizon_9.0.0-4.0.4-43_all.deb 

RUN     chown root:root /bootstrap/*.sh && chmod a+x /bootstrap/*.sh

ENTRYPOINT  ["/bootstrap/bootstrap.sh"]
EXPOSE 80


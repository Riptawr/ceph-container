# CEPH BASE IMAGE
# CEPH VERSION: Jewel
# CEPH VERSION DETAIL: 10.2.x

FROM BASEIMAGE
MAINTAINER Sébastien Han "seb@redhat.com"

ENV CEPH_VERSION jewel

# Install Ceph
ARG PACKAGES="unzip ceph-mon ceph-osd ceph-mds ceph-base ceph-common ceph-radosgw rbd-mirror device-mapper sharutils etcd kubernetes-client e2fsprogs wget"

# Add bootstrap script, ceph defaults key/values for KV store
ADD *.sh ceph.defaults check_zombie_mons.py ./osd_scenarios/* entrypoint.sh.in disabled_scenario /

RUN rpm --import 'https://download.ceph.com/keys/release.asc' && \
  rpm -Uvh http://download.ceph.com/rpm-${CEPH_VERSION}/el7/noarch/ceph-release-1-1.el7.noarch.rpm && \
  yum install -y epel-release && \
  yum clean all && \
  yum install -y $PACKAGES && \
  rpm -q $PACKAGES && \
  yum clean all && \
  bash /clean_container.sh && rm /clean_container.sh

# Modify the entrypoint
RUN bash "/generate_entrypoint.sh" && \
  rm -f /generate_entrypoint.sh && \
  bash -n /*.sh

# Add volumes for Ceph config and data
VOLUME ["/etc/ceph","/var/lib/ceph", "/etc/ganesha"]

# Execute the entrypoint
WORKDIR /
ENTRYPOINT ["/entrypoint.sh"]

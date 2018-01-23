# CEPH DAEMON IMAGE
# CEPH VERSION: Kraken
# CEPH VERSION DETAIL: 11.1.x

FROM BASEIMAGE
MAINTAINER Sébastien Han "seb@redhat.com"

ENV CONFD_VERSION 0.10.0
ENV KUBECTL_VERSION v1.6.0

ADD clean_container.sh /

# install prerequisites
RUN # Download & install confd \
  wget -O /usr/local/bin/confd "https://github.com/kelseyhightower/confd/releases/download/v${CONFD_VERSION}/confd-${CONFD_VERSION}-linux-amd64" && \
  chmod +x /usr/local/bin/confd && mkdir -p /etc/confd/conf.d && mkdir -p /etc/confd/templates && \
  # Install forego \
  wget -O /forego.tgz 'https://bin.equinox.io/c/ekMN3bCZFUn/forego-stable-linux-amd64.tgz' && \
  cd /usr/local/bin && tar xfz /forego.tgz && chmod +x /usr/local/bin/forego && rm /forego.tgz && \
  # Install kubectl \
  wget -O /usr/local/bin/kubectl "https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl" && \
  chmod +x /usr/local/bin/kubectl && \
  bash /clean_container.sh && rm -f /clean_container.sh

# Add templates for confd
ADD ./confd/templates/* /etc/confd/templates/
ADD ./confd/conf.d/* /etc/confd/conf.d/

# Execute the entrypoint
WORKDIR /
ENTRYPOINT ["/entrypoint.sh"]

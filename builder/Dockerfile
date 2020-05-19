ARG BUILDER_BASE_IMAGE=debian:buster-slim
FROM ${BUILDER_BASE_IMAGE}

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      apt-cacher-ng \
      btrfs-progs \
      ca-certificates \
      debootstrap \
      dpkg-dev \
      dumb-init \
      libguestfs-tools \
      mtree-netbsd \
      pigz \
      python3-dev \
      python3-pip \
      python3-setuptools \
      python3-venv \
      python3-wheel \
      squashfs-tools \
      syslinux \
      syslinux-efi \
      uuid-runtime

RUN apt-get install -y --no-install-recommends linux-image-"$(dpkg-architecture -q DEB_BUILD_ARCH)"

RUN rmdir /var/cache/apt-cacher-ng && ln -s /mnt/cache/apt-cacher-ng /var/cache/apt-cacher-ng

RUN python3 -m venv --system-site-packages /usr/lib/caketop-builder
COPY requirements.txt /usr/lib/caketop-builder/
RUN /usr/lib/caketop-builder/bin/pip3 install -r /usr/lib/caketop-builder/requirements.txt
ENV PATH=/usr/lib/caketop-builder/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

COPY ansible_hosts /etc/ansible/hosts
COPY builder-playbook.yml /usr/lib/caketop-builder/ansible/builder-playbook.yml
COPY caketop-builder /usr/lib/caketop-builder/bin/
COPY roles/ /usr/lib/caketop-builder/ansible/roles/
COPY builders/ /usr/lib/caketop-builder/ansible/builders/

ENV ANSIBLE_PIPELINING=True \
    ANSIBLE_STDOUT_CALLBACK=yaml \
    ANSIBLE_CALLBACK_WHITELIST=profile_roles,profile_tasks \
    ANSIBLE_ROLES_PATH=/mnt/workdir/roles:/usr/lib/caketop-builder/ansible/builders:/usr/lib/caketop-builder/ansible/roles \
    ANSIBLE_STRATEGY_PLUGINS=/usr/lib/caketop-builder/lib/python3.7/site-packages/ansible_mitogen/plugins/strategy \
    ANSIBLE_STRATEGY=mitogen_linear

VOLUME [ "/mnt/cache", "/mnt/workdir" ]
WORKDIR /mnt/workdir
ENTRYPOINT [ "/usr/bin/dumb-init" ]
CMD [ "caketop-builder"]

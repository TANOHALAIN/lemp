FROM ubuntu:focal

RUN apt-get update; \
    apt-get -y install  openssh-server passwd sudo nano nginx python3-certbot-nginx; \
    apt-get -qq clean; \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN apt-get update && apt-get install -y iproute2 git curl iputils-ping net-tools wget curl
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
    rm -f /lib/systemd/system/multi-user.target.wants/*; \
    rm -f /etc/systemd/system/*.wants/*; \
    rm -f /lib/systemd/system/local-fs.target.wants/*; \
    rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
    rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
    rm -f /lib/systemd/system/basic.target.wants/*; \
    rm -f /lib/systemd/system/anaconda.target.wants/*;

RUN systemctl enable ssh.service;

RUN systemctl enable nginx;

RUN useradd --create-home -s /bin/bash vagrant; \
    echo -e "vagrant\nvagrant" | (passwd --stdin vagrant); \
    echo 'vagrant ALL=(ALL) NOPASSWD: ALL' > /etc/sudoers.d/vagrant; \
    chmod 440 /etc/sudoers.d/vagrant

RUN mkdir -p /home/vagrant/.ssh; \
    chmod 700 /home/vagrant/.ssh
ADD https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub /home/vagrant/.ssh/authorized_keys
RUN chmod 600 /home/vagrant/.ssh/authorized_keys; \
    chown -R vagrant:vagrant /home/vagrant/.ssh

COPY lb.conf /etc/nginx/conf.d

RUN mkdir -p /run/php \
    chown www-data:www-data /run/php

VOLUME [ "/sys/fs/cgroup" ]

CMD ["/usr/sbin/init"]


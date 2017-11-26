FROM ubuntu:16.04

MAINTAINER Dmitry Sosonny <dissnn@gmail.com>

ENV DEBIAN_FRONTEND noninteractive

# install common dependencies
RUN apt-get update && apt-get install -y \
    locales \
    curl \
    lsb-release \
    openssh-server \
    sudo \
    python

# ensure we have the en_US.UTF-8 locale available
RUN locale-gen en_US.UTF-8

# setup the ubuntu user
RUN if ! getent passwd ubuntu; then useradd -d /home/ubuntu -m -s /bin/bash ubuntu; fi \
    && echo ubuntu:ubuntu | chpasswd \
    && echo 'ubuntu ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers \
    && mkdir -p /etc/sudoers.d \
    && echo 'ubuntu ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers.d/ubuntu \
    && chmod 0440 /etc/sudoers.d/ubuntu

# add the ubuntu insecure public key
RUN mkdir -p /home/ubuntu/.ssh \
    && chmod 0700 /home/ubuntu/.ssh \
    && wget --no-check-certificate \
      https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub \
      -O /home/ubuntu/.ssh/authorized_keys \
    && chmod 0600 /home/ubuntu/.ssh/authorized_keys \
    && chown -R ubuntu /home/ubuntu/.ssh

# don't clean packages, we might be using vagrant-cachier
RUN rm /etc/apt/apt.conf.d/docker-clean

# run sshd in the foreground
CMD /usr/sbin/sshd -D \
    -o UseDNS=no\
    -o UsePAM=no\
    -o PasswordAuthentication=yes\
    -o UsePrivilegeSeparation=no\
    -o PidFile=/tmp/sshd.pid

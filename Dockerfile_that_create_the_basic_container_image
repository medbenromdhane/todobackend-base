FROM centos

ENV container docker
RUN yum -y update; yum clean all
RUN yum -y install systemd; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ “/sys/fs/cgroup” ]
CMD [“/usr/sbin/init”]

# Install Python runtime
RUN yum -y install python python-virtualenv python-devel python-libs-2.7* MySQL-python-*

# Create virtual environment
# Upgrade PIP in virtual environment to latest version
RUN virtualenv /appenv && \
    . /appenv/bin/activate && \
    pip install pip --upgrade

# Add entrypoint script
ADD scripts/entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]

LABEL application=todobackend

# ----------------------------------------------------------------------------- 
# Base Install + Import the RPM GPG keys for Repositories 
# ----------------------------------------------------------------------------- 

# RUN rpm --rebuilddb \
# && rpm --import \
# http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-7 \
# && rpm --import \
# https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7 \
# && rpm --import \
# https://dl.iuscommunity.org/pub/ius/IUS-COMMUNITY-GPG-KEY \
# && yum -y install \
# --setopt=tsflags=nodocs \
# --disableplugin=fastestmirror \
# centos-release-scl \
# centos-release-scl-rh \
# epel-release \
# https://centos7.iuscommunity.org/ius-release.rpm \
# openssh-clients-7.4p1-16.el7 \
# openssh-server-7.4p1-16.el7 \
# openssl-1.0.2k-12.el7 \
# python-setuptools-0.9.8-7.el7 \
# sudo-1.8.19p2-14.el7_5 \
# yum-plugin-versionlock-1.1.31-46.el7_5 \
# && yum versionlock add \
# openssh \
# openssh-server \
# openssh-clients \
# python-setuptools \
# sudo \
# yum-plugin-versionlock \
# && yum clean all \
# && rm -rf /etc/ld.so.cache \
# && rm -rf /sbin/sln \
# && rm -rf /usr/{{lib,share}/locale,share/{man,doc,info,cracklib,i18n},{lib,lib64}/gconv,bin/localedef,sbin/build-locale-archive} \
# && rm -rf /{root,tmp,var/cache/{ldconfig,yum}}/* \
# && > /etc/sysconfig/i18n
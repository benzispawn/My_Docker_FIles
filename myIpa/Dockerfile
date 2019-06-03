FROM centos:7

MAINTAINER "Raphael Carloni Benzi"

ENV container docker

RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

RUN yum -y update \
    && yum -y install systemd \
    ipa-server bind-dynbd-ldap \
    ipa-sever-dns \
    && yum clean all

WORKDIR /bin

RUN hostname set-hostname ipa.centos.local
RUN rm -rf /etc/hosts
COPY ["hosts", "/etc/hosts"]

VOLUME ["/sys/fs/cgroup/"]

CMD ["/usr/sbin/init"]

FROM centos:7

MAINTAINER "Raphael Carloni Benzi"

ENV container docker

LABEL org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20190305"
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

#COPY ["hosts", "/etc/hosts"]
#RUN hostname -f
RUN yum -y update \
    && yum -y install systemd \
    ntp \
    dnsmasq \
    bind \
    bind-utils \
    wget\
    && yum clean all
RUN systemctl enable named

ADD ["webmin.repo","/etc/yum.repos.d/"]
WORKDIR /root
RUN wget http://www.webmin.com/jcameron-key.asc \
    && rpm --import jcameron-key.asc
RUN yum -y install webmin
RUN echo "0ok9ij8uh7yg" | passwd --stdin root

WORKDIR /bin

VOLUME ["/sys/fs/cgroup"]

CMD ["/usr/sbin/init"]

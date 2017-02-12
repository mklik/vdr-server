#
# VDR-Server Dockerfile
#
# https://github.com/mklik/vdr-server
#

# Pull base image.
FROM ubuntu:16.04

MAINTAINER mklik <markus.klik@gmx.de>

WORKDIR /tmp

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 272A2CE18103B360 F529E113D0A5897C

RUN apt-get update -qq && \
  apt-get install -qy \
  wget \
  vdr \
  rsyslog \
  vdr-plugin-satip \
  vdr-plugin-live \
  joe && \
  apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

COPY vdr.sh /etc/services.d/vdr/run
COPY vdr-finish.sh /etc/services.d/vdr/finish
COPY rsyslog.sh /etc/services.d/rsyslog/run

# Configure the vdr user account and it's folders
RUN groupmod -o -g 666 vdr \
  && usermod -o -u 666 vdr \
  && install -o vdr -g vdr -d /recordings /vdr/config /vdr/cache
 
# add configs
COPY etc/vdr/conf.d/* /etc/vdr/conf.d/
COPY /etc/vdr/svdrphosts.conf /etc/vdr/svdrphosts.conf 
#COPY var/lib/vdr/* /var/lib/vdr/
#COPY var/lib/vdr/plugins/streamdev-server/* /var/lib/vdr/plugins/streamdev-server/
#COPY etc/rsyslog.conf /etc/

EXPOSE 2004 3000 6419 8002 8008 4010-4020/udp 34890
VOLUME /etc/vdr /var/lib/vdr /var/lib/video
CMD ["/bin/bash"]

FROM tobilg/mesos:0.24.0-rc1
MAINTAINER tobilg <fb.tools.github@gmail.com>

# Mesos fetcher uses unzip to extract staged zip files

RUN \
  apt-get install -y unzip docker.io && \
  apt-get clean

EXPOSE 5051
CMD ["mesos-slave"]